---
title: "#86 SASS/SCSS (5) Extend"
datePublished: Tue Oct 07 2025 15:51:59 GMT+0000 (Coordinated Universal Time)
cuid: cmggqms42000002jvdrbt79ej
slug: scss-extend
tags: css3, css, sass, scss, extend

---

今天，我們要來學一個和昨天說的 `@mixin` 很像，但運作原理和使用情境卻截然不同的 SCSS 功能：`@extend`。

---

## 一、`@extend` 是什麼？和 `@mixin` 有何不同？

`@extend`，字面上的意思是「擴展、延伸」。

它的核心概念是「繼承」，讓一個選擇器可以**繼承**另一個選擇器所有的樣式規則，並將自己「合併」到被繼承的選擇器群組中。

這聽起來跟 `@mixin` 有點像，都是在重用樣式碼。  
但它們最大的差別在於**編譯出來的 CSS 結果**：

* `@mixin` 是**複製貼上**：它會把樣式碼完整地複製一份到每一個 `@include` 的地方。
    
* `@extend` 是**群組歸類**：它不會複製樣式碼，而是把繼承者（例如 `.b`）加到被繼承者（例如 `.a`）的選擇器列表中，讓它們共享同一份樣式碼。
    

---

## 二、如何使用？

`@extend` 主要有兩種使用方式：繼承一個「佔位符選擇器 (%)」或繼承一個「現有的 class」。

### 1\. 推薦作法：使用「`%` - 佔位符選擇器 (Placeholder Selector)」

在實務上，我們**最推薦**搭配 `%` 符號來使用 `@extend`。以 `%` 開頭的選擇器被稱為「佔位符」，它有一個很棒的特性：**它本身不會被編譯成任何 CSS，除非有其他選擇器去** `@extend` 它。

這代表這個樣式存在的目的**純粹是為了被繼承而存在**，能讓我們的 CSS 保持得非常乾淨。

#### **(1) SCSS 範例：訊息提示框**

假設我們有成功、警告、錯誤三種提示框，它們都有共同的基本樣式，但這些基本樣式本身不會單獨使用在 HTML 中。

```scss
// SCSS
// 用佔位符定義共用樣式
%alert-base {
    padding: 15px;
    margin-bottom: 20px;
    border: 1px solid transparent;
    border-radius: 4px;
}

// 讓各種提示框去 @extend 這個基本樣式
.alert-success {
    @extend %alert-base;
    background-color: #d4edda;
    color: #155724;
}

.alert-warning {
    @extend %alert-base;
    background-color: #fff3cd;
    color: #856404;
}
```

#### **(2) 編譯結果**

`%alert-base` 本身消失了，所有繼承它的 class 被群組在一起，共用一份樣式。

```css
/* CSS */
.alert-success, .alert-warning {
    padding: 15px;
    margin-bottom: 20px;
    border: 1px solid transparent;
    border-radius: 4px;
}

.alert-success {
    background-color: #d4edda;
    color: #155724;
}

.alert-warning {
    background-color: #fff3cd;
    color: #856404;
}
```

### 2\. 直接繼承現有的 `.class`

有時候，我們想繼承的對象本身就是一個會**直接在 HTML 中使用**的 class。在這種情況下，我們就可以直接 `@extend` 一個 class。

**範例：按鈕樣式** 假設我們有一個基礎按鈕 `.btn`，同時也有一些特定變化的按鈕如 `.btn-primary` 和 `.btn-danger`。我們希望 HTML 中可以同時使用 `<button class="btn">` 和 `<button class="btn-primary">`。

#### **(1) SCSS 範例：按鈕樣式**

```scss
// SCSS
// 定義一個基礎的 .btn class，它會被直接使用
.btn {
    display: inline-block;
    padding: 6px 12px;
    font-size: 14px;
    text-align: center;
    border-radius: 4px;
    cursor: pointer;
}

// .btn-primary 繼承 .btn 的所有樣式，並加上自己的顏色
.btn-primary {
    @extend .btn;
    background-color: #337ab7;
    color: white;
    border: 1px solid #2e6da4;
}

// .btn-danger 也繼承 .btn 的所有樣式
.btn-danger {
    @extend .btn;
    background-color: #d9534f;
    color: white;
    border: 1px solid #d43f3a;
}
```

#### **(2) 編譯結果**

注意，`.btn` 本身被保留下來了，而繼承它的 class 也被加入到選擇器群組中。

```css
/* CSS */
.btn, .btn-primary, .btn-danger {
    display: inline-block;
    padding: 6px 12px;
    font-size: 14px;
    text-align: center;
    border-radius: 4px;
    cursor: pointer;
}

.btn-primary {
    background-color: #337ab7;
    color: white;
    border: 1px solid #2e6da4;
}

.btn-danger {
    background-color: #d9534f;
    color: white;
    border: 1px solid #d43f3a;
}
```

