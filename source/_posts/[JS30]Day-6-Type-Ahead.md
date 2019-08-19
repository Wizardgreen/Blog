---
title: \[JS30\] Day 6 - Type Ahead
date: '2018-08-30 09:19:12'
categories: JavaScript 30
tags: JavaScript
---

> 運用 ES6 帶來的的新方法取得 API 資料，在搭配前面學過的 Array Method，讓使用者在輸入文字的同時就能獲得回饋！

<!-- more -->

{% asset_img cover.png %}

## <center>Day 6: Type Ahead</center> #
<center>預先判斷輸入內容</center>

---

## <center>ES6 Fetch</center> #

使用 ajax 進行資料傳輸的辦法有很多種，有些人會用 jQuery 的方法，像我則是常用 Axios 來處理繁雜的過程。但其實 ES6 本身就有一個簡化的`fetch()`可以進行 Request。一打開檔案就能看到 Wes 已經幫我們寫好了一個變數，並賦予它我們所需要的資料 url，那就直接來看如何應用到本次的題目上吧。

```js
const endpoint = 'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json';
const cities = [];

// 如果先用 console.log 看過一次 fetch() ，你會發現其實會回傳一個 Promise 物件，
// 還不熟悉 Promise 的話，可以看下方提供的連結，好，延續先前的話題，
// 在知道 fetch 是 promise 之後，我們就可以用 .then() 來進行後續動作，
// 先用 console.log() 來瞧瞧回傳了什麼樣的資料？

fetch(endpoint).then(res => console.log(res));
```

