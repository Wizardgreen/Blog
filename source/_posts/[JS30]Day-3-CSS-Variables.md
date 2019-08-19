---
title: \[JS30\] Day 3 - CSS Variables
date: '2018-04-10 22:50:17'
categories: JavaScript 30
tags: JavaScript
---

> CSS 也能變數管理嗎？

<!-- more -->

{% asset_img cover.png %}

## <center>Day 3: CSS Variables</center>
<center>製作 CSS 變數</center>

## <center>檔案結構</center>
### input type="range"
`range`可以將 input 轉變為橫向拉桿，再藉由`min="" max=""`來設定由右至左兩端的極限值，並且就跟其他 input 一樣使用`value=""`建立預設值。
### input type="color"
`color`是調色盤功能，能夠詳細的讓使用者選定需要的顏色。

---

## <center>原生 CSS 變數</center>
與常見的 CSS 預處理器不同，原生的變數可以再建立完畢後，透過 JS 逕行調整，跟 SASS / Less 用來加速開發過程的變數並不相同。我們可以透過`:root {--variable name: value}`的方式建立，再透過`var(--variable name)`來套用到指定的 CSS 語法中，接下來直接實做出第一段內容，讓大家更熟悉這個模式吧!
```css
/*建立變數*/
:root {
  --base: #ffc660;
  --spacing: 10px;
  --blur: 10px;
}

/*套用變數*/
img {
  padding: var(--spacing);
  background: var(--base);
  filter: blur(var(--blur));
}
.hl {
  color: var(--base);
}
```

---

## <center>JS 調整功能</center>
### Step1 : 建立 function
```js
function handleUpdate() {
  document.documentElement.style.setProperty(`--${this.name}`, this.value);
}
```
一上來就都是比較少用到的語法，分別列在下方一一解釋:
1.`document.documentElement`選定整個 HTML 文件。
2.`style.setProperty`設定 CSS 屬性。
3.`This`是一個 JS 使用頻率很高的功能，在這裡`this`會'指向'使用 function 的 DOM 元素，關於`this`的細節可以看我之前寫的{% post_link JavaScript-This-用途整理 This 用途整理 %}。

所以上述的 code 會取出該元素的 name、value 兩個 attribute 的值，再將這些值套用到 `<HTML>`的 Inline Style，藉此達到調整畫面的效果。

但這樣還不夠，許多 css 數值是需要寫上單位的，所以這邊我們就可以用到 Day 1 使用過的 data-\*，預先再 DOM 元素上建立需要的數值，再透過 JS 取出使用。
```js
function handleUpdate() {
  // 如果沒有 data-sizing，就帶入空字串，便可應用在不需要值的 CSS
  const suffix = this.dataset.sizing || '';
  document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);
}
```
### Step2 : 套上 function...就這樣
```js
const inputs = document.querySelectorAll('.controls input');
inputs.forEach(input => input.addEventListener('change', handleUpdate));
inputs.forEach(input => input.addEventListener('mousemove', handleUpdate));
```
這邊就都是大家比較熟悉的部分了，透過`querySelectorAll`選擇所有 input，再用`forEach()`帶入 function，比較不同的地方是，我們建立了兩個事件監聽器，除了`change`以外，如果在`mousemove`滑鼠移動時也能使用 function 的話，能夠更即時的反應出調整效果。

---

## <center>Full Script</center>
```js
const inputs = document.querySelectorAll('.controls input');
inputs.forEach(input => input.addEventListener('change', handleUpdate));
inputs.forEach(input => input.addEventListener('mousemove', handleUpdate));

function handleUpdate() {
  const suffix = this.dataset.sizing || '';
  document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);
}
```

---

## <center>相關連結</center>
- {% post_link JavaScript-This-用途整理  JavaScript This 用途整理 %}
- [六角學院 - JavaScript 的 this 到底是誰？
](http://idoc.hexschool.com/tobejsfighter/2017-12-12-javascript-this.html)
- {% post_link JavaScript30-介紹 回去 JavaScript30-介紹 %}
- {% post_link [JS30]Day-4-Array-Cardio-1 繼續 Day4 %}