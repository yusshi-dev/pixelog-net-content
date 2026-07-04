---
title: クリックで画像を拡大するプラグイン(Lightbox)をjQueryなしで自作してみる
date: 2020-02-11 16:00:00
updated: 2020-04-19 12:40:00
post_id: c73ni7
categories:
  - 開発
tags:
  - Web制作
  - JavaScript
  - HTML
  - CSS
---

Lightboxという単語は死語感が漂っており、今どき需要もなさそうですが...。

<!--more-->

というのはさておき、このブログで画像拡大プラグインを導入するにあたり、Photoswipeが候補に挙がったのですが、HTMLの構造の条件が厳しいのと、お手軽感がないのとで、結局自前で実装するに至りました。

## デモ

下の写真をクリック/タップしてみてください。モーダルウインドウが開いて画像が拡大表示されます。モーダルウインドウは、ウインドウ内をクリック/タップ、スクロールで閉じることができます。

![桜](/post/kqkyf5/sakura.jpg)

![スイセン](/post/kqkyf5/suisen.jpg)


## 仕様

- imgタグのsrc属性に指定されている画像を原寸大、もしくは画面いっぱいまで拡大します。HTMLに特別な構造、セレクタは必要ありません。
- 画像を拡大した際にaltに指定した代替テキストを左上に表示します。
- アニメーションは全てCSSに押し付けているので、柔軟にカスタマイズできます。
- IEは全バージョン対応していません（縦横比がおかしくなる or 動かない）。ご了承ください。

## コード

### CSS

Stylusで書いたものと、コンパイル済みのものを置いておきます。お好きな方をお使いください。

```stylus
#pixel-viewer
    display flex
    flex-direction column
    width 100%
    height 100%
    background rgba(0, 0, 0, .9)
    position fixed
    top 0
    left 0
    z-index 10
    animation fadein .2s ease-out both
    cursor pointer
    user-select none
    -webkit-tap-highlight-color transparent
    tap-highlight-color transparent

    &.fadeout
        animation fadeout .2s ease-out both
        pointer-events none /* ← IE救済措置（IEでこれがないと画像を閉じた後にページ全体がクリックできなくなります...）*/

        & #pixel-viewer__img
            animation scaledown .2s ease-out both

#pixel-viewer__alt
    width: 100%
    height 44px
    line-height 44px
    padding 0 48px 0 12px
    background rgba(0, 0, 0, .95)
    color #eee
    font-size 14px
    text-overflow ellipsis /* スマホなどでaltが横幅をはみ出たときに省略します */
    white-space nowrap
    overflow hidden

    &::before
    &::after
        content ""
        dispaly block
        width 24px
        height 3px
        background #eee
        border-radius 4px
        position fixed
        top 20px
        right 12px
        z-index 12
    &::before
        transform rotate(45deg)
    &::after
        transform rotate(-45deg)

#pixel-viewer__img
    display block
    margin auto
    max-width 100%
    max-height calc(100% - 44px)
    animation scaleup .2s ease-out both

@keyframes fadein
    0%
        opacity 0
    100%
        opacity 1

@keyframes scaleup
    0%
        transform scale(.95)
    100%
        transform scale(1)

@keyframes fadeout
    0%
        opacity 1
    100%
        opacity 0

@keyframes scaledown
    0%
        transform scale(1)
    100%
        transform scale(.95)
```

```css
#pixel-viewer {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.9);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 10;
  animation: fadein 0.2s ease-out both;
  cursor: pointer;
  user-select: none;
  -webkit-tap-highlight-color: transparent;
  tap-highlight-color: transparent;
}
#pixel-viewer.fadeout {
  animation: fadeout 0.2s ease-out both;
  pointer-events: none /* ← IE救済措置（IEでこれがないと画像を閉じた後にページ全体がクリックできなくなりますｗ）*/;
}
#pixel-viewer.fadeout #pixel-viewer__img {
  animation: scaledown 0.2s ease-out both;
}
#pixel-viewer__alt {
  width: 100%;
  height: 44px;
  line-height: 44px;
  padding: 0 48px 0 12px;
  background: rgba(0,0,0,0.95);
  color: #eee;
  font-size: 14px;
  text-overflow: ellipsis /* スマホなどでaltが横幅をはみ出たときに省略します */;
  white-space: nowrap;
  overflow: hidden;
}
/* 閉じるボタン */
#pixel-viewer__alt::before,
#pixel-viewer__alt::after {
  content: "";
  dispaly: block;
  width: 24px;
  height: 3px;
  background: #eee;
  border-radius: 4px;
  position: fixed;
  top: 20px;
  right: 12px;
  z-index: 12;
}
#pixel-viewer__alt::before {
  transform: rotate(45deg);
}
#pixel-viewer__alt::after {
  transform: rotate(-45deg);
}
#pixel-viewer__img {
  display: block;
  margin: auto;
  max-width: 100%;
  max-height: calc(100% - 44px);
  animation: scaleup 0.2s ease-out both;
}
@keyframes fadein {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
@keyframes scaleup {
  0% {
    transform: scale(0.95);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes fadeout {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
@keyframes scaledown {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(0.95);
  }
}
```


