---
title: 'Hexoで記事ごとに適用するCSSやJSファイルをFront-matterから自動で読み込む'
published_at: 2020-01-30 10:33:20
category: '技術メモ'
tags: ['Hexo', 'EJS']
---

Hexoでブログを書いていると、記事ごとにCSSやJavaScriptを書いて読み込みたいと思うことがよくあります。

<!--more-->

しかし記事中にインラインで記述するとごちゃごちゃになるので、style.cssやscript.jsというファイルにまとめ、Front-matterの切り替えで、ファイルが自動でテンプレートに挿入されるようにしてみます。

## 下準備

1. テンプレートのファイル内で、CSSファイルやjsファイルを読み込みたい場所に、下記のコードを配置します。

```ejs
<% if(page.css === true) { %>
    <link rel="stylesheet" href="/<%= page.path %>style.css">
<% } %>
```

```ejs
<% if(page.js === true) { %>
    <script src="/<%= page.path %>script.js"></script>
<% } %>
```

2. Hexoの[post_asset_folder](https://hexo.io/docs/asset-folders.html)機能を使用するので、\_config.ymlから有効にしておきます。
3. ファイルが記事としてレンダリングされてしまうのを防ぐため、\_config.yml内のskip_renderの項目を設定します。

```yml
# Directory

（中略）

skip_render:
  - _posts/*(*/)(style.css|script.js)
```

正規表現ではなくExtended globbingで書かないといけない、ということを理解するのに、丸1日かかってしまいました。ありがとうございます。


## 使い方

1. hexo newコマンドから自動で生成された、Markdownファイルと同じ名前のフォルダに、style.css、script.jsという名前でファイルを新規作成し、記事で読み込みたいものを書きます。
2. 記事のFront-matterでcss、jsをtrueにします。


```plaintext
title: ''
category: ''
tags:
published_at: 2020-01-30 10:33:20
css: true
js: true
```

3. その記事のみにファイルが読み込まれます。
