---
title: '基本型別(Basic Types)'
date: 2018-08-06 12:50:30
tags: TypeScript
categories: TypeScript-Handbook
---
![](https://i.imgur.com/BRnv8Fv.png)

<!-- more -->

# <center>基本型別(Basic Types)</center>

> <center>為了使程式運作，我們需要能夠處理一些最小單位的資料，也就是 數值(number)、字串(string)、布林值（boolean）、結構體(structures)，等等...。TypeScript 幾乎支援所有 JavaScript 中的型別，而且還帶來了好用的列舉(enumeration) 以處理某些狀況。</center>

<br/>

## <center>布林(Boolean)</center>

最基礎的資料型別就是簡單的 true/false , 也就是 JavaScript 與 TypeScript 通稱的`boolean` 。
```typescript
let completed: boolean = false;
```

<br/>

## <center>數值(Number)</center>

跟 JavaScript 一樣，所有 TypeScript 中的數值都是浮點數。這些浮點數的型別則是`number`。除了有十進制與十六進制，TypeScript 也有 ECMAScript 2015 引入的八進制與二進制。
```typescript
let decimal: number = 6;     // 十進制
let hex: number = 0xf00d;    // 十六進制
let binary: number = 0b1010; // 二進制
let octal: number = 0o744;   // 八進制
```

<br/>

## <center>字串(String)</center>

JavaScript 跟其他語言一樣，我們使用`string`型別來呈現文字資料。TypeScript 也同樣使用雙引號 \("\) 或單引號 \('\) 來包裹字串資料。
```typescript
let color: string = "blue";
color = "red";
```

你可以也能使用樣板字串(template strings)，用來包裹多行的字串，並同時夾帶表達示在其中。這樣的方法得使用反引號 \(`\) 來包裹字串，並再使用 ${ 表達示 } 來將表達示放進字串內。
```typescript
let name: string = `Greene`;
let age: number = 25;
let sentence: string = `嗨，我的名字叫做 ${name},

我再過四天就${age}歲了。
`
```

這樣的寫法同等於用下面這種方式宣告`sentence`：
```typescript
let sentence: string = "嗨，我的名字叫做" + ${name} + ",\n\n" +
  "我再過四天就" + age + "歲了。";
```

<br/>

## <center>陣列(Array)</center>

TypeScript 同樣也可以處理陣列中的資料，陣列型別可以透過兩種方法寫出來。第一種，在型別名稱後面加上`[]`，這樣就可以表示一個帶有該資料型別的陣列。
```typescript
let list: number[] = [1, 2, 3];
```

第二種方法，使用泛型陣列(generic array type)，`Array<elemType>`:
```typescript
let list: Array<number> = [1, 2, 3];
```

<br/>

## <center>元組(Tuple)</center>

元組型別用來表示一個已知、且帶有不同資料型別的陣列。舉例來說，你可以表達資料帶有一個字串與一個數值。
```typescript
// 宣告一個元祖型別
let x: [string, number];

x = ["早安", 1208]  // OK
x = [1208, "早安"]  // 不 OK 
```

當訪問一個元素，但卻不知道他的 index，將現顯示出他的正確型別：
```typescript
console.log(x[0].substr(1)); // OK
console.log(x[1].substr(1)); // 錯誤，'number' 並沒有 'substr' 方法
```

從外部訪問時，會用合併型別(union type)來代稱：
```typescript
x[3] = "world";               // OK, 'string' 可被指派至 'string | number'

console.log(x[5].toString()); // OK, 'string' and 'number' 都有 'toString' 方法

x[6] = true;                  // 錯誤，'boolean' 並不是 'string | number'
```

合併型別(union type)在比較後面的進階主題會提到更多。

<br/>

## <center>列舉(Enum)</center>

`enum`是給 JavaSCript 新增的基本資料類型，非常實用。就像 C# , enum 能夠為一組數值資料賦予易懂的名稱。
```typescript
enum Dinner {Meat, Rice, Egg}
let c: Dinner = Dinner.Meat;
```

預設的列舉會為資料從 0 開始賦予編號，你可以直接改變列舉內的編號。舉例來說，我們可以改從 1 開始: 
```typescript
enum Dinner {Meat = 1, Rice, Egg}
let c: Dinner = Dinner.Meat;
```

或者直接改變所有列舉中的資料：
```typescript
enum Dinner {Meat = 1, Rice = 3, Egg = 5}
let c: Dinner = Dinner.Meat;
```

列舉還有一個方便的功能，你可以直接使用數值來許得列舉中的資料。比如說我們有一個`2`的值，但不確定他對應到列舉中的哪一個內容，我們就可以直接用`2`來尋找匹配的資料：
```typescript
enum Dinner {Meat = 1, Rice, Egg}
let dinnerName: string = Dinner[2];

console.log(dinnerName); // 顯示 2 所批配的 'Rice'
```

<br/>

## <center>任意(Any)</center>

當我們在設計程式的時候，些變數的型別可能是我們沒辦法預期的。這些變數可能來自一些動態的內容，例如第三方函式庫。在這種情況下，我們可以使這些變數在編譯時省略不檢查，也就使是用任意型別。
```ts
let notSure: any = 4;
notSure = '也可能是一個字串吧';
notSure = false; // 好吧，布林值。
```

任意型別在改寫現行的 JavaScript 專案有極大的幫助，你能夠逐步的將程式碼加入到型別檢查。就跟其他語言的運作方式一樣，你可能會期待`Object`型別能帶來差不多的效果。但其實`Object`型別只允許你賦予任意的值給他，但你無法隨意調用其中的方法，就算這些方法真的存在。
```ts
let notSure: any = 4;
notSure.ifItExists(); // 好吧，ifItExists() 在執行時或許會存在.
notSure.toFixed();    // 好吧，toFixed() 確實存在 (但編譯過程不會去檢查)

let prettySure: object = 4;
prettySure.toFixed()  // 錯，toFixed 屬性不存在於 'object' 型別
```

如果你並不清楚所有可能的型別，那麼任意行別在這時候就很方便。比如說你有一個陣列，但陣列其中包有多種不同的變數：
```ts
let list: any[] = [1, true, '吃好飽'];

list[1] = 100;
```
　
<br/>

## <center>空(Void)</center>

空型別有點像是任意型別的相反，代表不會有任何型別。通常可以用於表示 function 不回傳任何資料：
```ts
function warnUser(): void {
  console.log('這是一個警告訊息');
}
```

宣告變數為空型別沒什麼用，因為你只能指派`undefined`跟`null`給他：
```ts
let unsueable: void = nudefined;
```

<br/>

## <center>空值與未定義(Null and Undefined)</center>

在 TypeScript 中，`undefined`與`null`，都有著自己的型別，分別就叫做`undefined`與`null`。跟`void`一樣，他們並不是很有用處：
```ts
// 沒什麼東西可以讓我們指派給這些變數
let u: undefined = undefined;
let n: null = null;
```

`null`與`undefined`預設為次型別\(subtype\)，這代表你可以把他們指派給像是數值型別的變數。

但是，如果你啟用了設定檔的`--strictNullChecks`，那麼`null`與`undefined`就只能指派給他們自己或是`void`型別。這功能可以避免非常多的常見錯誤。如果你希望指派`string`或`null`或 `undefined`給一個變數，那麼你就可以使用合併型別(union type) `string | null | undefined`，在強調一下，後面的進階內容會提到更多關於合併型別的細節。

> 小註解：可以的話，我們非常建議使用`--strictNullChecks`，但在這本學習手冊內，我們是假設他是關閉的。

<br/>

## <center>無(Never)</center>

`never`型別代表著那些永遠不會出現的值，可以用於總是丟出錯誤訊息或什麼都不會回傳的函式。假如變數被絕對不是`true`的型別所約束，那也會得到`never`型別。
`never`是所有型別的次型別，可以將`never`指派給任何型別。但是沒有任何型別可以指派給`never`(除了`never`自己)，就算是`any`也無法指派給`never`。

一些函式回傳`never`的範例：
```ts
// 回傳的函式一定不會有終點
function error(): never {
  throw new Error(message);
}

// 可推斷為 never 型別
function fail() {
  return error("something failed");
}

// 回傳 never 的函式必須沒有可聯繫的端點
function infinitiLoop(): never {
  while (true) {

  }
}
```

<br/>

## <center>物件(Object)</center>

`Object`型別代表著非原生的型別，也就是任何非`number`,`string`,`boolean`,`symbol`,`null`或`undefined`。
使用`Object`型別可以輕鬆的表示像`Object.create`這樣的 API：
```ts
declare function create(o: object | null): void

create({ data: 0 }); // ok
create(null);        // ok

create(1208);       // 錯誤
create("字串");      // 錯誤
create(false);      // 錯誤
create(undefined);  // 錯誤
```

<br/>

## <center>型別斷言(Type assertions)</center>
有些情況下，你會比 TypeScript 更了某個值的訊息，透過型別斷言就可以告訴編譯器"相信我，我知道自己在做什麼。"，型別斷言像是其他語言中的型別轉換，但並不會進行特殊的檢查或調整(restructuring)。斷言只會在編譯時代有效果，不會對實際運行程式帶來其他影響，TypeScript 會假設身為工程師的你，已經做了足夠的檢查。

型別斷言有兩種方式，首先是用尖括號(angle-bracket)：
```ts
let value: any = "這是一段字串";
let stringLength: number = (<string>value).length;
```

與另一種 - 使用 as 語法：
```ts
let info: any = "又是一段字串";
let stringLength: number = (info as string).length;
```

兩種寫法的效果是相等的，要用哪種全看個人喜好。但是如果 TypeScript 搭配上 JSX 的話，那就只能使用`as`方式的型別斷言。

<br/>

## <center>關於 `let`</center>

你可能會注意到，目前為止我們都是使用`let`，而不是大家在 JavaScript 中熟悉的`var`。`let`是 JavaScript 較新的概念，而 TypeScript 已經將這個功能實現了。我們晚點再討論這個部分，但透過使用`let`，可以解決許多 JavaScript 常見的問題，所以你應該都改用`let`來代替原本的`var`。

<br/>

## 連結
- {% post_link '[TSHB]TypeScript Handbook' 返回目錄 %}
