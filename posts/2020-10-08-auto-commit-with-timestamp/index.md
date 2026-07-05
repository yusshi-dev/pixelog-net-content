---
title: 'Gitで日付をコミットメッセージにして自動でリモートへプッシュする'
published_at: 2020-10-08 11:43:00
category: '技術メモ'
tags: ['Git', 'PowerShell']
---

Gitでステージングから、コミット、リモートまでの流れをPowerShellで自動化してみる。ゲームのセーブデータなどを保管するのに役立つ。

<!-- more -->

## PowerShell

```powershell
#!/bin/sh

$date = Get-Date -Format "yyyy-MM-dd HH:mm"

cd <レポジトリのパス>
git add .
git commit -m $date
git push origin master
```

これを`save.ps1`などの名前で保存し、パスの通ったディレクトリへ配置すれば、あとは

1. [Winキー]+[X]
2. [I]
3. `$ save`

でファイルのステージング、日付と時刻をコミットメッセージにしたコミット、リモートへのプッシュが自動で行われる。GitHubを倉庫みたいに使いたい人にとっては、一連の作業が隠蔽されるので便利。

最近PowerShellを起動するためのショートカットが便利なことに気づいた。ディレクトリを移動する作業は面倒でとにかく避けたいので、よく使う流れはシェルスクリプトでまとめておくと作業が楽になる。
