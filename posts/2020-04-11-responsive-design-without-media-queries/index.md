---
title: 出来るだけメディアクエリを書かずにレスポンシブに対応させるためのTips
date: 2020-04-11 13:17:27
post_id: 95fdn9
categories:
  - 開発
tags:
  - Web制作
  - CSS
---

サイトをレスポンシブデザインに対応する際には、メディアクエリを使って書き分けたり、上書きしたりする必要があります。

<!-- more -->

しかしメディアクエリを書くのは、たとえSassやStylusのmixinを活用したとしても、めんどくさいことに変わりはないので、できるだけ書かずに済ませたいところです。

先日、このブログのCSSのメディアクエリを見直してみたところ、メディアクエリの中に指定されているclassの数を7割削減（34個から10個）することができたので、今回はそのときに得た知見をもとに、できるだけメディアクエリを書かずにスマホ版に対応させるコツを3点まとめます。


## Grid layoutでレイアウトを組む

メインコンテンツとサイドバーで、2カラムのデザインを組む場合、floatやflexboxなどいろいろな方法が考えられますが、個人的におすすめなのが、Grid Layoutです。


### flexboxで組む場合

まずはflexboxで考えてみましょう。

メインコンテンツが850px、サイドバーが300px、コンテンツとサイドバーの隙間が50pxで、全体を中央に表示させることを考えます。

```html
<div class="container">
  <main>メインコンテンツ</main>
  <aside>サイドバー</aside>
</div>
```

```css
.container {
  display: flex;
  max-width: 1200px;
  margin: auto;
  justify-content: space-between;
}
main {
  width: 850px;
}
aside {
  width: 300px;
}

@media (max-width:1200px){
  .container {
    flex-direction: column;
  }
  main {
    width: 100%;
  }
  aside {
    width: 100%;
  }
}
```

以前はfloatで組んでいたものが、flexboxの登場によってclearfixなどを考えなくてよくなったので、楽になりました。

flexboxはIE対応が簡単なので、サイトをIEに対応させないといけない場合には今でもオススメな方法です。

### Grid Layoutで組む場合

同じHTMLでGrid Layoutを使って組んでみましょう。

```css
.container {
  display: grid;
  grid-template-columns: 850px 300px;
  gap: 50px;
  justify-content: center;
}

@media (max-width:1200px){
  .container {
      grid-template-columns: 1fr;
  }
}
```

**mainやasideにはCSSを一切当ててない** ところに着目してください。Grid Layoutは記述をコンテナーに集約できるので、管理がとても楽です。レスポンシブもコンテナーに一行追加するだけで対応できます。

上記のコードはIE非対応のものですが、IE対応にさせる場合は、prefixやモダンブラウザと異なるプロパティ名が必要だったりなど、特別な記述が必要なので注意が必要です。

## max-widthを使う

レイアウトを組む際に、要素の横幅を指定するのに`width`を使うことが多いと思いますが、その前に`max-width`が使えないか常に考えるようにしましょう。

よく考えれば`width`を使う必要はなかった、というパターンがかなりあります。

### 例

横幅が1200pxのコンテンツを画面の中央に配置し、タブレット・スマホ版では横幅を100%に広げたいときを考えます。

```html
<div class="container">
  <main></main>
</div>
```

```css
main {
  width: 1200px;
  margin: auto;
}

@media (max-width:1200px){
  main{
    width: 100%;
  }
}
```

もしこのような記述に心当たりがあれば、`max-width`を使って下のように置き換えることができます。


```css
main {
    max-width: 1200px;
    margin: auto;
}
```


## emを活用する

PC版とスマホ版で文字サイズやpaddingなどを変えたい、という場合には、単位にpxではなくemを使いましょう。

emはよく分からん！px大好き！<span class="shake">😎</span> という方も多いかと思いますが、emこそがメディアクエリの記述量を削減する鍵を握っています。

### emとは

emとは親要素の`font-size`を1とする長さの単位です。ブラウザのデフォルトのフォントサイズは16pxなので、親要素にフォントサイズを指定しない場合、1em = 16pxとなります。

フォントサイズというと、「文字の大きさ」のイメージが頭に先行してしまいますが、margin、paddingなどにも使えます。`font-size`は単位emあたりの長さを定義するプロパティだと考えましょう。

相対的な単位を使うということはつまり、親要素の`font-size`を上書きするメディアクエリを一行書くだけで、子要素のfont-size、margin、paddingなどを一度に調節できるということを意味します。

ブレイクポイントごとに、細々した要素の大きさや余白をpxで上書きしている...なんて心当たりのある方は、emでの指定に切り替えましょう。


## まとめ

サイトをレスポンシブに対応させる場合には、「いかにしてメディアクエリを書かずに済ませるか」ということを念頭に置いてCSSを書きましょう。

そうすれば自然と管理のしやすいシンプルなCSSが書けます。
