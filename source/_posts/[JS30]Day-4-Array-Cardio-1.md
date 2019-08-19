---
title: \[JS30\] Day 4 - Array Cardio 1
date: '2018-04-16 20:53:51'
categories: JavaScript 30
tags: JavaScript
---

<center>捲起你的袖子，我們要來場陣列特訓！</center>

<!-- more -->

{% asset_img cover.png %}

## <center>Day:4 Array Cardio 1 💪</center>


<center>陣列有氧操-上篇</center>

這次不玩 CSS 也不爬 DOM，我們要用完完全全的 JavaScript 來進行資料處理。可以看到檔案中兩個落落長的陣列，再加上註解的提示，我們將會學習到各種陣列方法。這是一個加強陣列技術的好機會，建議大家自己從 MDN 或其他網站上找出題目要求的語句，並且自己實作出來。如果真的想不出解答，再看我的寫法。

---

## <center>Ok, Let'up！</center>

### <center>filter()</center>
<center>過濾出在十六世紀出生的發明家</center>

```js
// filter() Example
 array.filter( function(element, index, array) {
   do something...
   return true or false 
 });
// 加入一個 callback function 並帶上三個參數，然後把每個項目都跑過一次內容
// 參數代表的內容是由順序做決定，因此稱呼可以依照自己的喜好來命名。
// 第一個參數(範例中的 element ..以此類推) 原陣列底下的項目
// 第二個參數代表“正在處理的項目在陣列中順位”，就如同 for 迴圈內我們定義的`i`
// 第三個參數代表原陣列，不常用到
// 陣列方法的寫法都大同小異，之後的只描述比較特別的狀況。

const array = ['apple', 'banana', 'pie', 'ham', 'pizza'];
const result = array.filter(function(food) {
  return food.length > 4
  });
console.log(result) // ["apple", "banana", "pizza"];
```

`filter()`從 callback function 帶入的第一個自訂名稱變數(範例中我取為food)，代表著陣列底下的項目，function 跑過底下所有項目，再回傳所有結果為`true`的項目並組成一個新陣列。我們就可以使用這方法來過濾出生於16世紀的發明家。 

```js
// filter() Ans
const fifteen = inventors.filter((inventor) => {
  inventor.year>=1500 && inventor.year < 1600
});
// arrow function 可以在減化成下面的寫法
// const fifteen = inventors.filter(inventor => inventor.year>=1500 && inventor.year < 1600);
console.table(fifteen);
// console.table 可以用更易讀的方式顯示陣列與物件
```
---

### <center>map()</center>
<center>製作一份這些發明家全名的陣列</center>

`.map(function(element, index, array){...});`

`map()`可以想像成一個加工程序，將陣列中每個項目丟進 map 加工處理後回傳，組成與原本長度相同的新陣列。是我個人在 React 中製作清單列表，非常愛用的方法。
```js
// map() Ans
const fullName = inventors
  .map(inventor => `${inventor.first} ${inventor.last}`);
console.log(fullName);
```
在解答中回傳資料的地方使用了 ES6 的樣板字面值(Template literals)，\`\`裡面的內容會被視為字串，能再透過`${}`將字串中的內容轉為變數。

---

### <center>sort()</center>
<center>從老到年輕，透過生日排序這些發明家</center>

`.sort(function(a, b){...});`

`sort()`就需要花時間弄熟一下了，sort 會一一拿陣列中的值互相比較，再藉由回傳結果改變兩個值在新陣列中的順序。如果不給予任何條件，直接使用`sort()`會根據字串的 Unicode 編碼位置來進行排序。通常會像下面範例的使用方式，給予參數來進行排序。
```js
// sort() Example
const array = [10, 7, 14, 26, 94, 52];
const sort = array.sort((a, b) => {
  if (a < b) {return -1} // 回傳值小於 0，a 排到 b 前方
  if (a > b) {return 1} // 回傳值大於 0，a 排到 b 後方
  return 0 // 回傳值等於 0，不改變順位
});
console.log(sort); // [7, 10, 14, 26, 52, 94]

// 如果單純只是要進行一連串數字的升/降冪排列，也可以用這種更精簡的寫法
array.sort((a, b) => a - b); // 升冪排列
array.sort((a, b) => b - a); // 降冪排列

// sort() Ans
const ordered = inventors.sort((a, b) => b.year - a.year);
console.log(ordered);

