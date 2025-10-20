---
title: "#27 網頁載入字體、Icon Font 與 CSS font-family、font-weight"
datePublished: Fri Oct 13 2023 15:55:11 GMT+0000 (Coordinated Universal Time)
cuid: clnosib6e000009jsbggz8hb9
slug: css-font-family-font-weight
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1697218252515/668c150c-6781-4197-be56-b4757c1ab995.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1697218263475/963e1891-a3a3-4f9e-ae06-5ce8d55f20f1.png
tags: font-awesome, fonts, css3, css

---

> #### **↓ 今日學習重點 ↓**
> 
> * 了解字體的基本知識
>     
> * 了解網頁如何載入字體
>     
> * 了解 CSS font-family、font-weight
>     
> * 了解如何使用 Icon Font
>     

我們關於版面編排與 RWD 的部分終於告一個段落了，現在大觀念了解後，接下來我們要深入各個樣式的細節，例如文字、圖片、圖形、互動等等。首先，我們將從文字的部分開始。

在網頁設計中，字型滿重要的，可以為網頁增加視覺吸引力、改善閱讀體驗，並且讓所有使用者的字體體驗一致。在這篇我們將學習字體的基本知識，以及如何在網頁中使用字體、Icon Font。

---

## 常見的字體的格式

字體常見的格式共有以下幾種：

1. **TTF （TrueType）：**  
    Windows 跟 Mac OS 最常用的字型格式，較舊的系統能支援，但是檔案偏大。
    
2. **OTF （OpenType）：**  
    Adobe 跟微軟一起改良了 TTF ，平滑、精細，品質表現比 TTF 好，如果字體有 TTF 與 OTF 可以下載，建議下載這種。
    
3. **WOFF （Web Open Font Format）：**  
    是由 Mozilla、微軟、Opera 一起為網站打造的字型格式，將 OTF 或 TFF 加上 Metadata 壓縮而成，是 W3C 官方建議的字型格式。
    
4. **WOFF2 （Web Open Font Format 2）：**  
    第二代的 WOFF，壓縮得更小，是目前最適合網頁的字體格式。
    

> 延伸閱讀：[簡介字型檔格式：TTF、EOT、WOFF、WOFF2-黑暗執行緒](https://blog.darkthread.net/blog/web-font-type/)

---

## 字體的種類

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697206770103/9fc303a1-bae0-4b24-8508-42de6436c27b.png align="center")

在網頁中，我們最常使用的字體種類是「Serif 襯線體/明體」與「Sans-Serif 無襯線體/黑體」：

### 1\. Serif 襯線體/明體

Serif 有著襯線裝飾（serifs）的特點，在字型的末端具有小小勾起的裝飾。在中文裡稱呼它為明體。

這種字體通常被認為更適合印刷品和長篇文章，因為它能幫助人在閱讀時更順暢。可是在螢幕顯示上，容易受到光暈的影響，而顯得比較不明顯。例如：

