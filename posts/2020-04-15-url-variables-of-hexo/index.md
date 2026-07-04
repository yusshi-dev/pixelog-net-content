---
title: HexoのURL関連のテンプレート変数まとめ
date: 2020-04-15 13:11:45
post_id: tv55m8
categories:
  - 開発
tags:
  - Hexo
---

HexoにはURL出力関連の変数がたくさん用意されているのですが、種類がたくさんあってあまりにも煩雑なので整理してみます。

<!-- more -->

## 変数を一度に出力してみる

```html
<div>
  config.url: <%= config.url %> <br>
  path: <%= path %> <br>
  url: <%= url %> <br>
  page.permalink: <%= page.permalink %><br>
  url_for(path): <%= url_for(path) %> <br>
  full_url_for(path): <%= full_url_for(path) %> <br>
  decodeURI(full_url_for(path)): <%= decodeURI(full_url_for(path)) %>
</div>
```
Hexoのバージョンは4.2.0です。上のようなHTMLをテンプレートに書いてデバッグしました。

また、当ブログではconfigからcategoriesディレクトリをcategoryへ変更しています。


## 検証結果

<div style="background:var(--bg-color);padding:1em">
<label><input id="toggle-config" type="checkbox">（config.yml の設定）trailing_indexを false にする</label>

<script>
const style =(e,t) =>{
    "loading" !== document.readyState
  ? document.querySelectorAll(e).forEach(e=>e.setAttribute("style",t))
  : document.addEventListener("DOMContentLoaded",() => document.querySelectorAll(e).forEach(e=>e.setAttribute("style",t)))
};
const toggleconfig = document.getElementById('toggle-config');

style('.trailing_index','color:#ff0000');

toggleconfig.addEventListener('change', function(){
  if(toggleconfig.checked){
    style('.trailing_index','color:#ff0000;display:none');
  } else {
    style('.trailing_index','color:#ff0000;display:inline-block');
  }
});
</script>
</div>

### トップページ

変数 | URL
--- | ---
config.url | https\://pixelog.net
path | index.html
url | https\://pixelog.net/index.html
page.permalink | (undefined)
url_for(path) | /<span class="trailing_index">index.html</span>
full_url_for(path) | https\://pixelog.net/<span class="trailing_index">index.html</span>
decodeURI(full_url_for(path)) | https\://pixelog.net/<span class="trailing_index">index.html</span>

### 2ページ目以降

変数 | URL
--- | ---
config.url | https\://pixelog.net
path | page/2/index.html
url | https\://pixelog.net/page/2/index.html
page.permalink | (undefined)
url_for(path) | /page/2/<span class="trailing_index">index.html</span>
full_url_for(path) | https\://pixelog.net/page/2/<span class="trailing_index">index.html</span>
decodeURI(full_url_for(path)) | https\://pixelog.net/page/2/<span class="trailing_index">index.html</span>


### カテゴリーページ（英文字のみの場合）

変数 | URL
--- | ---
config.url | https\://pixelog.net
path | category/web/index.html
url | https\://pixelog.net/category/web/index.html
page.permalink | (undefined)
url_for(path) | /category/web/<span class="trailing_index">index.html</span>
full_url_for(path) | https\://pixelog.net/category/web/<span class="trailing_index">index.html</span>
decodeURI(full_url_for(path)) | https\://pixelog.net/category/web/<span class="trailing_index">index.html</span>

### カテゴリページ（日本語が含まれる場合）

変数 | URL
--- | ---
config.url | https\://pixelog.net
path | category/雑記/index.html
url | https\://pixelog.net/category/%E9%9B%91%E8%A8%98/index.html
page.permalink | (undefined)
url_for(path) | /category/%E9%9B%91%E8%A8%98/<span class="trailing_index">index.html</span>
full_url_for(path) | https\://pixelog.net/category/%E9%9B%91%E8%A8%98/<span class="trailing_index">index.html</span>
decodeURI(full_url_for(path)) | https\://pixelog.net/category/雑記/<span class="trailing_index">index.html</span>


### 記事ページ

変数 | URL
--- | ---
config.url | https\://pixelog.net
path | post/tv55m8/index.html
url | https\://pixelog.net/post/tv55m8/index.html
page.permalink | https\://pixelog.net/post/tv55m8/
url_for(path) | /post/tv55m8/<span class="trailing_index">index.html</span>
full_url_for(path) | https\://pixelog.net/post/tv55m8/<span class="trailing_index">index.html</span>
decodeURI(full_url_for(path)) | https\://pixelog.net/post/tv55m8/<span class="trailing_index">index.html</span>


### 固定ページ

変数 | URL
--- | ---
config.url | https\://pixelog.net
path | about/index.html
url | https\://pixelog.net/about/index.html
page.permalink | https\://pixelog.net/about/<span class="trailing_index">index.html</span>
url_for(path) | /about/<span class="trailing_index">index.html</span>
full_url_for(path) | https\://pixelog.net/about/<span class="trailing_index">index.html</span>
decodeURI(full_url_for(path)) | https\://pixelog.net/about/<span class="trailing_index">index.html</span>


## まとめ

`path`や`url`はconfigの設定と関係なくindex.htmlありの形になります。

`page.permalink`は記事・固定ページ以外ではundefinedになるので使いづらい印象です。また、trailing_indexがtrueになっている場合、記事ページと固定ページでindex.htmlの有無に差が出ます。バグなのだろうか？

テンプレート内では、`url_for(path)`の省略絶対パスの形が使い良いと思います。
