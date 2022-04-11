---
sidebar_position: 4
---

# Node.js 的 環境變數
Node.js 上環境變數(environment variable)是每個由Node.js所執行的App所專屬的變數集合，其變數集合上的每個變數的scope會定義在各自被綁定的App上，用途 **是以一個變數集合來儲存能夠輔助App成功執行或者達成特定任務的內容，如資料庫連線設定**。

每一個App上的所有環境變數集合會由一個名為process的核心模組(core module)所提供：建立一個process的實體，並擁有env這屬性來儲存該App上的所有環境變數

Note: 
1. process 核心模組會包含在express 本身內建套件，只要安裝好express就即可使用其模組所提供的env屬性

## 如何讀取
如要存取名為xxxx的環境變數，就以下面形式來存取
```
process.env.xxxx
```

## 如何設定

1. 於Shell環境下，設定特定名稱的環境變數來當參數至給node程式當執行用的參數，形式會是如下：當在shell上執行node來委託它執行file.js時，node一執行就會讀取variable1這參數並建立process.env.variable1這變數，並儲存value來當這個file.js被執行時所會有的環境變數
```
variable1=value node file.js
```
Note: 
1. variable1=value一定要與等號貼近，否則在shell解析上會當作要執行variable1這指令或者程式模組。
```
variable1 = value node file.js
```
2. 於JS程式碼上直接設定(?)
3. 於

[How to read environment variables from Node.js](https://nodejs.dev/learn/how-to-read-environment-variables-from-nodejs)


## Node.js 的 process
1. 當一個JS檔案被執行的時候，NODE.js會生成process物件來儲存該JS檔案被執行的過程。
2. process物件是儲存目前即為該JS檔案被執行所擁有的狀態
3. process.env 會是一種物件，專門以屬性名稱來儲存該程式執行時會有的環境變數，比如說若有個名為test的環境變數，其值為"test"，那麼環境變數就會是
```
process.env.test
```