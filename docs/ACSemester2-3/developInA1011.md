---
sidebar_position: 34
---

# Development in A10 and A11


## Mongoose 沒反應的原因
1. 沒載入mongoose模組
2. 程式碼沒啟動連線
3. 沒開資料庫

## Express Middleware Stack
1. 每一個在Middleware Stack的Middleware function會被稱呼為Layer

## Express error middleware
1. 每一個在Express Server下的middleware必須滿足以下格式才會被系統辨識為負責處理錯誤的middleware：
  - 若是路由器階層，會使用use語法並搭配含有四個參數的函式物件，四個參數分別為代表錯誤訊息、請求、回應、代表下一個被呼叫的middleware。

  ```
  router.use(path, (err, req, res, next) => {
    // do something
  })
  ```
  - 若應用程式階層，會使用use語法並搭配含有四個參數的函式物件
  ```
  app.use(path, (err, req, res, next) => {
    // do something
  })
  ```

2. 每一個在Express Server的路由器都會以一個路徑為主，並從而構成一個Stack來處理那個路徑之下的子路徑/子路由，而Stack中會存放著是負責處理這些子路徑和子路由的函式物件，當路由器下某個子路徑發生錯誤時，會檢查該路由器所擁有的stack是否有處理error的middleware，若有的話就找它處理，否則就從應用程式階層來找負責處理error的middleware

- 比如如下，若router(path2, callback2)發生錯誤時，系統會去找這個路由器的stack是否有處理錯誤的middleware，結果在最後一行找到負責錯誤處理的(err, req, res, next)函式物件，接著由它來處理代表此時錯誤訊息的物件。
```
const router = express.Router()

router.method(path1, (req, res, next) => {
  // do something
})
router.method(path2, (req, res, next) => {
  // do something
})
.
.
.
router.use(pathN, (err, req, res, next) => {
  // do something
})
```

- 繼續拿上面的例子，假如該路由器不存在任何錯誤處理的middleware，且該路由器是由app.use(path router)進行綁定的，那麼若同樣的錯誤再次發生時，由於路由器不存在任何錯誤處理的middleware，所以會從應用程式階層找錯誤處理的middleware，也就是app.use((err, req, res, next))這行


```
const app = express()
app.use(path router)
app.use((err, req, res, next) => {
    ...
})
```

3. 參考資料：
 > Therefore, if an error is thrown in RouterA, the Router would firstly check its own middleware stack for a matching error Layer (i.e. a (err, req, res, next) function) and execute that, it would then bubble up to the app level and perform the same action.

 - [How to use express.Router instance for error handling](https://stackoverflow.com/questions/59814803/how-to-use-express-router-instance-for-error-handling)

## Express-Session
1. 一種第三方套件，非官方套件。
2. 可以幫開發者從伺服器中擷取cookie資訊、並從中替客戶端建立儲存對應id的cookie和對應的session在伺服器內部的套件。
3. 具體來說，當載入該模組時，客戶端和伺服器端只要處於request/response cycle的話，就會建立session來紀錄兩者的連線過程，同時間會賦予session id 給該session並讓客戶端建立cookie去儲存session id，而到時cookie只要拿著這session id發送請求至伺服器，伺服器收到便拿該id獲取對應先前的連線過程是什麼。 
4. 安裝方式：
```
npm install express-session
```
5. 載入以及設定方式：其套件的回傳內容是本身是一個middleware function，所以若要讓其他的middleware能夠使用其功能，必須先將套件以app.use來使用其midddleware function並放置在其他middleware之前，如下例子：首先先載入express-session回傳的middleware function至session變數，然後用options物件去設定該middleware function，然後後面的app.method或者其他middleware function就能因為use的特性而使用express-session功能。
```
// load express-session
const session = require('express-session')
app.use('/', seession( options ))

// other routes (other middleware functions)
app.method(path, callback)
app.method(path, callback)
.
.
```

5. 延續上個例子，當某個請求是可以對應某一個route時，在伺服器以那個route之前來處理請求，會先經過app.use所載入的session middleware來對req生成對應的屬性來代表著session和cookie的內容

```
app.use('/', seession( options ))

// other routes (other middleware functions)
app.method(path, callback)
app.method(path, callback)
.
.
```



## Express-Session Options
1. Options 是藉由物件來設定Express-Session所提供的middleware
2. 該物件常用的屬性為：
  - secret：一組字串，用來簽署每個儲存session id的cookie以及用該字串來解開每個被簽署過後的cookie是否由該伺服器所建立的cookie，比如說一個儲存session id的cookie會是如下，中間"."前面為session id，而後者則是伺服器用secret套入至加密演算法來生成的字串內容或者稱呼為簽章(signature)，換言之，cookie內容會夾雜session id和前面提到的字串內容，以簽章來辨明該session id是特定伺服器的。
  ```
  s%3ATR0HRHZCS4JQadtvhIJMrQ0lPwKPggXm.Ec668MuqLt%2Fz2FXI7EJqxq85VVXrSPljKa%2Bj7tmaxcU
  ```
  
  - name：字串，設定由模組建立的cookie 所擁有的名字。
  - saveUninitialized：布林值，原本當客戶端與伺服器開始進行連線時，就會開始建立session來紀錄兩者在連線時的狀態，且剛建立的session未在伺服器中被任意值來寫入，此session就會被當作未初始化的session，而若saveUninitialized被設定為true時，就便會將未初始化的session儲存在伺服器的session store，而saveUninitialized被設定為false時，就便不會將未初始化的session儲存在伺服器session store。
  
  true是會強制儲存未初始化的session至伺服器中的session store以及替客戶端建立儲存對應session id的cookie，false是不會強制儲存。
  - resave：布林值，若設為true，每一次客戶端和伺服器之間連線互動所對應的session都會強制繼續存在session store，在請求過程中，無論對應session內容是否有被伺服器修改，都會繼續存至session store，若為false，則有些session不會強制繼續存在session store而被刪除。

3. 需要注意的點：
  - 原本當客戶端與伺服器開始進行連線時，就會開始建立session來紀錄兩者在連線時的狀態，而若
  



