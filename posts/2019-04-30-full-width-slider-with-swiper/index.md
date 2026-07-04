---
title: 【Swiper.js】縦横比を保ったまま横幅いっぱいのスライダーをつくる
date: 2019-04-30 18:14:34
post_id: u35xpq
categories:
  - 開発
tags:
  - Web制作
  - JavaScript
  - JSライブラリ
  - HTML
---

[Swiper.js](https://idangero.us/swiper/)という、jQueryなしで動く高機能なライブラリがあります。

このライブラリで縦横比を保ったままウインドウ幅いっぱい + レシスポンシブ対応のスライドをつくる方法のメモ。

<!--more-->

## HTML・JavaScript

このサンプルではスライドの中に画像を配置したものを考えます。

```html
<div class="swiper-container">
  <div class="swiper-wrapper">
    <div class="swiper-slide"><img src="sample1.jpg"></div>
    <div class="swiper-slide"><img src="sample2.jpg"></div>
  </div>
  <div class="swiper-pagination"></div>
  <div class="swiper-button-prev"></div>
  <div class="swiper-button-next"></div>
</div>
```

```javascript
var mySwiper = new Swiper ('.swiper-container', {
  loop: true,

  pagination: {
    el: '.swiper-pagination',
  },

  navigation: {
    nextEl: '.swiper-button-next',
    prevEl: '.swiper-button-prev',
  },
})
```
HTML・JavaScriptに関しては特別な設定はしておらず、デフォルトのままです。


## CSS

```css
.swiper-container {
  position: relative; /* ← swiper.cssに含まれています */
}

.swiper-container:before {
  content: "";
  display: block;
  padding-top: calc(9/16*100%); /* 縦/横*100の値を書く */
}

.swiper-wrapper {
  position: absolute; /* ← swiper.cssと優先順位が干渉するときは工夫してください */
  top: 0;
  left: 0;
}

.swiper-wrapper img {
    width: 100%;
    height: auto;
}
```

上記のCSSを新たに追加します。

巷ではそこそこ有名な方法です。margin/paddingの割合指定はは要素の幅が基準になることを利用して、疑似要素にpadding-topを付しています。

padding-topの値は計算機で「縦/横*100」を割り出しても良いですが、大抵はきれいな値にならないので、calc()を使って導出しています。

そして、そのままだと疑似要素による謎のスペースが占拠してしまうので、残りの子要素を親要素の左上へ絶対配置します。
