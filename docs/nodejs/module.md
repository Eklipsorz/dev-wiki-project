---
sidebar_position: 3
---


# 模組

## 簡介
JavaScript 正式規範(ECMAScript)中並沒有明確定義模組化的機制，但在私底下的開發社群中有定義一個名為CommonJS的模組化標準，而Node.js本身有遵從並實現其標準，因此在一般非Node.js環境並不能確定使用該模組化相關的語法，Node.js的執行環境上支援模組化相關的語法。


## 模組種類
Node.js的模組根據原不原生(native)分為兩種：原生模組(native module)、非原生模組(cross platform module)，而原生原意為某個事物的形式上會是屬於某個部分，對這個部分而言，某個事物會是它的一部分，而套用在程式語言的話，會是套用在語言形式上是否屬於某個平台，而判斷屬不屬於就在於平台能否辨識語言形式，若能辨識的話，就代表著語言形式上是屬於平台的。 根據這個定義來看，原生模組會是只有特定平台能夠辨識且執行的模組，而非原生模組就是不特定平台就即可辨識且執行的模組。

### 原生模組形式
原生模組(native module)通常被編譯成只有本地端才能夠執行的模組，這些模組的語言是需要編譯，而編譯是指整個程式碼(模組)需要重新轉換語言後才能被執行的C/C++，而編譯後的語言形式會是只有負責編譯的主機能夠辨識並且執行的形式，比如http、fs等模組，這些程式碼不能夠被保證在其他平台上被正確執行。

在Node.js中，除了跟著Node.js一起編譯的模組(如http、fs)等內建原生模組以外，其他原生模組都可以事先在同一個平台/主機上編譯並當作函式庫中的函式那樣讓開發者自行去使用，這些原生模組會是以node為副檔名，同樣地，語言通常會是C/C++。

### 非原生模組形式
非原生模組通常能夠在不同平台上正常執行，不會因為平台的不同而不能夠執行，這些語言會是直譯語言所構成，直譯語言本身因為擁有邊翻譯邊執行的直譯器而不用先整個翻譯或者編譯，只需要讓每個平台上都擁有這些直譯器就能讓這些模組正常執行，這些語言會是JavaScript。

在Node.js中，基本上非原生模組會是JavaScript、JSOS為主，前者的話會直接透過直譯器來邊解析邊執行，後者的話，會直接當作JavaScript的物件來轉換看待。


## module 使用方法
關於模組化機制上，有兩個主要方法：exports和require，前者能將程式碼片段轉換為模組主要提供的功能，後者能將模組、外部資源以Node.js能夠辨識的形式來載入至內部程式碼。

### require 用法
用來調用模組(外部資源)至目前環境，形式通常會是如下所示，id會是指Node.js預設路徑下的模組，path則是用指定位置上來調用模組的，其位置可以是相對位置或者絕對位置，require會根據判定檔案機制(參考下一個小節)來將id/path辨識成特定形式來回傳內容，回傳內容會是對應模組下所開放給外部使用的程式碼。
```
require(id/path)
```
### require 判定檔案機制
當在路徑Y執行Require(X)的JS程式碼時，會先檢查X是否為Node.js內建的模組，若是的話，就直接載入，若不是的話就會被當作非官方提供的模組，接著在判斷X的路徑是否為絕對路徑，是的話，將Y設定為'/'，但不論是不是，後續會接著以X的路徑形式來進行，而後續流程將以第二張圖來說明，第二張圖主要說明Handle Path and LOAD的流程。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1635152431/blog/nodejs/simpleRequireFlow_lm64kv.png)

首先它會檢查X的路徑是否為絕對路徑，是的話，就將Y設定為'/'，接著根據X指定的路徑來處理，若指定路徑為絕對路徑的話，會將絕對路徑X的'/'去除，並與Y進行合併，而Y此時是'/'，將X和Y結合成一個指向一個檔案的路徑，接著若檔案沒，若不是的話，就會根據副檔名來判斷是否json、node，若是其中一種便停止後續判斷而直接載入，但若指定路徑為相對路徑的話，就會根據Y和X給予的相對路徑來拼湊出新的檔案路徑來判斷檔案是否為JavaScript、json、node，都載入不到就試著將X當作NPM套件來載入，還是載入不到就直接印出找不到。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1635154823/blog/nodejs/simpleIFileType_arw3xb.png)

舉例來說，當在/A/B/C路徑執行包含下面程式碼的JS檔，那麼根據require機制會組裝成/A/B/C/FILE的路徑來指向FILE這檔案，根據FILE的副檔名和內容來判定FILE是否為JS、JSON、node，若三種都不是的話，就改把X當作NPM套件來試著載入。

```
require(./FILE)
```

此小節為簡單版的判定檔案標準，若要研究詳細版的標準，可參考[high-level algorithm in pseudocode of what require()](https://nodejs.org/api/modules.html]來研究。


### exports 用法
該方法如同其名是負責輸出內容至某個地區，在這裏是會以目前檔案為一個模組並定義其模組要輸出的內容，模組內容會根據輸出內容而有所不同：
 - 若module.exports是物件型別，require會回傳物件型別
 - 若module.exports是原型型別，require會回傳原型型別
 - 若module.exports是函式物件，require會是函式物件

舉例：若對名為one.js的JS檔案中的multiplyByTwo函式進行輸出的話，那麼在two.js載入one.js的內容時，funct會因為輸出內容為函式物件而拿到one.js的multiplyByTwo函式，此時，可以當作函式來使用，比如funct(3)，會獲得6這個結果

```
/* inside one.js */
function multiplyByTwo(n) {
   return n * 2
}
module.exports = multiplyByTwo;


/* inside two.js */
const funct = require('./one')
console.log(funct(3))
```


## 參考資料
1. [韋伯字典對於native的定義](https://www.merriam-webster.com/dictionary/native)
2. [What is the definition for "native" in Nodejs?](https://stackoverflow.com/questions/24949108/what-is-the-definition-for-native-in-nodejs)
3. [What is native code?](https://www.quora.com/What-is-native-code)
4. [In programming, what does native mean?](https://www.quora.com/In-programming-what-does-native-mean)
5. [Node.js 核心(原生)模块整理](https://itbilu.com/nodejs/core/N1tv0Pgd-.html)
6. [[第三週] Node.js 基礎 — module.exports 和 require](https://miahsuwork.medium.com/第三週-node-js-基礎-module-exports-和-require-2f9f6915d9f0)
7. [Day5 - 關於 module.exports 的兩三事](https://ithelp.ithome.com.tw/articles/10185083)
