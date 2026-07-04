---
title: Netlify Managed DNSでGoogle Search Consoleの認証をする
date: 2019-03-16 16:29:18
post_id: pjayu5
categories:
  - 開発
tags:
  - Netlify
---
Google Search Consoleではドメイン単位でサイトを追加する際にDNSにTXTレコードを追加して、ドメインの所有権を確認する必要があります。

<!--more-->

このとき、Netlifyが提供しているDNSを利用している場合には、Netlify側でも作業を行います。

## 手順
### Google Search Consoleでの作業

![Google Search Console](1.png)

プロパティの追加からドメイン名を入力して、予め上の画面からTXTレコードを取得しておきます。

### Netlifyでの作業

![NetlifyでDomain settingsを選択](2.png)

Netlifyへログインして登録するサイトを選び、「Domain setteings」を選択。

![NetlifyでGo to DNS panelを選択](3.png)

Custom Domainsからドメインを選び、サブメニューから「Go to DNS panel」を選択。

![NetlifyでAdd new recordを選択](4.png)

「Add new record」を選択し、DNSレコード入力画面を開きます。

![NetlifyでTXTレコードを入力](5.png)

Record typeに「TXT」を選び、ValueにGoogle Search Consoleで取得したTXTレコードを入力します。
