---
title: CSSアニメーションだけでタイマーを作ってみた
date: 2019-03-14 17:00:47
post_id: bon9ry
categories:
  - 開発
tags:
  - Web制作
  - HTML
  - CSS
---

CSSの限界に挑戦ということで、CSS3のアニメーションとCSS変数（カスタムプロパティ）を駆使して、キッチンタイマーを作ってみました。

<!--more-->

## デモ

<p class="alert">
デベロッパーツールで.timer_displayに掛かっている overflow: hiddenを解除するとアニメーションの仕組みがよく分かります。(その際に、タイマーが2つあるので、片方は要素ごと削除してください)
</p>

### カウントダウンタイマー（2分計）

<div class="timer" style="--timer_min: 02;--timer_sec:00;--timer_direction:reverse;">
	<div class="timer_button"></div>
	<div class="timer_display">
		<div></div>
	</div>
</div>

### カウントアップタイマー

<div class="timer" style="--timer_min: 59;--timer_sec:60;--timer_direction:forward;">
	<div class="timer_button"></div>
	<div class="timer_display">
		<div></div>
	</div>
</div>

### 仕様
* カウントダウンとカウントアップを設定できます。カウントダウンタイマーは、計測する時間を自由に設定できます。
* CSS変数でカウントの方向、計測時間を指定できます。
* 真ん中の白いボタンにカーソルを当てている間は、タイマーが一時停止します。
* カウントダウンタイマーは終了したら数字が点滅します。

## 解説

### HTML

```html
<div class="timer" style="--timer_min: 0;--timer_sec: 60;--timer_direction:reverse;">
    <div class="timer_button"></div>
    <div class="timer_display">
        <div></div>
    </div>
</div>
```


カウントの方向、計測時間を管理している変数をHTML側で指定しています。変数にとれる値は次のとおりです。


|  変数               | 値    |
|------------------ |------|
|--timer_min（分）   |  0~60  |
|--timer_sec（秒）   |  0~59 |
|--timer_direction   | forward（カウントアップ）, reverse（カウントダウン）|

カウントアップのときは、`--timer_min`に60、`--timer_sec` に00を指定してください。

### CSS
当ページのデモでは、数字のフォントに別途Google Fontsの[Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono "Share Tech Mono")を読み込んでいます。

```css
/* 時計本体の装飾 */
.timer {
    display: flex;
    flex-direction: column;
    width: 240px;
    height: 280px;
    margin: 20px;
    padding: 30px;
    background: #eb6ea0;
    border-radius: 20px;
    box-shadow: 2px 2px 1px 0px #ebbcce;
    box-sizing: border-box;
}

/* 液晶部分の装飾 */
.timer_display {
    display: flex;
    justify-content: center;
    width: 100%;
    height: 100px;
    line-height: 100px;
    overflow: hidden;
    background: #949494;
    font-size: 60px;
    border-radius: 2px;
    box-shadow: 0px 0px 4px #666 inset;
    font-family: 'Share Tech Mono', monospace;
    text-align: center;
}

/* ボタンの装飾 */
.timer_button {
    content: "";
    display: block;
    order: 2;
    width: 40px;
    height: 40px;
    margin: 24px auto;
    background: #fff;
    border-radius: 50%;
    box-shadow: -70px 0px #fff, 70px 0px #fff;
}

/* キーフレームの設定 */
@keyframes time {
  0% {transform: translate(0,0px);}
  100%{transform: translate(0,-6000px);}
}

@keyframes flash {
  0% {opacity: 1;}
  100% {opacity: 0;}
}

/* 数字を準備する */
.timer_display::before,
.timer_display::after {
    display: block;
    content:'00\a 01\a 02\a 03\a 04\a 05\a 06\a 07\a 08\a 09\a 10\a 11\a 12\a 13\a 14\a 15\a 16\a 17\a 18\a 19\a 20\a 21\a 22\a 23\a 24\a 25\a 26\a 27\a 28\a 29\a 30\a 31\a 32\a 33\a 34\a 35\a 36\a 37\a 38\a 39\a 40\a 41\a 42\a 43\a 44\a 45\a 46\a 47\a 48\a 49\a 50\a 51\a 52\a 53\a 54\a 55\a 56\a 57\a 58\a 59';
    white-space: pre;
}

/* 分の計算・アニメーション */
.timer_display::before {
    animation: time 3600s steps(60, end), flash 1s steps(2, start);
    animation-direction: var(--timer_direction);
    animation-delay: calc(-1s * (3600 - 60*var(--timer_min) - var(--timer_sec))), calc(1s * (60*var(--timer_min) + var(--timer_sec)));
    animation-iteration-count: 1, infinite;
}

/* 秒の計算・アニメーション */
.timer_display::after {
    animation: time 60s steps(60, end), flash 1s steps(2, start);
    animation-direction: var(--timer_direction);
    animation-delay: calc(-1s* (60 - var(--timer_sec))), calc(1s * (60*var(--timer_min) + var(--timer_sec)));
    animation-iteration-count: calc(var(--timer_min) + 1), infinite;
}

/* コンマのアニメーション */
.timer_display  div::before {
    content: ":";
}

.timer_display  div {
    animation: flash 1s steps(2, start) infinite;
}

/* ボタンにhoverしたときにタイマーを止める */
.timer_button:hover + .timer_display::before,
.timer_button:hover + .timer_display::after {
    animation-play-state: paused;
}
```

