---
sidebar_position: 2
---

# Express.js 框架
它是奠基於 Node.js 的基礎之上的框架，所使用的語言也是JavaScript，由於 Node.js 本身只是提供主機們一個可以執行JavScript的環境，讓開發者能夠開發並在本機上直接執行，然而該環境並不專注於網頁開發上，在這樣背景之下，才有人開發基於Node.js開發環境的另一套開發環境 - Express.js，該開發環境會擁有專屬概念、語法、功能來封裝原本開發環境下的功能、概念、語法、一些套件去支持著網頁開發的功能上，並且留了一些空間給予開發者自行開發自己的應用程式，而應用程式會被這個開發環境封裝，表面上就是看Express.js的程式碼封裝開發者的程式碼，而執行的順序會先交由Express.js來決定開發者程式碼何時被執行。

## Express.js 主要功能
1. 架設(網頁)伺服器
2. 支援網頁路由系統
3. 能夠操作資料庫


## Express.js 安裝方式


1. 安裝express.js
```
npm install express
```


## Express.js 使用方式

1. 調用express模組下所匯出的express函式回傳物件，該物件主要是用來在本地端安裝/設定express 伺服器
```
const express = require('express')
const app = express()
```


## 快速架設一個express server

1. 安裝express
```
npm install express
```
2. 安裝eslint，設定lint初始判別設定
```
npm install eslint
npm init @eslint/config //設定lint初始判別設定
```

3. 安裝dotenv
```
npm install dotenv
```
4. 新增app.js來設定主要的express功能

5. 設定一些輔助用的指令至package.json 

```
 "scripts": {
    "start": "NODE_ENV=development node app.js",
    "dev": "NODE_ENV=development nodemon app.js",
 }
 
```
疑問：
2. 為什麼有些app.js需要匯出app物件
```
module.exports = app
```
供其他程式模組使用該模組app.js，或者說app.js本身就只是被呼叫的一方，所以得被匯出才能調用express

3.  process.env 和 app.set 有什麼差別

```
app.set('port', process.env.PORT || 3000);

var server = app.listen(app.get('port'), function() {
  debug('Express server listening on port ' + server.address().port);
});

```


## morgan 日誌系統
1. 一個紀錄系統運行狀態的系統

## lowdb
1. 以一份JSON文件來當作資料庫來紀錄 
> Tiny local JSON db
2. 適用於規模較小的專案
3. 屬於NoSQL

## nanoid
1. 一款專門產生獨特ID的程式模組
>  tiny, secure, URL-friendly, unique string ID
2. 與uuid相比，較為安全、且不易於人類閱讀。