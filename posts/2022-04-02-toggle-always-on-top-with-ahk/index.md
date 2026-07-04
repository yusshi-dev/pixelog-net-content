---
title: AutoHotkeyでウィンドウを最前面に固定+トースト通知
date: 2022-04-02 17:57:44
post_id: pin-a-window-always-on-top-with-ahk
categories:
  - 開発
tags:
  - AutoHotkey
---

Windowsでウィンドウを最前面に固定する機能をAutoHotkeyで実現します。


キーラベルはPowerToysの同様の機能と同じ`Win +Ctrl+T`にしていますが、お好みにあわせて変更してください。

```ahk
#^T::
	WinGetTitle, activeWindow, A
	if IsWindowAlwaysOnTop(activeWindow) {
		notificationTitle := "最前面に固定 +++"
		notificationMessage := activeWindow
	}
	else {
		notificationTitle := "解除 ---"
		notificationMessage := activeWindow
	}
	Winset, Alwaysontop, toggle, A
	TrayTip, %notificationTitle%, %notificationMessage%, 3000

	IsWindowAlwaysOnTop(windowTitle) {
		WinGet, windowStyle, ExStyle, %windowTitle%
		isWindowAlwaysOnTop := if (windowStyle & 0x8) ? false : true ; 0x8 is WS_EX_TOPMOST.
		Return isWindowAlwaysOnTop
	}
	Return
```

ウィンドウを固定・解除したとき、対象のウィンドウタイトルとともにトースト通知が表示されます。


## 参考
- [📇 (autohotkey) - wrap selected text in \*symbols\*](https://gist.github.com/gustavomdsantos/46f4b4615eeabb2089478cbcc83cda76)
- [TrayTip - Syntax & Usage | AutoHotkey](https://www.autohotkey.com/docs/commands/TrayTip.htm)
