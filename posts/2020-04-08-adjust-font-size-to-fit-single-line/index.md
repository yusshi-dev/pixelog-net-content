---
title: 2行以上の長いタイトルでも１行に収まるように文字サイズをJavaScriptで調整する
date: 2020-04-08 16:04:24
post_id: qezwv8
categories:
  - 開発
tags:
  - Web制作
  - JavaScript
  - HTML
---

記事タイトルが長くなって折り返され、２行になってしまっている...だけど、文字サイズを小さくしてでも１行にきっちり収めたい...！！<!--more-->Webサイトをデザインしているときに、そんな場面に遭遇したことはありませんか。僕はありません。

ということで、２行表示になっている文字列が１行に収まるように、JavaScriptでいい感じに文字サイズを小さくしてみます。

## デモ

この記事のタイトルを１行に収めてみましょう。下のボタンを押してみてください。

<button class="btn" onclick="textAdjust('post__title')">記事タイトルを１行に収めてみる</button>



## ソース

```javascript
function textAdjust(Class) {
  document.addEventListener('DOMContentLoaded', adjust);

  function adjust(){
    const title = document.getElementsByClassName(Class);
    for(i=0; i < title.length; i++) {
      title[i].style.display = 'block';
      const contentWidth = title[i].clientWidth;
      const font_size = document.defaultView.getComputedStyle(title[i], null).fontSize;

      title[i].style.display = 'inline-block';
      title[i].style.whiteSpace = 'nowrap';
      const titleWidth = title[i].clientWidth;

      if(titleWidth > contentWidth){
        const percent =  Math.floor(contentWidth / titleWidth * 100);
        const fontEm_size = percent / 100 + 'em';

        title[i].parentNode.style.fontSize = font_size;
        title[i].style.fontSize = fontEm_size;
      }
    }
  }
}
```

```javascript
textAdjust('post-header__title');
```

引数（緑色でハイライトされている部分）に、文字サイズを調整したい要素のclass名を書いてください。ドットは必要ありません。

調整したい要素が複数ある場合は以下のように関数を複数回実行してください。

```javascript
textAdjust('post-header__title');
textAdjust('post-list__title');
```

## 仕組み

emは親要素に指定されている`font-size`を1とする、相対的な指定方法です。このスクリプトではこの特性を活かして文字サイズを調整しています。

1. そのまま1行で表示したときの幅と、表示領域の幅を調べる
2. 文字列が表示領域より大きかったら？
3. 表示領域に対して文字列がどれくらい大きいのかを考えて文字サイズを計算する
4. 文字列とその親要素に、計算した`font-size`と基準となる`font-size`を指定する

といった流れです。



### 注意点

文字列の要素の幅が、flexboxで自動的に決まっている場合、意図したとおりに動作しない場合があります。

また、ブラウザの設定にはアクセシビリティのため最小フォントサイズという項目があり、PC版のChromeの場合、デフォルトで10pxに設定されています。スクリプトで調整した結果、その設定以下のフォントサイズとなった場合は、コンテンツ幅をはみ出す場合があります。


## 参考にさせていただいた記事

- [記事タイトルが長くたってしっかり１行に自動で表示させてくれるスクリプトをアタシも作ってみた](https://ameblo.jp/gegebax/entry-11450077338.html)

上記のページのものは、アメブロ専用のものでjQueryで書かれていますが、汎用的に使えるようにネイティブJSで書き直させていただきました。ありがとうございました。


<script>
function textAdjust(Class) {
    const title = document.getElementsByClassName(Class);
    for(i=0; i < title.length; i++) {
      title[i].style.display = 'block';
      const contentWidth = title[i].clientWidth;
      const font_size = document.defaultView.getComputedStyle(title[i], null).fontSize;

      title[i].style.display = 'inline-block';
      title[i].style.whiteSpace = 'nowrap';
      const titleWidth = title[i].clientWidth;

      if(titleWidth > contentWidth){
        const percent =  Math.floor(contentWidth / titleWidth * 100);
        const fontEm_size = percent / 100 + 'em';

        title[i].parentNode.style.fontSize = font_size;
        title[i].style.fontSize = fontEm_size;
      }
    }
    document.querySelector('.post__meta').style.fontSize = '14px';
}
</script>
