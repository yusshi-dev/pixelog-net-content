---
title: AutoHotkeyでCapsLock(F13)とAltとjの組み合わせが特定のキーボードで動かない
date: 2022-04-06 19:20:54
categories:
  - 開発
tags:
  - AutoHotkey
---

ChangeKeyやレジストリの直接編集でCapsLockをF13に割り当てているとき、CapsLock(F13) + Alt + jの組み合わせが反応しないという事象に遭遇しました。


## やりたいこと

- i, j, k, lに矢印キーの上下左右を割り当てる
- {blind}でShiftやAltキーとの同時押しに対応させ、Shiftとの同時押しで文字を選択したり、Altの同時押しでブラウザの戻る操作などをしたい



## 環境

- AutoHotkey 64bit Version 1.1.33.10
- キーボード: Logicool K270, K295

Logicool K835では意図した通りに動作することを確認した。



## 実験1: CapsLockにF13を割り当てる

レジストリ編集でCapsLockにF13を割り当てる。


```ahk
F13 & i::Send,{Blind}{Up}
F13 & j::Send,{Blind}{Left}
F13 & k::Send,{Blind}{Down}
F13 & l::Send,{Blind}{Right}
```


### 結果

|                          | i   | j   | k   | l   | 
| ------------------------ | --- | --- | --- | --- | 
| CapsLock(F13) +          | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F13) + Shift +  | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F13) + Ctrl +   | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F13) + Alt +    | 〇  | ×   | 〇  | 〇  | 


`CapsLock(F13)` + `Alt` + `j`の組み合わせだけ動かない。




## 実験2: CapsLockにF24を割り当てる

F13に原因があると考え、レジストリ編集でCapsLockにF24を割り当ててみる。


```ahk
F24 & i::Send,{Blind}{Up}
F24 & j::Send,{Blind}{Left}
F24 & k::Send,{Blind}{Down}
F24 & l::Send,{Blind}{Right}
```


### 結果

|                          | i   | j   | k   | l   | 
| ------------------------ | --- | --- | --- | --- | 
| CapsLock(F24) +          | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F24) + Shift +  | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F24) + Ctrl +   | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F24) + Alt +    | 〇  | ×   | 〇  | 〇  | 


`CapsLock(F24)` + `Alt` + `j`の組み合わせだけ動かない。



## 実験3: CapsLockにF12を割り当てる

レジストリ編集でCapsLockにF12を割り当て、本物のF12とCapsLock(F12)との違いを比較する。


```ahk
F12 & i::Send,{Blind}{Up}
F12 & j::Send,{Blind}{Left}
F12 & k::Send,{Blind}{Down}
F12 & l::Send,{Blind}{Right}
```


### 結果


F12キーとの組み合わせ

|                | i   | j   | k   | l   | 
| -------------- | --- | --- | --- | --- | 
| F12 +          | 〇  | 〇  | 〇  | 〇  | 
| F12 + Shift +  | 〇  | 〇  | 〇  | 〇  | 
| F12 + Ctrl +   | 〇  | 〇  | 〇  | 〇  | 
| F12 + Alt +    | 〇  | 〇   | 〇  | 〇  | 


CapsLock(F12)との組み合わせ

|                          | i   | j   | k   | l   | 
| ------------------------ | --- | --- | --- | --- | 
| CapsLock(F12) +          | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F12) + Shift +  | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F12) + Ctrl +   | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F12) + Alt +    | 〇  | ×   | 〇  | 〇  | 


`F12` + `Alt` + `j`は動くが、`CapsLock(F12)` + `Alt` + `j`の組み合わせは動かない。



## 実験4: hに←を割り当てる

jに原因があると考え、左(←)をhに割り当てる。


```ahk
F13 & i::Send,{Blind}{Up}
F13 & h::Send,{Blind}{Left}
F13 & k::Send,{Blind}{Down}
F13 & l::Send,{Blind}{Right}
```


### 結果

|                          | i   | h   | k   | l   | 
| ------------------------ | --- | --- | --- | --- | 
| CapsLock(F13) +          | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F13) + Shift +  | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F13) + Ctrl +   | 〇  | 〇  | 〇  | 〇  | 
| CapsLock(F13) + Alt +    | 〇  | 〇   | 〇  | 〇  | 

すべての組み合わせで動いた。



## 実験5: Alt+jに直接マッピング

`Alt` + `j`に直接マッピングしてみる。


```ahk
!j::Send !{Left}
```

### 結果

動く。Altキーを使う他のショートカットキーと干渉していることはなさそうだった。




## 実験6: 同時押しに直接マッピング

{blind}を使用せず`CapsLock(F13)` + `Shift` + `j`にAlt+{Left}をマッピングする。


```ahk
F13 & j::
  if GetKeyState("Shift") {
    Send 
    return
  }
  return
```


### 結果

動く。



## 実験7: 同時押しに直接マッピング3

今度はShiftをAltに変え、`CapsLock(F13)` + `Alt` + `j`にAlt+{Left}をマッピングする。


```ahk
F13 & j::
  if GetKeyState("Alt") {
    Send 
    return
  }
  return
```

### 結果

動かない。




## 結論・解決策

`CapsLock` + `Alt` + `j`の組み合わせは特定のキーボード(K275、K295)では動作しない。キー同時押しの制約などハード的な問題が要因になっている？



妥協の解決策としては...

- 無変換などCapsLock以外のキーを使う
- i, j, k, lは諦めてh, j, k, l(Vim風)にする。Alt+↓の組み合わせが動かなくなるが、Alt+←やAlt+↑より使用頻度は少ないので我慢する。
- i, j, k, lから右にずらしたo, k, l, +に割り当てる。



### 参考

- [CapsLock + Alt + J Help Please! - AutoHotkey Community](https://www.autohotkey.com/boards/viewtopic.php?t=78588)