### JS

```javascript
function pixelViewer(element){
    const img = document.querySelectorAll(element);

    for(let i=0; i < img.length; i++){
        img[i].addEventListener('click', open);
    }

    function open(){
        const filter = document.createElement('div');
        filter.id = 'pixel-viewer';

        const div_alt = document.createElement('div');
        div_alt.id = 'pixel-viewer__alt';
        div_alt.textContent = this.alt;

        const div_img = document.createElement('img');
        div_img.id = 'pixel-viewer__img';
        div_img.src = this.src;

        document.body.appendChild(filter);
        filter.appendChild(div_alt);
        filter.appendChild(div_img);

        filter.addEventListener('click', close, {once: true});
        window.addEventListener('scroll', close, {once: true}); // スクロールで閉じたくない場合はこの行を削除

        function close(){
            filter.className = 'fadeout';
            filter.addEventListener("animationend",function(){
                filter.remove();
            });
        }
    }
}

```

```javascript
pixelViewer('p > img');
```

引数に、記事内のimgタグを全て取得できるようなセレクタを指定してください。`querySelectorAll`なのでCSSと同じ感覚で書いて大丈夫です。

aタグの中に入った画像は対応していません。子セレクタなどを使って、リンクの中の画像を拾わないようにうまく指定してください。

### HTML（記事内）

```html
<p><img src="/post/only-css-popup-image/suisen.jpg" alt="スイセン"></p>
```

上のような感じで、基本的に何もしなくて大丈夫です。


## alt表示なし版

左上のalt表示がないバージョンです。

```javascript
function pixelViewer(element){
  const img = document.querySelectorAll(element);

  for(let i=0; i < img.length; i++){
    img[i].addEventListener('click', open);
  }

  function open(){
    const filter = document.createElement('div');
    filter.id = 'pixel-viewer';

    const div_img = document.createElement('img');
    div_img.id = 'pixel-viewer__img';
    div_img.src = this.src;

    document.body.appendChild(filter);
    filter.appendChild(div_img);

    filter.addEventListener('click', close, {once: true});
    window.addEventListener('scroll', close, {once: true}); // スクロールで閉じたくない場合はこの行を削除

    function close(){
      filter.className = 'fadeout';
      filter.addEventListener("animationend",function(){
          filter.remove();
      });
    }
  }
}
pixelViewer('p > img');
```

```stylus
#pixel-viewer
    display flex
    flex-direction column
    width 100%
    height 100%
    background rgba(0,0,0,0.9)
    position fixed
    top 0
    left 0
    z-index 10
    animation fadein .2s ease-out both
    cursor pointer
    user-select none
    -webkit-tap-highlight-color transparent
    tap-highlight-color transparent

    &.fadeout
        animation fadeout .2s ease-out both
        pointer-events none

        & #pixel-viewer__img
            animation scaledown .2s ease-out both


    &::before
    &::after
        content ""
        dispaly block
        width 24px
        height 3px
        background #eee
        border-radius 4px
        position fixed
        top 20px
        right 12px
        z-index 12
    &::before
        transform rotate(45deg)
    &::after
        transform rotate(-45deg)

#pixel-viewer__img
    display block
    margin auto
    max-width 100%
    max-height calc(100% - 44px)
    animation scaleup .2s ease-out both

@keyframes fadein
    0%
        opacity 0
    100%
        opacity 1

@keyframes scaleup
    0%
        transform scale(.97)
    100%
        transform scale(1)

@keyframes fadeout
    0%
        opacity 1
    100%
        opacity 0

@keyframes scaledown
    0%
        transform scale(1)
    100%
        transform scale(.97)
```