- [JavaScript ES6 Promise - 前端，沒有極限](https://wcc723.github.io/life/2017/05/25/promise/)
- [Promise物件建立與基本使用](https://eyesofkids.gitbooks.io/javascript-start-es6-promise/content/contents/basic_usage.html)

<br />

{% asset_img response.png %}

<center>回傳結果竟然是叫做 ReadableStream</center>

<br />

`fetch()`所回傳的結果會是一種叫做 ReadableStream 的物件，這個狀態下我們是沒辦法直接讀取的，我們得再透過幾種方法來轉換成我們需要的格式，在這裡就用 ReadableStream 內建的方法將它轉成 JSON。

```js
fetch(endpoint)
  .then(res => res.json())
  // 轉換後的結果，我們得再用一個 .then() 去進行其他操作
  // 就把結果資料塞入 cities 吧。
  .then(data => cities.push(...data);
```

<br />

依照轉換結果我們就能得出每筆資料結構都長得像下面這個樣子：
```js
{
  city: "New York",
  growth_from_2000_to_2013: "4.8%",
  latitude: 40.7127837,
  longitude: -74.0059413,
  population: "8405837",
  rank: "1",
  state: "New York",
}
```
看來`city`與`state`正是我們需要的項目，再來就是建立過濾的函示吧！
```js
// 預先設想這個函式要帶入 1.搜尋關鍵字 2.要搜尋的陣列
function findMatchs(word, dataArray) 
  // 使用第四章提過的 filter() 方法來篩選正確結果
  return dataArray.filter(place => {
    // 這裡我們使用到了一個之前沒提過的新東西 - 正規表達式
    // 正規表達式是一種電腦科學的概念，能夠運用簡易的字元去敘述一道規則
    // 而這個規則可以用來檢查指定的字串是否符合規則敘述。
    // 第一參數為我希望會出現的字串
    // 第二參數稱為 flag，是一種額外設定，
    // 以我們寫的內容舉例:
    // g 代表全局比對，不會在搜尋到之後就停止動作
    // i 代表忽略大小寫
    const regex = new RegExp(word, 'gi');
    return place.city.match(regex) || place.state.match(regex);
  })
}
```

- [正則表達式, 符號對照及範例 - Jex’s Note](http://blog.jex.tw/blog/2013/01/16/regex/)
- [正規表達式](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Regular_Expressions)

<br />

再來我們建立將資料呈現至畫面上的函式：
```js
const search = document.querySelector(".search");           // 輸入匡
const suggestions = document.querySelector(".suggestions"); // 呈現結果

search.addEventListener('change', displayMatchs);
search.addEventListener('keyup', displayMatchs);

function displayMatchs() {
  // 因為題目需求是在 input 內輸入資料時就觸發過濾與畫面呈現，
  // 所以我們能預期這個函式會綁定在 input 上，
  // 因此能再推斷用 this.value 取出輸入的關鍵字。
  const matchArray = findMatchs(this.value, cities);
  // 一樣是第四章出現過的陣列方法，能夠回傳長度相同的陣列
  // 但陣列是沒辦法直接用 innerHTML 轉化成 DOM 的
  // 所以我們在最後面用 join() 整合成一字串。
  const html = matchArray.map(place => {
    return `
      <li>
        <span class="name">${place.city}, ${place.state}</span>
        <span class="population">${place.population}</span>
      </li>
    `
  }).join();
  suggestions.innerHTML = html;
}
```

<br />

好~到這裡，基本的資料過濾與呈現都完成了。但是我們還缺了符合字串的 HighLight 以及人口數字的千分位符號，所以我們還要再修改一下`displayMatch()`：

Hight Light 部分：
```js
function displayMatchs() {
  const matchArray = findMatchs(this.value, cities);
  const html = matchArray.map(place => {
    // 組成字串的同時，再用一次正規表達式，
    // 將我們原本的資料用字串的方法 - replace 替換成預先寫好 CSS 的 DOM 字串
    const regex = new RegExp(this.value, 'gi');
    const city = place.city.replace(regex, `<span class="hl">${this.value}</span>`);
    const state = place.state.replace(regex, `<span class="hl">${this.value}</span>`);
    // 接著一樣帶入字串中
    return `
      <li>
        <span class="name">${city}, ${state}</span>
        <span class="population">${place.population}</span>
      </li>
    `;
  }).join('');
  suggestions.innerHTML = html;
}
```

<br />

千分位部分：
```js
/ 這是正規表達式的令一種寫法，能夠以 / 中間夾帶規則... / 的方式來寫在其中。
function numberWithCommas(x) {
  // 雖然正規表達式寫起來很簡潔，但平時要記住這些規則可得花上一點功夫
  // 好在網路上有很多人提供常用到的過濾方式，
  // 像是判斷是否為 email ，或是這次的千分位，都很容易 google 到。
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}

 // 最後再把他帶入到 displayMatch() 中...
 // ...in displayMatch()
  return `
    <li>
      <span class="name">${city}, ${state}</span>
      <span class="population">${numberWithCommas(place.population)}</span>
    </li>`
```
到這裡就大功告成囉！正規表達式不太容易上手，但習慣後可以大幅減少程式碼的量，而且在其他語言也用得到。雖然不算是第一重要，但如果有餘力的話，學起來也是非常實用的。

<br />

{% asset_img result.gif %}

<br />

最後分享一下我以前自己寫的千分位函式：
```js
onst formatWithCommas = Num => (
  Num
    .toString()
    .splite('')
    .reverse()
    .map((str, index, arr) => {
      if (!((index + 1) % 3) && index !== arr.length - 1) { str = `,${str}`; }
      return str;
    })
    .reverse()
    .join('');
)
```
哈哈，有沒有覺得正規表達式很簡潔呢？

呼．．．終於又生出一篇了，這樣就不會被 [Engine](https://medium.com/@linengine) 說我都斷尾了 (擦汗

---
<br />
## <center>相關連結</center>
- [JavaScript ES6 Promise - 前端，沒有極限](https://wcc723.github.io/life/2017/05/25/promise/)
- [Promise物件建立與基本使用](https://eyesofkids.gitbooks.io/javascript-start-es6-promise/content/contents/basic_usage.html)
- [正則表達式, 符號對照及範例 - Jex’s Note](http://blog.jex.tw/blog/2013/01/16/regex/)
- [正規表達式](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Regular_Expressions)
- {% post_link JavaScript30-介紹 回去 JavaScript30-介紹 %}