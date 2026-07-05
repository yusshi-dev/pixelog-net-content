---
title: 'CSS Gridでcodeブロックがある場合に横幅をはみ出してしまう現象の解決策'
published_at: 2020-11-08 11:45:26
category: '技術メモ'
tags: ['Web制作', 'CSS']
---

CSSのGridでレイアウトを組んでいて、グリッドアイテムにcodeブロックが含まれている場合に、横幅がGridの横幅をはみ出し制御できなくなる現象を解決する方法。

<!-- more -->


## 前提条件

```html
<div class="frame">
  <main class="main">
    <code>ながーーーーーーーーーーーーーーーーーーーーーーーいコード</code>
  </main>
  <aside>
  </aside>
</div>
```

```css
.frame {
    display: grid;
    grid-template-columns: 800px 300px;
    gap: 80px;
}
```

- CSSのGridで2カラムなどのレイアウトを組んでいる
- グリッドの格子の横幅を`grid-template-columns`で指定している
- 格子へ配置しているアイテムに`<code>`が含まれている
- `<code>`にはアイテムの幅を超える文字が入っていて、横スクロールバーを表示したい


## デモ

このブログでデベロッパーツールを開いて`<main class="main">`に掛かっている`min-width: 0;`を解除してみてください。

再現用のダミーコードブロック↓

```
V0ten0eCpmNXRDrLixXKKzlmxwN7CbXsb1o5PZnk71isQRzUmwKaSu4fDFnsuWD5o2tqaf70ozdQUOjyiBPNTkMoeku9Kek1HrCUAdmcubjXmOke3cOYjHf8efW91v26iTOLr0COMWgijqYacIIy94J4cEIi9zCnfqrhgmw5kEsxmyHTAuJAYF9Rid3k3N9Wg0qxL9OCPIyDxIArgn6KMPqEuvIUHHyGsSvy7uWjfC9b737K1xMO74NMVsDUvZWKgGLjU5e8ZMwGiR73hSPFtwtvRsSuYrVb5dyYylDDpyrZCiFtPMOlpOJcSpsQSfypFOHObZ0KMeNnDCKamGKP7GIR8CLhalxlOK0pQHQpP0Ei6gzzNLgZgahlJvSn418P5XiiSNr4qtpIMyLl
```

## 解決策

### その1 min-width

グリッドアイテムに`min-width: 0`を指定する。

```css
.main {
  min-width: 0;
}
```

なぜこれで直るのかは...謎。`overflow: scroll`は横幅の指定があってのものだが、グリッドレイアウトによる指定は横幅と認識しないのかもしれない。


### その2 overflow

グリッドアイテムに`overflow: hidden`を指定する。

```css
.main {
  overflow: hidden;
}
```

ただし、`overflow: hidden`は思わぬところで表示に影響が出やすいので注意する必要がある。
