---
title: "[F2E] Week 1 - TodoList"
date: "2018-06-10 16:02:27"
categories: "TheF2E"
tags:
---

<center>一個沒有刪除功能的待辦清單...</center>
<!-- more -->

{% asset_img cover.png %}

## <center>Week 1: TodoList</center>

[第一週設計稿](https://hexschool.github.io/THE_F2E_Design/todolist/)

幾個禮拜前，我期望的會是一個複雜的功能可以讓我好好思考該怎麼用 JS 解決這個難題。像這樣的過程總是非常吸引我，所以當看到第一題是代辦清單，讓我實在有點小小失望。不過隨後仔細看了一下老師列出的條目，就注意到其中還是有夾雜一些，我以往沒認真時做過的功能。但總體來說還是有點簡單，所以我幫自己額外擬定了一些學習項目，除了這次的第一題以外，接下來每週都會有各自的額外項目要學習。

## <center>自我學習項目</center>

- [Pug](https://pugjs.org/api/getting-started.html)
  一直以來我都只用過 CSS 的預處理器(Less/SCSS)，在第一次參加六角同學自行舉辦的讀書會時，我從雲龍大大那邊了解到像是 Pug 這累的 HTML 預處理器也非常直得一學，其實我自己在幾個月前有學過 ejs，但後來沒有常常使用，就給他全都忘光了。於是乎我就在心中默默地安排了這個目標。

- [Vue.Draggable](https://github.com/SortableJS/Vue.Draggable)
  Vue.js 的專用拖曳用套件，我從來沒做過拖曳功能，隨即上網查了一下，就發現這套似乎會是不錯的選擇。簡單好上手。

- Flexible Component
  這只是一個概念，在我剛到東南旅遊工作的時候\(也才兩個月前\)，前輩「百九」建議我在設計一個元件時，不單單只是把功能寫上、傳值而已，要多考量如何設計高彈性的自定義功能，如此一來才能讓元件能拿來反覆使用。我也透過這種思考方式學到了很多 JS 與現代框架的使用方法。

## <center>解析</center>

要學習如何適當的切出元件，[Think in React](https://reactjs.org/docs/thinking-in-react.html)非常好的一篇教學，這裡也有簡體中文的[版本](https://chenyitian.gitbooks.io/react-docs/content/docs/thinking-in-react.html)，對於切元件不是很熟系的人，真的很推薦讀讀這篇文章。

但對我來說，要切這麼細其實有點多餘，以下是我的切法：

{% asset_img cut.png %}

不同顏色的區塊個分別為一個元件，除此之外整個頁面也是一個大元件來收納底下的小元件。從設計圖上面可以得知，我們點擊輸入欄會展開一個編輯區塊，讓使用者對待辦事項寫入更詳細的內容。新增工作與代辦清單的編輯區看來可以設計成同一個元件來使用。但我隨後在實作上就發現兩個區塊傳遞的資料狀況有很多落差，最後導致我花了好多好多的時間在設計編輯區塊的元件上面。

我認為自然地移動效果能夠提升使用者體驗，而這樣的使用者體驗也能大幅增加對產品的黏著度，所以我是著在操作的過程增添了許多設計圖上沒有提到的平滑效果。
具體來說是什麼樣的效果，還是直接[操作看看](https://wizardgreen.github.io/hexSchool-TheF2E-Showcase/#/week1)，會比較好理解。

JavaScript  的部分就沒那麼值得一提了，就是很平淡地寫完而以，所以這邊就不多贅述。
我有附上 Github 連結，有興趣的話我也很樂於討論 : )

## <center>其他連結</center>

- [Github 原始碼](https://github.com/Wizardgreen/hexSchool-TheF2E-Showcase/)
- [F2E 作品展示台](https://wizardgreen.github.io/hexSchool-TheF2E-Showcase/#/)
- {% post_link The-F2E-前端修練精神時光屋-參賽紀錄 回TheF2E目錄 %}
