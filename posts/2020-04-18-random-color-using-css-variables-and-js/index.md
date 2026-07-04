---
title: CSS変数とJavaScriptでページを開く度に色をランダムに変える
date: 2020-04-18 17:46:30
post_id: f25ads
categories:
  - 開発
tags:
  - Web制作
  - JavaScript
  - CSS
---

サイトを開くたびに色が変わったらなんか面白いのでは？と、ふと思いついたので、JavaScriptでランダムに生成したカラーコードを、CSS変数に代入して遊んでみます。

<!-- more -->

半分お遊びですね。



## デモ

このページはアクセスする度にヘッダーの色や一部の文字色が変わります。下のボタンでも色を変えて遊べます。

<button class="btn" id="change">色を変える</button>
<div style="background:var(--bg-color);padding: 1em">
  ただ今のテーマカラーは「<span id="current-color" style="color:var(--theme-color);font-weight:bold;"></span>」です！
</div>



## 実装方法

まず、この関数をページのどこかに読み込ませておきます。

```javascript
function randomColor(name){
  document.addEventListener('DOMContentLoaded', function(){
    let result = "#";
    for(let i = 0; i < 6; i++) {
      result += (16 * Math.random() | 0).toString(16);
    }
    document.documentElement.style.setProperty(name, result);
  });  
}
```


そして、カラーコードを代入したいCSS変数名を括弧内に書き関数を実行します。2種類以上色をランダムにしたい場合は、関数を複数回実行してやればOKです。

```javascript
randomColor('--background-color'); // CSS変数名を書く
randomColor('--text-color');
```

色を適用したい要素には下のような感じで、CSS変数で色を指定しておきます。

```css
body {
  background: var(--background-color);
}
p {
  color: var(--text-color);
}
```


## 色を変えるボタンとカラーコードを表示

この記事のデモで使用しているものです。ページアクセス時だけでなく、ボタンを押しても色を変えられます。

```html
<button class="btn" id="change">色を変える</button>
<div>
  ただ今のテーマカラーは「<span id="current-color" style="color:var(--theme-color);font-weight:bold"></span>」です！
</div>
```

```javascript
function randomColor(name){
  document.addEventListener('DOMContentLoaded', function(){
    const btn = document.getElementById('change');
    const current = document.getElementById('current-color');

    randomColor();
    btn.addEventListener('click', randomColor);

    function randomColor(){
      let result = "#";
      for(let i = 0; i < 6; i++) {
        result += (16 * Math.random() | 0).toString(16);
      }
      document.documentElement.style.setProperty(name, result);
      current.textContent = result;
    }
  });  
}
randomColor('--theme-color');
```


## まとめ

ランダムで色が変わるということで、キレイな色になることもあれば目に悪い配色になることもあります。また場合によっては、背景色と文字色の兼ね合いも考えなければならないかもしれません。

ページに組み込むなら、どぎつい色になってもあまり影響を受けない面積の小さい箇所へ、アクセントとして取り入れるのが良いと思います。


<style>
.btn{
  display:inline-block;
  margin:.5em 0;
  padding:8px 2em;
  background:#007bff;
  color:#fff!important;
  text-decoration:none!important;
  border-radius:4px;
  transition:.15s all ease-in-out
  }
.btn:hover{
  background:#0069d9
}
.btn:active{
  background:#0062cc
}
</style>

<script>
function randomColor(name){
  document.addEventListener('DOMContentLoaded', function(){
    const btn = document.getElementById('change');
    const current = document.getElementById('current-color');

    randomColor();
    btn.addEventListener('click', randomColor);

    function randomColor(){
      let result = "#";
      for(let i = 0; i < 6; i++) {
        result += (16*Math.random() | 0).toString(16);
      }
      document.documentElement.style.setProperty(name, result);
      current.textContent = result;
    }
  });  
}
randomColor('--theme-color');
</script>
