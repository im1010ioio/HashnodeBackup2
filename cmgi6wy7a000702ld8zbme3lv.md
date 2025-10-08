---
title: "#88 SASS/SCSS (7) sass:color 顏色模組——color.adjust 與 color.scale 取代舊的 lighten() 和 darken()"
datePublished: Wed Oct 08 2025 16:15:33 GMT+0000 (Coordinated Universal Time)
cuid: cmgi6wy7a000702ld8zbme3lv
slug: scss-color-adjust-color-scale
tags: css3, css, sass, scss, color

---

今天我們就來學習 SASS/SCSS 中關於顏色處理的語法：`sass:color` **模組**，他即將取代過去 SCSS 中的 `lighten()` 和 `darken()` 語法。

---

## 一、即將被棄用舊語法：`lighten()` 和 `darken()`

在過去，當我們想讓一個顏色變亮或變暗時，在 SCSS 中就是使用 `lighten()` 和 `darken()` 語法：

```scss
// SCSS
.btn-primary-light {
    background-color: lighten(blue, 20%); // 將藍色調亮 20%
}
.btn-primary-dark {
    background-color: darken(blue, 20%); // 將藍色調暗 20%
}
```

不過[這兩個語法即將被棄用](https://sass-lang.com/documentation/breaking-changes/color-functions/#single-channel-adjustment-functions)，因為他背後的原理是去調整 HSL (色相、飽和度、亮度) 中的「**亮度 (Lightness)**」。

然而 HSL 屬於 sRGB 色彩空間，隨著 CSS Color 4 的出現，現在有了廣色域色彩空間（如 `lch()` 與 `oklch()`），所以 `lighten()` 和 `darken()` 就沒有辦法處理這些廣色域的顏色了。

關於色彩空間的解釋，以前有寫過，大家可以參考看看：

> 延伸閱讀：  
> [#31 CSS 顏色設定：基本的 hex、rgb()、cmyk()、hsl()、hsb() 、hwb() 與明日之星的 lch()、oklch()](https://ithelp.ithome.com.tw/articles/10340417)

所以，為了解決這個問題，Sass 推出了全新的、功能更強大的 `sass:color` 模組。

---

## 二、`sass:color` 顏色模組

從現在開始，當你需要處理任何顏色時，都需要在檔案的最上方引入 `sass:color` 模組。

```scss
// SCSS
@use "sass:color";
```

這個模組提供了兩個替代方案來取代 `lighten()` 和 `darken()`：

* `color.adjust()`
    
* `color.scale()`
    

---

## 三、`color.adjust()`

現在直接使用 `color.adjust()` 這個語法，告訴它要調整顏色的哪個屬性，以及要增加或減少多少固定的數值。

### 1\. 語法

如要是要達到原本 `lighten()` 和 `darken()` 的效果，在 `color.adjust()` 要調整亮度的話，只需要調整 `$lightness` 這個參數就好：

* \*\*調亮：\*\*給 `$lightness` 一個正數百分比。
    
* \*\*調暗：\*\*給 `$lightness` 一個負數百分比。
    

```scss
// SCSS
@use "sass:color";

$primary-color: #3498db;

.btn-primary-light {
    // 將 $lightness 增加 20%
    background-color: color.adjust($primary-color, $lightness: 20%);
}

.btn-primary-dark {
    // 將 $lightness 減少 20%
    background-color: color.adjust($primary-color, $lightness: -20%);
}
```

除此之外，`color.adjust()` 也可以同時調整其他參數，例如透明度（`$alpha`）：

```scss
// SCSS
.btn-primary-transparent {
    // 同時調亮 10% 並減少 0.3 的透明度
    background-color: color.adjust($primary-color, $lightness: 10%, $alpha: -0.3);
}
```

---

## 四、`color.scale()` (推薦)

`color.scale()` 是官方**最推薦**的方法，因為它更直覺。它和 `adjust` 的概念不同，`scale` 不是「增減一個固定值」，而是將顏色屬性「**朝著最大值或最小值，縮放一定的比例**」。

以 `$lightness` 參數來說，如果使用 `color.scale()` 會變成以下結果：

* **調亮：** `$lightness: 50%` 朝著 100% (純白) 的方向**推進 50% 的距離**。
    
* **調暗：** `$lightness: -40%` 朝著 0% (純黑) 的方向**推進 40% 的距離**。
    

### 1\. 語法

`color.scale()` 的寫法基本上與剛剛的 `color.adjust()` 一樣：

```scss
// SCSS
@use "sass:color";

$primary-color: #3498db;

.btn-primary-light {
    // 將亮度朝 100% 推進 20%
    background-color: color.scale($primary-color, $lightness: 20%);
}

.btn-primary-dark {
    // 將亮度朝 0% 推進 20%
    background-color: color.scale($primary-color, $lightness: -20%);
}
```

### 2\. 更直覺的原因

這樣就可以更直覺地調整顏色，怎麼說呢？

舉個例子，如果使用剛剛的 `color.adjust()` 調顏色，以下兩個顏色其實一樣都是白色，可是光用寫的完全看不出來：

* `color.adjust(blue, $lightness: 50%)`
    
* `color.adjust(CornflowerBlue, $lightness: 35%)`
    

因為 `CornflowerBlue` 比 `blue` 還要淺，所以加的 `$lightness` 比較少一點，就變為全白了。

而這時，如果使用的是 `color.scale()`，就可以很清楚地知道距離全白、全黑還有多少距離。

---

## 五、可用參數與注意事項

`color.adjust()` 與 `color.scale()` 的可用參數總共有這些：

### **1\. RGB 通道 (RGB Channels)**

這些參數用於調整基於紅、綠、藍三原光模式的色彩空間。

* `$red`
    
* `$green`
    
* `$blue`
    

### **2\. HSL 通道 (HSL Channels)**

這些參數用於調整基於色相、飽和度、亮度的色彩模型，這種方式更符合人類對顏色的直觀感知。

* `$hue`
    
* `$saturation`
    
* `$lightness`
    

### **3\. HWB 通道 (HWB Channels)**

HWB (Hue-Whiteness-Blackness) 是另一種對人類更直觀的色彩模型，透過調整純色的白度 (whiteness) 和黑度 (blackness) 來得到不同顏色。

* `$hue`
    
* `$whiteness`
    
* `$blackness`
    

### **4\. CIEXYZ 通道 (CIEXYZ Channels)**

CIEXYZ 是一個更科學的色彩空間，涵蓋了人類視覺可感知的所有顏色。`$x`、`$y` 和 `$z` 是它在這個三維色彩空間中的座標值。

* `$x`
    
* `$y`
    
* `$z`
    

### **5\. CIELCh 通道 (CIELCh Channels)**

CIELCh 是 CIELAB 的一種柱面表示法，對於色彩的描述也相當直觀。

* `$chroma`: 表示顏色的彩度或飽和度。`$chroma` 值越高，顏色越鮮豔。此參數也適用於 `oklch` 等其他色彩空間。
    

### **6\. 透明度通道 (Alpha Channel)**

此參數用於調整顏色的不透明度。

* `$alpha`
    

### **7\. 色彩空間參數 (Color Space Parameter)**

這個關鍵參數用於指定在哪個色彩空間中進行上述的通道調整。

* `$space`
    

### **8\. ⚠️ 使用注意事項 (根據**[**官方文件**](https://sass-lang.com/documentation/modules/color/)**)**

#### (1) 舊版色彩空間的通道混合限制

基於歷史因素，當您處理一個傳統色彩空間（如 sRGB）中的顏色時，**不能**在同一次 `color.adjust` 呼叫中，混合使用來自不同色彩模型的參數。例如：

* 同時指定 **RGB 通道** (`$red`, `$green`, `$blue`) 和 **HSL 通道** (`$hue`, `$saturation`, `$lightness`) 是錯的。
    
* 同樣地，同時指定 **RGB 或 HSL 通道** 和 **HWB 通道** (`$hue`, `$whiteness`, `$blackness`) 也是錯的。
    

#### (2) `$space` **的預設行為**

同樣是基於歷史因素，如果您的顏色是傳統顏色（例如，hex #000000、`rgb()` 或 `hsl()` 寫的顏色），並且省略了 `$space` 參數，Sass 會預設在 `hsl` 色彩空間中進行調整。

官方文件強烈建議：「無論如何，都應該好好地傳遞 `$space` 參數。」，因為明確指定 `$space` 可以讓 Code 得更清楚，避免因預設行為而導致的非預期結果。

---

## 六、我該用哪個？`color.adjust()` vs. `color.scale()`

| 函式 | 核心概念 | 適用情境 |
| --- | --- | --- |
| `color.adjust()` | **加/減一個固定值** | 適合需要精確控制顏色數值的**小幅調整**。 |
| `color.scale()` | **朝極端值縮放比例** | **最推薦的** `lighten/darken` 替代方案。適合用來產生一系列的亮色與暗色，適合做色彩系統。 |

---

## 七、更多好用的顏色工具

`sass:color` 模組還提供了許多強大的語法，例如：

* `color.mix($color1, $color2, $weight)`：將兩個顏色以特定比例混合。
    
* `color.grayscale($color)`：將顏色轉為灰階。
    
* `color.invert($color)`：產生反相色。
    
* `color.complement($color)`：產生互補色。
    

詳細的內容，大家可以進一步去看[官方的文件](https://sass-lang.com/documentation/modules/color/)。

今天的內容就先到這邊囉！

---

#### ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

感謝看到最後的你，若你覺得獲益良多，請不要吝嗇給我按個喜歡。❤️

如果你喜歡我的創作，還想看看其他有趣的分享與日常，  
可以追蹤我的 IG [@im1010ioio](https://www.instagram.com/im1010ioio/)，或者是[🧋送杯珍奶鼓勵我](https://im1010ioio.bobaboba.me/)，謝謝你🥰。

![Eva Chen 送杯珍奶鼓勵我](https://cdn.hashnode.com/res/hashnode/image/upload/v1682564435616/c15640fc-6cee-4c33-a898-9cd6820f165a.png align="left")