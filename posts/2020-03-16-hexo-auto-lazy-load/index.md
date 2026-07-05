---
title: 'Hexoで画像にloading=&quot;lazy&quot;を自動で追加して画像を遅延読み込みする'
published_at: 2020-03-16 16:42:43
category: '技術メモ'
tags: ['Hexo', 'HTML']
---

Chromeでいよいよ画像の遅延読み込みがネイティブでサポートされました。

<!--more-->

imgタグにloading="lazy"属性を指定するだけで、ライブラリなどを使わずとも画像が遅延読み込みされる、という簡単なものです。

しかし、毎回手で書き込むのは大変で保守性に欠けるので、今回はHexoでサイト生成時に自動で追加する方法を紹介します。

## Filterの準備

Hexoの[Filter](https://hexo.io/api/filter.html)を使って、記事内のimgタグの一部をloading="lazy"込みに書き換えます。

以下のコードを`lazyload.js`など適当な名前をつけて、テーマフォルダの`scripts`ディレクトリに保存してください。

scriptsフォルダにあるJSはNode.jsとしてサイト生成時に勝手に実行されます。


```javascript
hexo.extend.filter.register('after_post_render', function(data){
  data.content = data.content.replace(/<img src=/g, '<img loading="lazy" src=');
  return data;
});
```

srcの直前に入るのはいささか見た目が悪い気もしますが、後ろに入れるとなると正規表現を頑張らないといけないのでめんど...


## 属性を追加しなくても有効になっている？

余談ですが。

[Chromeのloading属性は、指定しなくてもデフォルトで"lazy"の挙動となりうる　～loading属性の落とし穴から学ぶ、機能拡張と初期値・デフォルト挙動の話～](https://qiita.com/spaceonly/items/9d9b3fe46e43524a535a)

loading属性の初期値は、従来の表示方法に当たるeagerではなく、あくまでautoであり、これは環境によっては何もしなくてもlazyloadが有効になるということであります。

ネットワーク環境が潤沢な場合には、画像はすぐ表示されるに越したことはないので、わざわざlazyを指定してlazyloadを強制する必要は果たしてあるのだろうか...。

仕様がまだ流動的で安定していないので、喜び勇んで記事内のタグを全て書き換えるとか、そういう段階にはまだ達していないのかな、という印象です。おしまい。
