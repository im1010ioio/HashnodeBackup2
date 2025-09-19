---
title: "#69 CSS scroll snap 做簡單的純 CSS 輪播"
datePublished: Fri Sep 19 2025 16:10:13 GMT+0000 (Coordinated Universal Time)
cuid: cmfr1cwks000d02kwd5kjczng
slug: css-scroll-snap
tags: css3, css, scroll-snap, carousel

---

以前，要做出輪播的「滾動吸附」的效果，十之八九都需要依賴 JS 套件（像是 [fullPage.js](https://alvarotrigo.com/fullPage/) 、[Swiper.js](https://swiperjs.com/) 或 [Slick.js](https://kenwheeler.github.io/slick/)）。這些工具雖然強大，但有時候只是想要一個簡單的橫向滑動，不需要複雜的自動播放或前後操作按鈕，引入一整個套件又有點太多。

現在，我們有了純 CSS 的原生解決方案，就是 **CSS Scroll Snap**。可以宣告滾動容器內的對齊點，當使用者滾動停止時，瀏覽器會自動產生一個平滑的動畫，將元素對齊到你指定的位置。

好的，那就讓我們來看看怎麼用吧！

---

## 一、Scroll Snap 的語法：容器與子項目

要實現 Scroll Snap，你需要記住一個簡單的「父子關係」：

* **滾動容器 (Scroll Container)**：也就是外層那個可以滾動的區塊。
    
* **滾動項目 (Scroll Items)**：也就是容器裡面那些要被對齊的子項目。
    

我們需要分別對這兩者設定 CSS 屬性。

### 1\. 對「滾動容器」的設定 `scroll-snap-type`

這是最重要的屬性，用來**開啟**這個容器的滾動吸附功能，並決定吸附的**方向**和**嚴格程度**。

它的值由兩部分組成：`[方向] [嚴格程度]`。

* **方向 (Axis)**：
    
    * `x`: 水平方向吸附。
        
    * `y`: 垂直方向吸附。
        
    * `both`: 兩個方向都吸附。
        
* **嚴格程度 (Strictness)**：
    
    * `mandatory`: **強制吸附**。  
        當使用者停止滾動時，瀏覽器**一定**會自動對齊到某個吸附點。這很適合用在一次只顯示一個項目的輪播牆。
        
    * `proximity`: **鄰近吸附**。  
        只有當使用者停止滾動的位置「足夠靠近」吸附點時，才會觸發對齊。如果停在離吸附點很遠的地方，就不會觸發。這比較適合用在像長篇文章，但你希望標題能盡量停在畫面頂端的場景。
        

**範例：**

```css
.container {
    /* 設定一個水平方向、強制吸附的滾動容器 */
    scroll-snap-type: x mandatory;
}
```

### 2\. 對「滾動項目」的設定 `scroll-snap-align`

這個屬性用來告訴瀏覽器，這個項目要**對齊容器的哪個位置**。

* `start`: 將項目的**起始邊**對齊容器的起始邊。（例如水平滾動時，對齊左邊）
    
* `center`: 將項目的**中心點**對齊容器的中心點。
    
* `end`: 將項目的**結束邊**對齊容器的結束邊。（例如水平滾動時，對齊右邊）
    

**範例：**

```css
.item {
    /* 讓這個項目在吸附時，總是對齊容器的正中間 */
    scroll-snap-align: center;
}
```

---

## 二、實戰範例：打造一個橫向滑動照片牆

那就我們來做一個最常見、最簡單的應用：一個可以左右滑動多張照片的輪播。

不過這個要讓使用者自己滑動喔！要有前後按鈕或自動播放還是要用 JS 做比較好。

### HTML 結構

結構非常單純，就是一個容器 (`.gallery`) 包著一堆項目 (`.img-container`)。

```xml
<div class="gallery">
    <div class="img-container">
        <img src="image1.jpg" alt="照片一">
    </div>
    <div class="img-container">
        <img src="image2.jpg" alt="照片二">
    </div>
    <div class="img-container">
        <img src="image3.jpg" alt="照片三">
    </div>
    <div class="img-container">
        <img src="image4.jpg" alt="照片四">
    </div>
    <div class="img-container">
        <img src="image5.jpg" alt="照片四">
    </div>
</div>
```

### CSS

```css
/* 1. 設定滾動容器 (父) */
.gallery {
    display: flex;
    overflow-x: auto;
    width: 100%;
    
    /* 開啟水平、強制吸附 */
    scroll-snap-type: x mandatory;

    /* 為了美觀，可以隱藏醜醜的滾動條 */
    scrollbar-width: none;  /* Firefox */
    &::-webkit-scrollbar {
        display: none; /* Chrome, Safari and Opera */
    }
}



/* 2. 設定滾動項目 (子) */
.img-container {
    /* 調整寬度，固定尺寸為 300px */
    flex: 0 0 300px;
    /* 或是每次出現 2 個 */
    /* flex: 0 0 50%; */
    
    padding: 1rem;
    
    /* 讓每個項目都對齊容器的開始 */
    scroll-snap-align: start;
    
    img {
        width: 100%;
        aspect-ratio: 1 / 1;
        object-fit: cover;
        border-radius: 1rem;
    }
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1758281179921/9c31135a-35c5-43e7-abb1-b6197baa9b1f.gif align="center")

> DEMO 連結：[Pure CSS Image Carousel by scroll-snap](https://codepen.io/im1010ioio/pen/KwVKZBY)

這樣我們就完成了一個功能完整、體驗滑順的圖片輪播效果。

範例是將容器對齊開頭位置，當然也可以依據需要改成對齊中間，或者改他的長寬比與滾動方向。

---

CSS Scroll Snap 不只可以用在圖片輪播，另外像是全滿版畫面滾動的網頁 (Full Page Scrolling)、產品卡片的滑動等等情境，都非常適合使用這個語法。

過去需要複雜 JS 才能實現的互動效果，現在變成了瀏覽器的原生功能，而且目前全瀏覽器都支援了（[Can I Use](https://caniuse.com/css-snappoints)），讓互動的 Code 更少，效能也更好、更順暢。

> 延伸閱讀：
> 
> * [MDN Web Docs: CSS Scroll Snap](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Scroll_Snap)
>     
> * [A Complete Guide to CSS Scroll Snap by CSS-Tricks](https://css-tricks.com/practical-css-scroll-snapping/)
>     
> * [Well-Controlled Scrolling with CSS Scroll Snap by web.dev](https://web.dev/articles/css-scroll-snap)
>     

---

#### ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

感謝看到最後的你，若你覺得獲益良多，請不要吝嗇給我按個喜歡。❤️

如果你喜歡我的創作，還想看看其他有趣的分享與日常，  
可以追蹤我的 IG [@im1010ioio](https://www.instagram.com/im1010ioio/)，或者是[🧋送杯珍奶鼓勵我](https://im1010ioio.bobaboba.me/)，謝謝你🥰。

![Eva Chen 送杯珍奶鼓勵我](https://cdn.hashnode.com/res/hashnode/image/upload/v1682564435616/c15640fc-6cee-4c33-a898-9cd6820f165a.png align="center")