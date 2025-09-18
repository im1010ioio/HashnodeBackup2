---
title: "#67 CSS :target 應用：純 CSS 光箱、純 CSS Tab 頁籤"
datePublished: Thu Sep 18 2025 15:40:56 GMT+0000 (Coordinated Universal Time)
cuid: cmfpkve07000002l4c8rl8qwf
slug: css-pseudo-target-lightbox-tab
tags: pseudo-elements, css3, css, lightbox, tab, target

---

上一篇我們提到 CSS 的 `:target` 的偽類，可以**針對 HTML 元素是否為目標狀態而去調整樣式**。所以，如果延伸想一想，有許多 UI 都是會有狀態的，其實是可以做許多應用的。

上一篇著重在滾動時的目標狀態，這一篇我們來試試把這種目標狀態應用在別的地方吧！

---

## 一、純 CSS 光箱

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1758205060608/b9603127-1c0a-4737-9969-32cde0fe639b.gif align="center")

光箱（Lightbox）效果是網頁中非常常見的 UI，通常用於點擊圖片或按鈕後，跳出一個覆蓋整個畫面的視窗來顯示更多內容。以前這大多需要 JavaScript 來控制顯示與隱藏，但現在透過 `:target`，我們只要純 CSS 就能做到！

### 怎麼做出來的？

核心概念是：

1. **一個觸發按鈕**：這是一個 `<a>` 連結，它的 `href` 指向光箱元素的 ID，例如 `href="#lightbox1"`。
    
2. **一個光箱容器**：這是一個 `<div>`，它有一個對應的 ID，例如 `id="lightbox1"`。
    
3. **一個關閉按鈕**：這也是一個 `<a>` 連結，它的 `href` 指向 `"#"` 或一個不存在的 ID，目的是清除網址列中的 hash，讓光箱變回非 `:target` 狀態。
    

當我們點擊「觸發按鈕」，網址列會加上 `#lightbox1`，此時 ID 為 `lightbox1` 的 `<div>` 元素就符合了 `:target` 的條件，我們就可以透過 CSS 選擇器 `.lightbox:target` 來改變它的樣式，讓它從原本隱藏的狀態（`opacity: 0`）變成顯示狀態（`opacity: 1`）。

#### HTML 的結構很單純：

```xml
<a class="btn btn1" href="#lightbox1">Open Lightbox #1</a>

<div class="lightbox" id="lightbox1">
    <figure>
        <a href="#" class="close"></a>
        <figcaption>
            <h2>Hello! I'm Lightbox #1.</h2>
            <hr>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec felis enim,
      placerat id eleifend eu, semper vel sem.
        </figcaption>
    </figure>
</div>
```

#### 接著是 CSS 的魔法：

```css
/* 預設狀態：光箱是透明的，而且無法被點擊 */
.lightbox {
    opacity: 0;
    pointer-events: none; /* 讓元素無法成為滑鼠事件的目標 */
    transition: .3s; /* 加個過渡效果更滑順 */
}

/* 目標狀態：當光箱成為 target 時，讓它完全不透明，且可以被點擊 */
.lightbox:target {
    opacity: 1;
    pointer-events: auto;
}

/* 關閉按鈕的容器，只是為了定位 */
.lightbox .close {
    position: relative;
    display: block;
}

/* 關閉按鈕本體，用 ::after 偽元素畫一個叉叉 */
.lightbox .close::after {
    position: absolute;
    top: -.5rem;    right: -.5rem;
    z-index: 1;
    width: 1.5rem;  height: 1.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #555;
    border-radius: 50%;
    color: white;
    content: "×";
    cursor: pointer;
}

/* 光箱背景：我們用 ::before 偽元素做出一個覆蓋全螢幕的半透明黑底 */
.lightbox .close::before {
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    position: fixed; /* 用 fixed 定位才能蓋住整個畫面 */
    background-color: rgba(0, 0, 0, 0.5);
    content: "";
    cursor: default;
}
```

我們利用了 `::before` 偽元素做出了背景遮罩，再用 `::after` 做出了關閉按鈕，整個過程完全沒有動到任何一行 JavaScript！

更詳細的細節，請看 DEMO：

> DEMO 連結：[Pure CSS Lightbox by :target](https://codepen.io/im1010ioio/pen/NPGwYGr)

---

## 二、純 CSS Tab 頁籤

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1758209786118/09c05040-5fb6-423d-89d9-6816e12444a2.gif align="center")

Tab 頁籤也是一個很常見的 UI 元件，使用者可以點擊不同的頁籤來切換內容。這個效果跟光箱非常類似，都是利用 `:target` 來控制不同內容區塊的顯示與隱藏。

