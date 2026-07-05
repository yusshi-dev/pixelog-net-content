---
title: 'Feedly・Inoreaderの購読ページのURLを生成'
published_at: 2020-11-19 15:55:47
category: '技術メモ'
tags: []
---

FeedlyとInoreaderの購読ページのURLを生成するツールです。ページ先ではログインなしでそれぞれの購読者数を確認することもできます。

<!-- more -->

<form id="feed" name="feed">
    <label>
      フィードURL
      <input type="text" name="feed_url">
    </label>
    <button id="generate" type="button">URL生成</button>
    <label>
      Feedly
      <input type="text" name="feedly">
    </label>
    <label>
      Inoreader
      <input type="text" name="inoreader">
    </label>
</form>

<style>

#feed > * {
  display: block;
  margin: 1em 0;
  line-height: 44px;
}
#feed input{
  display: block;
  width: 100%;
  border: 1px solid var(--border-color);
}
#generate{
  padding: 0 2em;
  background: var(--theme-color);
  color: #fff;
  border-radius: .25em;
}
</style>

<script>
(function(){
  const btn = document.getElementById('generate');
  btn.addEventListener('click', function() {
      const forms = document.feed;
      const feed_url = forms.feed_url.value;
      forms.feedly.value = 'https://feedly.com/i/subscription/feed%2F' + encodeURI(feed_url);
      forms.inoreader.value = 'https://www.inoreader.com/feed/' + feed_url;
  });
})();
</script>
