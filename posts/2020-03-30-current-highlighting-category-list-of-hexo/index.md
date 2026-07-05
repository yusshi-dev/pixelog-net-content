---
title: 'Hexoのカテゴリーリストにcurrentクラスを付けて現在地をハイライト表示する'
published_at: 2020-03-30 14:57:16
category: '技術メモ'
tags: ['Hexo', 'EJS', 'HTML', 'CSS']
---

Hexoでは[Helper](https://hexo.io/api/helper)という機能が備わっており、よく使う機能は簡単に出力できるようになっています。その機能を利用すると下のように記述することで、簡単にカテゴリー一覧を出力できます。

<!--more-->

```ejs
<%- list_categories( {
    show_count: true
}) %>
```
そこで、カテゴリー別ページや記事ページにおいて、そのページが属するカテゴリーにcurrentクラスをつけ、ハイライト表示できるようにしたいのですが、どうやらヘルパーにその機能は備わっていない模様...

それならば、ヘルパーを使わずに自力でカテゴリー一覧を出力し、現在地表示にも対応してみよう、というのがこの記事での試みです。


<div class="warning">
この記事で紹介する内容は並列複数カテゴリーには対応していません。（階層カテゴリーは可）ご了承ください。
</div>

## currentクラスをつける

カテゴリー一覧を表示したい箇所に以下のように記述します。

``` ejs
<ul class="category-list">
    <%
    let current_category = "";
    if(is_category()) {
        current_category = page.category;
    } else if(is_post() && page.categories.length > 0) {
        current_category = page.categories.data[page.categories.length - 1].name;
    }
    function displayCategories(parent = undefined) {
        site.categories
            .find({ parent })
            .sort("name")
            .each(function(category) {
                const childCount = site.categories.find({ parent: category._id }).count();
                %>
                <li class="category-list-item">
                    <a class="category-list-link <% if(current_category === category.name) { %>current<% } %>" href="<%=url_for(category.path)%>"><%=category.name%></a>
                    <span class="category-list-count"><%= category.length%></span>
                    <% if (childCount > 0) { %>
                        <ul class="category-list-child">
                            <% displayCategories(category._id); %>
                        </ul>
                    <% } %>
                </li>
            <%
            });
    }
    displayCategories();
    %>
</ul>
```

class名やHTML構造は、ヘルパーで出力されるものと揃えてありますので、すでにCSSで装飾している場合でもそのまま置き換え可能です。

## CSSで装飾

```css
.current {
    background: #eaeaea;
    font-weight: 700;
}
```

ページが属するカテゴリーにはcurrentクラスがつくので、CSSで好きなように装飾してください。

## 参考記事

GitHubのIssueに挙がっているものを参考に改造させていただきました。ありがとうございました。

- [how to write the categories? (no use list_categories)](https://github.com/hexojs/hexo/issues/3588)
