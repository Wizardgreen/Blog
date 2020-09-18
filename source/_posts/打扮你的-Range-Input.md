---
title: 打扮你的 Range Input
date: 2019-10-21
tags:
  - HTML
  - CSS
---

原生 Range Input 太醜了吧

# <center>緣起</center>
現在待的公司是以高複雜度的系統網站，作為主要的工作項目，都是在寫共用元件、各種 React&Redux 邏輯。一轉眼過了九個月，發現設計師的新稿上面有我不會實作的 Range Input，才突然驚覺這段時間並沒有學習更多的 CSS 技巧。

{% asset_img design.png %}


<center>我不知道怎麼實作拖曳左方的白色線條</center>
<center>其實我根本忘記怎麼修改 range input...</center>

---

# <center>實作</center>
```css
input[type="range"]::-moz-range-progress {
  background-color: green;
  height: 1em;
}
```
完工！
沒有，騙你的。
這種被騙的心情就跟當初 Google 如何實作的我一樣，
但仔細看 MDN 上會解釋說這項功能還沒實作好…

---

# <center>真。實作</center>
我們要用scss實作下圖的 range input.

{% asset_img square.gif %}
---

# Step 1 打底

```html
<!-- HTML -->
<input type="range" value="20" />
```

```scss
// SCSS
// 首先學習新屬性 "appearance", 可以將目標實作為指定 Tag 的外型
// ex: div 給他 apperance: button， 他就會看起來像是個 default button
// 如果值為 none 則消除原有外觀.
input[type="range"] {
  -webkit-appearance: none;
  height: 40px;
  width: 200px;
  background: none; // 清除不必要的東西
  outline: none; // 清除不必要的東西
  border-radius: 0; // 清除不必要的東西
  overflow: hidden; // 晚點會用到
  cursor: pointer;
}
```
只是`appearance`並不是支援度很完整的屬性，在實作的時候記得要加上對應的前綴詞，[細節看這](https://developer.mozilla.org/en-US/docs/Web/CSS/appearance)。
我們這篇都只用`-webkit-`實作，以防太多 code 讓人混亂.

---

# Step 2 認識新朋友
```scss
input[type="range"] {
  // ...上略
  // 是能調整 input 後方那一條狀物的朋友
  &::-webkit-slider-runnable-track {
    background-color: #ddd;
  }
  // 是能調整拖曳鈕的朋友
  &::-webkit-slider-thumb {
    -webkit-appearance: none; // 一樣要消除
    width: 20px;
    height: 40px;
    background-color: #fff;
    border: 2px solid #999;
}
```
---

# Step 3 box-shadow
最後是我們能做出像進度條的關鍵 - `box-shadow`

```scss
input[type="range"] {
  // ...略
  &::-webkit-slider-thumb {
    // ...略
    box-shadow: -100vw 0 0 100vw #69eaab;
  }
}
```

copy, paste and done, 但你清楚這是怎麼運作的嗎？

```md
box-shadow: -100vw   | 0        | 0           | 100vw         | #69eaab;
box-shadow: offset-x | offset-y | blur-radius | spread-radius | color;
```

|*名稱*        |*用途*     | *備註*|
|---          |---        |--- |
|offset-x     |水平陰影投射 | 值越大陰影越往左偏|
|offset       |垂直陰影投射 |值越大陰影越往下偏|
|blur-radius  |模糊程度    |值越大越模糊|
|spread-radius|陰影大小    |以元素本身大小為基準去修正，例如給 1px，陰影就會是比元素本身大 1px|
|color        |顏色       |就是顏色|
|inset        |嵌入(???   |特殊的關鍵字，默認不輸入，填入的話會改變陰影模式，但根本篇無關就不詳細了|

回頭比對我們剛才輸入的內容
```scss
input[type="range"] {
  // ...略
  overflow: hidden;
  &::-webkit-slider-thumb {
    // ...略
    box-shadow: -100vw 0 0 100vw #69eaab;
  }
}
```
我們把拖曳鈕投射出的陰影往左推 100vw 的距離，再將他放大到同樣單位的大小，
設計出色塊跟著拖曳紐移動的效果，最後搭配前面的`overflow`把多餘的部分裁掉，
Mission Completed, 目標達成。

可是如果我只是想做長相比較傳統的`input`怎麼辦？我不想要方型怎麼辦？

{% asset_img goal.png %}

當然，拖曳紐本身要圓型實在太簡單不過了，可是我們要怎麼把陰影變得比拖曳鈕小呢？
我第一直覺…

<center>當然是把spread-radius,offset-x變小啦！</center>

```scss
// ...略
&::-webkit-slider-thumb {
  // ...略
  box-shadow: -20px 0 0 -10px #69eaab;
}
```

{% asset_img wrong.png %}
<br />
<center>ㄜ ....</center>
<center>…對喔，我把陰影縮小就全都等比例縮小了，該怎麼辦？</center>

---

# step extra - box-shadow ADVANCED!
其實`box-shadow`五個值可以視為一組，然後可以填無限組值給他，
也就是說我們可以給他 N 組不同的陰影，搞得像是很多個光源打在同一個物件上，
那麼就可以利用這個特係，給他 N 個往左偏移的小陰影啦～
```scss
// ...略
&::-webkit-slider-thumb {
  // ...略
  box-shadow: -20px 0 0 -10px #69eaab, -30px 0 0 -10px #69eaab,
    -40px 0 0 -10px #69eaab, -50px 0 0 -10px #69eaab;
}
```

{% asset_img notsogood.png %}

才打五組肥宅我的手油就已經讓鍵盤變成炸物店的吸油紙，太累了吧?
而且仔細看其實一堆圈圈，這樣我不就要把間距改更小，又要打更多才能完成。

身為工程師還是要把這種事情自動運算完才對啊。
```scss
$inputWidth: 200;

@function getProgress($inputWidth, $color) {
  $val: -5px 0 0 -10px $color;
  @for $i from 6 through $inputWidth {
    $val: #{$val}, -#{$i}px 0 0 -10px #{$color};
  }
  @return $val;
}

input[type="range"] {
  // ...略
  width: #{inputWidth}px;
  // ...略
  &::-webkit-slider-thumb {
    box-shadow: makeProgress($inputWidth, #69eaab);
  }
```
再來微調一下大小…
```scss
&::-webkit-slider-runnable-track {
  // ...略
  height: 20px;
}
&::-webkit-slider-thumb {
  // ...略
  margin-top: -10px;
}
```

{% asset_img round.gif %}

<center>爽！</center>
<center>style 你的 range input 就是這麼簡單！(好像也沒有很簡單啦)</center>

最後補上完整的 [Code](https://codepen.io/wizardgreen/pen/OJJbGrP)

---

# <center>參考連結</center>
1.[MDN - css: appearance](https://developer.mozilla.org/en-US/docs/Web/CSS/appearance)
2.[MDN - css: box-shadow](https://developer.mozilla.org/zh-TW/docs/Web/CSS/box-shadow)