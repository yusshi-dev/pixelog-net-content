---
title: 'AutoHotkeyでEclipseの左ペインにフォーカス'
category: '技術メモ'
published_at: 2022-07-20 18:48:55
tags: ['AutoHotkey', 'Eclipse']
---

Eclipseはエディタ部分にフォーカスするショートカットキーが`F12`に割り当てられているが、プロジェクト・エクスプローラーなどが配置されている左側のペインにフォーカスするショートカットがデフォルトでは存在しない。

頻繁に使うにもかかわらずショートカットキーがないのは不便なので、キーボード操作のみで完結するようにする。



## その1 設定でショートカットキーを割り当てる

Eclipseの設定画面からショートカットキーを設定してみる。

1. ウィンドウ→設定→一般→キーを開く
2. 「フィルターを入力」にプロジェクト・エクスプローラーと入力し、ビューの表示 (プロジェクト・エクスプローラー)を選ぶ
3. バインディングに好きなショートカットキーを割り当てる

参考) [java - Shortcut key to activate project explorer in Eclipse - Stack Overflow](https://stackoverflow.com/questions/19130384/shortcut-key-to-activate-project-explorer-in-eclipse)

この方法では外部のツールを使うことなく完結するが、左ペインには常にプロジェクトエクスプローラーがあるわけではなく、パースペクティブによっては別のエクスプローラーが配置されることもあり、そういったケースには対応できない。



## その2 AutoHotkeyで解決する

一番手っ取り早い。

```ahk
#IfWinActive, ahk_exe eclipse.exe

; 左ペインに移動 無変換+Q
sc07B & q::
    ControlFocus, SysTreeView321, A
    Return


; 右ペインに移動 無変換+W
sc07B & w::Send, {F12}

#IfWinActive
```

EclipseはAutoHotkeyのWindow SpyでFocus Controlの情報が取れるので、そのコマンドを好きなキーに割り当てる。

AutoHotkeyのスクリプトはスタンドアロンのソフトとしてコンパイルでき、他のPCに持ち運ぶこともできるので、複数の環境で設定を共有したい場合はこちらに分がある。


## あわせて読ませたい

[AutoHotkeyでエクスプローラーの左側と右側にフォーカスする](http://localhost:4000/post/2022/04/01/185332/)
