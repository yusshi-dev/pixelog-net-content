---
title: AutoHotkeyでブラウザのカーソルブラウジングを完全に無効化する
date: 2022-02-17 18:55:00
post_id: disable-caret-browsing-with-ahk
categories:
  - 開発
tags:
  - AutoHotkey
---

ChromeやBraveなどのブラウザには、カーソルブラウジングという機能が備わっており、`F7`キーで有効にすると、点滅するキャレットを使ってブラウザを操作できます。

しかし日本語環境ではカタカナ変換でF7キーを押すなどして、意図せず有効になってしまうことも多く、必要のない人にとっては邪魔な機能なので、AutoHotkeyで恒久的にカーソルブラウングを無効にします。



## ソース

このスクリプトでは[IME.ahk](https://w.atwiki.jp/eamat/pages/17.html)の関数を使用しています。サイトからダウンロードして#includeや切り出すなどしてあらかじめ読み込んでください。

```ahk
#IfWinActive, ahk_exe brave.exe ; ブラウザを指定する
    ; カーソルブラウジング無効
    #If IME_GetConverting() == 0
        F7::Return
    #If
#IfWinActive
```


`IME_GetConverting()`でIMEの状態をチェックし、入力状態でない場合のみF7を無効にするので、F7キーでのカタカナ変換は引き続き可能です。F7でカタカナ変換をしない場合は常時無効化でもいいと思います。

以上です。