---
title: "[JS30] Day 5 - Flex Panel Gallery"
date: "2018-04-23 19:59:10"
categories: JavaScript 30
tags: JavaScript
---

> 這次比較著重在學習`Flex`(明明是 JS30，重點卻是 CSS XD)，透過 CSS3 的`Flex`我們輕鬆就能做出一個能夠適應各種平台的畫面，再加上 JavaScript 就能展現出更絢麗的視覺效果。

<!-- more -->

{% asset_img cover.png %}

## <center>Day:5 Flex Panel Gallery</center>

<center>響應式畫廊</center>

---

## <center>Flex</center>

台灣現在最主流的排版方式是使用`float`，不過這做法其實有點過時了，目前最新的技術則是`Grid`，但瀏覽器支援程度也還不是很理想。而這次的主題`Flex`，已經在各大瀏覽器都非常成熟也非常好用，可是台灣卻還不是那麼普及的用法。如果今天學到了一個新的技術，但不清楚到底支援度如何，可以到[Can I use...](https://caniuse.com/)搜尋看看，就能夠評估一下究竟適不適合使用在自己的網站上面囉。
<br />

{% asset_img caniuse.png %}

<center>Flex在各瀏覽器的支援度幾乎都是很好的綠色</center>

<br />
Flex 的概念比較特別，HTML元素可能會被分為 flex container 或 flex item，也可能兩者皆是，不論哪一個，都各有自己獨特的語句可以使用，概念會比以往的`float`還要抽象一點點。學習過程中，如果不太理解運作的方法，我建議搭配 codepen 的 [Flex Playground](https://codepen.io/enxaneta/full/adLPwv)，自己多操作個幾次，很快就會熟能生巧的。

### display: flex

我們打開起始檔案就會發現到，整個畫面是由五個連續往下排的`<div>`再加上`background-image`組合成的，與影片上的成品根本天差地遠 XD
左到右佔據整行是`block`元素預設的排版方式，我們可以在`.panels`加入`display: flex`將他轉換為 flex container，如此一來就能改變底下所有子元素的排列方式，而這些底下的子元素就是 flex item 。

```css
.panels {
  display: flex;
  .
  .
  .
}
```

<br />

### flex

_flex-grow, flex-shrink, flex-basis_

接下來是針對 flex items 才能使用的語句`flex`， 這個部分應該可以說是整個 Flex 裡面最抽象的部分。我們直接下`flex: 1`到`.panel`上面，就能發現原本全部塞在左邊的圖像，已經均勻充滿整個瀏覽器了，非常實用！

其實`flex`代表了三個屬性，分別是`flex-grow`、`flex-shrink` 和`flex-basis`，我們只輸入了一個數值，那就只會有`flex-grow`的效果，會有點抽象：以本題為例，五個子元素都有`flex-grow: 1`，那麼 container 底下如果有沒被分配的容量，就會分五等分給每個 flex item。如果其中一個值是 2、其他保持 1 ，那就會把剩下容量分六等份，再依照每個子元素設定的數值去分配。

```css
.panel {
  flex: 1;
  .
  .
  .
}
```

<br />
### Props for flex container #
*flex-direction, align-items, justify-content*

為了使用其他 flex 語句、達到更理想的排版，我們要把`.panel`也設定為 flex container，可是如此一來，內部的文字排序又全都被打亂了，這時候就可以加上`flex-direction: column`，來變回原本的版型。

```css
.panel {
  /* 建議按順序把一個屬性加進去，看看效果有什麼改變，再加入下一個 */
  display: flex;
  flex-direction: column;
  /*
  flex-direction 可以改變 container 的軸線，
  讓原本預設是X軸，由左往右的順序，變成不同的模式，
  column 這個值，就是讓主軸變成由上往下的Y軸.
  */
  justify-content: center;
  /*
  預設情況下 justify-content: center 會左右置中 container 底下的元素，
  但我們剛改變了 container 的主軸，
  整整轉了 90 度，因此在這時的效果變成了上下置中，
  如果有點抽象的話，建議在 flex playground 上面玩個幾次。
  */
  .
  .
  .
  flex: 1;
  align-items:center;
  /*
  注意，原本檔案內就有預設了 align-items
  但是在元素設定為 display: flex 之前都不會生效。
  跟 justify-content 類似，也因為 direction 的影響，
  從遠本的上下置中變成左右置中。
  */
```

再來讓每個`.panel`底下德元素都能夠平均分散、置中對齊，我們一樣再使用上面的技巧。

```css
.panel > * {
  display:flex;
  justify-content: center;
  align-items: center;
  flex: 1;
  .
  .
  .
}
```

到這邊為止，版型就都排好囉，再來就只剩一些簡單的`translate`與 class 的操作。

---

<br />
## <center>translateY</center>

使用`transform: transalateY()`與 css 選擇器，我們可以把特定的元素隱藏起來。

```css
.panel > *:first-child {
  transform: translateY(-100%);
}
/* panel 底下的第一個子元素，往上移動自身 100% 的距離 */
.panel > *:last-child {
  transform: translateY(100%);
}
/* 這邊應該就不用再多做解釋吧ＸＤ */
```

把兩個子元素長藏以來以後，我們先準備好使他們歸位的 class，等等再用 JS 觸發事件，移回原位。

```css
.panel.open-active > *:first-child { transform: translateY(0); }
.panel.open-active > *:last-child { transform: translateY(0); }
.panel.open {
  flex: 5;
  /*
  還記得嗎？如果等等某個 panel 加上了 .open ，那他會佔據多少的版面呢？
  */
  .
  .
  .
}
```

CSS Part end.

---

<br />
## <center>JavaScript toggle()</center>

先是點擊我們可以使用 classList.toggle() 來開關指定的 class。

```js
const panels = document.querySelectorAll(".panel");

function toggleOpen() {
  this.classList.toggle("open");
  // toggle() 會判斷目標元素是否有 .open ，沒就加上去，反之亦然。
}

panels.forEach((panel) => panel.addEventListener("click", toggleOpen));
```

最後，我們要在檔案內預設的`transition`結束時，把剛剛被我們藏起來的`<p>`回歸原位。

```js
function toggleActive(e) {
  if (e.propertyName.includes("flex")) {
    // transition 只需要挑一個來觸發我們的結束事件，
    // 但這個判斷用的 e.propertyName，在不同瀏覽器卻有不同的稱呼
    // Safari === flex , Chrome & FireFox === flex-grow
    // 因此我們可以使用 includes() 來判斷是否有共同的字母 “flex”

    this.classList.toggle("open-active");
  }
}

panels.forEach((panel) =>
  panel.addEventListener("transitionend", toggleActive)
);
```

<center>*大功告成！*</center>

---

<br />
## <center>Full Script</center>

```js
const panels = document.querySelectorAll(".panel");

function toggleOpen() {
  this.classList.toggle("open");
}

function toggleActive(e) {
  if (e.propertyName.includes("flex")) {
    this.classList.toggle("open-active");
  }
}

panels.forEach((panel) => panel.addEventListener("click", toggleOpen));
panels.forEach((panel) =>
  panel.addEventListener("transitionend", toggleActive)
);
```

---

<br />
## <center>相關連結</center>
- [卡斯伯 - 圖解：CSS Flex 屬性一點也不難](https://wcc723.github.io/css/2017/07/21/css-flex/)
- [Flex playground](https://codepen.io/enxaneta/full/adLPwv)
- [Can I Use...?](https://caniuse.com/)
- {% post_link JavaScript30-介紹 回去 JavaScript30-介紹 %}
