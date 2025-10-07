---
title: "#87 Sass/scss (6) 運算符號（加減乘除）"
datePublished: Tue Oct 07 2025 17:27:54 GMT+0000 (Coordinated Universal Time)
cuid: cmggu24pw000702lbfvq92mhy
slug: scss-operators
tags: css3, css, sass, scss, operators

---

今天，我們要來探索 SCSS 中一個非常實用的功能：**運算符號 (Operators)**，也就是加減乘除與取餘數，讓你可以在 CSS 裡面算數。

---

## 一、運算符號語法

### 1\. SCSS 數學運算符號

SCSS 支援標準的數學運算符號：

* 加法：`+`
    
* 減法：`-`
    
* 乘法：`*`
    
* 除法：`math.div()` 或 `/`
    
* 取餘數：`%`
    

---

## 二、加法 `+` 與減法 `-`

這兩個最簡單，直接對擁有相同單位的數值進行運算就可以了。最常用來微調尺寸。

```scss
// SCSS
$base-font-size: 16px;
$base-padding: 20px;

h1 {
    font-size: $base-font-size + 10px; // 16px + 10px
}

.container {
    padding: $base-padding - 5px; // 20px - 5px
}
```

**編譯後的 CSS：**

```css
/* CSS */
h1 {
    font-size: 26px;
}

.container {
    padding: 15px;
}
```

---

## 三、乘法 `*`

乘法也非常實用，例如計算一個元素的寬度是另一個的好幾倍。

```scss
// SCSS
$base-width: 200px;

.sidebar {
    width: $base-width;
}

.main-content {
    width: $base-width * 3; // 200px * 3
}
```

**編譯後的 CSS：**

```css
/* CSS */
.sidebar {
    width: 200px;
}

.main-content {
    width: 600px;
}
```

---

## 四、除法 `/` 與 `math.div()`

