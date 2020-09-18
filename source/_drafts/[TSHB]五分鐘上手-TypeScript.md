---
title: "五分鐘上手 TypeScript"
tags: TypeScript
categories: TypeScript-Handbook
date: 2018-08-01 17:24:44
---

![](https://i.imgur.com/BRnv8Fv.png)

<!-- more -->

# <center>五分鐘上手 TypeScript</center>

> <center>來用 TypeScript 建立一個簡單的網頁應用吧</center>

<br>

## 安裝 TypeScript

有兩個主要的方法可以安裝 TypeScript 的工具：

- 透過 NPM (Node.js package manager)
- 安裝 TypeScript 的 Visual Studio 相關插件

Visual Studio 2017 與 Visual Studio 2015 Update 3 預設就包含了 TypeScript。如果你的 Visual Studio 還有沒有 TypeScript，你也能從[這邊](https://www.typescriptlang.org/#download-links)下載。

NMP 使用者：
{% asset_img install.png %}

<br>

## 建立你的第一個 TypeScript 檔案

在你的編輯器中，建立一個`greeter.ts`，並輸入下列的 JavaScript 程式碼：

```js
function sayHi(preson) {
  return "Hello, " + preson;
}
let friend = "Greene";

document.body.innerHTML = sayHi(friend);
```

<br>

## 編譯你的程式碼

雖然我們使用`.ts`副檔名，但這段程式碼仍然是 JavaScript，你甚至能直接複製到任何現行的 JavaScript 應用。

在命令列執行 TypeScript 編譯器：
{% asset_img tsc.png %}
最後會產出一個`greeter.js`，內含了你所輸入的 JavaScript。這就是我們要如何使用 TypeScript 到 JavaScript 的應用上面！

現在我們要來聊聊 TypeScript 能夠帶來什麼樣的優點。加入一段型別註解(Type Annotation)`: string`到函式的'person'參數旁，就像下面這樣：

```typescript
function sayHi(preson: string) {
  return "Hello, " + preson;
}

let friend = "Greene";
document.body.innerHTML = sayHi(friend);
```

<br>

## 型別註解(Type Annotation)

型別註解在 TypeScript 中可以輕易的紀錄函式或變數預期會有什麼樣的內容，在這個範例中，我們期望 sayHi 函式能夠傳入一個字串參數。我們來改一下，改成讓呼叫 sayHi 時傳入一個陣列：

```typescript
function sayHi(preson: string) {
  return "Hello, " + preson;
}
let friend = [1, 2, 3];

document.body.innerHTML = sayHi(friend);
```

接著，重新編譯一次，你就會看到錯誤訊息：
{% asset_img tserror.png %}

同樣的，嘗試移除掉所有 sayHi() 帶入的參數，TypeScript 會告訴你的函示帶著數量不如預期的參數。在這兩個情況中可以看出，TypeScript 能夠靜態分析你的程式碼結構以及你寫上的型別註解。

注意一下，雖然掏出了錯誤訊息，但仍然會產出`greeter.js`檔案。所以就算程式碼出錯了，你仍然可以執行 TypeScript，可是 TypeScript 會警告你這段程式碼或許會不如預期的方式運作。

<br>

## 接口(Interface)

來寫點東西吧，我們可以使用一個接口來描述物件帶有`firstName`與`lastName`兩個屬性。在 TypeScript 中，如果兩個型別的內部結構是能合併的，那這兩個型別就能合併。我們只要按照接口的描述就可以實作期望的內容，並且不需要用到`implements`語句。

```typescript
interface Guy {
  firstName: string;
  lastName: string;
}
function sayHi(person: Guy) {
  return "hello, " + friend.firstName + " " + friend.lastName;
}
let friend = {
  firstName: "Gan",
  lastName: "Lee",
};

document.body.innerHTML = sayHi(friend);
```

<br>

## 類別(Classes)

最後，我們延續先前的範例來練習”類別“，TypeScript 支援各種 JavaScript 的新潮功能，像是撰寫以類為基礎的物件導向設計。

我們先創造一個`Student`類別，其中帶著一個建構子/建構函示(Constructor)與幾個公開欄位(Pubilc Field)。可以注意到類別與接口能夠正常共存，**開發者可以自行定義抽象的級別**(註:這段譯者我也不懂)

還能注意到，在建構子帶入的參數前面都有著`public`語句，這個縮寫能夠自動創造出對應名稱的屬性。

```typescript
class Student {
  fullName: string;
  constructor(
    public firstName: string,
    public middleInitial: string,
    public lastName: string
  ) {
    this.fullName = firstName + " " + middleInitial + " " + lastName;
  }
}
interface Kid {
  firstName: string;
  lastName: string;
}
function sayHi(person: Kid) {
  return "Hi, " + person.firstName + " " + person.lastName;
}
const friend = new Student("Qian", "fatnerd", "Lee");

document.body.innerHTML = sayHi(friend);
```

在執行一次`tsc greeter.ts`，你就能看到像之前一樣產出的 JS 程式碼。
類別在 TypeScript 中就是 JS 中基於原形的物件導向(prototype-based OO)的縮寫。

<br>

## 執行你的 TypeScript 網頁應用

將以下內容輸入至`greeter.html`：

```html
<!DOCTYPE html>
  <head>
    <title>Document</title>
  </head>
  <body>
    <script src="greeter.js"></script>
  </body>
</html>
```

在瀏覽器中打開`greeter.html`，執行你的第一個 TypeScript 應用吧！

額外選項： 從 Visual Studio 打開你的`greeter.html`，或者把程式碼複製到 [TypeScript Playground](https://www.typescriptlang.org/play/index.html)。你可以將滑鼠移到程式碼上面查看他們的型別，在某些情況下，這些型別能夠被自動推斷出來讓你檢視。試著重寫最後一行，會有自動補齊清單與參數依照 DOM 元素呈現。把滑鼠放到`sayHi()`上面，按下 fn + F12 (Mac)，可以自動移到定義函示的位置。還能按滑鼠右鍵，使用重構功能同步更改名稱。

這些型別資訊能夠與工具，在開發大型 JavaScript 應用的時候，能發揮很好的效果。更多相關的示範可以直接到[官網](https://www.typescriptlang.org/index.html)了解。

<br/>

## 連結

- {% post_link '[TSHB]TypeScript Handbook' 返回目錄 %}
- [TypeScript Playground](https://www.typescriptlang.org/play/index.html)
