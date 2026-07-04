---
title: CSSとChrome拡張機能でGoogle Adsenseの自クリックを防止する【全サイト対応】
date: 2019-05-31 21:40:05
post_id: anbib4
categories:
  - 開発
tags:
  - CSS
  - HTML
  - ユーザーCSS
---

Google Adsenseは自分で運営しているサイトに貼り付けた広告をクリックすることは規約で禁止しており、クリックするとペナルティを受けてしまいます。

<!--more-->

自分のサイトは管理をしていく上で頻繁に開くものなので、うっかりクリックしてしまうこともあるかもしれません。今回は、自分だけ広告をクリックできないように制限をかける方法を紹介します。

この記事で紹介する方法は、Google Chromeの拡張機能を使用するので、ログイン機能のない静的サイトでも有効です。

## 拡張機能の準備

まず、任意のスタイルシートをブラウザ側でサイトに反映できるブラウザ拡張機能をインストールします。

- [Stylus](https://chrome.google.com/webstore/detail/stylus/clngdbkpkpeebahjckkjfobafhncgmne?hl=ja)
- [Stylish](https://chrome.google.com/webstore/detail/stylish-custom-themes-for/fjnbnpbmkenffdnngjfgmeleoegfcffe?hl=ja)
- [User JavaScript and CSS](https://chrome.google.com/webstore/detail/user-javascript-and-css/nbhcbdghjpllgmfilhnhkllmkecfmpld?hl=ja)
- [User CSS](https://chrome.google.com/webstore/detail/user-css/okpjlejfhacmgjkmknjhadmkdbcldfcb?hl=ja)


URLを指定してCSSを設定できるものなら、どれでも可能です。好きなアドオンを選びましょう。

## サイト側の設定

まず、Google Adsenseで発行したコードをdivタグで囲みます。

```html
<div class="google-adsense">
// ここにGoogle Adsenseで発行したコードを貼り付ける
</div>
```

次に拡張機能の設定画面から新しいスタイルを作成します。適用先には自分のブログのURLを設定しましょう。

StylusやStylishの場合はURLを設定する際に「ドメイン上のURL」に設定しておくと、サイト上の全ての広告で有効になるので便利です。

次に、divタグの中にあるリンクをCSSで無効化します。下のCSSをコピペし、保存してください。

```css
.google-adsense {
  pointer-events: none;
}
```

以上で、自分のサイトの広告クリックを防止できます。広告自体は表示されるので、閲覧者と同じ表示でプレビューが可能です。
