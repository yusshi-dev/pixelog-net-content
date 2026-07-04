---
title: CSSアニメーションの開始位置をランダムにする
date: 2020-11-08 12:27:11
post_id: n2yg4u
categories:
  - 開発
tags:
  - Web制作
  - CSS
  - JavaScript
---

CSSアニメーションはロード直後に0%から開始するが、ページを開いていつも同じ場所から始まるのはつまらないのでJavaScriptで開始位置をランダムにしてみる。

<!-- more -->


## デモ

15秒周期でグラデーションが変化するアニメーション。

### 常に0%からスタート

<div class="colorbox">
</div>

### 開始位置がランダム

<div class="colorbox" id="random">
</div>

リロードする度にグラデーションの開始位置が変わる。


## ソース

`animation-delay`は負の値を指定することで、指定した分を遡った時間からアニメーションが始まっていたかのように表現でき、アニメーションのサイクルの途中から再生することができる。

これを応用し、JavaScriptでアニメーションする要素のstyle属性に乱数を当てはめる。


```javascript
function random_animation(element, duration){
  const value = 'animation-delay:-' + Math.floor( Math.random() * duration ) + 's';
  document.getElementById('random').setAttribute('style', value);
}
random_animation('random', '15');
```

```css
.colorbox {
  background: linear-gradient(230deg,#bb37b3,#378abb,#37bb4d);
  background-size: 300% 300%;
  animation: 15s sky ease infinite;
}

@keyframes sky{
  0%{
    background-position: 0% 84%;
  }
  50%{
    background-position: 100% 16%;
  }
  100%{
    background-position: 0% 84%;  
  }  
}
```


<style>
.colorbox {
  margin: 1.5em 0;
  height: 300px;
  background: linear-gradient(230deg,#bb37b3,#378abb,#37bb4d);
  background-size: 300% 300%;
  animation: 15s sky ease infinite;
}

@keyframes sky{
  0%{
    background-position: 0% 84%;
  }
  50%{
    background-position: 100% 16%;
  }
  100%{
    background-position: 0% 84%;  
  }  
}
</style>
<script>
function random_animation(element, duration){
  const value = 'animation-delay:-' + Math.floor( Math.random() * duration ) + 's';
  document.getElementById('random').setAttribute('style', value);
}
random_animation('random', '15');
</script>


## 関連記事

[CSSアニメーションだけでタイマーを作ってみた](/post/bon9ry/)

animation-delayを応用しCSSだけで作ったタイマー。animation-delayの値をカスタムプロパティ（CSS変数）で計算した。
