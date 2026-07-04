---
title: リセットCSSで消え去ったselectのスタイルを元に戻す
date: 2020-08-30 15:38:38
post_id: 9senxq
categories:
  - 開発
tags:
  - Web制作
  - CSS
---

リセットCSSといえば、destyle.cssとかnormalize.cssとかress.cssなどいろいろあります。

<!-- more -->

当ブログではressを使っているのですが、これを当てると`<select>`のデフォルトスタイルにくっついている、右端の三角マークのアレ（▼）が消えます。

このブログで利用する分には、これは消えて欲しくなくて、ress本体の該当部分をコメントアウトしてしまおう、とも考えたのですが、それはそれで保守が大変になるので、上書きで元に戻してみることにしました。

## 解決策

```
select {
    -moz-appearance: menulist-button;
    -webkit-appearance: menulist-button;
    appearance: menulist-button;
}
```

こんな感じでCSSを当ててやれば三角が表示されます。

ベンダープレフィックスが無いと、AndroidのChromium系ブラウザで見た時に適用されていなかったので（Chromeは未確認）、書いておいた方が良さげです。

`appearance`プロパティについて調べると、[HTMLクイックリファレンス](http://www.htmq.com/css3/appearance.shtml)の記事が検索に出てきて、そこでプルダウンのスタイル値は`pop-up-menu`だと書かれていますが、デベロッパーツールではinvalidになってしまい全く効果がありません。最終更新がいつなのか書いてないのでよく分からないですが古い記事なのだろうか。

ともかく、`appearance`は仕様が流動的っぽいので注意が必要です。
