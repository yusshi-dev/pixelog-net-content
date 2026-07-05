---
title: 'スマホ表示時のGoogle Adsenseで左右に余白(padding)を適用する'
published_at: 2020-04-22 18:56:21
category: '技術メモ'
tags: ['Web制作', 'HTML']
---

Google Adsenseでレスポンシブタイプの広告ユニットは、デフォルトではスマホ版で表示した際に、横幅が100%になります。

<!-- more -->

だったら`<div>`で囲ってpaddingを指定すればいいのでは、というのが普通の発想なのですが...

これがなかなか強敵でして、なんと！Adsenseのscriptが親要素のpaddingを取得して、そのネガティブmarginをinsタグのstyle属性へ動的にセットする、というヤクザみたいなことを仕掛けてきます。

なので、ちゃんと火の元から消火しておかなければ太刀打ちできません。



## 手順

消火といえども、Adsenseの方でオプションが切り替えられるようになっているので、手順は簡単です。

まずレスポンシブ広告のinsタグをソースの中から探します。

```html
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="(hoge)"
     data-ad-slot="(hoge)"
     data-ad-format="auto"
     data-full-width-responsive="true">
</ins>
```


上のコードでいう6行目、`data-full-width-responsive`属性の値を`false`にすればOKです。

```html
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="(hoge)"
     data-ad-slot="(hoge)"
     data-ad-format="auto"
     data-full-width-responsive="false">
</ins>
```

これで、Adsenseの横幅は親要素のwidthやpaddingに従って横幅を満たすようになります。Adsenseを囲う要素にpaddingを指定してやればOKです。


## 収益が下がる可能性がある

Googleのヘルプページには以下のような記述があります。

> モバイル デバイス画面でレスポンシブ広告ユニットが全幅サイズに自動拡張される動作をご希望でない場合は、data-full-width-responsive パラメータを "false" に設定します。ただし、これによって収益増加の機会が減る可能性があります。Google のテストにより、モバイル デバイスが縦表示の場合には全幅サイズのレスポンシブ広告の成果が高いことが判明しています。
> [広告ユニット レスポンシブ広告のタグパラメータの使用方法](https://support.google.com/adsense/answer/9183460?hl=ja)

やはり、広告の表示面積がpaddingの分小さくなるということで、収益は下がる可能性があるそうなので、収益に重点を置く方は注意が必要です。
