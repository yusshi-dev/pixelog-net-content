---
title: 開いているページの構造化マークアップを簡単にチェックするブックマークレット
date: 2020-03-17 18:48:14
post_id: gkma4r
categories:
  - 開発
tags:
  - JavaScript
  - ブックマークレット
---

Googleが提供しているツールに、[構造化データ テストツール](https://search.google.com/structured-data/testing-tool?hl=ja)というのがあり、URLを入力するだけでサイトの構造化マークアップを簡単にチェックすることができます。

<!--more-->

そのままでも十分お手軽ですが、せっかくなので、 開いているページの構造化マークアップの検証結果を、ワンクリックで表示できるブックマークレットを作成してみました。

他人のサイトでもテストツールで覗いてみれば、構造化マークアップの参考になるかもしれません。

## ブックマークレット

下のボタンをブックマークバーへドラッグして登録してください。

<a href="javascript:(function(){window.open('https://search.google.com/structured-data/testing-tool/u/0/#url='+encodeURIComponent(location.href))})()">構造化データ テストツール</a>


または、コードをURL欄に貼り付けて、任意の名前でブックマークに保存してください。

```javascript
javascript:(function(){window.open('https://search.google.com/structured-data/testing-tool/u/0/#url='+encodeURIComponent(location.href))})()
```

構造化データをチェックしたいサイトを開き、登録したブックマークをクリックすれば、新しいタブで自動で計測が始まります。

<p class="alert">
window.open()でポップアップがブロックされる場合は、ポップアップブロックを解除してください。Chromeの場合、同一ドメインでは設定が保存されます。
</p>