// 我們也可用三元運算子(ternary operator)
// X ? A : B  
// X = true, return A, X = false, return B.
const ordered2 = inventors.sort((a, b) => a.year > b.year ? 1 : -1);
console.log(ordered2);
```
---

### <center>reduce()</center>
<center>全部的發明家總共活了多少年？</center>

`.reduce(function(accumlator, element, index, array){...}, initVal)`

一看題目就知道`reduce()`是一個能對陣列進行統計的發法，寫法上也有點不太相同。
function 帶入的第一個參數變成了累加器(accumlator)，顧名思義就是統計當下累計的數值，每跑一次就會把 return 的數傳給累加器。接在 function 之後，我們還可以再帶入一個參數作為累加器的初始值，也就是在還沒跑第一個陣列項目之前，就預先設定好的內容，如果忘記輸入的話，很容易出現錯誤。
```js
// reduce() Example
const array = [1, 2, 3, 4, 5];
const sum = array.reduce((counter, num) => {
  return counter + num;
}, 3);
console.log(sum); // 18

// reduce() Ans
const totalYear = inventors.reduce((counter, inventor) => {
  return counter + (inventor.passed - inventor.year);
}, 0);
console.log(totalYear);
```

---

### <center>延伸題目</center>
#### <center>透過年齡排序發明家</center>
一看就知道是要用`sort()`來處理，只不過要進行一點運算。
```js
const oldest = inventors.sort((a, b) => {
  const aYear = a.passed - a.year;
  const bYear = b.passed - b.year;
  return aYear > bYear ? 1 : -1;
});
console.table(oldest);
```

#### <center>建立一個清單，列出巴黎所有名字帶有「de」的大道</center>
這題過程就會結合比較多 JS 運用了，直接到 [連結網址](https://en.wikipedia.org/wiki/Category:Boulevards_in_Paris) 使用 devTool 來作答吧！ 這題我會一行一行描述。可以先從 HTML 結構中看出每個大道的名稱都是一個`<a>`，一層一層往上，最後則是用一個 Class`.mw-category`包覆著所有名稱。
```js
// 選擇 .mw-category 底下所有的 <a>
const category = document.querySelectorAll('.mw-category a');

// querySelectorAll 出來的是 nodeList 很多陣列方法都不能使用，
// 所以這邊我們要先使用 Array.from() 將其轉為陣列。
const list = Array.from(category);

// 接著就是取出底下的字串並且過濾囉
const deStreet = list
                  .map(item => item.textContent)
                  .filter(name => name.includes('de'));
// includes() 可以判斷是否有帶入的參數內容
console.log(deStreet);
```

#### <center>sort() 鍛鍊</center>
<center>用名字按照字母順序排列 people 陣列</center>

看到排序就知道又要使用`sort()`了，不過我們這次會加入一些新東西，可能會比較難吸收，但這些新的寫法絕對會帶來很多方便。
```js
const alpha = people.sort(function(lastGuy, nextGuy) {
  const [aLast, aFirst] = lastGuy.split(', ');
  // 先注意後面的 split()，能夠檢查字串中是否含有帶入的參數，
  // 若有則將該參數視為分段點，拆成陣列。

  const [bLast, bFirst] = nextGuy.split(', ');
  // 這個奇怪的命名方式是 ES6 的新玩意，解構賦值(Destructuring Assignment)
  // 等號右方經過 splic() 之後成為有兩個項目的陣列，
  // 而這個寫法就是將陣列的內容存為兩個常數，下方會另外做範例。

  return aLast > bLast ? 1 : -1;
});
console.table(alpha);
```


#### <center>reduce() 鍛鍊</center>
<center>加總下方陣列所有東西的數量</center>

```js
const data = ['car', 'car', 'truck', 'truck', 'bike', 'walk', 'car', 'van', 'bike', 'walk', 'car', 'van', 'car', 'truck' ];
```

我們可以用簡單的 if 判斷讓每輛載具出現的次數，再把他累加至一個物件裡。
```js
const transportation = data.reduce((obj, item) => {
  // 物件中沒有這個項目，就建立此項目並設為 0
  if (!obj[item]) {obj[item] = 0};

  // 該項目 +1
  obj[item]++;

  // 將物件傳回至累加器
  return obj
}, {}); // 非常重要，預設累加器是個 obj 才能正常運作

console.log(transportation);
```

最後一題結束了，給自己一點小鼓勵，休息一下吧！

---

## <center>相關連結</center>
- [解構賦值](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- [三元運算子](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- {% post_link JavaScript30-介紹 回去 JavaScript30-介紹 %}
- {% post_link [JS30]Day-5-Flex-Panel-Gallery 繼續 Day5 %}