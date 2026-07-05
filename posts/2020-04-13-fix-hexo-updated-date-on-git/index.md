---
title: 'HexoでGitとNetlifyを使っていても記事の更新日を表示できるようにする'
published_at: 2020-04-13 19:02:35
category: '技術メモ'
tags: ['Hexo', 'Git', 'EJS', 'Netlify']
---

Hexoで作成したブログで、記事の公開日だけでなく更新日も表示できるようにする方法のメモ。Netlifyで公開したときに更新日がおかしくなる問題も解決します。

<!-- more -->


## Gitで管理したときの問題

Hexoでは`page.updated`という変数が用意されており、記事のfront-matterで`updated`に日付を指定した場合は、その日付が格納され、それ以外の場合はシステムファイルの更新日時が参照される、という仕様です。

なので、この変数をテンプレートで書き出せばOKです。

```ejs
<%- page.updated.format("YYYY.MM.DD") %>
```

これはローカルサーバーを立ち上げてプレビューしたときは正しく更新日時が表示され、うまくいっているように思えます。

しかし、Gitでサイトを管理している場合、Gitはファイルのタイムスタンプを管理しないので、Netlifyなどで、レポジトリを読み取ってサーバー側でサイトを生成すると、**すべての記事の更新日時が最後にコミットした日付になってぶっ壊れる！** という欠陥があります。


## 解決策

[Hexo 4.0.0](https://hexo.io/news/2019/10/14/hexo-4-released/)より、front-matterの記述がなかった場合、システムファイルの更新日を参照するのではなく、公開日と同じ日付をフォールバックするというオプションが選べるようになりました。

### config.ymlでの設定

Hexoが4.0.0以上になっているかを確認した後、以下のオプションをconfig.ymlへ追加します。

```yml
use_date_for_updated: true
```

### テンプレート(ejs)

テンプレートで更新日付を表示したい箇所に以下のように記述します。

```ejs
<% if (page.date.format() !== page.updated.format()) {%>
    <time class="post__date" datetime="<%- page.updated.format() %>">
      <%- '更新: '+page.updated.format("YYYY.MM.DD") %>
    </time>
<% } %>
```

公開日と同じ日付が2つ並ぶのはかっこ悪いので、更新日が公開日と同じ場合は表示しないようにしてみました。

記事を編集した際にはfront-matterで`updated`を必ず書くようにします。

```yml
---
title: 'hoge'
category: ''
  - hage
published_at: 2020-04-13 19:40:00
updated: 2020-04-14 08:00:00
---
```
