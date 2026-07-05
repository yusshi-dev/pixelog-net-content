---
title: 'Hexoで記事本文を抜粋表示する'
published_at: 2020-08-26 18:14:17
category: '技術メモ'
tags: ['Hexo', 'EJS']
---

トップページなどで、記事タイトルの下に本文冒頭の数十文字が抜粋表示されているアレをHexoでも表示させる方法。

<!-- more -->

## 実装方法

あらじめ[Helper](https://hexo.io/docs/helpers)として用意されている、`strip_html()`と`truncate()`の2つの関数を組み合わせれば簡単に実現できます。

`strip_html()`は記事データからHTMLタグを丸々取り除く関数です。ここに記事本文のデータを渡せば生の文章に変換できます。

その変換したデータを`truncate()`に渡せば抜粋表示の完成です。lengthで何文字で切り取るかを指定できます。

```ejs
<%= truncate(strip_html(post.content), {length: 80}) %>
```

`post.content`はテンプレートに依って違うものになると思いますが、記事ページであれば`page.content`など、とにかく抜粋表示させたい文章が入っている変数を入れればOKです。
