---
title: 'highlight.jsのスタイルをダークモードに応じて切り替える'
published_at: 2020-04-30 12:30:00
category: '技術メモ'
tags: ['Web制作', 'JSライブラリ', 'CSS']
---

ページ上のコードにシンタックスハイライトを適用するのに、[highlight.js](https://highlightjs.org/)というプラグインがあります。

ハイライトのスタイルもたくさんの種類が用意されていて、当ブログでは今までatom-one-darkを適用していたのですが、ライトモード時はコード部分を白背景にしたかったのもあって、ダークモードに応じてhighlight.jsのスタイルも切り替わるようにしてみました。

<!-- more -->


## 表示サンプル

~~ヘッダーにあるダークモードスイッチで切り替えてみてください。atom-one-lightとatom-one-darkが入れ替わります。~~

```typescript
class MyClass {
  public static myValue: string;
  constructor(init: string) {
    this.myValue = init;
  }
}
import fs = require("fs");
module MyModule {
  export interface MyInterface extends Other {
    myProperty: any;
  }
}
declare magicNumber number;
myArray.forEach(() => { }); // fat arrow syntax
```


## やったこと

highlight.jsのスタイルは、lightとdarkのセットで用意されているものがあり、その組み合わせであればCSSの構造も似通っているので、カスタマイズする際は、それらを使うのが簡単だと思います。

やることは単純で、
1. lightの方のファイルをコピーして、色指定部分をそれぞれCSS変数に置き換える
2. CSS変数にライトモードの色を代入する
3. darkの方を参考にしながら、ダークモード時の色をメディアクエリで変数に上書きする

という地道な作業です。これで2つのスタイルが1つにまとまるという訳ですね。

atom-one-lightは、コメントアウト部分に使う色がまとめてあったので、ハイフンを先頭につけるだけでそのままCSS変数として利用でき、作業が楽でした。



## atom-one-light/dark

当ブログで適用している、atom-one-lightとatom-one-darkが切り替わるスタイルです。BSDライセンスということで、配布してもよさそうなので下に置いておきます。

```css
/*

Atom One Light by Daniel Gamage
Original One Light Syntax theme from https://github.com/atom/one-light-syntax

base:    #fafafa
mono-1:  #383a42
mono-2:  #686b77
mono-3:  #a0a1a7
hue-1:   #0184bb
hue-2:   #4078f2
hue-3:   #a626a4
hue-4:   #50a14f
hue-5:   #e45649
hue-5-2: #c91243
hue-6:   #986801
hue-6-2: #c18401

*/
/*

Atom One Dark by Daniel Gamage
Original One Dark Syntax theme from https://github.com/atom/one-dark-syntax

base:    #282c34
mono-1:  #abb2bf
mono-2:  #818896
mono-3:  #5c6370
hue-1:   #56b6c2
hue-2:   #61aeee
hue-3:   #c678dd
hue-4:   #98c379
hue-5:   #e06c75
hue-5-2: #be5046
hue-6:   #d19a66
hue-6-2: #e6c07b

*/

.hljs {
  --base:    #fafafa;
  --mono-1:  #383a42;
  --mono-2:  #686b77;
  --mono-3:  #a0a1a7;
  --hue-1:   #0184bb;
  --hue-2:   #4078f2;
  --hue-3:   #a626a4;
  --hue-4:   #50a14f;
  --hue-5:   #e45649;
  --hue-5-2: #c91243;
  --hue-6:   #986801;
  --hue-6-2: #c18401;
}

@media (prefers-color-scheme: dark) {
  .hljs{
    --base:    #282c34;
    --mono-1:  #abb2bf;
    --mono-2:  #818896;
    --mono-3:  #5c6370;
    --hue-1:   #56b6c2;
    --hue-2:   #61aeee;
    --hue-3:   #c678dd;
    --hue-4:   #98c379;
    --hue-5:   #e06c75;
    --hue-5-2: #be5046;
    --hue-6:   #d19a66;
    --hue-6-2: #e6c07b;
  }
}

.hljs {
  display: block;
  overflow-x: auto;
  padding: 0.5em;
  color: var(--mono-1);
  background: var(--base);
}

.hljs-comment,
.hljs-quote {
  color: var(--mono-3);
  font-style: italic;
}

.hljs-doctag,
.hljs-keyword,
.hljs-formula {
  color: var(--hue-3);
}

.hljs-section,
.hljs-name,
.hljs-selector-tag,
.hljs-deletion,
.hljs-subst {
  color: var(--hue-5);
}

.hljs-literal {
  color: var(--hue-1);
}

.hljs-string,
.hljs-regexp,
.hljs-addition,
.hljs-attribute,
.hljs-meta-string {
  color: var(--hue-4);
}

.hljs-built_in,
.hljs-class .hljs-title {
  color: var(--hue-6-2);
}

.hljs-attr,
.hljs-variable,
.hljs-template-variable,
.hljs-type,
.hljs-selector-class,
.hljs-selector-attr,
.hljs-selector-pseudo,
.hljs-number {
  color: var(--hue-6);
}

.hljs-symbol,
.hljs-bullet,
.hljs-link,
.hljs-meta,
.hljs-selector-id,
.hljs-title {
  color: var(--hue-2);
}

.hljs-emphasis {
  font-style: italic;
}

.hljs-strong {
  font-weight: bold;
}

.hljs-link {
  text-decoration: underline;
}
```