根據[最新的 Sass 官方標準](https://sass-lang.com/documentation/breaking-changes/slash-div/)，除法的寫法 `/` 已經有了重大的改變。

過去，`100px / 2` 在 SCSS 中可以得到 `50px`。

但是，後來原生的 CSS 語法也有用到 `/` 這個符號，作為「分隔符」，例如：

* `font: 16px / 1.5;` (字體大小/行高) 、
    
* `grid-row: 1 / 3;` (排版)、
    
* `rgb(86 114 180 / 56%)` (顏色)、
    
* `aspect-ratio: 1 / 1;` (長寬比) 等等語法都會用到。
    

這樣就語法衝突了，Sass 編譯時會不知道 `/` 何時應該作為分隔符，何時該作為除法。

### 現在標準寫法：`math.div()`

為了解決這個問題，Sass 團隊推出了全新的寫法：  
使用內建的 `math.div()` 函式來進行除法運算，他的的使用步驟是：

1. 在檔案最上方，使用 `@use` 引入 Sass 的內建數學模組 `sass:math`。
    
2. 使用 `math.div(被除數, 除數)` 的語法來計算。
    

```scss
// SCSS
@use "sass:math"; // 一定要先引入

.container {
    width: 100%;
}

.column {
    // 將 100% 寬度均分成 4 欄
    width: math.div(100%, 4); // 步驟二：使用 math.div()
}
```

**編譯後的 CSS：**

```css
/* CSS */
.container {
    width: 100%;
}

.column {
    width: 25%;
}
```

雖然舊的 `/` 寫法在某些舊的編譯環境下可能還能運作（並跳出警告），但它已經被官方棄用，**未來隨時可能被完全移除**。因此，從現在開始，**所有除法運算都請務必使用** `math.div()`。

---

## 五、取餘數 `%`

「取餘數 `%` (Modulo) 」會回傳兩個數值相除後的「餘數」。這個功能在需要處理重複性規律的版面時特別有用，例如，讓表格或列表中的每 N 個項目有不同的樣式。

舉例來說，`10 % 3` 的結果是 `1`，因為 10 除以 3 等於 3，餘數為 1。

假設我們有一個項目列表，希望每第三個項目 (`.item`) 都沒有右邊距，以實現完美的換行對齊。  
我們可以使用 CSS 的 `:nth-child(3n)` 選擇器來達成目標：

```scss
// SCSS
.item {
    float: left;
    width: 30%;
    margin-right: 5%;
    // 3 的倍數
    &:nth-child(3n) {
        margin-right: 0;
    }
}
```

但是我們也可以使用取餘數（`%`）的方式來達成，搭配 `@for` 迴圈來產生樣式，並用 `%` 判斷：

```scss
// SCSS
// 假設有 10 個項目
@for $i from 1 through 10 {
    .item-#{$i} {
        // $i 除以 3 的餘數為 0，就表示為 3 的倍數
        @if $i % 3 == 0 {
            margin-right: 0;
        } @else {
            margin-right: 5%;
        }
    }
}
```

Sass 的 `%` 取餘數通常會與 `@if` 或 `@for` 等邏輯結合，用來處理更複雜的判斷。Sass 的 `@if` 與 `@for` 在接下來才會介紹，目前只需要先知道大概會用在哪裡就好。

---

## 六、關於「單位」的注意事項

在使用運算符時，Sass 對單位有嚴格的規定：

* **加減法** `+` `-` **與取餘數** `%`**：**  
    兩個數值的單位必須是**相容的** (例如 `px` 和 `px`，`rem` 和 `em`)。  
    你不能把 `10px` 和 `2rem` 直接相加，這樣會報錯，如果一定需要這麼做，請使用原生 CSS `calc()` 語法。
    
* **乘法** `*`**：**  
    至少要有一個數值是**沒有單位**的。  
    你不能計算 `10px * 2px`，因為 `20px²` (平方像素) 是沒有意義的 CSS 單位，要寫成 `10px * 2`。
    
* **除法** `math.div()`**：**  
    如果**兩個數值都有單位，單位會被除掉**，例如 `math.div(100px, 10px)`，會得到一個沒有單位的數值 `10`。
    

---

## 七、SCSS 運算 vs. 原生 CSS `calc()`

既然 CSS 自己也有 `calc()` 函式可以計算，那為什麼還需要 SCSS 的運算符呢？  
它們的用途和運作方式有根本上的不同。

### 1\. **SCSS 運算符**

編譯時計算 SCSS 的 `+`, `-`, `*`, `math.div()` 等運算，都是在「編譯」階段完成的。也就是說，當你儲存 `.scss` 檔案時，Sass 編譯器會立即算出結果，然後將一個固定的數值寫入最終的 `.css` 檔案中。

這適合用在計算的東西是專案中預先定義好的靜態變數。

非常適合用來建立設計系統的規範，例如：搭配邏輯判斷，根據 `$base-font-size` 計算出 `<h1>` 的字體大小、或根據 `$base-spacing` 計算元件的 `padding` 和 `margin`。

目的是為了**方便維護和管理**。

### 2\. **CSS** `calc()`

瀏覽器即時運算 `calc()` 則是 CSS 的原生功能，它的計算工作是由「瀏覽器」在渲染畫面時即時處理的。

這讓 `calc()` 能夠執行 SCSS 運算符做不到的動態計算，特別是混合不同的單位，像是：混合了相對單位 (如 `%, vw`) 和絕對單位 (如 `px, rem`)。

這在 RWD 中非常常用到，例如設定一個側邊欄，讓它的高度永遠是「視窗高度減去 `<header>` 的高度」。

```scss
// SCSS
$header-height: 80px;

.sidebar {
    // SCSS 無法計算 `100dvh - 80px`
    // 必須使用 calc() 交給瀏覽器處理
    height: calc(100dvh - #{$header-height});
}
```

> **注意：** 在 SCSS 中使用 `calc()` 時，如果內部要嵌入 Sass 變數，要使用 `#{}` (插值) 的語法將變數包起來，才能讓 Sass 能正確輸出 CSS 的 `calc()`。

SCSS 運算符號是開發時的工具，方便管理維護 Code 用；而 `calc()` 則是讓瀏覽器在渲染時有調整樣式的能力。  
這兩者其實是可以相輔相成的，就等待你來練習寫寫看囉！

---

#### ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

感謝看到最後的你，若你覺得獲益良多，請不要吝嗇給我按個喜歡。❤️

如果你喜歡我的創作，還想看看其他有趣的分享與日常，  
可以追蹤我的 IG [@im1010ioio](https://www.instagram.com/im1010ioio/)，或者是[🧋送杯珍奶鼓勵我](https://im1010ioio.bobaboba.me/)，謝謝你🥰。

![Eva Chen 送杯珍奶鼓勵我](https://cdn.hashnode.com/res/hashnode/image/upload/v1682564435616/c15640fc-6cee-4c33-a898-9cd6820f165a.png align="left")