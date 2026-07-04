---
title: 昔ながらの個人サイトを探せる検索エンジンを作った
date: 2020-11-13 19:19:13
post_id: nhm745
categories:
  - 雑記
tags:
---

昨今のトレンドブログしか出ない検索画面にうんざりしているので、Googleカスタム検索で昔ながらの個人サイト（訪問者カウンターやキリ番報告所があるようなサイト）を探せる検索エンジンを作ってみた。

<!-- more -->


## URL

<https://cse.google.com/cse?cx=d7bc44321af656388>


## 検索対象

検索対象のURLは次の通り。無料レンタルサーバーやプロパイダの提供するサービスに付属しているURLを中心に、ホワイトリスト方式で追加した。随時追加予定。


```
*.xrea.com/*
*.web5.jp/*
*.biglobe.ne.jp/*~
*.github.io/*
*.netlify.app/*
interq.or.jp/*
*.tiki.ne.jp/*
*.plala.or.jp/*
big.or.jp/*
*.nifty.com/*
*.coocan.jp/*
eurus.dti.ne.jp/*
*.web.fc2.com/*
*.sitemix.jp/*
*.sakura.ne.jp/*
*.xsrv.jp/*
*.land.to/*
*.ninja-web.net/*
*.upp.so-net.ne.jp/*
eonet.ne.jp/*
ne.jp/asahi/*
```

忍者ホームページはドメインが340種類あるらしくとにかく面倒...。気が向いたら対応するかもしれない。GitHub PagesやNetlifyはエンジニア達の遊び場になっているので迷ったけれど、ジオシティーズから移行したサイトもあるようなので追加した。


## 既知の問題点


- サブドメインでニュースサイトが出る場合がある
- さくらのレンタルサーバーやエックスサーバーなどで独自ドメインを設定しているサイトが、リダイレクト元の無料URLでも引っかかってしまう