---
title: 'ブログをNetlifyからGitHub Pagesへお引越し'
published_at: 2020-11-29 16:10:31
category: '雑記'
tags: ['Netlify', 'GitHub']
---

今までNetlifyでホスティングしていたのをGitHub Pagesへ乗り換えました。作業時間は1～2時間ほどです。

<!-- more -->


## 作業のポイント

HexoのブログをGitHub Actionsを使ってGitHub Pagesへ公開する手順は、[Hexoの公式ドキュメント](https://hexo.io/docs/github-pages)に詳しい解説があります。それ以外に気づいた点のメモ。


### タイムゾーン

ドキュメントに書かれているままにやると、記事の日付が-9時間で計算されてしまいます。

ページ生成時に使われる時間は実行環境に依存するらしいので、`npm run build`の箇所に環境変数で東京のタイムゾーンを指定すれば回避できます。


```
- name: Build
  run: npm run build
  env:
    TZ: 'Asia/Tokyo'
```


### 404ページ

GitHub PagesではNetlify同様404ページをカスタマイズすできますが、ファイルの配置がNetlifyとは異なります。

オリジナルの404ページは404というディレクトリの下にindex.htmlを配置するのではなく、ルート直下に404.htmlを置きます。

Hexoではsourceフォルダ直下に404.mdを作ればOK。


### CNAMEファイル

レポジトリの設定画面から自分のドメインを設定すると、CNAMEというファイルがgh-pagesブランチのルートに自動で作成されます。

このファイルはそのままだとGitHub Actionsでページを生成する度に消え去り、ドメインの設定が解除されてしまう（！）ので、そうならないようHexoのプロジェクトにも含める必要があります。

CNAMEファイルはHexoのsourceフォルダに配置し、_config.ymlからもCNAMEをコピーするように明示します。


```
# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
  - CNAME
exclude:
ignore:
```

## 所感

以前から静的サイトなのにページ遷移がやたら重いのが気になっていました。Netlifyはとにかく手軽だという理由で選びましたが、GitHub Actionsを使えばNetlifyと同等のことができるので重い腰を上げて引越ししました。

結果的にページの速度も大幅に上がったので乗り換えて正解でした。

---

Netlify Formsを使ったメールフォームは使えなくなってしまいました。メールフォームだけNetlifyに切り分けようかとも考えましたが、そもそも零細個人ブログに連絡なんかそう来ない...。のでaboutページにしれっとメアドを貼り付けメールフォームは廃止。ついでに断捨離もできました。
