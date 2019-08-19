---
title: '變數宣告(Variable Declarations)'
date: 2018-08-25 17:30:02
tags: TypeScript
categories: TypeScript-Handbook
---
![](https://i.imgur.com/BRnv8Fv.png)
<!-- more -->

# <center>變數宣告</center>

<br />

## 變數宣告
`let`與`const`是 JavaScript 比較新的變數宣告方式。就跟先前文章提到的一樣，`let`與`var`很相似，但能夠幫助使用者避免一些 JavaScript 中常見的小問題，`const`則是強化版的`let`，能夠避免這個變數被重複賦值。

由於 TypeScript 是 JavaScript 的超集合，所以 TypeScript 自然也支援`let`與`const`，接著我們會詳細說明這兩個新的宣告方式，還有為什麼他們會比`var`來得好。

如果你先前並沒有太在意 JavaScript 的細節，接下來的內容應該可以幫你回想一些記憶。如果你已經很熟悉`var`宣告常有的怪情況，那你可以輕易地略過這節。

## `var`宣告
在傳統的 JavaScript 裡面，總是透過`var`來宣告一個新的變數。
```ts
var a = 10;
```

你一定也很清楚，我們剛宣告了一個名為 `a` 的變數，並賦予 `10` 為值。
你也可以在一個函式中宣告變數：
```ts
function func() {
 var dinner = "hamburger";

 return dinner;
}
```

我們也可以在內部的其他函式中取得同樣的變數：
```ts
function func1() {
  var a = 10;
  return function func2() {
    var b = a + 1;
    return b;
  }
}

var func2 = func1();
func2(); // 11;
```
在上面的範例中，`func2`取得了在`func1`內宣告的變數`a`。在任何情況下調用`func2`時，都會取得`func1`中的變數`a`，甚至`func1`早就已經執行完畢了，`func1`仍ㄓ然能存取或是修改`a`。
```ts
function func1() {
  var a = 10;
  
  a = 20;
  var b = g();
  a = 30;

  return b;
  function g() {
    return a;
  }
}

func1() // 20
```

### 作用域規則(Scoping rules)
對於使用過其他語言的開發者來說，`var`宣告的作用域規則很奇怪。以下方為例子：
```ts
function func(shouldInitialize: boolean) {
  if (shouldInitialize) {
    var x = 10;
  }
  return x;
}

function(true); // "10";
function(false) // "undefined"
```
一些讀者可能要反覆看這段範例，變數`x`是在`if`的區塊(Block)作用域內宣告，但我們卻可以從外部取得它。這是因為`var`的宣告方式，能夠可任何包含它的函式、模組、命名空間以及全域範圍內的任何地方被取得(這邊的細節會在稍後提及)，而不用去管是否包含在區塊裡面。有些人稱此為`var`-作用域或是函式作用域。函式參數也同樣是函式作用域。
