---
title: 記事内で使えるアラート表示のテンプレート3種
date: 2020-11-09 12:15:06
post_id: usm859
categories:
  - 開発
tags:
  - Web制作
  - CSS
  - HTML
---

このブログの記事内で使っているアラート表示の装飾。ブログ記事作成に役立つ。

<!-- more -->

## デモ

<p class="alert">
ボタンを押してください
</p>

<p class="alert caution">
この記事は2年以上前に書かれたものです。
</p>

<p class="alert warning">
乾燥剤は食べないでください。
</p>

文字色は背景色とのコントラス比が7.00になるように調整している。classに`caution`をつければ黄色に、`warning`をつければ赤色になる。


## ソース

HTML

```html
<p class="alert">
ボタンを押してください
</p>

<p class="alert caution">
この記事は2年以上前に書かれたものです。
</p>

<p class="alert warning">
乾燥剤は食べないでください。
</p>
```

サンプルではpタグにしてあるが、記事とある程度独立した内容であればfigureタグ、よく分からない場合はdivタグでOK。

CSS

```css
.alert {
    padding: 1em 1.25em;
    font-size: .88em;
    border-left: .4em solid;
    background: #d6efff;
    border-color: #b8daff;
    color: #1d4f87;
}
.alert.caution {
    background: #fff3cd;
    border-color: #ffdaa6;
    color: #674f19;
}
.alert.warning {
    background: #ffe1e4;
    border-color: #f5c6cb;
    color: #91202d;
}
```

Stylus

```stylus
.alert
    padding 1em 1.25em
    font-size .88em
    border-left .4em solid
    background #d6efff
    border-color #b8daff
    color #1d4f87

    &.caution
        background #fff3cd
        border-color #ffdaa6
        color #674f19

    &.warning
        background #ffe1e4
        border-color #f5c6cb
        color #91202d
```
