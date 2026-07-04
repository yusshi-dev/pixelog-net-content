---
title: クリックで開閉するドロップダウンメニューをCSSとJavaScriptで作ってみよう
date: 2020-04-17 20:10:00
post_id: rb8orj
categories:
  - 開発
tags:
  - Web制作
  - JavaScript
  - CSS
  - HTML
---

目に見えるところに貼るほど重要ではないけど、開閉メニューでリンクを格納しておきたいというケースがあるかと思います。

<!-- more -->

今回はボタンクリックで開閉するシンプルなドロップダウンメニューを、HTML + CSS + JavaScriptで簡単につくる方法を順を追って紹介したいと思います。


## STEP1 メニュー部分を作る

まず、メニューを開閉させることはいったん忘れて、メニューとボタン部分をHTMLとCSSで作ります。メニューが開いた状態をイメージしてデザインしましょう。


<div class="dropdown step1">
  <button class="dropdown__btn">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><circle cx="256" cy="256" r="64"/><circle cx="256" cy="448" r="64"/><circle cx="256" cy="64" r="64"/></svg>
  </button>
  <div class="dropdown__body">
    <ul class="dropdown__list">
        <li class="dropdown__item"><a href="https://www.google.com/" class="dropdown__item-link">Google</a></li>
        <li class="dropdown__item"><a href="https://www.yahoo.co.jp/" class="dropdown__item-link">Yahoo! JAPAN</a></li>
        <li class="dropdown__item"><a href="https://www.bing.com/" class="dropdown__item-link">Bing</a></li>
    </ul>
  </div>
</div>


アイコンは、よく見るものをSVGで再現してみました。ボタンの中はテキストでも画像でもFont Awesomeでも何でもOKです。

```html
<div class="dropdown">
  <button class="dropdown__btn" id="dropdown__btn">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><circle cx="256" cy="256" r="64"/><circle cx="256" cy="448" r="64"/><circle cx="256" cy="64" r="64"/></svg>
  </button>
  <div class="dropdown__body">
    <ul class="dropdown__list">
      <li class="dropdown__item"><a href="https://www.google.com/" class="dropdown__item-link">Google</a></li>
      <li class="dropdown__item"><a href="https://www.yahoo.co.jp/" class="dropdown__item-link">Yahoo! JAPAN</a></li>
      <li class="dropdown__item"><a href="https://www.bing.com/" class="dropdown__item-link">Bing</a></li>
    </ul>
  </div>
</div>
```

HTMLはいろいろな構造が考えられますが、今回はボタンと本体部分を子要素としてdivで覆ってみます。`button`は後でJavaScriptでも操作するので、idでネーミングしておきます。

```css
.dropdown {
  margin: 0 0 0 auto;
  position: relative; /* 絶対配置の基準を設定 */
}
.dropdown__btn {
  display: block;
  width: 1.5em;
  padding: 0.2em;
  background: none; /* デフォルトスタイル リセット */
  border: 0; /* デフォルトスタイル リセット */
}
.dropdown__body {
  width: 250px;
  background: #fff;
  box-shadow: 0 2px 6px 2px rgba(60,64,67,0.149), 0 1px 2px 0 rgba(60,64,67,0.302);
  border-radius: 4px;
  overflow: hidden;
  position: absolute; /* メニュー部分は絶対配置させます */
  top: 32px;
  left: 0;
}
.dropdown__list { /* デフォルトスタイル リセット */
  margin: 0;
  padding: 0;
}
.dropdown__item {
  display: block;
}
.dropdown__item-link {
  display: block;
  padding: 1em 1.2em;
  font-size: 0.95em;
  text-decoration: none;
  color: #444;
}
.dropdown__item-link:hover {
  background: #fbfbfb;
}
```

### ポイント

CSSを書くときの注意点として、メニュー本体部分は`position: absolute`で **絶対配置させるようにしてください。** メニューは他の部分と重なって表示したいので、要素を浮かせる必要があります。

そのとき`.dropdown`に`position: relative`を指定して、絶対配置の基準をボタンと本体を包むdivに設定します。基準の指定を怠ると、ページの一番上など思わぬ場所にメニューが表示される、という結果を招きかねません！



