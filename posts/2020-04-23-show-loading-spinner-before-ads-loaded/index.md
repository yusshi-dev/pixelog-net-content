---
title: 'Google Adsenseが読み込まれる前にローディングアニメーションを表示する'
published_at: 2020-04-23 15:44:07
category: '技術メモ'
tags: ['Web制作', 'HTML', 'CSS']
---

Google Adsenseは広告が表示されるまでに時間がかかります。空白の部分にいきなり広告が現れるとなんかびっくりするというか...

<!-- more -->

ということで、広告が表示される前にローディングスピナーを表示して、閲覧者のストレスを軽減しよう、というのがこの記事での試みです。


## デモ

<div class="loading" style="width:100%;height:250px;background:var(--bg-color);"></div>

読み込み前にアニメーションが表示されます。このサイトでも実装済みです。


## 実装方法

まず下のCSSをサイトに読み込んでおきます。

```css
.loading {
  position: relative;
}

.loading::before {
  --spinner-size: 36px;
  content: "";
  width: var(--spinner-size);
  height: var(--spinner-size);
  border: 2px solid #e3e3e3; /* リングの灰色部分 */
  border-top-color: #3fb5bd; /* リング色がついてるところ */
  border-radius: 50%;
  animation: spinner 0.4s linear infinite; /* 回転速度 */
  position: absolute;
  top: calc(50% - 0.5 * var(--spinner-size));
  left: calc(50% - 0.5 * var(--spinner-size));
  z-index: 0;
}

@keyframes spinner {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
```

自分でカスタマイズできる箇所は以下のとおりです。

|プロパティ|説明|
|-|-|
|--spinner-size|円の大きさ|
|border|リングの灰色部分の色|
|border-top-color|リングの色がついている部分の色|
|animation|スピナーの回転速度やイージング|
|z-index|もし広告より手前に表示されてしまう際には値を適宜`-1`などに変更してください|


HTMLの部分は、まずアドセンスのコードを`div`で囲っておき、classに`loading`を付け加えるだけでOKです。

```html
<div class="ad loading">
// ここにアドセンスのコード
</div>
```

ローディングスピナーの位置は、デバイスや広告のサイズを問わず、常に上下左右中央に表示されます。


## まとめ

軽量なCSSアニメーションなので、ページの表示速度にはほぼ影響を与えません。ちょっとした工夫でユーザーフレンドリーになるので、ぜひ挑戦してみてください。

<style>
.loading {
  position: relative;
}

.loading::before {
  --spinner-size: 36px;
  content: "";
  width: var(--spinner-size);
  height: var(--spinner-size);
  border: 2px solid #e3e3e3; /* リングの灰色部分 */
  border-top-color: #3fb5bd; /* リング色がついてるところ */
  border-radius: 50%;
  animation: spinner 0.4s linear infinite; /* 回転速度 */
  position: absolute;
  top: calc(50% - 0.5 * var(--spinner-size));
  left: calc(50% - 0.5 * var(--spinner-size));
  z-index: 0;
}

@keyframes spinner {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
