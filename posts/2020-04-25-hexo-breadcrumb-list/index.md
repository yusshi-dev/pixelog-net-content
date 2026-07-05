---
title: 'Hexoでプラグインを使わずパンくずリストを表示する'
published_at: 2020-04-25 18:15:25
updated: 2020-07-30 15:00:00
category: '技術メモ'
tags: ['Hexo', 'HTML', 'EJS']
---

<p class="alert">
<a href="#新版">(2020-07-30) コードを書き直しました。こちらをご覧ください。</a>
</p>


このブログはHexoで構築しており、全ページにパンくずリストを設置しています。このパンくずリストを設置するのがなかなか大変だったので、メモを残しておきます。

<!-- more -->

使用しているテンプレートエンジンはEJSです。



<del>

## ソース

```ejs
<nav class="breadcrumbs">
    <ol class="breadcrumbs__list">
        <li class="breadcrumbs__item">
            <a class="breadcrumbs__item-link" href="/">
                ホーム
            </a>
        </li>
        <% if (is_home()) { %>
            <% if(page.current !== 1){ %>
                <li class="breadcrumbs__item">
                    <a class="breadcrumbs__item-link" href="/page/<%= page.current %>/">
                        ページ<%= page.current %>
                    </a>
                </li>
            <% } %>
        <% } else if (is_category()) { %>
            <%
            const category_slug = page.path.replace(config.category_dir + '/', '').replace(/\/page\/.+/,'').replace(/\/index.html/, '').split('/');
            let category_url = '';
            %>
            <% category_slug.forEach(function(item, i){ %>
                <%
                category_url += '/' + item;

                const category_name = Object.keys(config.category_map).filter( (key) => {
                  return config.category_map[key] === item;
                 });
                %>
                <li class="breadcrumbs__item">
                    <a class="breadcrumbs__item-link" href="/<%= config.category_dir %><%= category_url %>/">
                        <% if(category_name.length === 0){ %><%= item %><%}else{%><%= category_name %><%}%>
                    </a>
                </li>
            <% }) %>
            <% if(page.current !== 1){ %>
                <li class="breadcrumbs__item">
                    <a class="breadcrumbs__item-link" href="/<%= config.category_dir %><%= category_url %>/page/<%= page.current %>/">
                        ページ<%= page.current %>
                    </a>
                </li>
            <% } %>
        <% } else if (is_tag()) { %>
            <%
            const tag_slug = page.path.replace(config.tag_dir + '/', '').replace(/\/page\/.+/,'').replace(/\/index.html/, '').split('/');
            let tag_url = '';
            %>
            <% tag_slug.forEach(function(item, i){ %>
                <%
                tag_url += '/' + item;

                const tag_name = Object.keys(config.tag_map).filter( (key) => {
                  return config.tag_map[key] === item;
                 });
                %>
                <li class="breadcrumbs__item">
                    <a class="breadcrumbs__item-link" href="/<%= config.tag_dir %><%= tag_url %>/">
                        #<% if(tag_name.length === 0){ %><%= item %><%}else{%><%= tag_name %><%}%>
                    </a>
                </li>
            <% }) %>
            <% if(page.current !== 1){ %>
                <li class="breadcrumbs__item">
                    <a class="breadcrumbs__item-link" href="/<%= config.tag_dir %><%= tag_url %>/page/<%= page.current %>/">
                        ページ<%= page.current %>
                    </a>
                </li>
            <% } %>
        <% } else if (is_archive()) { %>
            <li class="breadcrumbs__item">
                <a class="breadcrumbs__item-link" href="/<%= config.archive_dir %>/<%= page.year %>/">
                    <%= page.year %>年
                </a>
            </li>
            <% if(page.month){ %>
              <li class="breadcrumbs__item">
                  <a class="breadcrumbs__item-link" href="<%- url_for(path) %>">
                      <%= page.month %>月
                  </a>
              </li>
            <% } %>
        <% } else if(is_post()) { %>
            <% page.categories.forEach(function(item){ %>
                <li class="breadcrumbs__item">
                    <a class="breadcrumbs__item-link" href="<%= url_for(item.path) %>">
                        <%= item.name %>
                    </a>
                </li>
            <% }) %>
                <li class="breadcrumbs__item">
                    <a class="breadcrumbs__item-link" href="<%- url_for(path) %>">
                        <%- page.title %>
                    </a>
                </li>
        <% } else if (is_page()) { %>
            <li class="breadcrumbs__item">
                <a class="breadcrumbs__item-link" href="<%- url_for(path) %>">
                    <%- page.title %>
                </a>
            </li>
        <% } %>
    </ol>
</nav>
```

