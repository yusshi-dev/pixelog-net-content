---
title: Twitterでユーザーのツイートを検索しやくするブックマークレット
date: 2020-09-06 15:01:51
post_id: les7du
categories:
  - 開発
tags:
  - JavaScript
  - ブックマークレット
---

Twitterの検索は`from:ユーザー名`とすれば特定のユーザーのツイートに限定したり、`until:YYYY-MM-DD`とすれば期間を指定したりできる。これらのコマンドを使いやすくするブックマークレットを作った。

<!-- more -->

## ソース

```javascript
javascript:const query = prompt('検索キーワード');const until_date = prompt('until:YYYY-MM-DD');const twitter_id = location.href.replace('https://twitter.com/', '');let url = `https://twitter.com/search?q=from%3A${twitter_id}`;if(until_date){url += ` until%3A${until_date}`;}url += ` ${query}`;location.href = url;
```

```javascript
javascript:
const query = prompt('検索キーワード');
const until_date = prompt('until:YYYY-MM-DD');
const twitter_id = location.href.replace('https://twitter.com/', '');

let url = `https://twitter.com/search?q=from%3A${twitter_id}`;
if(until_date){
  url += ` until%3A${until_date}`;
}
url += ` ${query}`;location.href = url;
```


## 使い方

1. ユーザーのプロフィールページを開いて実行する
2. プロンプトに検索キーワードを入力する
3. プロンプトにいつまでのツイートを表示したいかを`YYYY-MM-DD`の形で指定する（指定しない場合は空欄で可）
4. 検索ページが開く
5. 🤗
