---
title: 'HexoのテーマでTailwind CSSを利用する'
published_at: 2024-12-31 12:21:10
category: '技術メモ'
tags: ['Tailwind CSS']
---

HexoのテーマにTailwind CSSを利用する方法の覚え書き。


## インストール

Tailwind CSSを導入するには、PostCSSの環境構築が必要となります。これに加え、Hexo上でPostCSSを動作させるために、以下のパッケージを併せてインストールします。

```
npm install -D tailwindcss postcss autoprefixer postcss-import postcss-load-config
```

なお、最新バージョンのHexoにおいてhexo initを使用して環境をセットアップした場合、pnpmがデフォルトのパッケージマネージャーとして利用される仕様となっています。この場合、以下のコマンドを使用してインストールを行ってください。


```
pnpm add -D tailwindcss postcss autoprefixer postcss-import postcss-load-config
```



## PostCSSの準備

PostCSSの設定ファイルを作成するには、プロジェクトルートに.postcssrc.jsというファイルを新規作成し、以下の内容を記述します。
```
module.exports = {
    from: undefined,
    plugins: {
        'postcss-import': {},
        'tailwindcss': {},
        'autoprefixer': {},
    }
}
```


次に、Hexoのテーマフォルダ内にあるscriptsディレクトリ直下に、以下のスクリプトを配置します。このスクリプトは、Hexoのレンダリング処理とPostCSSの間を橋渡しする役割を担います。ファイル名は任意で問題ありません。

```
'use strict'

const postcss = require('postcss')
const postcssrc = require('postcss-load-config')

hexo.extend.renderer.register(
    'css',
    'css',
    data => {
        return postcssrc()
            .then(({plugins, options}) => postcss(plugins).process(data.text, options))
            .then(result => result.css)
    }
);
```


## TailwindCSSの設定

Tailwind CSSの初期設定を行うには、以下のコマンドを実行します。


```
npx tailwindcss init
```


これにより、プロジェクトルート直下にtailwind.config.jsという設定ファイルが生成されます。このファイルを以下のように修正してください。


```
/** @type {import('tailwindcss').Config} */

const colors = require('tailwindcss/colors');

module.exports = {
  content: ["./themes/hexo-theme-foo/layout/**/*.ejs"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

PostCSSは、ソースコードを解析して必要なCSSクラスのみを最終的な出力ファイルに含める仕組みとなっています。このため、contentプロパティに解析対象のファイルパスを正確に指定する必要があります。


## HTML（EJS）でCSSを読み込む

TMLまたはEJSファイルにおいてTailwind CSSを適用するには、以下のようにCSSを記述します。

```
@import "./themes/hexo-theme-foo/source/css/bar.css";

@tailwind base;
@tailwind components;
@tailwind utilities;
```

Tailwind CSS以外のCSSファイルを併用する場合は、@tailwindアノテーションよりも前に@importを記述する必要があります。この順序を守らないと正しく動作しないので注意してください。

```
@import "./themes/hexo-theme-foo/source/css/bar.css";

@tailwind base;
@tailwind components;
@tailwind utilities;
```
