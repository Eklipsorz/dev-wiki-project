---
sidebar_position: 2
---

# Express Server 的 Router
1. router 本身是一個能夠提供路由功能管理的程式模組(函式物件)，主要是將應用層的路由層級模組分離出來至一個單獨模組
2. 應用層級模組本身內建的路由器來管理路由功能，而這裡分離並非真的將內建的路由功能分離出來，而是將應用層級的路由功能交由一個只負責路由功能管理的路由層級模組來負責
3. 該路由層級模組可以想像成只具有路由功能管理的應用層級模組，該路由層級模組可以和應用層級模組內建的路由器一樣具有以下功能：
  - 提供 middleware 的機制
  - 和內建路由器提供類似的語法和相同參數來對應特定的路由，如router.use (app.use)、router.method(app.method)
4. 應用層級模組可以藉由以下語法來將特定路徑path下的路由管理交由router來管理，而router的路由會是以path為主來構成相對路徑
```
app.use(path, router)
```
5. 舉例： 若將路由層級模組router綁定在todos上，會因為路由器定義/和/something而將合法的對應路由設定在是/todos/和/todos/something上
```
// application:

app.use('/todos', router)

// router


router.get('/', callback1)
router.method('/something', callback2)
```

## 重構Express server的路由
1. 將集中在同一個檔案(app.js)的路由設定分離至另一個目錄下做管理，進而得到以下優點：
  - 維持文件易讀性：使app.js在程式碼閱讀上增加易讀性
  - 關注點分離：藉由將路由設定分離至另一個目錄，來讓原本關注點有兩點的app.js縮減至各只有一個關注點的程式碼模組，分別為負責整個Web APP的設定和啟動的app.js 和 負責管理Web APP的路由設定的程式模組


