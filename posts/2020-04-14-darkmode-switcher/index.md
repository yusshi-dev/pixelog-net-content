---
title: OSでもWebサイト側でも切り替えられるダークモードを実装する
date: 2020-04-14 18:49:31
post_id: 2xc2mg
categories:
  - 開発
tags:
  - Web制作
  - JavaScript
  - CSS
---

昨年(2019年)あたりからダークモードが流行しているということで、様々なデバイスやアプリでダークモードが実装されています。というわけで、少し前にこのブログもダークモードに対応させました。そのときのメモ。

<!-- more -->


## 求める機能

- OSのダークモードと連動すること。
- サイト側でもモードの切り替えができること。
  - （理由1）Android9以下などOSレベルでのダークモードに対応していないデバイスが存在する。（アプリレベルで実装されていることはあるが）
  - （理由2）OSはダークモードでもサイトは明るく見たい人がいるかもしれない。
  - （理由3）Windows10のダークモードの設定が奥深いところにあって、切り替えるのが面倒。スケジューラー機能も備わってない。めんどくさい。
- ページ遷移してもダークモードの設定が保存されること。
- CSS変数で管理できること。


## ダークモードを実装する

```javascript
(function () {
    const toggle_switch = document.getElementById('toggle-dark');
    const isDark = window.matchMedia('(prefers-color-scheme: dark)');
    const htmlElement = document.documentElement;
    const keyLocalStorage = 'theme';
    const localTheme = sessionStorage.getItem(keyLocalStorage);

    if(localTheme) { // サイト側の設定が優先
        changeMode(localTheme);
    } else if(isDark.matches) { // OSがダークモードだったら
        changeMode('dark');
    }

    toggle_switch.addEventListener('change', () => { // スイッチが押されたとき
        if(toggle_switch.checked){
            changeMode('dark', 'set');
        } else {
            changeMode('light', 'set');
        }
    });
    isDark.addListener(e => { // サイトを開いている間にOSのモードが変わったとき（そんなことあるのか？）
        if (e.matches) {
            changeMode('dark', 'remove'); // サイト側の設定は消しておく（OSを優先にする）
        } else {
            changeMode('light', 'remove');
        }
    });

    function changeMode(mode, storage){
        if(mode === 'dark') {
            htmlElement.dataset.mode = mode;
            toggle_switch.checked = true;

        } else if(mode === 'light') {
            delete htmlElement.dataset.mode;
            toggle_switch.checked = false;
        }
        if(storage === 'set'){
            sessionStorage.setItem(keyLocalStorage, mode);
        } else if(storage === 'remove') {
            sessionStorage.removeItem(keyLocalStorage);
        }
    }
}());
```

```html
<div class="switch">
  <input class="switch__input" id="toggle-dark" type="checkbox">
</div>
```

サイト側でモードを切り替えるチェックボックスを設置します。チェックボックスはスイッチUI風にするなど好きなように装飾してください。

OSの設定は上書きできませんが、OSがダークモードかどうかはJavaScriptで判定できるので、ダークモード時にhtmlタグへ`data-mode="dark"`を動的に追加します。

チェックボックスで切り替えた際も同様に`data-mode="dark"`を追加し、その情報をsessionStorageへ保存し、タブを閉じるまで設定を保持させます。localStorageにすれば永続的に保存することもできます。


## CSSの書き方

通常ダークモードはメディアクエリで指定しますが、今回はdata属性を使って動的に操作するので、CSSでは以下のように記述します。

```css
/* ライトモード時 */
:root {
    --theme-color: #aaa;
}

/* ダークモード時 */
[data-mode='dark'] {
    --theme-color: #000;
}

[data-mode='dark'] img {
    filter: grayscale(20%);
}
```

色はCSS変数にまとめると非常に楽です。 （IEはかなぐり捨てることになってしまいますが）

今まで地に書いていた色指定をこれから変数にまとめるという場合は、よほどCSSがシステマティックに管理されていない限り、かなり大変かと思います。（この作業が一番大変でした）覚悟して望みましょう。
