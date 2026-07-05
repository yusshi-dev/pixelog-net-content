---
title: 'Netlifyは大文字URLが使えない件とHexoを運用する上で確認すべき設定'
published_at: 2020-01-29 10:30:04
category: '技術メモ'
tags: ['Hexo', 'Netlify']
---

このブログはNetlifyという静的サイトホスティングサービスを使って運用しているのですが、サイドバーに設置しているカテゴリー一覧の英字を含んだカテゴリーをクリックしたとき、毎回301リダイレクトが挟まることに気づきました。

<!--more-->

これはリンクが大文字小文字混合なのに対し、ページのURLは小文字に統一されていることから発生していました。


## Netlifyで大文字URLは使えない

ローカルでサーバーを立ち上げて確認するときURLは大文字小文字混合になっているので、Netlify側に問題があると思い調べてみると...ほんの僅かに情報がありました。

> However, I found that unfortunately this is not a thing that works with Netlify (which I use to host my site) because it actually makes all filenames lowercase, removing any case sensitivity you had set up.
> This is an underdocumented gotcha that I found on their community forums. This has meant that unfortunately I've had to change the implementation of my random filename generation to not use uppercase characters.

[Gotcha: Netlify Makes All Your Filenames Case-Insensitive](https://www.jvt.me/posts/2019/11/11/gotcha-netlify-lowercase/)

Netlifyのドキュメントでは書かれていませんが、コミュニティフォーラムでこのことについて言及している投稿が2件ほどありました。どうやらNetlifyでは大文字のファイルをアップロードすることは許可されておらず、管理画面から設定を変更することもできない模様です。

リダイレクト処理は初回では500ミリ秒以上かかることも多く、ページ読み込み速度も大幅に低下するので、このまま使い続けるという選択肢はないと思います。


## Hexo側の設定

Hexoでは元のファイルネームに関わらず、ページを生成するときに大文字、もしくは小文字へ統一する機能が標準で備わっています。\_config.ymlの項目を確認します。

``` yml
#Writing

（中略）

filename_case: 1 # 0→デフォルト 1→小文字へ変換 2→大文字へ統一
```

当ブログではカテゴリー名が日本語の場合でもスラッグは設定しておらず、アルファベットのみ小文字へ変換していますが特に問題なく使えています。

HexoとNetlifyの組み合わせで運営しているサイトを見て回ると、リダイレクトが挟まっているサイトがたくさん見受けられます。同じ組み合わせで運用している方は一度確認することをおすすめします。
