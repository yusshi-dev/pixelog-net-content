---
title: 'Hexoでmarkdown-itを導入して記事内に脚注を書く'
published_at: 2020-04-28 13:18:38
category: '技術メモ'
tags: ['Hexo']
---

HexoでMarkdownについていろいろ調べてみると、デフォルトで入っているhexo-renderer-markedの代わりにhexo-renderer-markdown-itを導入しているブログが多いようなんですね。

<!-- more -->

え、Markdownのパーサーなんて別に何でもよくね？なんかmarkedに恨みでもあるの？

と思って導入をずっと先送りにしていたのですが、先日、記事内で注釈を使いたいなと思ったときに、調べてみるとmarkedの方は注釈に対応していない、ということだったので、markdown-itを入れてみることにしました。

## markdown-itとは

markdown-itのプラグインはmarkedと同様、Hexoのオフィシャルプラグインです。

レポジトリのドキュメントより

- GFMとCommonMarkに対応
- 細かい設定項目
- hexo-renderer-markedより速い
- 見出しが被ったたときにアンカーが衝突するのを回避してくれる
- 注釈対応
- `<sub>` `<sup>` `<ins>`に対応


## 導入方法

デフォルトで入っているmarkedをアンインストールして、markdown-itをインストールするだけです。

```console
$ npm un hexo-renderer-marked --save
$ npm i hexo-renderer-markdown-it --save
```

また、config.ymlからfootnoteのプラグインを有効にしておきます。

```yml
markdown:
  plugins:
    - markdown-it-footnote
```

詳しいオプションは[hexo-renderer-markdown-it](https://github.com/hexojs/hexo-renderer-markdown-it)のreadmeやWikiを参照


## 脚注の書き方

脚注はMarkdownを用いて下のように書けます。

```markdown
覚悟はいいか？オレはできてる[^1]
[^1]: ジョジョの奇妙な冒険より
```

括弧の中の文字はリンクと脚注のラベリングで、2つが一致していれば数字でなくとも可、とのことです。
