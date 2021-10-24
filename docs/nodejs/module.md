---
sidebar_position: 3
---


# 模組
1. JavaScript 正式規範(ECMAScript)中並沒有明確定義模組化的機制，但在私底下的開發社群中有定義一個名為CommonJS的模組化標準，而Node.js本身有遵從並實現其標準，因此在一般非Node.js環境並不能夠使用模組化相關的語法，而Node.js的執行環境上支援模組化相關的語法。
2. 模組本身是一個特殊物件，可以將JavaScript任意型別的資料(含函式物件)轉換成模組物件來處理。


## require
1. 用來調用外部資源至目前環境，形式通常會是如下所示，require會根據fileName所對應的檔案類型而具有不同的對應檔案處理，主要會有四種檔案類型能被require處理：模組、js檔、json檔、node檔

```
const obj = require(fileName)
```
2. 而require判定檔案類型的標準，則是根據檔案名稱是否明確寫出，假使不寫副檔名的話，
 - 預設require會檢查是否為模組，不是就往下，是模組就按照模組的形式來處理
 - 檢查是否為js檔案，不是就往下，是js檔案就按照js檔案來處理
 - 檢查是否為json檔案，不是就往下，是json檔案就按照json檔案處理，這邊會直接把json檔案內容視為JavaScript 物件來處理
 - 檢查是否為node檔案，不就往下，是node檔案就按照node檔案來處理
 - 都不是的話，就沒辦法被正確讀取。

3. 舉例：會先判定目前目錄是否有movies這模組或者js檔案，都沒有的話，就會朝著movies.json來尋找，接著就是movies.node來尋找。

```
const movieList = require('./movies')
```


### 參考資料
1. [[第三週] Node.js 基礎 — module.exports 和 require](https://miahsuwork.medium.com/第三週-node-js-基礎-module-exports-和-require-2f9f6915d9f0)
2. [Day5 - 關於 module.exports 的兩三事](https://ithelp.ithome.com.tw/articles/10185083)




module.exports：
1. 定義模組實際要提供的東西
2. 當module.exports是物件，require會回傳物件
3. 當module.exports是原型，require會回傳原型
4. 當module.exports是函式，require會是函式


載入其他js檔案方法：
1. require(path/filename)，若沒添加指定path/的話，系統會去預設路徑去尋找filename是否存在，若指定path/則會以指定路徑來找到filename
2. Filename可以不添加.js後綴名，系統會預設為該檔案為js檔案
3. Require 回傳結果會根據module.exports指定的內容而定


http module：
1. 由Node.js內建的模組，它允許Node.js能夠透過HTTP協定來傳遞資料
2. 會使用 require(‘http’)來調用該模組，而require會是一個物件的形式來回傳http本身
const http = require(‘http’)