### 怎麼做出來的？

這次的 HTML 結構需要一點小巧思。為了讓 CSS 選擇器可以順利運作，我們需要將\*\*「內容區塊」放在「頁籤按鈕」的前面\*\*。

為什麼呢？因為我們要用這個波浪選擇器 `~` 符號。這個選擇器的作用是「選取某個元素**後面**的弟弟」。

我們的邏輯是：當某個內容區塊（例如 `#tab1`）變成 `:target` 時，我們要去改變它**後面**對應的頁籤按鈕（`.btn-tab`）的樣式，讓它看起來是「被選中」的狀態。

#### HTML 結構如下：

```xml
<div class="content">
    <!-- 內容區塊要先放 -->
    <div id="tab1" class="tab-content"> ... </div>
    <div id="tab2" class="tab-content"> ... </div>
    <div id="tab3" class="tab-content"> ... </div>

    <!-- 頁籤按鈕後放 -->
    <div class="tabs-row">
        <a class="btn-tab" href="#tab1">一月</a>
        <a class="btn-tab" href="#tab2">二月</a>
        <a class="btn-tab" href="#tab3">三月</a>
    </div>
</div>
```

#### CSS 的部分：

```css
/* 預設狀態：所有內容區塊都隱藏 */
.tab-content{
    display: none;
    padding: 1rem 2rem;
}

/* 目標狀態：當內容區塊成為 target 時，顯示它 */
.tab-content:target{
    display: block;
}

/* 頁籤按鈕的基本樣式 */
.tabs-row{
    display: flex;
}

.btn-tab{
    text-decoration: none;
    color: #79a6d0;
    background: #d3d886;
    padding: 1rem 1rem .5rem 1rem;
    border-top-left-radius: 1rem;
    border-top-right-radius: 1rem;
    transition: .3s;
}

/* 當 #tabN 是 target 時，選取它後面的第 N 個 .btn-tab */
#tab1:target ~ .tabs-row .btn-tab:nth-child(1),
#tab2:target ~ .tabs-row .btn-tab:nth-child(2),
#tab3:target ~ .tabs-row .btn-tab:nth-child(3){
    background: #f2f5bc; /* 變成 active 的顏色 */
}

/* 預設顯示第一個頁籤 */
/* :not(:has(...)) 的意思是「當 .content 裡面沒有任何 .tab-content 是 target 狀態時」 */
.content:not(:has(.tab-content:target)){
    /* 讓第一個內容區塊顯示 */
    .tab-content:nth-child(1) {
        display: block;
    }
    /* 讓第一個頁籤按鈕也顯示 active 樣式 */
    .tabs-row .btn-tab:nth-child(1){
        background: #f2f5bc;
    }
}
```

透過安排 HTML 順序，並搭配 `~` 選擇器，我們就成功實現了純 CSS 的頁籤切換效果，甚至還做出了預設狀態！

雖然說稍微有一點不夠彈性，要一個個頁籤設定；不過頁籤通常不會太多，所以在維護上還可以。如果更進一步，會使用 SASS/SCSS 的話，可以使用 for 迴圈寫出來，就會乾淨許多。

更詳細的細節，請看 DEMO：

> DEMO 連結：[Pure CSS tabs by the CSS :target](https://codepen.io/im1010ioio/pen/dPGyPZW)

---

今天我們看到了 `:target` 的兩個實際應用，它讓我們可以根據網址的 hash 來改變頁面樣式，進而實作出以往需要 JS 才能完成的互動效果。雖然它有其限制（例如會改變 URL、無法處理複雜的狀態管理），但在許多簡單的場景下，`:target` 無疑是一個輕量、有趣的解決方法。

> 延伸閱讀：
> 
> * [MDN CSS :target](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:target)
>     
> * [「CSS3:target選擇器」免程式!純CSS就可實現Tab頁籤式的互動切換](https://www.minwt.com/webdesign-dev/css/16987.html)
>     
> * [CSS沒有極限 - CSS3 :target選取器](https://www.casper.tw/css/2013/09/29/css-target/)
>     

---

#### ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

感謝看到最後的你，若你覺得獲益良多，請不要吝嗇給我按個喜歡。❤️

如果你喜歡我的創作，還想看看其他有趣的分享與日常，  
可以追蹤我的 IG [@im1010ioio](https://www.instagram.com/im1010ioio/)，或者是[🧋送杯珍奶鼓勵我](https://im1010ioio.bobaboba.me/)，謝謝你🥰。

![Eva Chen 送杯珍奶鼓勵我](https://cdn.hashnode.com/res/hashnode/image/upload/v1682564435616/c15640fc-6cee-4c33-a898-9cd6820f165a.png align="center")