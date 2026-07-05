---
title: 'AutoHotkeyでキーラベルからキーの名前に変換'
category: '技術メモ'
published_at: 2023-12-31 14:36:07
tags: ['AutoHotkey']
---

気づけば今年は一度もブログを書けていなかったので、大晦日に悪あがきでブログを更新...。ここ数日、ノートテイキングアプリをObsidianへの移行作業をしており、Markdownで文章を書く環境が整いつつあるので、来年は頻度を上げて更新できるかもしれません。


## キーの名前を取得

AutoHotkeyの埋め込み変数である`A_ThisHotkey`は、直前に入力したキーが格納されていますが、修飾キーは記号の形になっておりGUIで表示させるときに不便だったので、表示用の文字列に変換します。

処理はいたって単純で、記号を雑に名前に置き換えているだけです。無変換や変換キーも修飾キーとして使用するので、そちらも組み込んでみました。


```ahk
/**
 * ホットキーラベルをキーの名前に変換
 * 
 * @param {string} label A_ThisHotkeyなどの文字列
 * @returns {string} キーの名前
 */
GetKeyStr(label){
    keyName := StrReplace(label, "+", "Shift + ")
    keyName := StrReplace(keyName, "^", "Ctrl + ")
    keyName := StrReplace(keyName, "!", "Alt + ")
    keyName := StrReplace(keyName, "#", "Win + ")
    keyName := StrReplace(keyName, "sc07B & ", "無変換 + ")
    keyName := StrReplace(keyName, "sc079 & ", "変換 + ")
    keyName := RegExReplace(keyName, "\+\s([a-z])", "+ $U1")
    return keyName
}
```


```ahk
#^r::{
  MsgBox(GetKeyStr(A_ThisHotkey)) ; -> Win + Ctrl + R
}

```


