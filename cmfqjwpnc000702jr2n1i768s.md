---
title: "#68 CSS overscroll-behavior：解決滾動穿透問題 & 阻止手機瀏覽器下拉重整"
datePublished: Fri Sep 19 2025 08:01:44 GMT+0000 (Coordinated Universal Time)
cuid: cmfqjwpnc000702jr2n1i768s
slug: css-overscroll-behavior
tags: css3, css, css-scroll, overscroll-behavior

---

你有沒有遇過這種情況？網頁跳出一個對話框，你想滑動看看裡面的內容，結果才滑到底，整個後面的網頁竟然也跟著動了起來！

或是，你在手機上瀏覽一個很酷的 Web App，只是想上下滑動一下，結果不小心觸發了瀏覽器的「下拉重新整理」，整個頁面就這樣重載了...

這種「滾動失控」的狀況，我們稱之為「**滾動穿透** (Scroll Chaining)」。 過去要處理這種問題，可能需要動用一些 JS，甚至搞得有點複雜。

但好消息是，現在我們有了一個純 CSS 的新解法：`overscroll-behavior`。

---

## 一、什麼是滾動穿透 (Scroll Chaining)？

在我們深入 `overscroll-behavior` 之前，先來搞懂它要解決的問題。

「滾動穿透」或稱為「滾動鏈」，是瀏覽器的預設行為。 想像一下網頁上有兩個可以滾動的區域，一個是外層的整個頁面 (`<html>`)，另一個是內層的 `<div>`（例如對話框、側邊欄、聊天視窗等）。

當你在內層的 `<div>` 裡滾動，滑鼠滾輪滾到底部（或頂部）後，如果繼續朝同一個方向滾動，這個「滾動事件」就會像接力棒一樣，傳遞給外層的 `<html>`，導致整個頁面也開始滾動。

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1758265000746/7be3da4b-acb6-4c75-bb9b-3dd013fe237e.gif align="center")

*(這就是惱人的滾動穿透)*

雖然這是瀏覽器的「貼心」設計，但在很多 UI 設計中，這反而會干擾使用者體驗，讓使用者覺得介面不聽話、很難控制。

---

## 二、`overscroll-behavior`：你的滾動行為管理大師

`overscroll-behavior` 這個 CSS 屬性，就是用來**控制當一個元素滾動到邊界後，接下來會發生什麼事**。 它可以讓你輕鬆地「切斷」滾動鏈，告訴瀏覽器：「這個框框滾到底了，就停在這裡，不要再去動後面的東西了！」

它有三個主要的屬性值：

1. `auto` (預設值) 這就是我們前面說的預設行為，允許滾動穿透。滾動事件會一路向上傳遞。
    
2. `contain` 這大概是你最常用的值！它會**阻止滾動穿透**，也就是滾動事件會被「抑制」在當前的元素內。 不過，元素本身的「邊界效果」還是會保留，例如在手機上滾到底部時，那個小小的「回彈」(bounce) 效果依然存在。
    
3. `none` 最強力的選項！它不但會**阻止滾動穿透**，還會**順便禁用元素本身的邊界效果**。 例如，在手機上那個「下拉重新整理」或「回彈」效果，通通都會被取消。
    

你也可以分開設定 X 軸和 Y 軸的行為，例如 `overscroll-behavior-x: none;` 或 `overscroll-behavior-y: contain;`。

---

## 三、實戰應用場景

說了這麼多，我們來看看實際怎麼用吧！

### 1\. 應用一：拯救你的對話框

這大概是 `overscroll-behavior` 最經典的應用了。  
我們只需要在一開始的這個範例中**可以滾動內容的那個對話框容器**上，加上一行 CSS：

```css
.scrollable {
    overflow: auto; /* 讓內容可以滾動 */
    overscroll-behavior: contain;  /* 加上這行 */
}
```

就可以了！現在當你在對話框裡的 `.scrollable` 裡面滾動時，就算滾到最頂部或最底部，背景的 `<html>` 也不會再跟著亂動了，他們兩者的滾動從此互不相關。

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1758265608655/4e0f2c2b-6f7f-4e3a-b567-a385005bc137.gif align="center")

> DEMO 連結：[Prevent Scroll Chaining With Overscroll Behavior](https://codepen.io/im1010ioio/pen/xbZxrVQ)

### 2\. 應用二：禁用手機上的「下拉重新整理」

在開發 Web App 或一些沉浸式體驗的網頁時，瀏覽器預設的「下拉重新整理」功能可能會變得很礙事。我們不希望使用者只是想滑動頁面，卻不小心觸發了整個頁面重載。

這時候，我們可以對整個頁面下手：

```css
html {
    overscroll-behavior-y: none;
}
```

把 `overscroll-behavior-y` 設定為 `none`，就可以有效地阻止手機瀏覽器預設的下拉重整手勢。

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1758267657831/dd76d01a-f243-4bd9-a3e9-1cd7c5ad5aa2.gif align="center")

> DEMO 連結：[Prevent Pull-to-Refresh on Mobile with Overscroll Behavior](https://codepen.io/im1010ioio/pen/YPwzrwL)

（請用手機打開 CodePen DEMO 的 Debug Mode 試試看）

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1758268445908/2484c0a4-acf3-4f89-ab8c-44eabd2fc770.png align="center")

---

`overscroll-behavior` 是一個非常實用且能大幅提升使用者體驗的 CSS 屬性。它解決了長久以來需要靠 JS 才能搞定的「滾動穿透」問題，讓我們能用更簡單的方式來管理網頁的滾動行為。

> 延伸閱讀：
> 
> * [MDN Web Docs: overscroll-behavior](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overscroll-behavior)
>     
> * [Prevent Scroll Chaining With Overscroll Behavior by Ahmad Shadeed](https://ishadeed.com/article/prevent-scroll-chaining-overscroll-behavior/)
>     
> * [CSS overscroll-behavior讓滾動嵌套時父滾動不觸發 by 張鑫旭](https://www.zhangxinxu.com/wordpress/2020/01/css-overscroll-behavior/)
>     
> * [【Day16】滾動條 - 避免滾動穿透 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10301290)
>     
> * [捲動行為 scroll-behavior、overscroll-behavior](https://steam.oxxostudio.tw/category/css/content/scroll-behavior.html#a2)
>     
> * [前端冷知识：overscroll-behavior滚动控制](https://juejin.cn/post/6973451851702206501)
>     

---

#### ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

感謝看到最後的你，若你覺得獲益良多，請不要吝嗇給我按個喜歡。❤️

如果你喜歡我的創作，還想看看其他有趣的分享與日常，  
可以追蹤我的 IG [@im1010ioio](https://www.instagram.com/im1010ioio/)，或者是[🧋送杯珍奶鼓勵我](https://im1010ioio.bobaboba.me/)，謝謝你🥰。

![Eva Chen 送杯珍奶鼓勵我](https://cdn.hashnode.com/res/hashnode/image/upload/v1682564435616/c15640fc-6cee-4c33-a898-9cd6820f165a.png align="center")