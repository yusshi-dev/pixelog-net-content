---
title: 複数のファイルを横断して文字列を一括で置き換えたい
date: 2020-07-10 19:17:03
post_id: ucwep9
categories:
  - 雑記
tags:
  - Node.js
  - JavaScript
---

複数のテキストファイル（バイナリファイルの反対の意）に含まれる文字列を他の文字列に一括で置き換えたい場面って偶にありますよね。

<!-- more -->

自分の場合は、過去に書いたブログ記事のとあるソースを修正したくて、試行錯誤していました。WordPressなら便利なプラグインがありますが、静的サイトジェネレーターとなると、そうはいかないので...

結局Node.jsで解決したのでその方法のメモ。


## お借りしたもの

[ディレクトリ内のファイルを列挙する | まくまくNode.jsノート](https://maku77.github.io/nodejs/io/walk-dir.html)

[天才まくまくノート](https://maku77.github.io/)さんの上記の記事で配布されているモジュール「dirutil.js（見つかったファイルを配列で返すバージョン）」をお借りしました。ありがとうございます。

## コード

サンプルはhttpプロトコルをhttpsに置き換えるものです。

```javascript
var fs = require('fs');
var dirutil = require('./dirutil');

function replaceContent(dir){
  dirutil.walkDir(dir, function (err, path) {
    var filterItems = path.filter(function(value) {
      return value.match(/.md/); // markdownファイルに限定
    });

    for(let i=0; i<filterItems.length; i++){
      fs.readFile(filterItems[i], 'utf8', function (err, data) {
        if (err) throw err;
        data = data.replace(/http/g, 'https'); // 文字列を置き換え

        fs.writeFile(filterItems[i], data, function (err) {
          if (err) throw err;
          console.log(filterItems[i] + ' completed');
        });

      });
    }
  });
}
replaceContent('_posts')
```

```plaintext
__post/
　├ 2018/
　│　└ file0.txt
　├ 2019/
　│　└ file1.md
　├ 2020/
　│　└ 04/
　│　　　└ file2.md
　│　　　└ sample.img
　└ file3.md
```

`replaceContent`の引数にフォルダの名前を渡して実行すると、サブディレクトリも含めたファイル全てを取得し文字を置き換えられます。

7行目のところで対象をMarkdownファイルに限定しているので、上のようなディレクトリ構造であればfile1.mdからfile3.mdまでが対象になります。

13行目で`.replace`を使って置き換えする文字列を指定しています。正規表現で複雑な指定も可能です。

というわけで、今回初めて自力でNode.jsのJavaScriptを書いてみました。ツッコミどころ満載のような気がしますが、独学のトーシローによるものなのでお許しください...。プロのエンジニアの人に怒られそう。

このスクリプトを書くよりも、その時間で地道に修正したほうが早かったのでは、という気しかしませんが、勉強になったので良いということで。
