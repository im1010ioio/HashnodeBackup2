---
title: "#73 CSS :nth-child 的新寫法：:nth-child of S (N of Selector)"
datePublished: Wed Sep 24 2025 15:39:03 GMT+0000 (Coordinated Universal Time)
cuid: cmfy5g37l000002ldhrt7c54s
slug: css-nth-child-of-s
tags: css3, css, css-nth-child-selectors

---

在寫 CSS 時，你是否也曾對 `:nth-child` 又愛又恨呢？它雖然強大，但有時在較複雜的 HTML 結構中，要選到對的東西，有時就只差那麼一點點…

為了解決這問題 CSS 出了一個超實用的新語法 `of S`，讓 `:nth-child` 和 `:nth-last-child` 的選取變得更直覺、更有效率！

---

## 一、`of S` 語法是什麼？

簡單來說，`of S` 語法允許你在計算元素位置之前，**先過濾出你想要的特定元素**。  
這裡的 `S` 指的是選擇器（selector），像是 class、ID 或屬性選擇器等等。

它的基本語法長這樣：

* `:nth-child(An+B of S)`
    
* `:nth-last-child(An+B of S)`
    

其中 `An+B` 是我們熟悉的計數規則（例如 `2n+1` 代表選取奇數位置的元素），  
而 `of S` 則是在這個計數開始前，先篩選出符合 `S` 條件的元素。

---

## **二、**`of S` 實際應用

直接看個例子，比較快懂。  
假設我們有一個列表，其中有些項目被標示為 `.highlight`，而我們想選取**第二個**帶有 `.highlight` 類別的項目。

傳統寫法可能會很頭痛，可能會需要寫複雜的選擇器組合，甚至動用到 JS。  
但是有了 `of S`，一切都變簡單了，比方說：

**HTML**

```html
<ul>
    <li>項目 1</li>
    <li>項目 2</li>
    <li class="highlight">項目 3 (第一個 highlight)</li>
    <li>項目 4</li>
    <li class="highlight">項目 5 (第二個 highlight)</li>
    <li>項目 6</li>
    <li class="highlight">項目 7 (第三個 highlight)</li>
</ul>
```

**CSS**

```css
/* 選取所有 <li> 中，第二個 .highlight Class */
li:nth-child(2 of .highlight) {
    background-color: gold;
}
```

在這個例子中，`:nth-child(2 of .highlight)` 的意思是：

1. 先找出所有帶有 `.highlight` 類別的 `<li>` 元素
    
2. 在這些被篩選出來的元素中，**再選取第二個**
    

就是這麼簡單！

---

## 三、`of S` 和傳統 `nth-child` 有什麼不同？

你可能會好奇：

* `li.highlight:nth-child(2)` 和
    
* `li:nth-child(2 of .highlight)`
    

有什麼不一樣呢？

讓我們來仔細看一下語法：

* `li.highlight:nth-child(2)` 選取的是：  
    「同時是父層的**第二個**子元素，**並且**帶有 `.highlight` Class 的 `<li>`」，  
    在上面的例子中，它會選不到東西，因為父層 `<ul>` 的第二個小孩，**剛好沒有** `.highlight` **Class**。
    
* `li:nth-child(2 of .highlight)` 選取的是：  
    「在所有帶有 `.highlight` 類別的子元素中，排在**第二個**」的 `<li>`」，  
    在上面的例子中，它會選到「項目 5」，因為他是第二個 `.highlight`。
    

實際效果可到 CodePen 上試試看：

> DEMO 連結：[CSS :nth-child of S](https://codepen.io/im1010ioio/pen/pvgjbxR)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1758728246291/dd4d9d92-4d8e-47e7-8144-511f8b31b231.png align="center")

---

`of S` 語法真的是 `:nth-child` 的一大升級，讓我們在處理複雜的 HTML DOM 結構時，能夠用更簡潔、更語意化的方式選取元素。下次當你需要從特定類型的元素中挑出第 N 個時，不妨試試這個方便的新語法吧！

`of S` 目前全部主流的瀏覽器都已支援，可以安心地用（詳情請看 [Can I Use](https://caniuse.com/css-nth-child-of)）。

> 延伸閱讀：
> 
> * [Chrome for developers - 使用 S 語法進一步控管 :nth-child() 選項](https://developer.chrome.com/docs/css-ui/css-nth-child-of-s?hl=zh-tw)
>     

---

#### ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

感謝看到最後的你，若你覺得獲益良多，請不要吝嗇給我按個喜歡。❤️

如果你喜歡我的創作，還想看看其他有趣的分享與日常，  
可以追蹤我的 IG [@im1010ioio](https://www.instagram.com/im1010ioio/)，或者是[🧋送杯珍奶鼓勵我](https://im1010ioio.bobaboba.me/)，謝謝你🥰。

![Eva Chen 送杯珍奶鼓勵我](https://cdn.hashnode.com/res/hashnode/image/upload/v1682564435616/c15640fc-6cee-4c33-a898-9cd6820f165a.png align="left")