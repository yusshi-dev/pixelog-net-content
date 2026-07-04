---
title: Hexoのアーカイブページの設定まとめ
date: 2019-06-05 19:11:23
post_id: q87e6x
categories:
  - 開発
tags:
  - Hexo
---

Hexoのアーカイブページの設定方法のまとめです。アーカイブページはhexo-generator-archiveというプラグインで生成されています。

<!--more-->

hexo-generator-archiveはデフォルトでインストールされているプラグインですが、`_config.yml`には設定の記述がないので、自分で追加します。

## 設定

### config.yml

```yml
archive_generator:
  per_page: 10
  yearly: true
  monthly: true
  daily: false
  order_by: -date
```

| 項目    | 説明                                                 |
|---------|-----------------------------------------------------|
|per_page |1ページあたりに表示する記事数（0でページネーションをオフ）|
|yearly   |年単位のアーカイブページを生成                          |
|monthly  |月単位のアーカイブページを生成                          |
|daily    |日単位のアーカイブページを生成                          |
|order_by |記事の順番（デフォルトは降順）                          |


## URL例


| ページ      | URL                                |
|------------|------------------------------------|
|年アーカイブ |https//example.com/2019/            |
|月アーカイブ |https&#58;//example.com/2019/04/     |
|日アーカイブ |https&#58;//example.com/2019/04/01   |
