---
title: \[JS30\] Day 1 - JavaScript Drum Kit
date: '2018-03-31 01:39:35'
categories: JavaScript 30
tags: JavaScript
---
<center>透過 JavaScript 製作一個簡易的打擊樂器。</center>

<!-- more -->

{% asset_img cover.png %}

## <center>Day 1: JavaScript Drum Kit</center>

## <center>檔案結構</center>
### data-\*
首先我們觀察 HTML 內的元素，不難發現每個 `.Key` 能搭配一個 `audio` ，藉由相同的`data-key`兩兩成一組。
```html
<div data-key="65" class="key">
  <kbd>A</kbd>
  <span class="sound">clap</span>
</div>
<audio data-key="65" src="sounds/clap.wav"></audio>
```
data-\* 是一種讓開發者自定義的屬性， \* 可以自訂名稱，接著透過 JS 將資料取出運用，藉由這些能自由嵌入的資訊，可以實踐許多功能。更多 data-\* 的詳細資訊可以參考 [MDN 的 data-\* 介紹頁面](https://developer.mozilla.org/zh-TW/docs/Web/HTML/Global_attributes/data-*)。

### KeyCode
每一個鍵盤上的按鍵對電腦來說都對應的一組數字，電腦是透過接收到這組數字才能判斷使用者是按下什麼樣的按鍵。想知道每個按鍵的編碼可以透過，下面的程式碼來查詢 ：
```js
window.addEventListener('keydown', e => console.log(e.keyCode));
// 監聽每一個 keydown 事件，觸發後透過控制台顯示出按下的 keyCode
```
或簡單一點，直接透過 [JavaScript Event Keycode](http://keycode.info) 來查詢，只要按下按鍵，畫面中間就會跳出對應的數字。細心一點就不難發現，上面每一個`data-key="..."`的數字就是對應著同一層`<kbd>`內的英文字

---

## <center>播放功能</center>
一般來說我們會把 JS 另外寫成一個獨立的檔案方便管理，但因為這是教學課程，所以我們就先忽略這個部分，直接將程式寫在 HTML 裡面 ：
```html
<!DOCTYPE html>
<html lang="en">
<head></head>
<body>
.
.
.
<script>...</script>
<!--建議將 Script 寫在 body 最末端-->
</body>
```
由於 HTML 是由上往下載入的，所以當需要把程式寫在 HTML 檔案內的話，建議將 `<script>` 寫在 `<body>` 的最末端，避免 JS 抓不到 HTML 的結構。

### Step1 : 建立事件監聽器
```js
window.addEventListener('keydown', function(e) { 
  console.log(e.keyCode)
});
```
為了確定我們的按鍵能夠正確的觸發，先加入一個`console.log(e.keyCode)`來判斷有無正常運作，順便看看 KeyCode 有無正確。這是一個非常重要的習慣，建議每個開發者每一小段都可以運用 `console.log()` 來進行檢查，避免到時出一堆錯誤卻無從找起。確認無誤的話，接著教學的速度就會變快一點囉。

### Step2 : 寫入播放功能
```js
window.addEventListener('keydown', function(e) { 
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);

  if (!audio) {return} // 如果指定的 audio 不存在，中斷整個 function 。
  audio.play(); // 影片、音樂可以透過 .play() 來撥放。 
});
```
當按下畫面上的按鍵，就會撥方出對應的聲音囉!  不過這樣還不夠完美，如果你連續點擊同樣按鈕，就會發現在一段音效結束之前，不會再撥第二次，這一點都不像打擊樂器阿，騙我。

### Step3 : audio.currentTime
```js
audio.currentTime = 0;
```
我們在`audio.play()`前面加入這段，就可以在每次觸發事件的同時，將音效的時間重置為 0 秒，也就是重頭播放。同樣的原理也可以運用在影片上面喔。

---

## <center>視覺效果</center>

### Step1 : 加入特效
我們從 style.css 裡面可以注意到有一個 .playing 並沒有在 HTML 當中被使用，這個就是老師提供的播放特效。
```js
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`); // 選擇對應 keyCode 的 .key
  if (!audio) {return}; // 如果找不到對應的 audio，則中斷 function
  audio.currentTime = 0;
  audio.play();
  key.classList.add('playing'); // 在撥放後加上 .playing
```
`.classList`是個很常用的功能，除了加入新 Class 也可以用`classList.remove('style')`的方式來移除不要的 Class。

### Step2 : 還原特效
現在我們在按下按鍵之後，已經能看到 `div.key` 會跑出特效了，但他就這樣卡住，不會變回原本的樣子了。
```js
setTimeout( function(){
  key.classList.remove('playing');
}, 1000);
```
當然我們可以透過一個很簡單的`setTimeout()`來在幾秒後移除掉原本加入的 Class，可是這樣有一個問題，如果今天設計師改變了原本動畫的時間怎麼辦? 如果今天撥放的特效變的比你設定的時間還要長，那不就又要在這些數字上做微調了嗎? 煩都煩死啦~
```js
const keys = document.querySelectorAll('.key'); 
// 使用 querySelectorAll 選擇所有的 .key

keys.forEach(key => key.addEventListener('transitionend', removeTransition));
// 事件監聽器只能對單一的 node 產生作用，而 querySelectorAll 的結果會是一個陣列，
// 因此我們要用 forEach() 來監聽陣列內的每個節點。

function removeTransition(e) {
  if (e.propertyName !== 'transform') return; // 詳見下方說明
  this.classList.remove('playing');
};
```
建立一個新的變數，還有新的事件監聽器，這裡我們用到一個比較沒那麼常見的事件`transitionend`，也就是網頁上有 css Transition 結束時，會觸發的事件。
如果我們在 function 裡面使用`console.log(e)`就能發現一次 Transition 結束會有好幾個相關的`transitionend`事件，但我們只需要一個就可以移除掉 Class，因此我們就用到上面提過的 `if (true) {return}`，隨便選一個事件來繼續我們要做的事情，將其他的都中斷。

一般來說`this`會指向`window`，但在這裡`this`會指到觸發這個事件的節點，舉例來說我按下 A ，那麼`this`就會是`<div data-key="65" class="key"></div>`，可以多多運用`console.log(this)`來了解一下在什麼情況會指向哪裡。
(特別要注意一下，如果使用的是 ES6 的 Arrow function ，那麼 `this` 還是會指向 `window`)


### Step3 : 整理
到這裡基本上功能都寫好囉，不過身為一個良好的前端工程師，我們還要把 code 整理的清晰有條理才行，不然不用說你的同事，搞不好自己幾個月後要回來修改程式碼，也要花上好一段時間才能讀懂。
基本上我們移除特效的寫法已經很清楚了，一個常數、一個監聽器、一個 function，非常好懂 !
我們只要再回頭整理一下一開始寫的播放功能就好 ：
```js
window.addEventListener('keydown', playSound); 
// 將監聽器與 function 分離，讓 code 更容易閱讀。

function playSound(e) {
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  if (!audio) {return};
  audio.currentTime = 0;
  audio.play();
  key.classList.add('playing');
}
```
好啦! 到這裡，這次的課程就結束囉，回想一下實作的過程中有沒有比較不了解的地方，反覆練習就能夠精進自己囉。

---

## <center>Full Script</center>

```js
window.addEventListener('keydown', playSound);

function playSound(e) {
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  if (!audio) {return}; // 如果找不到對應的 audio，則中斷 function
  audio.currentTime = 0;
  audio.play();
  key.classList.add('playing');
}

// 播放後移除動畫效果
const keys = document.querySelectorAll('.key');
keys.forEach(key => key.addEventListener('transitionend', removeTransition));

function removeTransition(e) {
  if (e.propertyName !== 'transform') {return};
  this.classList.remove('playing');
};
```

---

## <center>相關連結</center>
- [MDN data-\* 介紹頁面](https://developer.mozilla.org/zh-TW/docs/Web/HTML/Global_attributes/data-*)
- [MDN this 介紹頁面](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/this)
- [JavaScript Event Keycode](http://keycode.info)
- {% post_link JavaScript30-介紹 回去 JavaScript30-介紹 %}
- {% post_link [JS30]Day-2-JS-and-CSS-Clock 繼續 Day2 %}