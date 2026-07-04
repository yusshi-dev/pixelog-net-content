---
title: pointer-events&#058;noneするとcursorプロパティが効かないのを回避する
date: 2020-07-04 18:15:19
post_id: 53qy95
categories:
  - 開発
tags:
  - Web制作
  - CSS
---

このブログのトップページのページャーで、disabledになっているボタンにhoverしたとき、cursorをnot-allowedにしようとしたらうまく適用されず、気になって調べたことのメモ。

<!-- more -->


## やろうとしたこと

```html
<a class="btn disabled">ボタン</a>
```

```css
.btn.disalbed {
  pointer-events: none;
  cursor: not-allowed; /* ← 効かない */
}
```

<a class="btn disabled " style="pointer-events:none;cursor:not-allowed;">ボタン</a>

同じclassに`pointer-events: none` と `cursor: not-allowed`を指定してボタンにホバーしても、カーソルが禁止マークになりません。pointer-eventsの動作はカーソルにまで及んでしまうみたいです。


## 解決方法

ボタンをdivか何かで囲ってあげて、外側の要素に`pointer-events: none`を、ボタン本体に`cursor: not-allowed`を指定すればOKです。

```html
<div class="btn-wrapper disabled">
  <a class="btn">ボタン</a>
</div>
```

```css
.btn-wrapper.disalbed {
  cursor: not-allowed; /* ← 効かない */
}
.btn {
  pointer-events: none;
}
```

<span style="cursor:not-allowed;">
  <a class="btn disabled" style="pointer-events:none;">ボタン</a>
</span>


## 余談

とは言っても、これだけの為にわざわざ、もう一つ要素増やして囲みたくなんかないですよね。

自分が`pointer-events: none`を指定する理由は、hover時に背景色がつくのを取りやめたい、というだけものだったので、結局、disabledボタンにhoverしたときは、`background: none`にする、という方法で対処しました。

pointer-eventsは便利なプロパティですが、JSやその他諸々の動作をCSSで押さえつけるという姑息な手段で使ってしまわないよう、他の手段を考えてみるのもいいかもしれません。
