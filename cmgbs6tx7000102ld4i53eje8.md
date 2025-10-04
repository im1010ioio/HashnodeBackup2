---
title: "#83 SASS/SCSS (2) 變數 Variables / 巢狀寫法 Nesting"
datePublished: Sat Oct 04 2025 04:36:43 GMT+0000 (Coordinated Universal Time)
cuid: cmgbs6tx7000102ld4i53eje8
slug: scss-variables-nesting
tags: css3, css, sass, scss

---

上一篇我們已經順利設定好 SASS/SCSS 的開發環境了，這次，就讓我們正式來學習 SCSS 兩個最基本也最實用的語法：變數 (Variables) 和巢狀 (Nesting)。

雖然原生 CSS 現在已經有[變數](https://ithelp.ithome.com.tw/articles/10327133)和[巢狀](https://ithelp.ithome.com.tw/articles/10328353)寫法了，但是最一開始沒有的時候，都是使用預處理器這麼做的。而且他們還是有一點根本上的不同。

---

## 一、SCSS 變數 vs. CSS 自訂屬性

要理解它們如何協作，首先必須明白它們最核心的差異，這個差異就在於它們的「生命週期」：

| 特性 | SCSS 變數 (`$`) | CSS 自訂屬性 (`--`) |
| --- | --- | --- |
| **存在時間** | **編譯前 (Pre-compilation)** | **執行中 (Runtime)** |
| **存在地點** | 只存在於你的 `.scss` 檔案中 | 存在於瀏覽器讀取的最終 `.css` 檔案中 |
| **本質** | **靜態的**。一旦編譯，它的值就被寫死在 CSS 裡了。 | **動態的**。它是一個「活」在瀏覽器裡的變數，可以被 JS 即時讀取和修改。 |

**一個簡單的比喻：**

* **SCSS 變數 (**`$`) 就像一份**食譜**。你在廚房裡（開發環境）照著食譜把麵粉和糖（`$width`, `$color`）混合好，烤出一個蛋糕（編譯出 `style.css`）。客人拿到蛋糕時，已經吃不出麵粉和糖原本的樣子了。
    
* **CSS 自訂屬性 (**`--`) 就像餐廳菜單上的**客製化選項**。菜單上寫著「牛排熟度：`--steak-doneness`」。你可以告訴服務生（JS）你想要「五分熟」，廚房（瀏覽器）就會即時為你烹飪。
    

所以 SCSS 變數適合存放那些**專案中固定不變、不需要在執行階段改變**的基礎值，例如柵格系統的欄數、基本的間距單位等。CSS 變數適合用在那些**需要在執行階段改變**的值（主要是顏色、字體等主題相關屬性）定義為 CSS 自訂屬性。

至於原生 CSS 變數如何與 SCSS 語法搭配，我們之後再詳細說明，這篇先簡單介紹寫法。

---

## 二、SCSS 變數 (Variables) 寫法

在 SCSS 中，你可以用 `$` 符號來宣告一個變數，把常用的值存起來。 之後只要需要用到這個值的地方，都用這個變數來代替。未來當你需要修改時，只要改動宣告變數的那一行，所有用到這個變數的地方就都會一起更新，是不是超方便的！

### **1\. 宣告變數：**

語法很簡單，就是 `$` 加上你的變數名稱，後面接一個冒號 `:`，再加上你想要儲存的值。

```scss
// SCSS
$primary-color: #3498db;
$font-size: 16px;
$border-style: 1px solid #ccc;
```

### **2\. 使用變數：**

在需要的地方，直接使用變數名稱就可以了。

```scss
// SCSS
$primary-color: #3498db;
$font-size: 16px;

body {
    font-size: $font-size;
    color: $primary-color;
}

.button {
    background-color: $primary-color;
    font-size: $font-size;
}
```

### **3\. 編譯後的 CSS：**

當你存檔後，Live Sass Compiler 會自動幫你把 SCSS 編譯成瀏覽器看得懂的 CSS，變數會被替換成實際的值。

```css
/* CSS */
body {
    font-size: 16px;
    color: #3498db;
}

.button {
    background-color: #3498db;
    font-size: 16px;
}
```

---

## 三、巢狀 (Nesting) 寫法

跟原生 CSS 一樣 SCSS 也可以有「巢狀」的寫法。過去沒有辦法用巢狀寫法時，通常需要重複寫很多次父層的選擇器，像這樣：

```css
.header {
    width: 100%;
}

.header .logo {
    max-width: 150px;
}

.header .nav {
    list-style: none;
}

.header .nav li {
    display: inline-block;
}

.header .nav li a {
    text-decoration: none;
    color: #333;
}
```

當專案變很大時，這樣的寫法會讓程式碼看起來很分散，而且不易閱讀。

SCSS 的巢狀語法允許我們把子選擇器的樣式，直接寫在父選擇器的樣式裡面，同時對應到 HTML 的層級結構，讓程式碼看起來更直覺、更有結構，也會更好找到 Code。

### **1\. SCSS 巢狀寫法：**

```scss
// SCSS
.header {
    width: 100%;

    .logo {
        max-width: 150px;
    }

    .nav {
        list-style: none;

        li {
            display: inline-block;
        }
    }
}
```

這樣一來，編譯出來的結果會跟我們剛剛 CSS 完全一樣。

是不是看起來清爽多了呢？這種寫法不僅減少了重複的程式碼，也讓整個樣式的結構一目了然。

### 2\. 使用 `&` 參照父選擇器

在巢狀結構中，跟原生 CSS 一樣，`&` 這個符號代表的是「父選擇器」。

```scss
// SCSS
.button {
    padding: 10px 20px;
    background-color: #3498db;
    color: white;

    &:hover {
        background-color: #2980b9;
    }

    &::before {
        content: "🚀";
    }
}
```

**編譯後的 CSS：**`&` 會被父選擇器 `.button` 所取代。

```css
/* CSS */
.button {
    padding: 10px 20px;
    background-color: #3498db;
    color: white;
}

.button:hover {
    background-color: #2980b9;
}

.button::before {
    content: "🚀";
}
```

今天我們學會了 SCSS 中最重要的兩個基礎語法：變數和巢狀寫法，快用昨天開的專案來試試看吧！

---

#### ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

感謝看到最後的你，若你覺得獲益良多，請不要吝嗇給我按個喜歡。❤️

如果你喜歡我的創作，還想看看其他有趣的分享與日常，  
可以追蹤我的 IG [@im1010ioio](https://www.instagram.com/im1010ioio/)，或者是[🧋送杯珍奶鼓勵我](https://im1010ioio.bobaboba.me/)，謝謝你🥰。

![Eva Chen 送杯珍奶鼓勵我](https://cdn.hashnode.com/res/hashnode/image/upload/v1682564435616/c15640fc-6cee-4c33-a898-9cd6820f165a.png align="left")