---
title: "#65 CSS 簡易的滑順捲軸滾動動畫 scroll-behavior"
datePublished: Tue Sep 16 2025 15:09:05 GMT+0000 (Coordinated Universal Time)
cuid: cmfmoupyj000302kv32ibe4vb
slug: css-scroll-behavior
tags: css3, css, css-animation, scroll, scroll-animation, scroll-behavior

---

前幾篇我們已經學會怎麼設定 CSS 的滾動範圍，現在我們要再對滾動的操作行為做一些很簡單的設定，就是：滾動時的行為 `scroll-behavior` 。

---

## 一、使用 HTML hash `#` 連結（錨點），滾動到指定 HTML ID 範圍

### (1) hash 錨點的設定

在 CSS 中指定 ID 時是使用井字號代表，在 HTML 中其實也是一樣的，我們可以使用 `<a>` 連結連到這個 ID，點了連結後就會在同個畫面中跳到這個 ID 的所在區域。

舉個例子來說：

```xml
<!-- 點了這個連結後 -->
<a href="#about">關於我</a>

<!-- 會跳到頁面上 ID 叫做 about 的區域 -->
<div id="about"> ... </div>
```

這種需要快速跳轉的行為，常常會用在技術文件或是法條等很長的內容。

### (2) hash 錨點的特性

點了這個連結後，除了發現捲軸跳轉外，你還會發現網址上多了 `#about` 的結尾，並且這些 hash `#` 連結（錨點）會記錄在你的瀏覽器的歷史紀錄中，按上一頁時會跳轉這些 hash `#` 連結（但是不一定會跳轉），像下圖：

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1758031501856/c0e74cd5-defc-4cb9-8fa4-5ff4b26d60c7.gif align="center")

> 注意：如果你不想要 ID 出現在網址列上，而且記錄在瀏覽器的特性，那這個效果就要使用 JS 去寫囉！

另外，你也會發現這些跳轉行為沒有動畫，是瞬間跳轉的。  
這時候就要介紹我們本篇 CSS 的重點語法 `scroll-behavior` 。

---

## 二、使用 `scroll-behavior` 設定簡單的平滑動畫

這個語法非常簡單，只要在想要滑順滾動的地方設定 `scroll-behavior: smooth` 就行了：

```css
.scrollable {
    scroll-behavior: smooth;
    /* 預設為 auto，沒有動畫 */
}
```

只不過，這個滾動的平滑動畫是依據瀏覽器而定，以 Edge 為例，滾動的時間是固定的，所以點擊越遠的 ID，滾動的速率會越快：

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1758032672730/951a9c71-22a2-4e4c-8f1a-35bae60aa25e.gif align="center")

> 注意：如果需要自訂滾動的速率、時間或速率曲線，就需要自己寫 JS 才能設定詳細的參數。

不過這個方法，是目前最簡單的滾動寫法，而且可以適用大部分的場景。

> DEMO 連結：[CSS scroll-behavior smooth](https://codepen.io/im1010ioio/pen/ogjoqGR)

---

## 三、額外小知識：網址上常見的井字號 `#` 與問號 `?` 有何不同？

這篇我們介紹了網頁上的錨點 hash `#oooxxx`。

另外，順便跟大家介紹在網址上也很常出現的 **Query String**（查詢字串） `?id=000`，因為這兩者很容易讓人搞混。

簡單來說：

網頁上的錨點 hash **不會**重新渲染網頁並重整網頁，而且也**不會**有資料透過這個方式被傳送到網站的伺服器，多半用於網頁內的「頁內導覽」，或者是「狀態切換」。

而 Query String（查詢字串），也就是像是「 `?id=000` 」這樣的網址結尾，**問號後面的所有資訊通常都會被傳送到網站的伺服器進行處理**，所以會使網頁**重新載入**，而多個參數之間會使用 `&` 符號來分隔，例如：

* `https://www.example.com/search?keyword=abc&page=2`  
    這段網址將告訴伺服器使用者正在搜尋關鍵字「abc」，並且希望查看第二頁的結果。
    

好的，希望這樣大家對網頁的知識會更清楚喔！  
今天就先到這邊囉！

---

#### ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

感謝看到最後的你，若你覺得獲益良多，請不要吝嗇給我按個喜歡。❤️

如果你喜歡我的創作，還想看看其他有趣的分享與日常，  
可以追蹤我的 IG [@im1010ioio](https://www.instagram.com/im1010ioio/)，或者是[🧋送杯珍奶鼓勵我](https://im1010ioio.bobaboba.me/)，謝謝你🥰。

![Eva Chen 送杯珍奶鼓勵我](https://cdn.hashnode.com/res/hashnode/image/upload/v1682564435616/c15640fc-6cee-4c33-a898-9cd6820f165a.png align="center")