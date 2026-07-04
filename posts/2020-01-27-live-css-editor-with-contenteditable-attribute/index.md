---
title: contenteditable属性で入力したCSSがページにリアルタイムで反映されるフォームをつくる
date: 2020-01-27 15:55:50
post_id: nzj3d9
categories:
  - 開発
tags:
  - Web制作
  - HTML
  - CSS
---

HTML5で追加された属性に`contenteditable`というものがあります。これを付けることで、あらゆる要素のテキストノードの編集が可能になります。ブラウザ版Twitterのツイート入力フォームもこの属性が使われています。

<!--more-->

これを`<style>`タグに追加して、さらにdisplay: blockで可視化すると、入力したCSSがページにリアルタイムで反映されるフォームを簡単につくることができます。

<p class="alert caution">
&lt;style&gt;タグは、&lt;head&gt;間または&lt;body&gt;直下の子要素へ配置しないと、<a href="https://validator.w3.org/">W3Cのバリデーション</a>でエラーになるのでご注意ください。
</p>

## デモ

body{background: #000} など適当なCSSを入力して遊んでみてください。

<style class="stylebox" contenteditable="true" type="text/css"></style>

styleタグ内にdivなどの余計なタグが生成されるのを防ぐため、改行する際はShiftキーを押しながらEnterキーを押してください。



## デモで使用しているソース

```html
<style class="stylebox" contenteditable="true" type="text/css">
</style>
```
```css
.stylebox{
    display: block;
    height: 400px;
    padding: 12px;
    overflow: auto;
    resize: both;
    border: 1px solid #ddd;
    white-space: pre;
    font-family: Monaco, monospace;
}

.stylebox:focus{
    box-shadow: 0 0 5px #ccc inset;
}
```


<style>.stylebox{display: block;margin: 20px 0;height:400px;overflow:auto;padding: 12px;border:1px solid #ddd;resize:both;white-space: pre;font-family: Monaco, monospace;}.stylebox:focus{box-shadow:0 0 5px #ccc inset;}</style>
