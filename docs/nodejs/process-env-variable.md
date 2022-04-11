---
sidebar_position: 4
---

# Node.js 的 環境變數
Node.js 上環境變數(environment variable)是每個由Node.js所執行的App所專屬的變數集合，其變數集合上的每個變數的scope會定義在各自被綁定的App上，用途 **是以一個變數集合來儲存能夠輔助App成功執行或者達成特定任務的內容，如資料庫連線設定**。

每一個App上的所有環境變數集合會由一個名為process的核心模組(core module)所提供：建立一個process的實體，並擁有env這屬性來儲存該App上的所有環境變數

Note: 
1. process 核心模組會包含在express 本身內建套件，只要安裝好express就即可使用其模組所提供的env屬性

## 如何讀取
如要在JS的執行過程中存取名為xxxx的環境變數，就以下面形式來存取
```
process.env.xxxx
```

## 如何設定

1. 於Shell環境下，設定特定名稱的環境變數來當參數至給node程式當執行用的參數，形式會是如下：當在shell上執行node來委託它執行file.js時，node一執行就會讀取variable1這參數並建立process.env.variable1這變數，並儲存value來當這個file.js被執行時所會有的環境變數
```
variable1=value node file.js
```

2. 於JS程式碼上直接設定，形式會是如下，設定一個名為variable1的環境變數來儲存value1這值
```
process.env.variable1 = value1
```
3. 安裝dotenv模組，於.env檔案內容定義名為variable1環境變數，最後於要被執行的JS檔案中載入dotenv中的config()就會先去找目前JS檔案所在的檔案是否有.env，若有的話，就以轉換成JS環境變數的形式來進行
  - 安裝dotenv模組
  ```
  npm install dotenv
  ```
  - 設定名為variable1的環境變數

  ```
  // 方式1
  variable1 = value1

  // 方式2
  variable1 =value1

  // 方式3
  variable1=value1
  ```
  - 於要被執行的JS檔案中載入dotenv模組中的config()來將.env內容轉換為該JS檔案的環境變數。
  ```
  // 純粹載入的話
  require('dotenv').config()

  // 若考量到環境上會因為轉換開發狀態：development狀態、test狀態、production狀態下，
  // 而會有不同模式下會有不同的環境變數之情形

  // 若不是production狀態的話就以dotenv來新增環境變數
  if (process.env.NODE_ENV !== 'production') {
    require('dotenv').config()
  }
  ```

Note: 
1. variable1=value一定要與等號貼近，否則在shell解析上會當作要執行variable1這指令或者程式模組。
```
variable1 = value node file.js
```
[How to read environment variables from Node.js](https://nodejs.dev/learn/how-to-read-environment-variables-from-nodejs)


## 補充知識：Node.js 的 process
1. 當一個JS檔案被執行的時候，NODE.js會生成process物件來儲存該JS檔案被執行的過程。
2. process物件是儲存目前即為該JS檔案被執行所擁有的狀態
3. process.env 會是一種物件，專門以屬性名稱來儲存該程式執行時會有的環境變數，比如說若有個名為test的環境變數，其值為"test"，那麼環境變數就會是
```
process.env.test
```