## STEP2 JavaScriptで開閉させてみる

メニュー部分がデザインできたら次はJavaScriptを使ってメニューを開閉させてみましょう。

一気に難しくなるように思えるかもしれませんが、JavaScriptで行うことは、`is-open`というclassをボタンクリックに応じて付け外しするだけです。


```javascript
(function () {
  document.addEventListener('DOMContentLoaded', function() { // HTML解析が終わったら
    const btn = document.getElementById('dropdown__btn'); // ボタンをidで取得
    if(btn) { // ボタンが存在しないときにエラーになるのを回避
      btn.addEventListener('click', function(){ //ボタンがクリックされたら
        this.classList.toggle('is-open'); // is-openを付加する
      });
    }
  });
}());
```


### .is-openに合わせてCSSを書く

スクリプトが正しく動作するのが確認できたら、`.is-open`に合わせてCSSを書きます。

普段は`display: none`で非表示にしておいて、`.is-open`が付加されたら`display: block`で表示させるという簡単な仕組みです。

```css
.dropdown__body {
  display: none;
}
.dropdown__btn.is-open + .dropdown__body {
  display: block;
  z-index: 4;
}
```


↓ こんな感じのメニューが表示されたら正解です。ボタンクリックでちゃんと開閉しますね！

<div class="dropdown step2">
  <button class="dropdown__btn" id="step2">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><circle cx="256" cy="256" r="64"/><circle cx="256" cy="448" r="64"/><circle cx="256" cy="64" r="64"/></svg>
  </button>
  <div class="dropdown__body">
    <ul class="dropdown__list">
      <li class="dropdown__item"><a href="https://www.google.com/" class="dropdown__item-link">Google</a></li>
      <li class="dropdown__item"><a href="https://www.yahoo.co.jp/" class="dropdown__item-link">Yahoo! JAPAN</a></li>
      <li class="dropdown__item"><a href="https://www.bing.com/" class="dropdown__item-link">Bing</a></li>
    </ul>
  </div>
</div>



## STEP3 どこをクリックしても閉じられるようにする

これで完成にも思えますが、このメニューは開いた後、もう一度同じボタンをクリックしないと閉じられません。

やはりユーザビリティを考えると、メニューを開いた後は画面のどこをクリックしても閉じられるようにしたいですね。

実はこの問題はJavaScriptに手を加えなくても、CSSだけで解決可能です。

```css
.dropdown__btn.is-open::after {
  content: "";
  width: 100%;
  height: 100%;
  cursor: default; /* カーソルはデフォルトに戻しておく */
  position: fixed;
  top: 0;
  left: 0;
  z-index: 5;
}
```

ボタンの疑似要素にも当たり判定はある、という特性を活かして、メニューが開いている間だけ透明の疑似要素を画面いっぱいに敷き詰めます。

これで画面全体がボタンの反応エリアとなり、メニューを閉じることが可能になります。疑似要素は`z-index`でメニュー部分より下になるようにしておきます。疑似要素が上になってしまうとメニューがクリックできなくなります。

間違っても`z-index: 2147483647`などと指定してz-index戦争―――いわば空中戦を勃発させないように。（ちゃんと設計すれば値は大概1桁で事足ります）


## 完成品

STEP1～3をまとめたものです。この記事をご覧いただいたお土産にお持ち帰りください。

### デモ

<div class="dropdown">
  <button class="dropdown__btn" id="dropdown__btn">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><circle cx="256" cy="256" r="64"/><circle cx="256" cy="448" r="64"/><circle cx="256" cy="64" r="64"/></svg>
  </button>
  <div class="dropdown__body">
    <ul class="dropdown__list">
      <li class="dropdown__item"><a href="https://www.google.com/" class="dropdown__item-link">Google</a></li>
      <li class="dropdown__item"><a href="https://www.yahoo.co.jp/" class="dropdown__item-link">Yahoo! JAPAN</a></li>
      <li class="dropdown__item"><a href="https://www.bing.com/" class="dropdown__item-link">Bing</a></li>
    </ul>
  </div>
