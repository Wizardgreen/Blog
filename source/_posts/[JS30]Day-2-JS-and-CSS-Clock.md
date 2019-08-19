---
title: \[JS30\] Day 2 - JS and CSS Clock
date: '2018-04-03 16:29:43'
categories: JavaScript 30
tags: JavaScript
---
<center>使用 JS 與 CSS 製作時鐘</center>

<!-- more -->

{% asset_img cover.png %}

## <center>Day 2: JS and CSS Clock</center>

## <center>檔案結構</center>
### CSS-Transform
打開檔案之後，我們能看到所有螢幕上的元素都已經清楚的排序在 HTML 的`<body>`，下方的`<style>`也可以看到每一項元素對應的 CSS。

我們可以馬上注意到時、分、秒，三個指針是重疊並且都停在九點鐘方向，先運用比較進階的 CSS 技巧來將調整到12點整的位子，接著再用 JS 來與當下的時間做搭配，而這個 CSS 功能就叫做`Transform`。

```css
.hand {
  transform: rotate(90deg);
}
```
`Transform` 有非常多種的參數可以使用，顧名思義就是要使指定的元素"變形"，而`rotate()`參數是翻轉的意思，要注意這邊的單位是`deg`角度，也就是使`.hand`往X軸正向轉90度，不太熟悉二元座標的人可以嘗試看看把參數改成負值，這樣應該會比較好理解。

大家改完以後看畫面一定滿臉困惑，因為`Transform`本身是以元素的中心點做軸心去變形的，所以我們要再加入下面這行。
```css
.hand {
  transform-origin: 100%;
  transform: rotate(90deg);
}
```
`Tnsform-origin`能夠改變`Transform`效果的軸心，也是X軸的原理，輸入了100%就代表著將元素的最右方做為軸心，於是三個指針就能從九點鐘方向移到 12 點整囉，相信大家都很聰明，這項 CSS 功能就會是時鐘移動的主要關鍵。我們可以再加上前一堂課提過的`Transition` 讓時鐘在位移的時候會有循序漸進的效果。
```css
.hand {
  transform-origin: 100%;
  transform: rotate(90deg);
  transition: all .05s; 
  // .hand 的所有 css 特效在改變的時候會有 0.05秒的漸變效果。
}
```
還有一個叫`transition-timing-function`的功能，可以改變整個漸變過程的速度曲線，不過在講下去可能會離題，我會將相關的資料整理在最後一段讓大家研究，想更厲害的人絕對不能錯過。

---

## <center>配合現在時間</center>
### Step1 : 建立 function
由於大多概念都比較基礎，我這邊只會註解上一堂沒提到的內容，如果看了還不是很懂的話，歡迎來信讓我了解一下自己哪邊表達的不夠好。
```js
<script>
  const secondHand = document.querySelector('.second-hand');
  function setDate() {
    const now = new Date();
    // 建立 Date 物件，其值就是當下的時間。
    
    const seconds = now.getSeconds();
    // Date 物件可以透過 getSeconds() 的方式取出'秒'的值。

    const secondsDegrees = ((seconds / 60) * 360) + 90;
    // 一圈共 60 秒，因此 seconds/60 能獲得秒針在時鐘上的經過的比例
    // 接著乘上一圈 360 度，我們就能知道在指針在對應的位置所需要的數值。
    // 最後別忘了一開始提過 +90 度，讓指針從12點鐘方向開始運作。

    secondHand.style.transform = `rotate(${secondsDegrees}deg)`;
    // 將 CSS 套上計算結果就好囉。
  }
```

### Step2 : 動起來!
```JS
setInterval(setDate, 1000);
```
`setInterval(function, time)`很簡單，就是每`time`的時間就執行一次`function`，但要注意時間是以毫秒做單位，所以上述的寫法結果是每一秒執行一次我們所寫的 function。

接下來使用`getMinutes()`與`getHours()`取出剩下的值，原理都相同，大家試著自己補上其餘內容吧。小提醒一下，秒針分針一圈都是60個單位，但是別忘記時針是12個單位，所以在寫法上要有些微調整，我會將寫完的結果放在下一段，但記得要自己先動腦嘗試過，這樣的學習成果才會扎實的記在腦中。

---

## <center>Full Script</center>
```js
  const secondHand = document.querySelector('.second-hand');
  const minHand = document.querySelector('.min-hand');
  const hourHand = document.querySelector('.hour-hand');

  function setDate() {
    const now = new Date();

    const seconds = now.getSeconds();
    const secondsDegrees = ((seconds / 60) * 360) + 90;
    secondHand.style.transform = `rotate(${secondsDegrees}deg)`;

    const mins = now.getMinutes();
    const minsDegrees = ((mins / 60) * 360) + 90;
    minHand.style.transform = `rotate(${minsDegrees}deg)`;

    const hours = now.getHours();
    const hoursDegrees = ((hours / 12) * 360) + 90;
    hourHand.style.transform = `rotate(${hoursDegrees}deg)`;
  }
  setInterval(setDate, 1000);
```

---

## <center>相關連結</center>
- [MDN Transform](https://developer.mozilla.org/zh-TW/docs/Web/CSS/transform)
- [MDN Transition timing function](https://developer.mozilla.org/zh-TW/docs/Web/CSS/transition-timing-function)
- [MDN JavaScript Data()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Date)
- {% post_link JavaScript30-介紹 回去 JavaScript30-介紹 %}
- {% post_link [JS30]Day-3-CSS-Variables 繼續 Day3 %}