> 延伸閱讀：[桃園機場用新細明體，有什麼問題？ - INSIDE](https://www.inside.com.tw/article/3060-airport-pminliu)

當然為了達到各種不同的風格，並不是在螢幕上就不能用，而是要考慮到字級大小與目標族群是否合適。

這種字體給人的感覺通常是古典的、經典的。

### 2\. Sans-Serif 無襯線體/黑體

Sans-Serif 去掉了襯線裝飾，字體末端平滑俐落。在中文裡稱呼它為黑體。

和襯線體相反，由於尾巴沒有裝飾，所以比較不會受到光暈的影響，所以常常被用在數位系統、看板上。

這種字體給人感覺充滿了現代感。

### 3\. Monospace 等寬字體

這種字體在我們使用的程式碼編輯器中常常看見，在網頁上的程式碼顯示段落也會套用這種字體。前面兩種字體為了設計視覺上的和諧，每個字的實際寬度並不一定是相等的，這樣在寫程式時反而會導致上下行不對齊，結果變得不易閱讀程式碼。

所以，一般來說程式碼會使用等寬字體顯示，常見的是打字機的那種字體。

### 4\. 其他

其他的字體還有：手寫體、書法體、圓體、藝術字體等等。通常為了吸睛，特殊字體會被當成標題，而不會被當成內文，因為內文若是太花俏反而會不易閱讀。

---

## 在網頁中載入字體

現在，讓我們試試看在網頁中使用不同的字體。

### 1\. 使用 `<link>` 元素載入字體

通常我們會在 HTML 的 `<head>` 使用 `<link>` 載入字體，越早載入字體使用者體驗會越好，所以放在網頁的一開頭 `<head>` 中載入。

我們以使用 [Google 提供的字體](https://fonts.google.com/)為例，會使用以下方式載入：

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC&display=swap" rel="stylesheet">
```

這邊 Google 建議我們可以加上 `<link rel="preconnect">`，事先告訴瀏覽器接下來要載入外部網站的資源，告訴它這個外部資源的 domain，讓瀏覽器可以提早建立連線，讓載入速度更快一些。

因為中文字體通常檔案比較大，速度的考量尤其重要。

### 2\. 使用 `@import` 載入字體

另外一種載入方式是使用，CSS 的 `@import` 載入字體：  
（這兩種方式擇一就好，個人偏好第一種）

```css
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+TC&display=swap");
```

### 3\. 使用 `font-family` 指定字型

一旦我們有載入了字型，在 CSS 的部分，我們就可以使用 `font-family` 設定字體：

```css
body{
	font-family: Arial, "Noto Sans TC", sans-serif;
}
```

這裡我們使用逗號隔開字體，如果文字在這個字體中沒有對應的字的話，就會套用下一個字體。一般來說，最後一個設定的字體會是 `sans-serif` 或 `serif` 這類的通用字型名稱（generic-family），指的是使用者電腦中預設的無襯線體或襯線體。

以上的例子，英文字會使用 Arial 這個字型；而中文字在 Arial 字型找不到編碼後，就會使用思源黑體（Noto Sans TC）。

如果字體為一個單字，就可以不需要使用冒號框 `""` 起來，直接寫單字就好。

其餘的通用字型名稱（generic-family），請參考：

> [font-family - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family)

如果我們沒有載入字體，但是在 CSS 中有設定的話，瀏覽器會去找使用者電腦中的字體，這樣就會導致有些人可以正常顯示字體，有些人則不行，除非你使用的是系統內建字體（如：Arial、Time New Roman 是許多人電腦中有的字體）。

---

讓我們再試試載入一個特殊字體看看，換了個字體就文青了起來：  
（Code Pen 中 HTML `<head>` 的設定在 Settings 中）

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697133594875/0103cf9b-eec8-4e85-9430-b4eb5bf82067.png align="center")

> DEMO 連結：[Font ChenYuluoyan](https://codepen.io/im1010ioio/pen/JjwVdgK)

附帶一提，這個辰宇落雁體是由台灣高中生製作的，很優秀！

> Github：[Chenyu-otf/chenyuluoyan\_thin: 辰宇落雁體](https://github.com/Chenyu-otf/chenyuluoyan_thin)

---

## 字體粗細 `font-weight`

我們還可以為這個字體設定粗細，只不過要載入對應粗細的字體才有作用喔！

```css
p {
    font-weight: 400;
}
```

CSS 中常見的粗細，有 10 級可以設定：

| `font-weight` | 對應字型的命名 |
| --- | --- |
| `100` | Thin |
| `200` |  |
| `300` | Light |
| `400`（等同 `normal`，預設值） | Regular |
| `500` | Medium |
| `600` |  |
| `700`（等同 `bold`） | Bold |
| `800` |  |
| `900` | Black |
| `1000` |  |

另外，還可以設定 `lighter` 與 `bolder` ，不過他們是依據由父層繼承而來的 `font-weight` 而有不同，詳細請見下表：

| 由父層繼承的 `font-weight` | `bolder` | `lighter` |
| --- | --- | --- |
| `100` | `400` | `100` |
| `200` | `400` | `100` |
| `300` | `400` | `100` |
| `400` | `700` | `100` |
| `500` | `700` | `100` |
| `600` | `900` | `400` |
| `700` | `900` | `400` |
| `800` | `900` | `700` |
| `900` | `900` | `700` |

---

## 常見的網頁字體資源

### 1\. Google Font

> 連結：[Browse Fonts - Google Fonts](https://fonts.google.com/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697210692049/5f966ffb-7a12-4af5-9ee1-04a30a10493d.png align="center")

我們在一開始的範例就是使用 Google Font，Google Font 非常齊全，有各種受歡迎的字體，而且是免費的。只是可惜的是，中文字體只有兩種，就是思源黑體（Noto Sans TC）與思源明體（Noto Serif TC）。

所以在設計時，有的時候我會使用日文字體，比較多樣化，只是容易缺字，與筆順與中文有一些些不同。

### 2\. Adobe Font

> 連結：[Adobe Fonts | 探索無限字體](https://fonts.adobe.com/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697210704381/b245ed3b-5ce2-42be-833e-a54105289f49.png align="center")

Adobe Font 也是非常有名，也很齊全的字體庫，只不過要使用的話，需要有訂閱他們的方案才能使用。

### 3\. justfont

> 連結：[字型列表 - jf store](https://store.justfont.com/fonts)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697210721105/42b18d67-a501-4a2c-9907-d9f88ee9853d.png align="center")

justfont 是台灣很有名研究中文字體的公司，之前很火紅的金萱體、蘭陽明體就是他們設計的，他們字體都非常好看！大部分需要購買才能使用。不過，他們曾經有為了回饋大眾，而推出了[粉圓體](https://justfont.com/huninn/)，非常可愛實用。

另外，他們也有經營 Facebook 粉絲專頁與社團：[字戀](https://www.facebook.com/lovefonts)與[字嗨](https://www.facebook.com/groups/149874075167476)，裡面很多關於有趣字體的討論，有興趣的朋友可以去看看。

### 4\. emfont

> 連結：
> 
> * [emfont](https://font.emtech.cc/)
>     
> * [emfont - 純 CSS 載入字體](https://font.emtech.cc/docs/pure-css)
>     

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760931395507/cc14ff33-be9d-431f-8bf8-3e97d461b42a.png align="center")

如果想使用免費的中文字型，emfont 這個網站整理了很多開源、免費的字體，也可以試試看喔！詳細使用方法請看他們的[文件](https://font.emtech.cc/docs/setup)。

---

## 常見的網頁 Icon Font

網頁上很多 icon 圖形其實是做成了字體，這樣能夠方便我們變大變小變顏色。

### 1\. Google Material Symbols / Icon

> 連結：[Material Symbols and Icons - Google Fonts](https://fonts.google.com/icons)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697211709888/5d2d7c21-5300-47d4-9bbe-8b78489d9780.png align="center")

Google 推出的 Icon 也很齊全，共有兩種，第一種是舊版本的 Material Icon，第二種則是新版本的 Material Symbols，新版本可以調整 Icon 的粗細、預設大小。

它們的字體運作原理，是將特定的「英文單字」顯示為不同的圖示。不過，這樣有個問題，就是當使用瀏覽器的翻譯功能時，「英文單字」被翻譯了，圖示就會不見，然後變成被翻譯的文字。QQ

所以使用時建議考慮一下使用者會不會有需要翻譯的行為，再決定要不要使用它。

### 2\. Font Awesome

> 連結：[Font Awesome](https://fontawesome.com/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697211717340/4364aa8b-9463-44bf-ad8e-f9fd33f7a727.png align="center")

Font Awesome 是比起 Material Symbols / Icon 更齊全的網頁 icon，但是它需要註冊並且建立專案，有一些 icon 要額外收費。

它的字體運作原理，是將是特定的「代碼」顯示為不同的圖示。它是使用偽元素 `::before` 的 `content` 設定這個代碼，例如：`content: "\f167";`。

### 3\. **Taiwan Icon Font**

> 連結：[Taiwan Icon Font](https://www.twicon.page/index.html)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697217888219/28e35f95-7446-4254-b456-1715c1756269.png align="center")

台灣也有專屬於自己的 Icon Font，是由生活在台南的日本設計師 holoko 和居住在台灣的英國程式設計師 Rob 共同開發，是免費的。雖然我沒有用過，但這些 Icon 應該滿適合使用在國內旅遊的專案上。

它的字體運作原理和 Font Awesome 一樣。

### 4\. 單純只使用 CSS 設定 Icon Font

知道了 Icon Font 的運作原理後，我們也可以用和他們同樣的方式，使用 CSS 設定 Icon 顯示在不同地方，例如改變清單 `<ul>` 的圖示（不過我這裡是使用[舊版的 Font Awesome](https://fontawesome.com/v4/icons/) 作 DEMO）：

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697217410528/2c5178bd-361f-49aa-b3da-ee2a28c2c87d.png align="center")

> [DEMO 連結： List with font awesome icon](https://codepen.io/im1010ioio/pen/PKpObM)

---

> 參考資料：
> 
> [網頁中英文字型(font-family)跨平台設定最佳化＠WFU BLOG](https://www.wfublog.com/2014/02/font-family-chinese-cross-platform.html)  
> [Font-family - 金魚都能懂的CSS必學屬性 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天 (](https://ithelp.ithome.com.tw/articles/10238571)[ithome.com.tw](http://ithome.com.tw)[)](https://ithelp.ithome.com.tw/articles/10238571)

---

#### ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

感謝看到最後的你，若你覺得獲益良多，請不要吝嗇給我按個喜歡。❤️

如果你喜歡我的創作，還想看看其他有趣的分享與日常，  
可以追蹤我的 IG [@im1010ioio](https://www.instagram.com/im1010ioio/)，或者是[🧋送杯珍奶鼓勵我](https://im1010ioio.bobaboba.me/)，謝謝你🥰。

![Eva Chen 送杯珍奶鼓勵我](https://cdn.hashnode.com/res/hashnode/image/upload/v1682564435616/c15640fc-6cee-4c33-a898-9cd6820f165a.png align="center")