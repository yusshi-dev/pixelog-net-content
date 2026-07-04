---
title: PC版Twitterの右カラム(トレンド、おすすめユーザー)を消すCSS
date: 2020-07-15 15:00:00
post_id: 7ze3ba
categories:
  - 開発
tags:
  - CSS
  - ユーザーCSS
---

Twitterは公式アカウントを情報収集目的で見るくらいで、他人の反応とかトレンドとか1ミリも興味ねえよ！って方向けの、PC版Twitterをカスタマイズする記事です。

<!-- more -->

Twitterの右カラムにあるトレンドやおすすめユーザー、その他諸々をユーザーCSSで全て消し飛ばします。


## 準備するもの

Google Chrome（もしくはChromium製のChromeのアドオンが使えるブラウザ）と、独自に定義したCSSをサイトに適用することができる拡張機能を準備してください。おすすめはStylus、もしくはStylishです。

- [Stylus](https://chrome.google.com/webstore/detail/stylus/clngdbkpkpeebahjckkjfobafhncgmne?hl=ja)
- [Stylish](https://chrome.google.com/webstore/detail/stylish-custom-themes-for/fjnbnpbmkenffdnngjfgmeleoegfcffe?hl=ja)

使い方はすでに他サイトで丁寧に解説されているのでそちらをご覧ください。


## イメージ画像

### CSS適用前

![CSS適用前](1.png)

CSS適用前の状態です。右カラムにいらない情報がどっさりですね。


### ログアウト時

![ログアウト時の適用後](2.png)

ログアウト時は右カラムが完全に吹き飛んで1カラムになります。


### ログイン時

![ログイン時の適用後](3.png)

ログイン時も右カラムの情報は全て消えます。ついでに左カラムにある「#話題を検索」リンクも消します。





## CSS

以下のCSSをコピペでお使いください。

Twitterはclass名がほとんど自動生成されたようなネーミングになっていますが、`data-testid`属性で指定しているので、おそらく仕様変更にも強いと思います。

```css
[data-testid="sidebarColumn"],
[href="/explore"]{
    display: none
}
```

### 適用先

適用先は以下のURLを入力し、「次で始まるURL」もしくは「ドメイン上のURL」を選んでください。

```plaintext
https://twitter.com/
```
