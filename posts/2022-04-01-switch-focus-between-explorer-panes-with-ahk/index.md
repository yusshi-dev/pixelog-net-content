---
title: AutoHotkeyでエクスプローラーの左側と右側にフォーカスする
date: 2022-04-01 18:53:32
post_id: improve-usage-of-explorer-with-ahk
categories:
  - 開発
tags:
  - AutoHotkey
---

Windowsエクスプローラーではナビゲーションウィンドウ（左ペイン:フォルダ一覧）とコンテンツウィンドウ（右ペイン:ファイルが表示される場所）を移動するショートカットが割り当てられておらず、キーボードで操作するのが大変です。これをAutoHotkeyで解決します。


## スクリプト

キーラベルはお好みに合わせて変更してください。

下の例では、矢印キーのみで操作するときに便利なCtrlとの組み合わせ、Visual Studio Codeのデフォルトショートカットと同じ`Ctrl+0`, ` Ctrl+1`、Alt+Tabと同じ手の形で押せる`無変換+Q`,`無変換+W`に割り当てています。


```ahk
; キー単独押下時
*sc07B::Send, {sc07B}


; ナビゲーションウィンドウにフォーカス Ctrl+←, Ctrl+0, 無変換+Q
^Left::
^0::
sc07B & q::
    ControlFocus, SysTreeView321, A
    Return


; コンテンツウィンドウにフォーカス Ctrl+→, Ctrl+1, 無変換+W
^Right::
^1::
sc07B & w::
    ControlFocus, DirectUIHWND2, A
    Send, {Space}
    Return
```


このホットキーと、英字キーによるファイルの頭出しを組み合わせれば大抵の操作はキーボードで行えます。マウスから手を離したい方はぜひお試しください。