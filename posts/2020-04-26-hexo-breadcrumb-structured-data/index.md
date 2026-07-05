---
title: 'Hexoでパンくずリストの構造化マークアップをする'
published_at: 2020-04-26 15:24:44
updated: 2020-07-30 15:00:00
category: '技術メモ'
tags: ['Hexo', 'HTML', 'EJS']
---

<p class="alert">
<a href="#新版">(2020-07-30) コードを書き直しました。こちらをご覧ください。</a>
</p>

[前回](/post/xzr273/)はページにパンくずリストを表示させましたが、今回は構造化マークアップ編です。これを設置することによって、Googleにページの階層構造をより的確に伝えることができるようになります。

<!-- more -->

使用しているテンプレートエンジンはEJSです。


<del>

## ソース

```ejs
<script type="application/ld+json">
{
  "@context": "http://schema.org",
  "@graph": [
    {
      "@type": "BreadcrumbList",
      "itemListElement":
      [
        {
          "@type": "ListItem",
          "position": 1,
          "item":
          {
            "@id": "/",
            "name": "ホーム"
          }
        }
        <% if (is_home()) { %>
            <% if(page.current !== 1){ %>
                ,{
                  "@type": "ListItem",
                  "position": 2,
                  "item":
                  {
                    "@id": "/page/<%= page.current %>/",
                    "name": "ページ<%= page.current %>"
                  }
                }
            <% } %>
        <% } else if (is_category()) { %>
            <%
            const category_slug = page.path.replace(config.category_dir + '/', '').replace(/\/page\/.+/,'').replace(/\/index.html/, '').split('/');
            let category_url = '';
            %>
            <% category_slug.forEach(function(item, i){ %>
                <%
                i += 2;
                category_url += '/' + item;

                const category_name = Object.keys(config.category_map).filter( (key) => {
                  return config.category_map[key] === item;
                });
                %>
                ,{
                  "@type": "ListItem",
                  "position": <%= i %>,
                  "item":
                  {
                    "@id": "/<%= config.category_dir %><%= category_url %>/",
                    "name": "カテゴリー: <% if(category_name.length === 0){ %><%= item %><%}else{%><%= category_name %><%}%>"
                  }
                }
            <% }) %>
            <% if(page.current !== 1){ %>
                ,{
                  "@type": "ListItem",
                  "position": <%= category_slug.length + 2 %>,
                  "item":
                  {
                    "@id": "/<%= config.category_dir %><%= category_url %>/page/<%= page.current %>/",
                    "name": "ページ<%= page.current %>"
                  }
                }
            <% } %>
        <% } else if (is_tag()) { %>
            <%
            const tag_slug = page.path.replace(config.tag_dir + '/', '').replace(/\/page\/.+/,'').replace(/\/index.html/, '').split('/');
            let tag_url = '';
            %>
            <% tag_slug.forEach(function(item, i){ %>
                <%
                i += 2;
                tag_url += '/' + item;

                const tag_name = Object.keys(config.tag_map).filter( (key) => {
                  return config.tag_map[key] === item;
                });
                %>
                ,{
                  "@type": "ListItem",
                  "position": <%= i %>,
                  "item":
                  {
                    "@id": "/<%= config.tag_dir %><%= tag_url %>/",
                    "name": "タグ: <% if(tag_name.length === 0){ %><%= item %><%}else{%><%= tag_name %><%}%>"
                  }
                }
            <% }) %>
            <% if(page.current !== 1){ %>
                ,{
                  "@type": "ListItem",
                  "position": <%= tag_slug.length + 2 %>,
                  "item":
                  {
                    "@id": "/<%= config.tag_dir %><%= tag_url %>/page/<%= page.current %>/",
                    "name": "ページ<%= page.current %>"
                  }
                }
            <% } %>
        <% } else if (is_archive()) { %>
            ,{
              "@type": "ListItem",
              "position": 2,
              "item":
              {
                "@id": "/<%= config.archive_dir %>/<%= page.year %>/",
                "name": "<%= page.year %>年"
              }
            }
            <% if(page.month){ %>
            ,{
              "@type": "ListItem",
              "position": 3,
              "item":
              {
                "@id": "<%- url_for(path) %>",
                "name": "<%= page.month %>月"
              }
            }
            <% } %>
        <% } else if(is_post()) { %>
            <% page.categories.forEach(function(item, i){ %>
                <%
                i += 2;
                %>
                ,{
                  "@type": "ListItem",
                  "position": <%= i %>,
                  "item":
                  {
                    "@id": "<%= url_for(item.path) %>",
                    "name": "<%= item.name %>"
                  }
                }
            <% }) %>
            ,{
              "@type": "ListItem",
              "position": <%= page.categories.length + 2 %>,
              "item":
              {
                "@id": "<%- url_for(path) %>",
                "name": "<%- page.title %>"
              }
            }
        <% } else if (is_page()) { %>
            ,{
              "@type": "ListItem",
              "position": 2,
              "item":
              {
                "@id": "<%- url_for(path) %>",
                "name": "<%- page.title %>"
              }
            }
        <% } %>
      ]
    }
  ]
}
</script>
```
</del>

## 解説

HTMLの形が変わっているだけで、やっていることはパンくずリストとほぼ同じですので、詳しくは前回の記事をご覧ください。

構造化マークアップではpositionの値を入れる必要があるので少々面倒です。今回はループの回数や配列の長さなどで指定しています。

config.ymlの`category_map` `tag_map`によるスラッグに対応しています。JSONを置く場所は`<head>`間でも`<body>`閉じタグ直前でもどちらでもOKです。




## 新版

あまりにもひどい仕様だったので書き直してみました。

```ejs
<%
// パンくずリスト
    let breadcrumbs = {};

    if(is_post()) {
        page.categories.forEach(function(item){
            breadcrumbs[item.name] = url_for(item.path);
        })
        breadcrumbs[page.title] = url_for(path);
    } else if (is_page()) {
        breadcrumbs[page.title] = url_for(path);
    } else if (is_category()) {
        cat = get_category();
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

<script type="application/ld+json">
{
  "@context": "http://schema.org",
  "@graph": [
    {
      "@type": "BreadcrumbList",
      "itemListElement":
      [
        {
          "@type": "ListItem",
          "position": 1,
          "item":
          {
            "@id": "<%= url_for('/') %>",
            "name": "ホーム"
          }
        }
        <% let i = 1; %>
        <% for(var key in breadcrumbs) { %>
        <% i += 1 %>
        ,{
          "@type": "ListItem",
          "position": <%= i %>,
          "item":
          {
            "@id": "<%= breadcrumbs[key] %>",
            "name": "<%= key %>"
          }
        }
        <% } %>
      ]
    }
  ]
}
</script>
```

### 改善点

以前のものはループが複雑怪奇を極めていたので、まず最初にパンくずリストを配列でつくって、最後にまとめてHTMLに出すようにしました。

旧版ではカテゴリーページの場合に、URLからカテゴリーを取得するというトリッキーなことをやっていたのですが、カテゴリー名から親カテゴリーを取得する関数を書いたので、かなりシンプルになりました。

`_config.yml`のカテゴリーのスラッグ、filename_caseの設定にも対応しています。旧版はスラッグの指定が必須でしたが、新版はスラッグを設定していなくても不具合は出ません。




## 前の記事

[Hexoでプラグインを使わずパンくずリストを表示する](/post/xzr273/)
