---
title: HTMLだけでコンテンツを開閉できるdetailsタグを試してみる
date: 2019-05-05 21:00:00
post_id: rn7zja
categories:
  - 開発
tags:
  - Web制作
  - HTML
---

`<details>`タグはHTML5.1で追加されたタグで、コンテンツを開閉できるウィジェットを生成します。

<!--more-->

## 概要

```html
<details>
    <summary>日程</summary>
    2019年4月1日（月）開場：13:00
</details>
```

<details>
    <summary>日程</summary>
    2019年4月1日（月）開場：13:00
</details>

`<details>`の中には1つの`<summary>`タグ、そしてフローコンテンツに分類されるbody内で使われるほとんどの要素を置くことができます。

参考：[フロー・コンテンツについて](https://www.tagindex.com/html5/basic/flow.html)


## 装飾をしてみる

detailsを開くと`open`という属性が付されるので、それを利用してCSSでカスタマイズしてみます。


<details id="designed">
    <summary>装飾をしてみたテスト</summary>
    <div class="details_content">
      CSSでアニメーションを付けてみました。
      <br><br><br><br>
      ...
      <br><br><br><br>      
     何も書くことがない！！
    </div>
</details>


```html
<details id="designed">
    <summary>タイトル（サマリー）</summary>
    <div class="details_content">
      ここに中身
    </div>
</details>
```

```css
summary {
  outline: none;
  cursor: pointer;
}

/*デフォルトの三角を消す */
summary::-webkit-details-marker {
  display:none;
}

/* 疑似要素で三角を付ける */
summary::before {
  display: inline-block;
  content: "▶";
  margin: 0 .3em 0 0;
  transform: rotate(0deg);
  transition: .2s transform ease;
}

details[open] summary::before {
  transform: rotate(90deg);
}

.details_content {
    background: #f5f5f5;
    padding: 16px;
    border-radius: 2px;
}

details[open] .details_content{
  animation: .8s details_content ease;
}

@keyframes details_content {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
```

今のところ、detailsタグ自体にはアニメーションを掛けられないので、中にdivを配置してそこにアニメーションを掛けることになります。

summaryをクリックしてコンテンツを閉じると、高さがすぐ0になってしまうので、閉じるときのアニメーションを付けることはできません。閉じるアニメーションがなくても不自然にならないよう、アニメーションの装飾は最低限に留めたほうがいいかもしれません。



## まとめ
detailsタグはHTMLのみでコンテンツの開閉という動きを、WEBサイトにつけることができるので魅力的ですね。

HTMLでマークアップをする以上意味を考えなければならないので、単にコンテンツを開閉させたいのであれば、JavaScriptで実装する方が手っ取り早いと思います。

<style>
#designed summary {
  outline: none;
  cursor: pointer;
}

#designed summary::-webkit-details-marker {
  display:none;
}

#designed summary::before {
  display: inline-block;
  content: "▶";
  margin: 0 .3em 0 0;
  transform: rotate(0deg);
  transition: .2s transform ease;
}

#designed[open] summary::before {
  transform: rotate(90deg);
}

#designed .details_content {
    background: #f5f5f5;
    padding: 16px;
    border-radius: 2px;
}

#designed[open] .details_content{
  animation: .8s details_content ease;
}
@keyframes details_content {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
