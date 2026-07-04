---
title: AutoHotkeyでホームポジションから離れずにカーソルを移動
categories:
  - 開発
date: 2022-04-07 19:11:53
tags:
  - AutoHotkey
---


AutoHotkeyでホームポジションから指を離さずにカーソルを移動するショートカット。無変換キーとの組み合わせで文字編集が簡単になる。




## ソース


```ahk
; キー単独押下時の動作
*sc07B::Send, {sc07B}
*sc079::Send, {sc079}


; キャレット移動
; i, j, k, l
sc07B & i::Send,{Blind}{Up}
sc07B & j::Send,{Blind}{Left}
sc07B & k::Send,{Blind}{Down}
sc07B & l::Send,{Blind}{Right}
sc07B & u::Send,{Blind}{Home}
sc07B & o::Send,{Blind}{End}

; Mac風
sc07B & p::Send,{Blind}{Up}
sc07B & b::Send,{Blind}{Left}
sc07B & n::Send,{Blind}{Down}
sc07B & f::Send,{Blind}{Right}
sc07B & a::Send,{Blind}{Home}
sc07B & e::Send,{Blind}{End}
sc07B & d::Send,{Blind}{Delete}
sc07B & h::Send,{Blind}{BackSpace}

sc07B & m::Send,{Blind}{Enter}

    
; キャレット 選択+移動
; i, j, k, l
sc079 & i::Send,+{Up}
sc079 & j::Send,+{Left}
sc079 & k::Send,+{Down}
sc079 & l::Send,+{Right}
sc079 & u::Send,+{Home}
sc079 & o::Send,+{End}

; Mac風
sc079 & p::Send,+{Up}
sc079 & b::Send,+{Left}
sc079 & n::Send,+{Down}
sc079 & f::Send,+{Right}
sc079 & a::Send,+{Home}
sc079 & e::Send,+{End}
sc079 & d::Send,+{End}{BackSpace}
sc079 & h::Send,+{Home}{BackSpace}

sc079 & m::Send,{Blind}{Enter}
```

カーソル移動は、i,j,k,lとf,b,p,nによるMac風のショートカットの両方に対応している。それぞれが干渉しないキー配置になっていることに気付き、同時にホットキーを割り当てるという天啓を得て実装してみたら、やはり気分に合わせて指使いを変えることができ便利だった。

文字の選択は無変換とShiftの同時押しで可能だが、3つのキーの同時押しは指が辛いので、変換キーにShiftを組み込んだ。カーソル移動のキーがスペースキーを挟んで左右対称になるので、ある意味合理的である。