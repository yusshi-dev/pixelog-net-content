---
title: aタグにclassを1つ追加するだけでリンクをブログカード風に変身させる
date: 2020-04-16 19:23:20
post_id: x8xjr4
categories:
  - 開発
tags:
  - Web制作
  - HTML
  - CSS
---

最近はブログにリンクを貼る手段として、テキストリンク以外にも、カード型のリンクを埋め込むことが増えてきました。

<!-- more -->

よく見るブログカードは、サムネイルや投稿日時などのメタデータが盛り込まれていて、自前で実装しようとすると、リンク先の情報をどのように取得するか、など考えることがたくさんあって面倒です。

ブックマークレットでHTMLを作成するという手法も紹介されていたりしますが、OGP画像が直リンになってしまうのがどうも気になる...。（OGP画像は直リンしてもOKな風潮なのか...？）

正直そこまでの情報はいらない、という場合も多いかと思いますので、CSSだけでブログカード風のものを作ってみたいと思います。


## デモ

<a class="card-link" href="/post/x8xjr4/">aタグにclassを1つ追加するだけでリンクをブログカード風に変身させる</a>

リンク先タイトルとURLだけ、という至極シンプルなデザインです。（これをブログカードと呼ぶと怒られるかもしれませんが...）記事のサムネイル画像は表示されませんが、リンクをクリックする決め手となるには必要十分だと思います。


## 使い方

### CSS

ブログに以下のCSSを予め読み込んでおきます。

```css
.card-link {
  display: block;
  margin: 1.5em 0;
  padding: 1em;
  font-weight: bold;
  text-decoration: none;
  color: #3884ff;
  border: 1px solid #e3e3e3;
  border-radius: 4px;
  transition: .15s border ease;
}
.card-link:hover {
  border-color: #bbb;
}
.card-link::after {
  display: block;
  margin: 0.7em 0 0 0;
  content: attr(href);
  font-size: 0.75em;
  color: #767676;
  font-weight: normal;
}
```

Stylusを使っています！という僕のような奇特な方は下のコードをどうぞ。

```stylus
.card-link
    display block
    margin 1.5em 0
    padding 1em
    font-weight bold
    text-decoration none
    color #3884ff
    border 1px solid #e3e3e3
    border-radius 4px
    transition .2s border ease

    &:hover
        border-color #bbb

    &::after
        display block
        margin .7em 0 0 0
        content attr(href)
        font-size .75em
        color #767676
        font-weight normal
```


### リンクの貼り方

ブログカードの貼り方はとても簡単です。普通のテキストリンクに`card-link`というclassを追加するだけで、あっという間にブログカード型に変身します。

```html
<a class="card-link" href="https://www.google.co.jp/">Google</a>
```

### 応用

WordPressをお使いの場合は、正規表現でサイト内リンクの全てに自動で`card-link`を追加するよう、function.phpへ書いておくと保守性も上がって便利ですね。

当ブログで使っているHexoの場合は、下のようなスクリプトを書いて自動でclassを追加しています。

```javascript
hexo.extend.filter.register('after_post_render', function(data){
  data.content = data.content.replace(/<a href="\/post\//g, '<a class="card-link" href="\/post\/');
  return data;
});
```


## やってみようとしたこと

Googleが提供しているAPIに、サイトのファビコンを簡単に取得できるものがあります。

```plaintext
https://www.google.com/s2/favicons?domain=ここにURL
```

ブログカードのURLの前にファビコンを表示したくて、「ここにURL」の場所にCSS変数を駆使してhrefを代入し、疑似要素へ突っ込もうと試行錯誤したのですが、現状では無理だということが判明しました。残念...

<style>
.card-link {
  display: block;
  margin: 1.5em 0;
  padding: 1em;
  font-weight: bold;
  text-decoration: none;
  color: #3884ff;
  border: 1px solid #e3e3e3;
  border-radius: 4px;
  transition: .15s border ease;
}
.card-link:hover {
  border-color: #bbb;
}
.card-link::after {
  display: block;
  margin: 0.7em 0 0 0;
  content: attr(href);
  font-size: 0.75em;
  color: #767676;
  font-weight: normal;
}
</style>
