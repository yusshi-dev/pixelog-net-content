---
title: 'Hexoにテキストリンクのシェアボタンを設置する'
published_at: 2020-07-24 16:44:18
category: '技術メモ'
tags: ['Hexo', 'EJS', 'HTML']
---

Hexoのブログの記事下にシェアボタンを設置する方法。<!-- more -->現在このブログには、思想信条上の理由でシェアボタンは設置していないのですが...。

昔このブログに使っていたものを修正したもので、そのときはSVGアイコンを使っていたのですが、アイコンの権利関係などがいろいろ面倒くさそうなので
、テキストリンクのみという形にしました。これはこれで格好いいかも。


## 対応しているサービス

- Twitter
- Facebook
- Tumblr
- はてブ
- Pocket
- Feedly


## シェアボタン

記事下の位置に下のコードを挿入してください。テンプレートエンジンはEJSです。

```html
<ul class="social-btn">
    <li class="social-btn__item">
        <a class="social-btn__link--twitter" href="https://twitter.com/share?url=<%= url %>&text=<%= encodeURI(page.title) %>">Twitter</a>
    </li>
    <li class="social-btn__item">
        <a class="social-btn__link--facebook" href="http://www.facebook.com/share.php?u=<%= url %>">Facebook</a>
    </li>
    <li class="social-btn__item">
        <a class="social-btn__link--tumblr" href="http://www.tumblr.com/share/link?url=<%= url %>&name=<%= encodeURI(page.title) %>">Tumblr</a>
    </li>
    <li class="social-btn__item">
        <a class="social-btn__link--hatebu" href="http://b.hatena.ne.jp/add?mode=confirm&url=<%= url %>&title=<%= encodeURI(page.title) %>">はてブ</a>
    </li>
    <li class="social-btn__item">
        <a class="social-btn__link--pocket" href="http://getpocket.com/edit?url=<%= url %>&title=<%= encodeURI(page.title) %>">Pocket</a>
    </li>
    <li class="social-btn__item">
        <a class="social-btn__link--feedly" href="https://feedly.com/i/subscription/feed/★★★RSSのリンク★★★">Feedly</a>
    </li>
</ul>
```

class名はBEMでネーミングしていますが、好みにあわせて適当に改良してください。RSSのURLは使っているプラグインによって違うかもしれないので、手動で入れてください。
