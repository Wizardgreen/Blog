---
title: \[F2E\] Week 5 - ComicViewer
date: '2018-07-08 17:10:50'
categories: TheF2E
tags:
---
<center>漫畫閱讀器</center>

<!-- more -->

{% asset_img cover.png %}

## <center>Week 5: ComicViewer</center>

[第五週設計稿](https://hexschool.github.io/THE_F2E_Design/week5-comic%20viewer/)

[完成作品連結](https://wizardgreen.github.io/hexSchool-TheF2E-Showcase/#/week5)

高中開始注意到很多同學都會到中國的網站找連載漫畫來看，我總覺得不論介面或翻譯品質都很糟糕，所以向來不喜歡這樣的閱讀體驗。這次的主題剛好可以讓我試著來做出自己喜歡的介面，不過大致上來說我還是被設計圖與傳統的形式綁死，並沒有真的跳脫出什麼框架...嗚，一個禮拜要整個從零開始果然太困難了。

<br />

## <center>自我學習項目</center>

-[Vue Property Decorator](https://github.com/kaorun343/vue-property-decorator)
承上週都沒什麼用到的 Decorator ，這週可是大用特用，整個 class component 都是滿滿的 @@@@@@@@@！但 github 上的 issue 討論提到，Property Decorator 其實是以 TypeScript 為而生的套件，雖然我實作上沒遇到什麼狀況，但 vscode 滿片關不掉的紅字讓對於程式碼有潔癖的我很困擾，所以最後在我整個作品即將完成之際，我又把整個class component給翻修一次把它通通移除掉，等 TypeScript 實裝以後再玩吧～

<br />

-[Swiper.js](http://idangero.us/swiper/)
-[Vue Awesome Swiper](https://github.com/surmon-china/vue-awesome-swiper)
非常好用的輪播工具與他的 Vue 版好朋友，我原本是想使用在六角群組內大推的 Slick，但是一打開 Document 後才發現他是一個以 jQuery 為底的套件，個人偏見的關係最後就改用了這款\(特別感謝雲龍大的推薦\)。原套件的官方文件算是好懂，不過 Vue 版則並沒有清楚說明許多細部的運作。到最後翻來翻去，把 Vue 版的 Demo 都看個一下才找出想要的 API。

<br />

-CSS 毛玻璃 & SCSS 檔案管理
實作了能夠穿透背景又同時讓閱讀元素保持清晰的特效，吃的效能很高，算是有點 hack 的方法，希望哪天 CSS 能開發出預設的屬性讓瀏覽器能夠更節省效能。還有這次終於比較徹底的規劃專案的 SCSS 檔案了，把變數，Mixin等等做了完整的切割，能夠輕鬆的在各個專案之間反覆運用，之後找點時間也來修正前幾週的檔案。

<br />

-[Vuex 大型專案管理模式](https://vuex.vuejs.org/guide/structure.html)
喔~我最愛這種能夠將檔案結構與程式碼等等...逐一條理分明的辦法，其實 Vuex 的文件只給出了一點輪廓。所以整個架構還是靠自己來的比例居多，總歸來說我往四個方向實行這個管理方法：
1. 遵照 Vuex 官方文件的檔案結構
2. 加入 Redux 的 action type 概念來管理 commit
3. 加入 React-Redux Container/Component 分離的概念 
  \(沒用在檔案命名上，我覺得這樣在目前的專案會混亂
4. 使用 Vuex 的 Module 將 store 拆分

除此之外我也一併整理了檔案的命名方式，竟量確保在各種情況下都不容易混淆。

{% asset_img filestructure.png %}
{% asset_img typefile.png %}
<center>應用 Vuex module 與 Redux action type 的檔案結構...爽快!</center>

<br />

## <center>第五週感想</center>
下一步會是期待已久的 TypeScript，成功導入以後，除了前文的 Property Decorator 以外，還有個 Vuex Class 也會是個好方向，但後兩者還不是很活躍，或許我可以先從研究他們的原理下手，看看能不能自己也想出一套好工具？說到底這對小專案與單人作業來講沒什麼幫助，只是徒增作業時間而已。但我就是喜歡條理分明的把每樣東西都整理好，更何況我更渴望想要參與一項偉大又複雜的開源計畫，就當作是為了那天而準備吧XD

不知不覺活動就已經過半了，真期待接下來的題目呀~

<br />

## <center>其他連結</center>
- [Github 原始碼](https://github.com/Wizardgreen/hexSchool-TheF2E-Showcase/)
- [F2E 作品展示台](https://wizardgreen.github.io/hexSchool-TheF2E-Showcase/#/)
- {% post_link The-F2E-前端修練精神時光屋-參賽紀錄 回TheF2E目錄 %}
