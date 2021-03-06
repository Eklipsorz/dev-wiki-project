---
sidebar_position: 4
---


# 建立伺服器步驟
1. 載入 Node.js HTTP 模組
2. 定義和伺服器有關的變數
3. 建立一個http.server實體
4. 並於server內部設定回應的 HTTP 狀態碼 (status code)
5. 並於server內部設定回應的內容類型 (Content-Type)
6. 並於server內部把回應的訊息傳送給瀏覽器
7. 啟動並監聽伺服器




第一個步驟：載入 Node.js HTTP 模組
```
const http = require(‘http’)
```
第二個步驟：定義和伺服器有關的變數，以方便未來從這來調整，而不必特別花力氣去從程式碼找到
```
const hostname = 'localhost'
const port = 3000
```

第三個步驟：建立一個http.server實體
```
const server = http.createServer((req, res) => { 
		……
})
```
第四-六個步驟：設定狀態碼、回傳內容類型、回傳的內容，整體會是如下所示：

```
const server = http.createServer((req, res) => { 
    res.statusCode = 200
    res.setHeader('Content-Type', 'text/plain')
    res.end('This is my first server created in Node.js')
})
```

第七個步驟：啟動並監聽伺服器

```
server.listen(port, hostname, () => {
  console.log(`The server is listening on http://${hostname}:${port}`)
})
```



Content-Type
1. 設定回傳內容的類型是什麼
2. 類型有text/html(HTML檔案)、text/plain(純文字)、application/pdf(pdf檔案)、video/mpeg4(影片檔)、application/json(JSON檔案)

response 的 write 方法
1. 指定回應封包之Body部分內容是什麼
2. 格式會是，第一種的data是字串，會以data作為body的內容。
```
response.write(data)
```

response 的 end 方法
1. 告訴伺服器要傳遞的Header和Body就到end這裡結束了，也就是說傳遞訊息夾雜的範圍就被決定下來了
2. 這個方法必須存在每個response之後來告知，若end是以end(data)時，便等同於
```
response.write(data)
response.end()
```
3. 常見的格式為以下：
```
response.end(data)
response.end()
```



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



http.createServer([options][, requestListener])
1. 建立一個http.Server實體/物件，而這個物件是主要負責去向監聽客戶端的請求並回應
2. 參數有兩個，第一個是options這物件，而 requestListener則是負責定義伺服器如何回應客戶端的函式
3.  該函式是回傳http.Server實體/物件

requestListener(request, response)
1. 被createServer當作負責定義伺服器如何回應客戶端的函式
2. 函式內有兩個參數(request, response)，request 是存放客戶端發送至伺服器端的請求之物件Request object，而response是指伺服器回應給客戶端的訊息物件ServerResponse，request這名稱可以是任意名稱，因為無論如何第一個參數會直接被當作Request object，通常會是req，而response亦是如此，通常會取名為res。
3. 當requestListener函式的第一個參數是函式時，便會省略掉options這物件
4. response物件擁有比較常見的屬性：
  - statusCode 屬性：設定給狀態碼給客戶端了解傳送接收的狀況
  - setHeader 方法：設定回應給客戶端的封包之Header部分，其方法為(name, value)，會根據Header是否存在name屬性值來更改成value。
  - res 方法


server 的 listen
1. 為server物件的方法之一
2. 主要負責啟動監聽客戶端的請求
3. 語法為：第一種語法中的port、hostname、callback分別是向系統註冊port號碼來監聽請求、向系統註冊自己主機名稱是什麼來方便在封包內容顯示這是從何而來以及後續處理、當listener被建立時會直接執行callback函式

```
server.listen(port, hostname, callback)
```
https://www.w3schools.com/nodejs/met_server_listen.asp






## 建立簡單的網頁伺服器：
以下面程式碼來修改成一個使用者只要向對應主機名稱和對應埠發送請求就能獲得HTML網頁的程式碼，在這裏主要會修改setHeader和end的部分
```
const http = require(‘http’)

const hostname = 'localhost'
const port = 3000

const server = http.createServer((req, res) => { 
    res.statusCode = 200
    res.setHeader('Content-Type', 'text/plain')
    res.end('This is my first server created in Node.js')
})

server.listen(port, hostname, () => {
  console.log(`The server is listening on http://${hostname}:${port}`)
})
```

首先先將 Content-Type 修改成 text/html，
```
res.setHeader('Content-Type', 'text/html')
```

接著就是將res.end的內容修改想呈現的HTML檔案內容，其中會使用``這符號作為內容中的起始符號以及結尾符號，而這個符號雖說可以用單引號或者雙引號來代替，但由於這些符號在網頁內容存在著許多問題因素：
 - HTML檔案內容本身會有許多單引號或者雙引號，在開發上可能會有誤判難以維護的可能。
 - 單引號和雙引號只能夠將一行的內容當作是字串來處理，無法換行處理。
 - 當要將變數內容轉變換字串一部分，單引號和雙引號得獨立處理。


所以在這裡會直接使用反引號``來當作內容的起始符號和結尾符號來以字串進行輸出，最後利用反引號以及夾帶著HTML檔案內容的res.end來回應每次請求者發出的請求。
```
  res.end(`
      <h1>Create your own server with Node.js</h1>
      <h3>Popular movies in 2018</h3>
      <ul>
        <li>
          <img
              src="https://movie-list.alphacamp.io/posters/c9XxwwhPHdaImA2f1WEfEsbhaFB.jpg"
              alt="Jurassic World: Fallen Kingdom"
              width="50px"
          />
          Jurassic World: Fallen Kingdom
        </li>
        <li>
          <img
              src="https://movie-list.alphacamp.io/posters/rv1AWImgx386ULjcf62VYaW8zSt.jpg"
              alt="Ant-Man and the Wasp"
              width="50px"
          />
          Ant-Man and the Wasp
        </li>
      </ul>
  `)
```

