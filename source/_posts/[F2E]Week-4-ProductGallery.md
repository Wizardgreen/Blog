---
title: "[F2E] Week 4 - ProductGallery"
date: "2018-07-01 12:37:01"
categories: TheF2E
tags:
---

<center>產品...圖鑑？</center>

<!-- more -->

{% asset_img cover.png %}

## <center>Week 4: ProductGallery</center>

[第四週設計稿](https://hexschool.github.io/THE_F2E_Design/week4-product%20gallery/)

[完成作品連結](https://wizardgreen.github.io/hexSchool-TheF2E-Showcase/#/week4)

本週好像沒什麼好說的...雖然的確有學到東西，但我果然還是喜歡 JS 與 CSS 兩者併用的創作。這週實在太無聊了，所以我自己額外加入了一些很北七的驚喜，希望看到的人能會心一笑 XD

## <center>自我學習項目</center>

-[Vue Property Decorator](https://github.com/kaorun343/vue-property-decorator)
恩...雖然是會用了，但因為這次的結構太簡單，
所以沒有什麼好好運用的感覺，下週題目會再多多研究。

<br />

-[CSS Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)
這次最大的收穫了！ Grid 算是比較嶄新的 CSS 屬性，與以往的 Float 排版或 Flex 不同，Grid 用了有點類似二元座標的方式進行繪製，特別適合用在向本週這樣排版很不規則的設計，可是從 [Can I Use ?](https://caniuse.com/#feat=css-grid) 可以看到有些瀏覽器還不能完全支援\(對 IE 我就是在說你\)，但其便利性絕對是指日可待。除此之外也附上一些本週學習 Grid 所讀的文章，希望能幫上想深入 Grid 的朋友們！

- [A Complete Guide to Grid - CSS Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/)
- [CSS Grid 屬性介紹 - 前端，沒有極限](https://wcc723.github.io/css/2017/03/22/css-grid-layout/)
- [與 CSS Grid 的第一次接觸 - TechBridge](https://blog.techbridge.cc/2017/02/03/css-grid-intro/)

另外 JS30 系列的作者 - Wes Bos，也有推出[四小時的免費課程](https://cssgrid.io/)唷！

<br />

-[CSS background: linear-gradient](https://www.w3schools.com/css/css3_gradients.asp)
linear-gradient 是一個可以用來製作漸層色彩的語句，結構如下：

```CSS
background: linear-gradient(角度or方向, 色彩1, 色彩2, ...)
```

連結中就有基本的使用方式，除了普通的漸層效果，我們也可以用來做出本週的斜線條，
我就直接寫上 Code 一同解說吧：
{% asset_img slash.png %}

<br />

從上面的設計稿中，我們可以推估每條線大概是 20px 左右的寬度。

```CSS
.slash {
  width: 100px;
  height: 100px;

  // deg 是角度的單位，這邊直接寫好，純粹是因為我已經做過，不是推理出的XD
  // 將每個段落都寫上不同的顏色，即可做出線條的效果
  // 這種排版方式比較仍易閱讀，我是覺得蠻不錯的.
  background: linear-gradient(135deg
    #50E3C2 0px #50E3C2 20px,
    transparent 20px, transparent 40px,
    #50E3C2 40px #50E3C2 60px,
    transparent 60px, transparent 80px,
    #50E3C2 80px #50E3C2 100px,
  );
}

// 好，這樣就大功告成拉。但，其實這樣是個笨方法，
// 樣板上雖然是 100 * 100，可實際草稿卻是 240 * 100
// 如果又有不同的尺寸不就又要寫更多類似的 Code ? 所以還有這招！

.slashWithGoodWay {
  // whatever 代表你能隨便填數值，我們接著的寫法都能應變
  width: whatever;
  height: whatever;

  // 將將～ 智慧生活，真 der 很簡單～
  background: repeating-linear-gradient(135deg,
    #50E3C2 0px, #50E3C2 20px,
    transparent 20px, transparent 40px
  );
}
```

## <center>第四週感想</center>

....\(放空 ， 上一週規模很大，這週突然又變得超簡單 :\(
希望...希望下週可以多一點 JS 的作品吧...不然感覺好像什麼都沒發揮到 XD

## <center>其他連結</center>

- [Github 原始碼](https://github.com/Wizardgreen/hexSchool-TheF2E-Showcase/)
- [F2E 作品展示台](https://wizardgreen.github.io/hexSchool-TheF2E-Showcase/#/)
- {% post_link The-F2E-前端修練精神時光屋-參賽紀錄 回TheF2E目錄 %}
