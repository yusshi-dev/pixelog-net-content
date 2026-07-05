---
title: 'JavaScriptでCSSを動的にセットする方法2種 (styleタグ、style属性)'
published_at: 2020-05-07 19:26:43
category: '技術メモ'
tags: ['Web制作', 'JavaScript', 'CSS']
---

CSSを動的にセットしたいときに便利なJavaScriptの関数の雛形です。

<!-- more -->

動的にセットしたいという際には、`<style>`タグの中に書いたCSSをhead間に差し込みたいときと、要素のstyle属性にCSSを書きたいときの2つのケースが想定されるので両方紹介します。


## styleタグ版

```JavaScript
const css = e => {
  const t = document.createElement('style');
  t.textContent = e;
  document.head.appendChild(t);  
}
```
```JavaScript
css(`.post__title{font-size:32px;}`);
```

あらかじめ関数をページのどこかに読み込ませておいて、引数にCSSを書いて実行してやると、`<style></style>`で囲まれたCSSがhead間に挿入されます。


また、バッククォートを使っているので改行して書くことも可能です。

```JavaScript
css(`
.post__title{
  font-size:32px;
}
`);
```


## style属性版

要素にstyle属性を追加して、スタイルを指定するバージョンです。

JavaScriptで追加する前にもともとstyle属性が存在する場合は、上書きされてしまうので注意してください。逆手に取れば、トグルスイッチ（オンオフを切り替える）の場合に便利です。


```javascript
const style =(e,t) =>{
    "loading" !== document.readyState
  ? document.querySelectorAll(e).forEach(e=>e.setAttribute("style",t))
  : document.addEventListener("DOMContentLoaded",() => document.querySelectorAll(e).forEach(e=>e.setAttribute("style",t)))
};
```
```javascript
style('.category-list-link','color:#ff0000;font-size:36px;');
style('a[href="/post/o3tidc/"]','display:none;');
```

セレクタの指定は、classでもidでも属性セレクタでも間接セレクタでも、ネイティブCSSで書けるものはすべてOKです。

DOMContentLoadedは、既にレタリングが終わった後に実行すると発火しないので、三項演算子で分岐させました。

## 関数の使い方の例

たとえばブログの記事の中で、ボタンを押したら特定の部分の文字色を変えたいとき（どういうとき？）を考えます。

<div style="background:var(--bg-color);padding:.5em 2em;">

下の漢字の中には2つだけ違うものが混ざっています。探してみてね！

券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券<span class="red">劵</span>券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券<span class="red">劵</span>券券券券券券券券券


<button class="btn" id="answer">答えを見る</button>

</div>

<script>
const style =(e,t) =>{
    "loading" !== document.readyState
  ? document.querySelectorAll(e).forEach(e=>e.setAttribute("style",t))
  : document.addEventListener("DOMContentLoaded",() => document.querySelectorAll(e).forEach(e=>e.setAttribute("style",t)))
};
const btn = document.getElementById('answer');
btn.addEventListener('click', ()=>{
    style('.red', 'color: #ff0000');
});
</script>


この記事の関数を使えば、このような装置が手早く簡単に作れます。

```html
<p>下の漢字の中には2つだけ違うものが混ざっています。探してみてね！</p>
<p>券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券<span class="red">劵</span>券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券券<span class="red">劵</span>券券券券券券券券券</p>

<button class="btn" id="answer">答えを見る</button>
```


```javascript
<script>
// 関数をコピペする（思考停止でOK）
const style =(e,t) =>{
    "loading" !== document.readyState
  ? document.querySelectorAll(e).forEach(e=>e.setAttribute("style",t))
  : document.addEventListener("DOMContentLoaded",() => document.querySelectorAll(e).forEach(e=>e.setAttribute("style",t)))
};

const btn = document.getElementById('answer'); // ボタン取得

btn.addEventListener('click', ()=> { // 押されたら
    style('.red', 'color: #ff0000'); // CSSセット
});
</script>
```

ちゃんとDOMの解析が終わった後に実行しなきゃとか、対象の要素をどうやって取得するのかとか、要素が複数ならループさせないと...などなどを考える必要がないので楽ですね！


<style>
.btn{
  display:inline-block;
  margin:.5em 0;
  padding:8px 2em;
  background:#007bff;
  color:#fff!important;
  text-decoration:none!important;
  border-radius:4px;
  transition:.15s all ease-in-out
  }
.btn:hover{
  background:#0069d9
}
.btn:active{
  background:#0062cc
}
</style>
