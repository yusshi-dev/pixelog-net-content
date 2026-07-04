---
title: 旧愛生会病院HPをCSSアニメーションで再現するサンプル4種
date: 2020-11-18 15:36:02
post_id: y5xwcr
categories:
  - 開発
tags:
  - Web制作
  - CSS
  - CSSアニメーション
---

サイケデリックで目の痛いデザインで有名な旧愛生会病院のホームページをCSSアニメーションで再現してみるサンプル。

<!-- more -->

旧愛生会病院のホームページ（Wayback Machine）  
<https://web.archive.org/web/20130515083615/http://www.aiseikai.or.jp/>


## 流れる文字

マーキータグをCSSで再現。文章の長さでスクロールの速度が変わるのでspanのstyle属性で調整するのがオススメ。


<p class="marquee-anim" style="line-height: 48px;font-size: 1em;color: #00ffff;">
  <span style="animation-duration: 10s;">推奨する Browser は Microsoft Internet Explore（Ver 6.0）で、全画面表示とするのが至適の様です。</span>
</p>


```html
<div class="marquee-anim" style="line-height: 48px;font-size: 1em;color: #00ffff;">
  <span style="animation-duration: 10s;">推奨する Browser は Microsoft Internet Explore（Ver 6.0）で、全画面表示とするのが至適の様です。</span>
</div>
```

```css
.marquee-anim {
  overflow: hidden;
}

.marquee-anim span {
  display: inline-block;
  padding-left: 100%;
  white-space: nowrap;
  animation: marquee linear infinite;
}

@keyframes marquee {
  from {
    transform: translate(0);
  }
  to {
    transform: translate(-100%);
  }
}
```


## 点滅させる

文字をチカチカと点滅させる。opacityで再現しているので点滅は画像に適用することもできる。

<p class="flash-anim">Welcome！心から歓迎！</p>


```html
<div class="flash-anim">Welcome！心から歓迎！</div>
```

```css
.flash-anim {
  animation: flash .2s steps(1) infinite;
}

@keyframes flash {
    0%   {opacity:.5;}
    50%  {opacity: 1;}
}
```


## 回転する文字

連続的に1回転する動きをstepsでパラパラに切り取る。


文字サイズは「<span class="rotate-anim">中</span>」で！


```html
文字サイズは「<span class="rotate-anim">中</span>」で！
```


```css
.rotate-anim {
  display: inline-block;
  animation: rotate 2s steps(4) infinite;
}

@keyframes rotate {
    0%   {transform: rotate(0);}
    100% {transform: rotate(360deg);}
}
```



## 3色に光る文字

<p class="rainbow-anim">看護部スタッフ<br>監理部スタッフ</p>

```html
<div class="rainbow-anim">看護部スタッフ<br>監理部スタッフ</div>
```


```css
.rainbow-anim {
  animation: rainbow .4s steps(3) infinite;
}

@keyframes rainbow {
    0%   {color: #ff0000;}
    33%  {color: #00ff00;}
    66%  {color: #ffff00;}
}
```







<style>

.marquee-anim {
  overflow: hidden;
}

.marquee-anim span {
  display: inline-block;
  padding-left: 100%;
  white-space: nowrap;
  animation: marquee linear infinite;
}

@keyframes marquee {
  from {
    transform: translate(0);
  }
  to {
    transform: translate(-100%);
  }
}


.rainbow-anim {
  animation: rainbow .4s steps(3) infinite;
}

@keyframes rainbow {
    0%   {color: #ff0000;}
    33%  {color: #00ff00;}
    66%  {color: #ffff00;}
}


.flash-anim {
  animation: flash .2s steps(1) infinite;
}

@keyframes flash {
    0%   {opacity:.5;}
    50%  {opacity: 1;}
}



.rotate-anim {
  display: inline-block;
  animation: rotate 2s steps(4) infinite;
}

@keyframes rotate {
    0%   {transform: rotate(0);}
    100% {transform: rotate(360deg);}
}
</style>
