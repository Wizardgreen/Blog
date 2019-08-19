---
title: \[F2E\] Week 3 - AdminOrder
date: '2018-06-24 19:25:46'
categories: TheF2E
# tags: 
---

<center>管理後台</center>
<!-- more -->

{% asset_img cover.png %}

## <center>Week 3: AdminOrder</center>

[第三週設計稿](https://hexschool.github.io/THE_F2E_Design/week3-admin%20order/)

[完成作品連結](https://wizardgreen.github.io/hexSchool-TheF2E-Showcase/#/week3)

以活動條件來說，我是達標了，但以我自己的要求來說，本週是徹徹底底的大失敗，覺得有點小難過 Orz. 不過這次題目的難度一口氣上升一大截，應該很多人都沒辦法在時間內完全攻略這次的題目。特別是 API 的部分，到底要去哪裡找可以涵蓋這麼多東西的 API 勒 ＱＱ

最後我打算交出 CSS 達成最基本條件就好，但在交出前看著我用的三個主色，突然靈機一動，
馬上找 Github API 串起來把首頁做成三大 SPA 框架的 Star 比較圖 XD不過太臨時了沒辦法弄完，還是有點可惜，如果能再加上 commit 紀錄 與 star 走勢圖那就更棒了！


## <center>自我學習項目</center>

- [Vuetify](https://vuetifyjs.com/en/) - 挑戰失敗

Material Design 風格的 Vue 外觀框架其實蠻多的， Vuetify 在我看來是比較脫穎而出的一款，不論維護或社群互動都有一定的熱度，我也早在去年就持續官網這款套件。但實際寫起來覺得 Document 不怎麼好上手，彈性也不是很高，特別是排版設定上會使 HTML 的標籤層數過於複雜，何況我還是用 Pug 來撰寫的，換回原生 HTML 可能對我來說會是一場災難。
會歸類為失敗主要是因為 style 嚴重覆蓋了我原先寫好的作品...

{% asset_img wtf.png %}
看來我還是以後有其他全新專案再來玩玩看這些潮到出水的 Material Design 吧 ＱＱ

<br />

- [Chart.js](http://www.chartjs.org/)

聞名已久的圖表套件，官網配色實在太美拉，美到我就直接決定用他的色系作為這週作品的色調。套件本身的功能也很好上手!只要在 HTML 上放個 Canvas ，再把資料套上去就完工了～

以我這次題目的圖表來說，只要下方的 code 就能輕鬆達成：
```javascript
  // 選擇要哪種類型的圖表
type: 'line',
chartData: {
  // 標籤資料，作為圖表下方的對照資訊
  labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
  datasets: [
    // 資料組，也就是要視覺化的資料，我總共做了三組，所以就只 show 一個
    {
      label: 'Vue',
      backgroundColor: 'rgba(81, 192, 191, .6)',
      borderColor: 'rgba(81, 192, 191)',
      borderWidth: '1px',
      data: [40, 20, 12, 39, 25, 40, 39, 20, 40, 20, 12, 11]
    },
  ]
},
```
是不是很簡單rrrr，我也好興奮啊啊啊！
[官網](http://www.chartjs.org/samples/latest/)也提供了好多不同的範本，不只圖表，也可以利用各種數據作出炫砲的效果!

<br />

- [Vue-chart.js](https://github.com/apertureless/vue-chartjs)

我沒有事先作太多功課，就直接挑了這款在 github 上星星比較多的套件。顧名思義，就是以 chart.js 為底，製作成 Vue 的元件來反覆使用，不過建議還是先把 Chart.js 給弄懂再來搭配這個套件，才不會在 documnet 卡太久。

<br />

- [Faker.js](https://github.com/marak/Faker.js/)

又是一個簡易又強大的套件，提供了非常多樣化的 API 能夠幫前端生出許多假資料，如此一來前端就可以不需要串接資料就能呈現網站囉。如果覺得麻煩，也有特定的 API 可以產生一整組的 userCard 或其他常見的資料類型，非常直得一學。

<br />

- [JSON Sever](https://github.com/typicode/json-server)

將 JSON 模擬成伺服器，也就是說只要有 JSON 檔，就算後端提供的 API 壞掉了，也可以直接撰寫 AJAX 模擬與伺服器串接。如果再搭配前面的 Faker.js ，更是如虎添翼啊！ ~~誰還需要後端~~ 這種想法姆湯姆湯...

<br />

- [Element](http://element.eleme.io/#/zh-CN)

發現 Vuetify 沒辦法套用在作品上之後，我隨即找上了這款套件，Element 應該是中國目前 Vue 社群內最大宗的外觀套件了，外觀風格中規中局，整體來說都算優秀。但是 document 非常平易近人，我到最後三天整個打掉重做，最後還能趕上交件，這麼容易上手又不會有什麼錯誤，難怪能成為目前最主流的外觀套件。

<br />

## <center>第三週感想</center>
這週真是跌跌撞撞的，時而氣餒時而興奮 (情緒這麼極端，是小孩子嗎XD)
我原本在上週就打算學 Element 快速搭建這次作品的輪廓，但實在受不了 Material Design 潮到不行的風格而跑去學 Vuetify，好不容易啃了很難理解的文件，結果卻遇到相容信的死胡同，繞了一大圈又打掉重做，好險 Element 基本上跟 Bootstrap 一樣平易近人，挽救了整個局面。

可惜這週在 TypeScript 上並沒有太多的進步，也沒有其他更深入的 JS 學習。取而代之的，我學了許多小型套件，還有超棒的 Chart.js！這讓我更期待再過一陣子要跟[十六進位](https://hexadecimalteachblog.github.io/)的夥伴們一起學習得 D3.js 了！

我發現隨著活動作品越來越多， Vuex store 也漸漸龐大起來，我應該需要花點心思整理一下檔案結構，希望下週我能讀上幾篇相關的討論，並且整理出自己最合適的方法。如果時間允許的話，我也想要把`vue-property-decorator`納入專案中，能夠使用很酷的 Decorator 來撰寫 prop、watch 等功能，簡單了解一下，雖然不是官方開發的套件，但似乎在社群的推薦度是蠻高的，真令人期待。

## <center>其他連結</center>
- [Github 原始碼](https://github.com/Wizardgreen/hexSchool-TheF2E-Showcase/)
- [F2E 作品展示台](https://wizardgreen.github.io/hexSchool-TheF2E-Showcase/#/)
- {% post_link The-F2E-前端修練精神時光屋-參賽紀錄 回TheF2E目錄 %}