```css
#pixel-viewer {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.9);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 10;
  animation: fadein 0.2s ease-out both;
  cursor: pointer;
  user-select: none;
  -webkit-tap-highlight-color: transparent;
  tap-highlight-color: transparent;
}
#pixel-viewer.fadeout {
  animation: fadeout 0.2s ease-out both;
  pointer-events: none /* ← IE救済措置（IEでこれがないと画像を閉じた後にページ全体がクリックできなくなります...）*/;
}
#pixel-viewer.fadeout #pixel-viewer__img {
  animation: scaledown 0.2s ease-out both;
}
#pixel-viewer::before,
#pixel-viewer::after {
  content: "";
  dispaly: block;
  width: 24px;
  height: 3px;
  background: #eee;
  border-radius: 4px;
  position: fixed;
  top: 20px;
  right: 12px;
  z-index: 12;
}
#pixel-viewer::before {
  transform: rotate(45deg);
}
#pixel-viewer::after {
  transform: rotate(-45deg);
}
#pixel-viewer__img {
  display: block;
  margin: auto;
  max-width: 100%;
  max-height: calc(100% - 44px);
  animation: scaleup 0.2s ease-out both;
}
@keyframes fadein {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
@keyframes scaleup {
  0% {
    transform: scale(0.97);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes fadeout {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
@keyframes scaledown {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(0.97);
  }
}
```


## まとめ

素人設計ですが使いたいという方がいらっしゃいましたら、ご自由にお使いください。

このプラグインをJavaScriptを使わずCSSのみで再現するというお遊び記事も書いてるので、実用性はありませんが、よろしければご覧ください。

[CSSのみでクリックで画像を拡大する（Lightboxもどき）](/post/kqkyf5/)


<style>
p > img {
  cursor: zoom-in;
}

#pixel-viewer {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.9);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 10;
  animation: fadein 0.2s ease-out both;
  cursor: pointer;
  user-select: none;
  -webkit-tap-highlight-color: transparent;
  tap-highlight-color: transparent;
}
#pixel-viewer.fadeout {
  animation: fadeout 0.2s ease-out both;
  pointer-events: none;
}
#pixel-viewer.fadeout #pixel-viewer__img {
  animation: scaledown 0.2s ease-out both;
}
#pixel-viewer__alt {
  width: 100%;
  height: 44px;
  line-height: 44px;
  padding: 0 48px 0 12px;
  background: rgba(0,0,0,0.95);
  color: #eee;
  font-size: 14px;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}
#pixel-viewer__alt::before,
#pixel-viewer__alt::after {
  content: "";
  dispaly: block;
  width: 24px;
  height: 3px;
  background: #eee;
  border-radius: 4px;
  position: fixed;
  top: 20px;
  right: 12px;
  z-index: 12;
}
#pixel-viewer__alt::before {
  transform: rotate(45deg);
}
#pixel-viewer__alt::after {
  transform: rotate(-45deg);
}
#pixel-viewer__img {
  display: block;
  margin: auto;
  max-width: 100%;
  max-height: calc(100% - 44px);
  animation: scaleup 0.2s ease-out both;
}
@keyframes fadein {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
@keyframes scaleup {
  0% {
    transform: scale(0.95);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes fadeout {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
@keyframes scaledown {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(0.95);
  }
}
</style>
<script>
function pixelViewer(element){
    const img = document.querySelectorAll(element);

    for(let i=0; i < img.length; i++){
        img[i].addEventListener('click', open);
    }

    function open(){
        const filter = document.createElement('div');
        filter.id = 'pixel-viewer';

        const div_alt = document.createElement('div');
        div_alt.id = 'pixel-viewer__alt';
        div_alt.textContent = this.alt;

        const div_img = document.createElement('img');
        div_img.id = 'pixel-viewer__img';
        div_img.src = this.src;

        document.body.appendChild(filter);
        filter.appendChild(div_alt);
        filter.appendChild(div_img);

        filter.addEventListener('click', close, {once: true});
        window.addEventListener('scroll', close, {once: true}); // スクロールで閉じたくない場合はこの行を削除

        function close(){
            filter.className = 'fadeout';
            filter.addEventListener("animationend",function(){
                filter.remove();
            });
        }
    }
}

pixelViewer('p > img');
</script>
