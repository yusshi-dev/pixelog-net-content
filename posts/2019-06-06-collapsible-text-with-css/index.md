---
title: CSSだけで文章を折りたたむ（続きを読む）その1
date: 2019-06-06 21:38:40
post_id: sh4gsv
categories:
  - 開発
tags:
  - Web制作
  - HTML
  - CSS
---

今回はHTMLとCSSのみでコンテンツの開閉、いわゆる「続きを読む」を再現してみます。

<!--more-->

CSSだけでクリックイベントを扱うには、主にtargetを使う方法とチェックボックスを使う方法がありますが、今回は`:target`の方を掘り下げてみたいと思います。



## パターン1（原型）

普段は非表示にしておいてターゲットになったら表示させる、という最も基本的な形です。

開閉するときに画面がガタつくという弱点があります...(´・ω・\`)


### デモ

<a href="#more1">続きを読む▽</a>
<div class="more1" id="more1">

ここに内容<br>
ここに内容<br>
ここに内容<br>

<a href="#close">閉じる△</a>
</div>


### ソース

```css
.more1 {
  display: none;
}
.more1:target {
  display: block;
}
```

```html
<a href="#more1">続きを読む▽</a>
<div class="more1" id="more1">

ここに内容
ここに内容
ここに内容

<a href="#close">閉じる△</a>
</div>
```


## パターン2（ガタつき修正版）

パターン1の開閉時のガタつきが残念なので改良してみます。

コンテンツの前にdivをもう一つ挟み、間接セレクタを使ってCSSを適用させます。間のdivには、position: fixedで画面に固定させることにより、無理やりですがきれいに開閉できます。

### デモ

<a href="#more2">続きを読む▽</a>
<div class="morefix2" id="more2"></div>
<div class="more2">

ここに内容<br>
ここに内容<br>
ここに内容<br>

<a href="#close">閉じる△</a>
</div>


### ソース

```css
.more2{
  display: none;
}
.morefix2:target + .more2{
  display: block;
}
.morefix2{
    position: fixed;
    top: 0;
}
```

```html
<a href="#more2">続きを読む▽</a>
<div class="morefix2" id="more2"></div>
<div class="more2">

ここに内容
ここに内容
ここに内容

<a href="#close">閉じる△</a>
</div>
```


## パターン3（アニメーション付き）

最後はやっぱりアニメーション！ということで、開閉時のアニメーションを付け足してみます。

高さ0からautoにはtransitionが効かないので、max-heightで無理やりアニメーションさせています。これはもう少し賢いやり方があるかもしれない...。

### デモ

<a href="#more3">続きを読む▽</a>
<div class="morefix3" id="more3">&nbsp;</div>
<div class="more3">

ここに内容<br>
ここに内容<br>
ここに内容<br>

<a href="#close">閉じる△</a>
</div>

### ソース

```css
.more3{
    max-height: 0;
    transform: scaleY(0);
    transform-origin: center top;
    transition: .3s ease;
    transition-property: transform,max-height;
}
.morefix3:target +  .more3{
    max-height: 99999px;
    transform: scaleY(1);
}
.morefix3{
    position: fixed;
    top: 0;
}
```

```html
<a href="#more3">続きを読む▽</a>
<div class="morefix3" id="more3">&nbsp;</div>
<div class="more3">

ここに内容
ここに内容
ここに内容

<a href="#close">閉じる△</a>
</div>
```


## まとめ

今回は、コンテンツを折りたたむという動作をCSSだけでやってみました。HTMLが煩雑になってしまうのが弱点ですが、JavaScriptが書けない無料ブログサービスなどもあるので、そのような場所では重宝すると思います。

現在は、&lt;details&gt;タグという使い勝手のいいタグがあるので、正しくマークアップ出来るのであればそちらの方も便利です。

[HTMLだけでコンテンツを開閉できる&lt;details&gt;を試してみる](/post/rn7zja/)

<style>
.more1 {
  display: none;
}
.more1:target {
  display: block;
}


.more2{
  display: none;
}
.morefix2:target + .more2{
  display: block;
}
.morefix2{
    position: fixed;
    top: 0;
}


.more3{
    max-height: 0;
    transform: scaleY(0);
    transform-origin: center top;
    transition: .3s ease;
    transition-property: transform,max-height;
}
.morefix3:target +  .more3{
    max-height: 99999px;
    transform: scaleY(1);
}
.morefix3{
    position: fixed;
    top: 0;
}
</style>
