---
title: コンマのつく大きい数字を読む練習ツール
date: 2020-01-31 21:36:01
post_id: number-reading
categories:
  - 雑記
tags:
---

ブラウザ上でケタの大きい数字を読む練習ができるツールです。10万から100億までの数字をランダムで表示します。

<!--more-->

<div class="number-display">
    <div id="input__number"></div>
    <div id="input__japanese"></div>
</div>

<div class="controller">
    <button id="generate">答えをみる</button>
</div>

<style>
.number-display {
    display: flex;
    justify-content: space-between;
}

#input__number,
#input__japanese {
    width: 48%;
    height: 60px;
    line-height: 36px;
    margin: 0 0 24px 0;
    padding: 12px;
    font-size: 18px;
    font-weight: bold;
    border: 1px solid #ddd;
}

@media only screen and (max-width: 470px) {
    .number-display {
        flex-wrap: wrap;
    }
    #input__number,
    #input__japanese {
        width: 100%;
    }
}


#input__japanese {
    color: #F50057;
}

.controller {
    margin: 12px 0;
}

#generate {
    width: 100%;
    padding: 16px;
    font-size: 16px;
    font-weight: bold;
    background: var(--theme-color);
    color: #fff;
}
</style>
<script>
const generate_btn = document.getElementById('generate');
const answer_btn = document.getElementById('answer');

const input_number = document.getElementById('input__number');
const input_japanese = document.getElementById('input__japanese')

var min = 100000
var max = 10000000000


function getRandom(min, max) {
    const r = Math.random()
    var number = Math.floor(  r * r * (max + 1 - min) );

    input_number.innerHTML = number.toLocaleString();
    return number;
}

function num2ja(num, opt) {
    var sign = {
        '+': '',
        '-': '−'
    };
    var zero = '零';
    var point = '点';
    var zero2nine = ['〇', '一', '二', '三', '四', '五', '六', '七', '八', '九'];
    var ten2thou = ['', '十', '百', '千'];
    var suffices = ['', '万', '億', '兆', '京', '垓', '禾予', '穣', '溝', '澗', '正', '載,', '極', '恒河沙', '阿僧祇', '那由他', '不可思議', '無量大数'];

    num = num.replace(/,/g, '');
    num.match(/([+-])?(\d+)(?:\.(\d+))?/i);
    var sig = RegExp.$1;
    var int = RegExp.$2;
    var fract = RegExp.$3;
    var seisuu = '';
    var shousuu = '';
    var shins = new Array();
    var counter = 0;

    for (var i = int.length; i > 0; i -= 4) {
        shins.push(int.substring(i, i - 4));
    }
    if (shins.length >= 18) {
        return suffices[17];
    }

    var suffix = 0;
    for (var i = 0; i < shins.length; i++) {
        var shin = shins[i];
        if (shin == '0000') {
            suffix++;
            continue;
        }
        var sens = '';
        var keta = 0;
        var digits = shin.split('').reverse();
        for (var j = 0; j < digits.length; j++) {
            var digit = digits[j];

            if (opt['fixed4'] || opt['with_arabic']) {
                if (opt['with_arabic']) {
                    var flg = 0;
                    // 余分な 0 を削除する
                    if (digit == '0') {
                        for (var k = j + 1; k < digits.length; k++) {
                            flg += (digits[k] == '0') ? 0 : 1;
                        }
                        if (flg == 0) digit = '';
                    }
                    sens = digit + sens;
                } else {
                    sens = zero2nine[digit] + sens;
                }
            } else {
                var suuji = (digit == 1 && !opt['p_one'] && keta > 0) ? '' : zero2nine[digit];
                if (digit != 0) sens = suuji + ten2thou[keta] + sens
            }
            keta++;
        }
        seisuu = sens + suffices[suffix++] + seisuu;
    }
    var result = (sign[sig] || '') + seisuu;
    result = result || zero;
    if (fract) {
        result = result + point + fract;
    }
    return result;
}

number = getRandom(min, max);
generate_btn.addEventListener('click', function(){

    if(input_japanese.childNodes.length === 0) {
        var japanese_number = num2ja(String(number), {'with_arabic':1 });
        input_japanese.innerHTML = japanese_number;
        generate_btn.innerHTML = '次の問題へ';
    } else {
        input_japanese.innerHTML = '';
        number = getRandom(min, max);
        generate_btn.innerHTML = '答えをみる';
    }
});
</script>