用 `@extend` 能讓 CSS 非常乾淨、符合 DRY (Don't Repeat Yourself) 原則，檔案大小也會更小。

---

## 三、`@mixin` vs. `@extend`，該用哪個？

這的確是 SCSS 語法讓人容易混淆的地方，我們在這裡整理了一下他們的特性，先釐清清楚：

### 1\. 他們的特性

| 特性 | `@mixin` | `@extend` |
| --- | --- | --- |
| **核心概念** | **複製**樣式 (像 Function) | **繼承**樣式 (像群組) |
| **適用情境** | 1\. 需要傳入**引數 (Arguments)** 的動態樣式。 | 1\. 元素之間有明確的**群組**關係。(例如：`.btn-primary` 是一種 `.btn`) |
|  | 2\. 只是單純想重複貼上，元素之間沒有語意關聯。 | 2\. 整包樣式都要一樣的時候，減少重複的行為，想要產生更簡潔的 CSS 結果。 |
|  | 3\. **RWD 斷點**管理 |  |
| **編譯結果** | 在每個 `@include` 的地方產生**重複的**樣式碼。 | 將選擇器**合併**在一起，共享同一份樣式碼。 |

### 2\. 判斷法則

接下來，提供給大家一些簡單的判斷方法：

* **你需要傳遞參數嗎？**
    
    * **是** → 毫無疑問，請使用 `@mixin`。
        
    * **否** → 繼續問下一個問題。
        
* **這些元素之間，是否存在語意上的關聯？**
    
    * **是** (例如各種按鈕 `.btn-primary`, `.btn-secondary` 都繼承自 `.btn-base`)  
        → 使用 `@extend` 是個好主意。
        
    * **否** (例如 `.header` 和 `.footer` 剛好都有一個清除浮動的樣式，但它們本身毫無關聯)  
        → 使用 `@mixin` 更合適，避免產生無意義的選擇器群組，否則之後會想不起來他們彼此的關聯，造成管理樣式的困難。
        
* **這個被繼承的樣式，會單獨出現在 HTML 中嗎？**
    
    * **會** (例如 `<button class="btn">`)  
        → 使用 `@extend .class`。
        
    * **不會** (樣式純粹是為了被繼承)  
        → **強烈建議**使用 `@extend %placeholder`，以保持 CSS 乾淨。
        

---

## 四、使用 @extend 的注意事項

`@extend` 非常強大，但也是一把雙面刃，需要小心使用。如果濫用，尤其是在複雜的專案中，可能會導致 CSS 輸出結果不如預期，產生所謂「又長又難懂的選擇器鍊」，反而造成後續維護的災難。

### 1\. 又長又難懂的選擇器鍊

這個問題的根源在於 `@extend` 的運作方式：**它會將繼承者的整個「巢狀結構」複製到被繼承者的選擇器列表中**。

讓我們來看一個具體的例子：

假設我們有一個通用的樣式 `.box`，它被定義在一個巢狀結構裡：

```scss
// SCSS
.sidebar {
    .widget {
        .box { // 這是我們想繼承的樣式
            border: 1px solid #ccc;
            padding: 10px;
        }
    }
}
```

現在，我們在另一個完全不相干的地方，例如文章內容區塊 `.main-content`，有一個 `.info-panel` 想要繼承 `.box` 的樣式：

```scss
// SCSS
.main-content {
    .article {
        .info-panel {
            @extend .box; // 試圖繼承 .box
            background-color: #f0f0f0;
        }
    }
}
```

**實際的編譯結果會是這樣：**

```css
/* 實際編譯結果 */
.sidebar .widget .box,
.sidebar .widget .main-content .article .info-panel { /* 注意看！這就是選擇器鍊 */
    border: 1px solid #ccc;
    padding: 10px;
}

.main-content .article .info-panel {
    background-color: #f0f0f0;
}
```

Sass 為了確保 `.info-panel` 在繼承 `.box` 樣式的同時，也保有它原本的巢狀情境，它將 `.main-content .article .info-panel` **整個**貼到了 `.sidebar .widget` 後面，形成了一個超長的選擇器群組：`.sidebar .widget .main-content .article .info-panel`。

這個選擇器鍊不僅**毫無語意**（側邊欄的元件跟主要內容的文章面板根本沒關係），而且**提高了權重**，未來若想覆蓋樣式會變得更加困難。當專案變大、跨檔案繼承的情況變多時，這種意想不到的選擇器群組會大量產生，讓 CSS 檔案變得肥大且極難 Debug。

### 2\. 安全地使用 `@extend`

為了避免以上的災難，請依循以下原則：

1. **優先使用「佔位符選擇器 (**`%`**)」**：  
    將所有只為了被繼承而存在的共用樣式都定義成佔位符（例如 `%alert-base`）。因為佔位符本身不會產生任何 CSS，Sass 在處理繼承時就能產生最乾淨、最沒有意外的群組選擇器。
    
2. **避免繼承巢狀結構中的選擇器**：  
    盡量只讓 `@extend` 用在那些位於頂層、沒有被包在其他選擇器裡的 class 或 placeholder。  
    如果你發現你想繼承的樣式被深埋在好幾層的巢狀結構裡，那通常意味著這個樣式應該被抽出來，獨立成一個可重複使用的 placeholder。
    
3. **限制** `@extend` 的使用範圍：  
    盡量將 `@extend` 的關係限制在同一個功能模組或檔案內。  
    跨越好幾個檔案去繼承一個定義在很遠地方的樣式，也會大幅增加追蹤和維護的難度。
    

---

`@extend` 是專門為**樣式、語意上的繼承關係**而生的，而 `@mixin` 則是個靈活的、可傳遞參數的**樣式果汁機**。只要好好了解它們的本質，依據他們的本質而運用，就能讓你的 CSS 乾淨又好維護喔！

---

#### ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

感謝看到最後的你，若你覺得獲益良多，請不要吝嗇給我按個喜歡。❤️

如果你喜歡我的創作，還想看看其他有趣的分享與日常，  
可以追蹤我的 IG [@im1010ioio](https://www.instagram.com/im1010ioio/)，或者是[🧋送杯珍奶鼓勵我](https://im1010ioio.bobaboba.me/)，謝謝你🥰。

![Eva Chen 送杯珍奶鼓勵我](https://cdn.hashnode.com/res/hashnode/image/upload/v1682564435616/c15640fc-6cee-4c33-a898-9cd6820f165a.png align="left")