---
title: Hexoのカテゴリーページで親カテゴリーを配列で取得する
date: 2020-07-30 15:41:41
post_id: g0kb8m
categories:
  - 開発
tags:
  - Hexo
  - JavaScript
  - EJS
---

Hexoには`page.category`という変数が用意されていて、カテゴリーページでカテゴリーの名前を出せるのですが、カテゴリーを階層化させているときに、そのカテゴリーが属する親カテゴリーの名前やURLを出す変数がありません。

<!-- more -->

なので、何とかして親カテゴリーを取得してみようという企画です。

## ソース

親カテゴリーが存在する場合は、parentというキーで親のidが入ってるので、それを頼りにフィルターで親カテゴリーを探します。

```javascript
<%
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


const cat = get_category();
console.log(cat);
%>
```

`get_category()`を実行すれば、カテゴリー名とURLのセットが連想配列で返ってきます。

これをこのブログの`https://pixelog.net/category/web/hexo/`で実行すると、下のような配列が得られます。

```plaintext
[ Web: '/category/web/', Hexo: '/category/web/hexo/' ]
```


## 応用

この関数を使えばパンくずリストや構造化マークアップもできます。

- [Hexoでプラグインを使わずパンくずリストを表示する](/post/xzr273/)
- [Hexoでパンくずリストの構造化マークアップをする](/post/surt79/)
