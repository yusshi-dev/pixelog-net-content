---
title: HexoのショートコードでAmazonアソシエイトリンクを挿入
date: 2020-11-15 15:15:29
post_id: y7cfvk
categories:
  - 開発
tags:
  - Hexo
  - JavaScript
  - ブックマークレット
---


Hexoでショートコードを使ってAmazonの商品リンクを簡単に挿入する方法のメモ。

<!-- more -->


以前から収益以前にただAmazonの商品の画像をパク...お借りしたいと思っていて、Amazonアソシエイトプログラムに登録した。したもののそこで生成されるタグがあまりに汚く残念だったので、タグは自分で考えることに。

将来他のCMSへ移行したくなったときのために、記事の保守性が落ちるような記述は避けたいと思っていた。しかしHugoでも同じ仕組みがあるのと、記事内はシンプルな方がいいという風に考えが変わったので、HexoでAmazonの商品リンクを挿入できるショートコードの作成に踏み切った。



## デモ

ショートコードで商品リンクを貼ったときの表示。

{% amazon B000UVXI7G 'Amazon | debut | 辻井伸行 | 室内楽・器楽曲 | 音楽' %}

余談ですがこのCD、2020年11月時点で新品が285円まで下落していてオススメ。（上記リンクはAmazonアソシエイトのリンクを使用しています）



### スクリプト

テーマの中のscriptsフォルダの中に下のコードを配置。トラッキングIDの箇所を埋めることに注意する。

```javascript
hexo.extend.tag.register('amazon', function(args){
    const id = 'トラッキングID'
    const asin = args[0];
    const title = args[1];
    return `<p><a href="https://www.amazon.co.jp/dp/${asin}/?tag=${id}"><img src="https://m.media-amazon.com/images/P/${asin}.jpg" alt="${title}" width="400"><br>${title}</a></p>`  
});
```

記事内ではリンクを挿入したい箇所へ下のように書く。

```markdown
{% amazon 10ケタのASIN 'ページのタイトル' %}
```


## ブックマークレット

ショートコードと言えどもそのショートコードを書くのすらめんどくさいと思ってしまったので、それも自動で作るブックマークレットを作った。

Amazonで商品のページを開きブックマークレットを実行すればショートコードをコピーできる。

コピペ用

```javascript
javascript:(function(){var asin = document.getElementById('ASIN').value;var title = document.title;window.prompt('ショートコード', `{% amazon ${asin} '${title}' %}`);})();
```

中身

```javascript
javascript:
(function(){
  var asin = document.getElementById('ASIN').value;
  var title = document.title;
  window.prompt('ショートコード', `{% amazon ${asin} '${title}' %}`);
})();
```


## 自己責任で

この記事の方法はアソシエイトツールバーで発行されるタグに存在する、アクセスカウント用の画像を読み込んでいない。あくまで公式の方法ではないのでご利用は自己責任で。

参考記事

- [【ASINをコピペ】Amazonアソシエイトのテキストリンクの簡単な作り方](https://omocam.net/easy-associate-text-link/)
- [アソシエイト・セントラル - リンクの動作確認ツール](https://affiliate.amazon.co.jp/home/tools/linkchecker)
