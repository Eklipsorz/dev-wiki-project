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
用來調用模組(外部資源)至目前環境，形式通常會是如下所示，


require會根據fileName所對應的檔案類型而具有不同的對應檔案處理，主要會有四種檔案類型能被require處理：模組、js檔、json檔、node檔

```
require(id)
```
### exports 用法

### require 判定檔案標準




### exports：
該方法如同其名是負責輸出內容至某個地區，在這裏是會以目前檔案為一個模組並定義模組內容，模組內容會根據輸出內容而有所不同：
 - 當module.exports是物件，require會回傳物件
 - 當module.exports是原型，require會回傳原型
 - 當module.exports是函式，require會是函式 


## require
1. 用來調用外部資源至目前環境，形式通常會是如下所示，require會根據fileName所對應的檔案類型而具有不同的對應檔案處理，主要會有四種檔案類型能被require處理：模組、js檔、json檔、node檔

```
const obj = require(fileName)
```




2. 而require判定檔案類型的標準，則是根據檔案名稱和路徑是否明確寫出，假使有寫路徑但就是不寫副檔名的話，
 - 檢查是否為js檔案，不是就往下，是js檔案就按照js檔案來處理
 - 檢查是否為json檔案，不是就往下，是json檔案就按照json檔案處理，這邊會直接把json檔案內容視為JavaScript 物件來處理
 - 檢查是否為node檔案，不就往下，是node檔案就按照node檔案來處理
 - 都不是的話，就沒辦法被正確讀取。
3. 假使不寫路徑和副檔名的話，Node.js就會認為這是模組，然後就去預設放模組的路徑尋找該模組是否存在，若不存在就停止後續判斷

4. 舉例：會先判定目前目錄是否有movies這模組或者js檔案，都沒有的話，就會朝著movies.json來尋找，接著就是movies.node來尋找。

```
const movieList = require('./movies')
```

模組會以其他形式來封裝實現功能JS檔案，本身是和JS檔案為不同的檔案

### 參考資料
1. [[第三週] Node.js 基礎 — module.exports 和 require](https://miahsuwork.medium.com/第三週-node-js-基礎-module-exports-和-require-2f9f6915d9f0)
2. [Day5 - 關於 module.exports 的兩三事](https://ithelp.ithome.com.tw/articles/10185083)







載入其他js檔案方法：
1. require(path/filename)，若沒添加指定path/的話，系統會去預設路徑去尋找filename是否存在，若指定path/則會以指定路徑來找到filename
2. Filename可以不添加.js後綴名，系統會預設為該檔案為js檔案
3. Require 回傳結果會根據module.exports指定的內容而定


http module：
1. 由Node.js內建的模組，它允許Node.js能夠透過HTTP協定來傳遞資料
2. 會使用 require(‘http’)來調用該模組，而require會是一個物件的形式來回傳http本身
const http = require(‘http’)



## 參考資料
1. [韋伯字典對於native的定義](https://www.merriam-webster.com/dictionary/native)
2. [What is the definition for "native" in Nodejs?](https://stackoverflow.com/questions/24949108/what-is-the-definition-for-native-in-nodejs)
3. [What is native code?](https://www.quora.com/What-is-native-code)
4. [In programming, what does native mean?](https://www.quora.com/In-programming-what-does-native-mean)
5. [Node.js 核心(原生)模块整理](https://itbilu.com/nodejs/core/N1tv0Pgd-.html)
