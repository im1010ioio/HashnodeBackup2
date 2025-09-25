---
title: "#74 CSS 也能控制作用域了！@scope 語法"
datePublished: Thu Sep 25 2025 15:54:32 GMT+0000 (Coordinated Universal Time)
cuid: cmfzlfu7b000102l8fjon2i9z
slug: css-at-scope
tags: css3, css, scope, css-scoping, css-scope

---

當 CSS 越寫越龐大，樣式一不小心就互相打架了，怎麼辦呢？今天，我們來學一個 CSS 的新功能：`@scope`，它就像是為你的 CSS 加上了結界，讓你能更精準地控制樣式的作用範圍，告別「樣式污染」。

`@scope` 的優勢在 CSS 整體的管理上，只用簡單 DEMO 有點無法呈現，所以這篇先沒有寫 DEMO 喔！

> 註：不過 Firefox 暫不支援，但是等全面支援後會很實用，我覺得很值得期待，可以先學起來。  
> 各瀏覽器詳細支援度請看 [Can I Use](https://caniuse.com/?search=scope)。

---

## 一、 `@scope` 是什麼？

`@scope` 可以讓你把樣式**限定在特定的 HTML DOM 區域**内。

過去，為了避免樣式互相干擾，我們可能會用很長的選擇器 (例如 `.article-card .content .author-name`)，或是像 BEM 這樣很長的命名法（例如 `.menu__item--active`），這樣不僅很難讀，也容易讓 CSS 和 HTML 結構綁得太緊。

而 `@scope` 提供了一個原生的解決方案，讓樣式管理變得更直覺、更模組化。

---

## 二、`@scope` 的基本用法

### 1\. 語法

`@scope` 最基本的語法很簡單，就是告訴瀏覽器： 「接下來的樣式，只在『**這個選擇器**』裡生效」。

```css
@scope (選擇器) {
    ...
}
```

### 2\. 範例：只為特定區塊的文章標題上色

假設我們有兩個區塊，都包含 `<h2>` 標題，但我們只想讓 `.article-block` 裡的 `<h2>` 變成藍色。

**HTML：**

```html
<div class="article-block">
    <h2>這是一個文章標題</h2>
    <p>文章內文...</p>
</div>

<div class="sidebar-block">
    <h2>這是側邊欄標題</h2>
    <ul>
        <li>項目一</li>
        <li>項目二</li>
    </ul>
</div>
```

**CSS：**

```css
@scope (.article-block) {
    h2 {
        color: steelblue;
    }
}
```

在這個例子中，只有 class 為 `.article-block` 的 `<div>` 裡面的 `<h2>` 會變成藍色，側邊欄的 `<h2>` 則完全不受影響。

### 3\. 為什麼比較好？

看到這裡，你可能會想：  
「這跟直接寫 `.article-block h2 { color: steelblue; }` 有什麼不一樣？」

在這個簡單的例子中，視覺上的結果確實完全相同。 但 `@scope` 的優點在它更精準、更穩定，解決了傳統寫法的幾個痛點：

1. **防止樣式污染**：  
    傳統的 `.article-block h2` 會選取到 `.article-block` 內部**所有**的 `h2`，不管它被包了多少層。如果文章區塊內又放了另一個有 `h2` 的元件，樣式就會「**污染**」進去，造成非預期的結果。  
    `@scope` 則將樣式嚴格限制在一個範圍內，我們在下個段落會學到的「甜甜圈作用域」更能體現這個優勢。
    
2. **降低選擇器權重**：  
    `@scope` 本身不增加權重，所以區塊內的 `h2` 其權重就是一個單純元素選擇器的權重。而 `.article-block h2` 的權重則是一個 class 加上一個元素。在大型專案中，使用 `@scope` 可以避免不必要的權重疊加，減少「權重戰爭 (specificity wars)」，讓樣式更容易被覆蓋和管理。
    
3. **減少與 HTML 結構的耦合**：  
    `@scope` 關注的是一個範圍，而不是嚴格的父子或祖先後代關係，這讓我們的 CSS 更有彈性，當 HTML 結構需要調整時，樣式比較不容易失效。
    

---

## 三、進階用法：甜甜圈般的精準控制 (Donut Scope)

`@scope` 還有一個語法是 `to`，可以讓你設定樣式的「終點」，也就是排除掉範圍內的某個區塊。 形成了一個「中間有空洞」的範圍，所以也被稱為「甜甜圈作用域 (Donut Scope)」。

### 1\. 語法

```css
@scope (起點) to (終點) {
    ...
}
```

### 2\. 範例：設定文章段落樣式，但排除「引言區塊」

延續剛才的例子，假設我們想設定文章區塊內所有段落 `<p>` 的樣式，但文章裡可能會有一個特殊的「引言區塊」 `.quote-block`，我們不希望它的樣式被影響。

**HTML：**

```html
<div class="article-block">
    <h2>這是一個文章標題</h2>
    <p>文章內文段落一...</p>
    <p>文章內文段落二...</p>

    <div class="quote-block">
        <p>這是一段引言，我們希望它有自己的特殊樣式，不受文章樣式影響。</p>
    </div>
</div>
```

**CSS：**

```css
@scope (.article-block) to (.quote-block) {
    p {
        line-height: 1.8;
        color: #333;
    }
}

/* quote-block 的獨立樣式 */
.quote-block p {
    font-style: italic;
    color: #555;
    border-left: 3px solid steelblue;
    padding-left: 1rem;
}
```

**結果：** `.article-block` 內的兩個主要段落會套用 `line-height` 和 `color`，但在 `.quote-block` 被排除了，完全不受干擾。

### 3\. 為什麼比較好？

比方說，`.quote-block` 可能在很多地方都有，不只是在 `.article-block` 內，也可能出現在 `header` 之類的地方，我們又希望 `.quote-block` 的樣式只寫一次就好。

過去的做法，為了讓他不被影響，可能要：

* 把 `.quote-block` 寫在 `.article-block` 和 `header` 後面，
    
* 把被 `.article-block` 和 `header` 影響的 CSS 屬性，在 `.quote-block` 內再寫一次覆蓋過去。
    

過去這樣子的開發方式，會非常容易改 A 錯 B，改 B 錯 A，而且徒增了許多只為了覆蓋而寫的樣式，專案一大、時間一久，CSS 就會變得髒髒的。

有了 `@scope` 後就能讓各自的區域屬於自己，有想要要排除的也可以很方便地排除。

---

## 四、作用域鄰近性 (Scoping Proximity)

`@scope` 為 CSS 權重計算帶來了一個新規則：「作用域鄰近性」。 簡單來說，當兩個作用域的樣式衝突時，DOM 結構中離目標元素「比較近」的那個作用域會勝出，這比傳統的原始碼順序更直覺。

### 範例：文章區塊內巢狀的主題切換

假設我們的 `.article-block` 預設是淺色主題，但我們可以在文章內放入一個深色主題的元件 `.dark-theme-component` 來強調內容。

**HTML：**

```html
<div class="article-block theme-light">
    <h2>這是一個文章標題</h2>
    <p>這段文字在淺色主題區塊中。</p>

    <div class="dark-theme-component">
        <p>這段文字被包在深色主題元件裡。</p>

        <div class="theme-light">
            <p>最內層的文字，又回到了淺色主題。</p>
        </div>
    </div>
</div>
```

**CSS：**

```css
@scope (.theme-light) {
    p {
        color: black;
        background-color: #f0f0f0; /* 淺灰色背景 */
        padding: 0.5rem;
    }
}

@scope (.dark-theme-component) {
    p {
        color: white;
        background-color: #222; /* 深灰色背景 */
        padding: 0.5rem;
    }
}
```

**結果：** 最內層的 `<p>` 會正確顯示為**淺灰底、黑字**。

即使它也被包在 `.dark-theme-component` 裡面，但因為最內層的 `<p>` 離它的直接父層 `.theme-light` 作用域根**更近**，所以淺色主題的樣式獲勝了，這就是「鄰近性」優先的原則。

---

## 五、方便的好幫手：`:scope` 虛擬類別

在 `@scope` 的區域內，你可以使用 `:scope` 來直接選取作用域的根元素本身，其中：

> * `:scope`：表示該作用域的「根元素」，類似整份 HTML 的 `:root`。
>     
> * `&`：表示巢狀結構中的「自己」，也就是 `@scope` 的選擇器本身，會增加全中。
>     

### 1\. 語法

```css
@scope (.element) {
    :scope p {
        ...
    }
    & p {
        ...
         /* 會增加權重，因為加上了 .element 的權重 */
    }
}
```

### 2\. 範例：為文章區塊本身加上樣式

回到我們最初的 DEMO，我們除了想改變 `<h2>` 的顏色，也想為 `.article-block` 這個容器本身加上背景和邊框。

**HTML：**

```html
<div class="article-block">
    <h2>這是一個文章標題</h2>
    <p>文章內文...</p>
</div>
```

**CSS：**

```css
@scope (.article-block) {
    :scope {
        background-color: #f9f9f9;
        border: 1px solid #ddd;
        padding: 1rem;
        border-radius: 8px;
    }
    h2 {
        color: steelblue;
        margin-top: 0; /* 順便把 h2 的預設上邊距拿掉 */
    }
}
```

### 3\. 為什麼比較好？

上面 `:scope` 的那段，跟直接寫 `.article-block { ... }`，視覺效果不是一樣嗎？

是的，視覺效果完全一樣，但 `@scope` 的寫法在**程式碼的組織與維護**上，是更好的選擇：

* **降低權重，避免權重之戰**：  
    在傳統寫法中，為了選取內部的 `<h2>`，你必須寫 `.article-block h2` ，這樣會增加這個 `<h2>` 的權重。  
    但在 `@scope` 寫法中，內部的 `<h2>`，其權重就**只是** `<h2>` **本身的權重。**  
    因為在大型專案中，樣式很容易互相覆蓋。低權重的樣式更容易被其他特定情境的樣式覆蓋，這給了你更大的彈性，能有效避免為了覆蓋一個樣式而寫出越來越長、權重越來越高的選擇器，最終陷入「權重之戰」。
    
* **更清楚的意圖**：  
    當你使用 `@scope`，你是在明確地宣告一個樣式邊界：「從這裡開始，到那裡結束，都是屬於 .article-block 這個元件的樣式範圍。」其他共同開發的人看到了後，也能夠很清楚知道專屬於某個元件的樣式，而不是一個可能影響全局的通用 class。
    

總之，`@scope` 鼓勵我們用「元件化」的思維來組織 CSS，將同一個元件所有相關的樣式都寫在一起，不用擔心他影響到其他的元素。

尤其像是 `.active`、`.open` 或 `.hide` 這種功能性的 Class，經常在很多元件裡都有，一旦被全域樣式污染了就會很麻煩，如果用了 `@scope` 寫法，就能確定它們只在自己的範圍內生效。

---

雖然 Firefox 尚未支援，但是非常實用，是個值得期待的 CSS 屬性！  
等 `@scope` 全面普及後，甚至可能會改變許多命名或架構方式。  
大家有空時可以來試試看這個寫法。