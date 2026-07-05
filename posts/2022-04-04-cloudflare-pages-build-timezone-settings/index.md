---
title: 'Cloudflare Pagesでビルド環境のタイムゾーンを変更'
published_at: 2022-04-04 18:03:06
category: '技術メモ'
tags: ['Cloudflare']
---

当ブログのホスティング先をGitHub PagesからCloudflare Pagesへ移行して、記事のパーマリンクを乱数ベースから日付時刻ベースへ変更したのですが、ローカル環境とCloudflareのタイムゾーンが違うせいでURLがずれてしまうので、Cloudflare Pagesのタイムゾーンを日本に変更します。




## 環境変数の追加

![Cloudflare Pagesで環境変数を編集する](1.png)

1. ダッシュボード>Pages>任意のプロジェクト>設定>環境変数
2. 変数を編集する>変数名に`TZ`、値に`Asia/Tokyo`を入力
3. 保存

同じく静的サイトジェネレーター等を使っている方はお試しください。
