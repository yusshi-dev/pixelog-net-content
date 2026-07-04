---
title: 画像のtitle属性をキャプションとして利用する
date: 2020-11-09 15:00:00
post_id: 6u9kwn
categories:
  - 開発
tags:
  - Hexo
---

下のサイトで紹介されているtitle属性をキャプションに変換するアイデアが素晴らしかったので、参考にさせていただいた。

[画像のキャプション title属性編](https://r7kamura.com/articles/2020-11-07-image-caption-revised)

<!-- more -->

## デモ

![滝](1.jpg "滝")


Markdown
```markdown
![滝](1.jpg "滝")
```

HTML
```html
<p>
  <img src="1.jpg" alt="滝" title="滝">
  <span class="caption">滝</span>
</p>
```

title属性に入れた文字列がキャプションとして下にも表示される。

キャプションに使うのがaltではなくtitleなのが重要で、altはアクセシビリティの観点から入力しなくてはならない要素なのに対し、titleは任意なのでキャプションの有無を選べる。

参考にさせていただいたサイトでは、画像を`<figure>`で囲み、キャプションに`<figcaption>`を利用しているが、当ブログではあくまで画像を自己完結型のコンテンツではなく、文脈の中のものとして扱いたいので、画像は`<p>`、キャプションは`<span>`で囲むことにした。

参考: [画像をp要素にするかfigure要素にするか](https://vanillaice000.blog.fc2.com/blog-entry-747.html)


## 注意点

### pタグの中にdivは入れられない

pタグは子要素にフレージングコンテンツしか持たないので、divタグを入れることはできない。今回はキャプションを中央寄せするためdisplay値をblockに変更することで対処した。


### 疑似要素ではできない

CSSのみでできるのではないかと最初に思い浮かんだ案。

```css
img::after{
  content: attr(title);
}
```

`attr()`というCSSの関数で属性の値を擬似要素に出すことができるが、imgは子要素を持たない空の要素なので、疑似要素を使うことはできない。


## Hexoの場合

テーマの中のscriptsフォルダに↓のようなスクリプトを入れることで、サイト生成時にタグを置き換えることができる。

```javascript
hexo.extend.filter.register('after_post_render', function(data){
  data.content = data.content.replace(/<img(.*)title="(.*)">/g, '<img$1title="$2"><span class="caption">$2</span>');
  return data;
});
```