* `.timer_display::before`と`.timer_display::after`の中に、分と秒で使う0から60の数字を改行込みで入れています。
* CSSのanimationを用いて分と秒をバラバラに動かしています。
* `animation-delay`は負の値を設定することで、再生をはじめるキーフレームの位置を指定できます。  （参考：<https://teratail.com/questions/9518>）今回は入力された変数を元にどこから再生を始めるかをcalcで導出しました。
* `animation-iteration-count`ではアニメーションの回数を指定します。分は60分で1サイクルなので1回、秒は分を元に回数を計算しています。


<style>
@font-face {
  font-family: 'Share Tech Mono';
  font-style: normal;
  font-weight: 400;
  src: local('Share Tech Mono'), local('ShareTechMono-Regular'), url(https://fonts.gstatic.com/s/sharetechmono/v8/J7aHnp1uDWRBEqV98dVQztYldFcLowEF.woff2) format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
.timer {
    display: flex;
    flex-direction: column;
    width: 240px;
    height: 280px;
    margin: 20px;
    padding: 30px;
    background: #eb6ea0;
    border-radius: 20px;
    box-shadow: 2px 2px 1px 0px #ebbcce;
    box-sizing: border-box;
}

.timer_display {
    display: flex;
    justify-content: center;
    width: 100%;
    height: 100px;
    line-height: 100px;
    overflow: hidden;
    background: #949494;
    font-size: 60px;
    border-radius: 2px;
    box-shadow: 0px 0px 4px #666 inset;
    font-family: 'Share Tech Mono', monospace;
    text-align: center;
}

.timer_button {
    content: "";
    display: block;
    order: 2;
    width: 40px;
    height: 40px;
    margin: 24px auto;
    background: #fff;
    border-radius: 50%;
    box-shadow: -70px 0px #fff, 70px 0px #fff;
}

@keyframes time {
  0% {transform: translate(0,0px);}
  100%{transform: translate(0,-6000px);}
}

@keyframes flash {
  0% {opacity: 1;}
  100% {opacity: 0;}
}

.timer_display::before,
.timer_display::after {
    display: block;
    content:'00\a 01\a 02\a 03\a 04\a 05\a 06\a 07\a 08\a 09\a 10\a 11\a 12\a 13\a 14\a 15\a 16\a 17\a 18\a 19\a 20\a 21\a 22\a 23\a 24\a 25\a 26\a 27\a 28\a 29\a 30\a 31\a 32\a 33\a 34\a 35\a 36\a 37\a 38\a 39\a 40\a 41\a 42\a 43\a 44\a 45\a 46\a 47\a 48\a 49\a 50\a 51\a 52\a 53\a 54\a 55\a 56\a 57\a 58\a 59';
    white-space: pre;
}

.timer_display::before {
    animation: time 3600s steps(60, end), flash 1s steps(2, start);
    animation-direction: var(--timer_direction);
    animation-delay: calc(-1s * (3600 - 60*var(--timer_min) - var(--timer_sec))), calc(1s * (60*var(--timer_min) + var(--timer_sec)));
    animation-iteration-count: 1, infinite;
}

.timer_display::after {
    animation: time 60s steps(60, end), flash 1s steps(2, start);
    animation-direction: var(--timer_direction);
    animation-delay: calc(-1s* (60 - var(--timer_sec))), calc(1s * (60*var(--timer_min) + var(--timer_sec)));
    animation-iteration-count: calc(var(--timer_min) + 1), infinite;
}

.timer_display  div::before {
    content: ":";
}

.timer_display  div {
    animation: flash 1s steps(2, start) infinite;
}

.timer_button:hover + .timer_display::before,
.timer_button:hover + .timer_display::after {
    animation-play-state: paused;
}
</style>