</div>


### ソース


```html
<div class="dropdown">
  <button class="dropdown__btn" id="dropdown__btn">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><circle cx="256" cy="256" r="64"/><circle cx="256" cy="448" r="64"/><circle cx="256" cy="64" r="64"/></svg>
  </button>
  <div class="dropdown__body">
    <ul class="dropdown__list">
      <li class="dropdown__item"><a href="https://www.google.com/" class="dropdown__item-link">Google</a></li>
      <li class="dropdown__item"><a href="https://www.yahoo.co.jp/" class="dropdown__item-link">Yahoo! JAPAN</a></li>
      <li class="dropdown__item"><a href="https://www.bing.com/" class="dropdown__item-link">Bing</a></li>
    </ul>
  </div>
</div>
```

```css
.dropdown {
  margin: 0 0 0 auto;
  position: relative;
}
.dropdown__btn {
  display: block;
  width: 1.5em;
  padding: 0.2em;
  background: none;
  border: 0;
}
.dropdown__btn.is-open::after {
  content: "";
  width: 100%;
  height: 100%;
  cursor: default;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 4;
}
.dropdown__btn.is-open + .dropdown__body {
  display: block;
  z-index: 5;
}
.dropdown__body {
  display: none;
  width: 250px;
  background: #fff;
  box-shadow: 0 2px 6px 2px rgba(60,64,67,0.149), 0 1px 2px 0 rgba(60,64,67,0.302);
  border-radius: 4px;
  overflow: hidden;
  position: absolute;
  top: 32px;
  left: 0;
}
.dropdown__list {
  margin: 0;
  padding: 0;
}
.dropdown__item {
  display: block;
}
.dropdown__item-link {
  display: block;
  padding: 1em 1.2em;
  font-size: 0.95em;
  text-decoration: none;
  color: #444;
}
.dropdown__item-link:hover {
  background: #fbfbfb;
}
```

```javascript
(function () {
  document.addEventListener('DOMContentLoaded', function() {
    const btn = document.getElementById('dropdown__btn');
    if(btn) {
      btn.addEventListener('click', function(){
        this.classList.toggle('is-open');
      });
    }
  });
}());
```


<style>
.dropdown {
  margin: 0 0 0 auto;
  position: relative;
}
.dropdown__btn {
  display: block;
  width: 1.5em;
  padding: 0.2em;
}
.dropdown__btn.is-open::after {
  content: "";
  width: 100%;
  height: 100%;
  cursor: default;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9;
}
.dropdown__btn.is-open + .dropdown__body {
  display: block;
  z-index: 10;
}
.dropdown__body {
  display: none;
  width: 250px;
  background: var(--bg-sub-color);
  box-shadow: 0 2px 6px 2px rgba(60,64,67,0.149), 0 1px 2px 0 rgba(60,64,67,0.302);
  border-radius: 4px;
  overflow: hidden;
  position: absolute;
  top: 32px;
  left: 0;
}
.dropdown__list {
  margin: 0!important;
}
.dropdown__item {
  display: block;
  margin: 0!important;
}
.dropdown__item-link {
  display: block;
  padding: 1em 1.2em;
  font-size: 0.95em;
  text-decoration: none!important;
  color: var(--text-color)!important;
}
.dropdown__item-link:hover {
  background: var(--bg-hover-color);
}


.dropdown.step1,
.dropdown.step2 {
  margin: 4em 0;
}

.dropdown.step1 .dropdown__body {
  display: block;
  position: relative;
  top: 16px;
  left: 0;
}

.dropdown.step2 .dropdown__btn.is-open::after {
  display: none;
}
</style>
<script>
(function () {
  const btn2 = document.getElementById('step2');
  if(btn2) {
    btn2.addEventListener('click', function(){
      this.classList.toggle('is-open');
    });
  }

  const btn = document.getElementById('dropdown__btn');
  if(btn) {
    btn.addEventListener('click', function(){
      this.classList.toggle('is-open');
    });
  }
}());
</script>
