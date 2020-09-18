---
title: "[TypeScript] 型別斷言(annotations)使用時機"
tags: TypeScript
categories: Note
date: "2019-08-30 07:29:00"
---

<center>Type Script 學習筆記 - 型別斷言(annotations)</center>

<!-- more -->

VScode & TypeScript 的型別推斷(Type inference)總是可以讓開發人員省掉許多時間，我們不需要每一行每一個變數都一步一步寫下他們的型別，但總會有些情境是沒辦法讓 TS 自動推斷出結果，有四種情況需要開發者親自撰寫型別斷言：

1. 只宣告變數，但之後才給予初始值

```ts
// 僅宣告變數會自動判斷為 any，所以賦予任何值都不會有警告
let name;
name = "Greene";
name = 1208;

// 同樣的，因為 hasGreen 會是 type any，可能被帶入任何值導致後續運作出錯
const array = ["red", "red", "green", "red"];
let hasGreen;
array.forEach((item: string) => {
  if (item === "green") {
    hasGreen = true;
  }
});
// 所以我們應該要在宣告時就加入型別
let hasGreen: boolean;
// 但以這段 Code 來說，最好還是這樣寫會比較清楚
let hasGreen = false;
```

2. 無法被推斷的型別

```ts
// 這是一段糞 Code，但透過範例就能理解運用的情境
// 而且人在江湖...難免就是會遇到這種事...
const numberList = [-82, -12, 8];
const numberAboveZero = false;

// 假設有個需求，要我們遍歷 numberList，如果有大於 0 的內容，就將其值賦予 numberAboveZero
// 這就是一個型別推斷無法正確推斷出我們希望的結果
numberList.forEach((n: number) => {
  if (n > 0) {
    // Type 'number' is not assignable to type 'boolean'
    numberAboveZero = n
  }
})

// 因此我們需要型別斷言來解決這個問題
const numberAboveZero: boolean | number = false;
```

3. 當某個函式會回傳`any`型別，但我們需要清楚明瞭的結果

```ts
const json = `{
  "x": 12,
  "y": 8,
}`;

// JSON.parse 會依照傳入字串，回傳不同型別的結果，TS 無法預測型別
// coordinates 會因此判為 any，但這明顯違反了使用 TS 的初衷
const coordinates = JSON.parse(json);
coordinates.mightyGreen; // 不會出現任何問題

// 加入型別斷言
const coordinates: { x: number; y: number } = JSON.parse(json);
coordinates.mightyGreen; // TS 跳出錯誤警告
```

4. 函式參數

```ts
// TS 能夠藉由運算邏輯推斷出函式回傳的型別
// 但輸入參數則需要開發者自行斷言
const adder = (a: number, b: number) => a + b;
const subtractor: (a: number, b: number) => = (a, b) => a - b;

// 當然，如果你想避免邏輯寫錯，永遠都可以自行斷言回傳型別
const multiplier = (a: number, b: number): number => a * b;
const divider: (a: number, b: number) => number = (a, b) => a / b;
```
