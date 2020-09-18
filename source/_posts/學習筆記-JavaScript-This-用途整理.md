---
title: 學習筆記 JavaScript This 用途整理
date: 2018-04-07 21:54:28
tags: JavaScript
categories: Note
---
## <center>JavaScript This 用途整理</center>
後天就是我掛上工程師頭銜的第一個上班日，回想起半年前剛開始自學的時候，光是強者學弟寫給我的 React example 就得花上好幾個小時才能讀懂，印象最深刻的就是`this`，強者學弟都懶得解釋，直喊著「乾哥你還是上網查好了，很難講清楚...」。
&nbsp;
&nbsp;
<center>`this`並沒有一個明確的答案，而是要依照當下的狀況，**指向**出不同的結果。</center>
<center>雖然聽起來很多變數，但只要掌握好幾項規則，大家都一定能駕輕就熟!</center>

## <center>指向調用函式的物件</center>
```js
// Example 1
const obj = {
  passwd: 528491,
  func: function() {console.log (this.passwd)}
};
obj.func(); // 528491


// Example 2
const obj = {
  passwd: 2018,
  b: {
    passwd: 528491,
    func: function() {console.log (this.passwd)}
  }
}
obj.func(); // 528491


// Example 3
function func() {console.log (this.passwd)};
const obj = {
  passwd: 528491,
  b: func
};
func(); // undefine
obj.b(); // 528491
```
從上面的範例中我們可以得知，當透過物件調用`this`時，`this`就會指向該物件。我們可以看出一個**關鍵規則**：當我們以`obj.func()`的方式調用 function，`this`就會指向`obj`。從多層結構來看，我們可以得出更明顯的結論，例如`obj1.obj2.func()`，`this`則會指向`obj2`。也就是說當以`物件.方法()`的形式調用時，`this`會指向同一層、存放這個方法的物件。


## <center>指向全域物件</center>
```js
// Example 1
function func() {console.log(this)}
func() // window


// Example 2
const passwd = 528491;
const obj = {
  passwd: 1234;
  func: function() {console.log(this.passwd)}
}
const func2 = obj.func();

obj.func() // 1234
func2 // 528491
```
如果你看到這邊，應該就能明顯看出運作規則。不透過物件，直接調用 function 的時候，我們會直接寫`方法()`，這寫法也同等於`window.方法()`。看出模式了嗎? 就跟前段的規則一樣，`this`會指向同一層、存放方法的物件，在這裡就是全域物件`window`(在 Node.js 裡則是`Global`)。

## <center>指向 DOM 元素</center>
```html
<!-- Example HTML part --->
<button id="btn">press me</button>
```
```js
// Example JS part
const btn = document.getElementById('btn');

function showThis() {
  console.log(this);
}

btn.addEventListener('click', showThis);
// this = <button id="btn">press me</button>
```
當 DOM 元素調用帶有`this`的 function 時，`this`就會指向該 DOM 元素。

## <center>其他</center>