## 仕様

スパゲッティーコードになっているのは申し訳ありません。自分のスキルではこれが限界です😥

問題はカテゴリーページの方にありまして、カテゴリーを階層化している場合で、子カテゴリーのページを表示させているとき、そのカテゴリーが属する親カテゴリー名を出す変数がないんですよ。たぶん。

なのでどうしているかと言いますと、URLからドメインと"categories"(デフォルト)を取り除き、スラッシュでカテゴリー名に切り分け変数に格納、そしてそれを基にURLとカテゴリー名を作りHTMLに出す...みたいなことをやっています。


## カテゴリーのスラッグについて

Hexoでは、日本語カテゴリー・タグの英字スラッグを_config.ymlで指定することができます。これで日本語URLを回避できるという訳ですね。

```yml
category_map:
  CSS: css
  ドライブ: drive
  日記: diary
  未分類: uncategorized

tag_map:
  食べ物: food
```

先述の通り、カテゴリーページやタグページでは、URLを元にパンくずリストを作成するので、URLに現れる文字列とカテゴリー名が一致しないということが起こり得ますが、config.yml内でスラッグが指定されている場合はそれらを参照し、元のカテゴリー名を表示できる仕組みになっています。

`filename_case: 1`でのURL小文字統一には対応していないので、これを利用する場合でも、

```yml
category_map:
  CSS: css
  HTML: html
  Hexo: hexo
  JavaScript: javascript
```

のように、逐一category_mapを設定してください。

</del>


## 新版

あまりにもひどい仕様だったので書き直しました。

```ejs
<%
let breadcrumbs = { ホーム: url_for('/')};

if(is_post()) {
    page.categories.forEach(function(item){
        breadcrumbs[item.name] = url_for(item.path);
    })
    breadcrumbs[page.title] = url_for(path);
} else if (is_page()) {
    breadcrumbs[page.title] = url_for(path);
} else if (is_category()) {
    let cat = get_category();
    for(var key in cat) {
        breadcrumbs[key] = cat[key];
    }
} else if (is_tag()) {
    breadcrumbs['#'+page.tag] = url_for(path);
} else if (is_archive()) {
    breadcrumbs['アーカイブ'] = '/' + config.archive_dir + '/';
    if(page.year){
        breadcrumbs[page.year+'年'] = '/' + config.archive_dir + '/' + page.year + '/';
    }
    if(page.month){
        breadcrumbs[page.month+'月'] = url_for(path);
    }
}
if(page.current > 1){
    breadcrumbs['ページ'+page.current] = '/' + config.pagination_dir + '/' + page.current + '/';
}

function reverse_array(a) {
    var key = [];
    for (var i in a) {
        key.push(i);
    }
    key.reverse();
    var ret = [];
    for (var i in key) {
        ret[key[i]] = a[key[i]];
    }
    return ret;
}

function get_category(){
    let arr = {};
    arr[page.category] = url_for(page.path);
    get_parent(page.category);

    function get_parent(current){
        const current_cat = site.categories.data.filter(x => x.name === current );
        const parent_id = current_cat[0].parent;

        if(parent_id) {
            const parent_name = site.categories.data.filter(x => x._id === parent_id);
            arr[parent_name[0].name] = url_for(parent_name[0].path);
            get_parent(parent_name[0].name);
        }
    }
    return reverse_array(arr);
}
%>

<nav class="breadcrumbs">
    <ol class="breadcrumbs__list">
      <% for(var key in breadcrumbs) { %>
          <li class="breadcrumbs__item">
              <a class="breadcrumbs__link" href="<%= breadcrumbs[key] %>">
                  <%= key %>
              </a>
          </li>
      <% } %>
    </ol>
</nav>
```

### 改善点

以前のものはループが複雑怪奇を極めていたので、まず最初にパンくずリストを配列でつくって、最後にまとめてHTMLに出すようにしました。

旧版ではカテゴリーページの場合に、URLからカテゴリーを取得するというトリッキーなことをやっていたのですが、カテゴリー名から親カテゴリーを取得する関数を書いたので、かなりシンプルになりました。

`_config.yml`のカテゴリーのスラッグ、filename_caseの設定にも対応しています。旧版はスラッグの指定が必須でしたが、新版はスラッグを設定していなくても不具合は出ません。


## 次の記事

[Hexoでパンくずリストの構造化マークアップをする](/post/surt79/)
