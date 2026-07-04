---
title: uBlacklist用のリスト購読URLを公開しました
date: 2020-05-01 12:18:18
post_id: ma5z69
categories:
  - 雑記
tags:
---

Google Chromeに「[uBlacklist](https://chrome.google.com/webstore/detail/ublacklist/pncfbmialoiaghdehhbnbhkkgmjanfhe?hl=ja)」という拡張機能があり、指定したドメインのサイトをGoogleの検索結果から排除することができます。

<!-- more -->

さらにこのプラグインには、WEB上に公開されているブラックリストを購読する機能が備わっています。既にある程度完成されたものを公開している方がおられるのでどうするか悩んだのですが、一応公開しておくことにしました。


## リスト選定基準

リスト選定基準は以下のとおりです。

- 5ch(2ch)、まとめブログなどの関連サイト（2ch、5ch、matomeの文字列が含まれるURLは全て表示しません）、ガールズちゃんねるなど
- naverまとめなどのキュレーションメディア
- 「●●●●の身長やWik風プロフィール！顔写真はや高校は？結婚している？●●●との馴れ初めは？●●●って本当？」のような記事があるサイト

<p class="alert warning">このリストに挙がっているサイトを攻撃する意図は一切ございません。ご理解くださいますよう、よろしくお願いします。
</p>


## 購読方法

右上のuBlacklistのアイコンを右クリック → オプション より設定画面を開きます。

![オプション画面](1.png)

下方にある購読の項目より「購読を追加する」ボタンをクリックします。

![URLを貼り付ける](2.png)

名前には任意の名前をつけ、URL欄に下のURLをコピペし保存してください。

```
https://web-blacklist.netlify.app/url.txt
```

## URLの追加について

URLリストはGitHubで管理しています。URLの追加に協力していだける方は、下のレポジトリをForkして編集していただき、プルリクを送ってください。

<a href="https://github.com/yussio/web-blacklist" class="card-link">yussio / web-blacklist</a>
