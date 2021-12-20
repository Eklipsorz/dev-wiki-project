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

