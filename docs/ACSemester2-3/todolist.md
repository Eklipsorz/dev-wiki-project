---
sidebar_position: 23
---

# todolist 實作流程

## todolist 案例下的路由設計
1. 理想下的路由設計：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1638262176/blog/srello/idealRouteDesign_a2ttbi.png)

2. 但由於HTML本身對於PUT和Delete的支援性不高，實際上的路由設計會是：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1638262176/blog/srello/realRouteDesign_ejwpxb.png)

## todolist 瀏覽所有todo的實作
1. 滿足該瀏覽所有todo的路由會是如下，只要瀏覽根目錄就能即可能瀏覽每個todo
```
GET /
```
2. 實際下MVC架構會是如何做：
  - 客戶端發送 GET / 的請求
  - 伺服器收到 GET / 的請求，並到路由清單尋找與 GET / 請求相符的路由，執行該路由的Controller
  - 該Controller 向 Model-todoModel 發出獲取對應collection的所有資料之請求
  - Model 透過 Mongoose 的幫助下，向MongoDB資料庫發出Query請求
  - MongoDB 資料庫回傳資料
  - Model - todoModel 再將資料轉送給Controller 
  - Controller 再將資料轉接給View引擎(handlebars)，要求渲染GET /對應的頁面，並加入Model轉交過來的資料
  - View 引擎工作完成後，並把合併後的HTML模板網頁回傳給Controller
  - Controller 再將合併後的模板網頁回傳給客戶端
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1638283689/blog/srello/viewAlltodos_tv7doh.png)

3. 在這裡MVC負責以下工作：
  - Controller：路由清單尋找與 GET / 請求相符的路由，執行該路由的Controller、該Controller 向 Model-todoModel 發出獲取對應collection的所有資料之請求、Controller 向View引擎(handlebars)要求渲染對應請求的模板網頁、回傳View轉交過來的模板網頁至客戶端
  ```
    /* 載入Model模組，取得對應collection的document constructor */
    const todoModel = require('./models/todoModel')
    app.get('/', (req, res) => {

    // 藉由document constructor所擁有的方法-find來對對應的collection建立特定資料搜查的請求(Query)
    //，而由於沒有參數會直接從collection取得所有資料，最後todoModel.find()本身回傳為代表搜查請求Query

    /* Controller 向 Model-todoModel 發出獲取對應collection的所有資料之請求 */
    todoModel.find()
    
    /* Model 要求 Query 最後執行的結果格式轉化成handlebars能夠接收的格式 */
      .lean()
    // 以Promise形式來執行Query，換言之，Model 透過 Mongoose 的幫助下，向MongoDB資料庫
    // 發出Query請求.exec()

    // 當執行完Promise之後，當搜尋成功，其todos會是Query執行後的搜尋結果，接著用它來對index
    // 這渲染檔案和搜尋結果合併渲染

    /* 
       Controller 再將資料轉接給View引擎(handlebars)，要求渲染GET /對應的頁面，
       並加入Model轉交過來的資料，View 引擎工作完成後，並把合併後的HTML模板網頁回傳給
       Controller
    */
      .then(todos => res.render('index', { todos }))
    /* 當搜尋失敗，就以eror物件來回傳搜尋失敗後得到的錯誤訊息 */
      .catch(error => console.log(error))

    })
  ```
  
  - Model： 在這裏負責接收Controller的請求來在對應集合中找到所有資料、向 MongoDB 資料庫發出 query 請求、當MongoDB 資料庫回傳資料時，再由Model把資料回傳給Controller，該Model設定內容如下，但實際上執行的地方是在Controller的路由系統中，由Controller調用著Model。

  ```
    // 載入mongoose 套件
    const mongoose = require('mongoose')
    
    // 在應用程式層面定義collection的schema
    const Schema = mongoose.Schema
    const todoSchema = new Schema({
    name: {
      type: String, // 資料型別是字串
      required: true // 這是個必填欄位
    }
    })

    // 在指定的資料庫當中建立名為todos的collection，並且回傳以Schema作為內容
    // 儲存限制 之 document constructor，最後以模組內容來輸出，只要有人以目前
    // 所在檔案為模組，該模組內容將會是mongoose.model(....)
    module.exports = mongoose.model('todo', todoSchema)
  ```


  - View：在這裏負責接收Controller的請求和資料來將資料和對應請求的模板網頁做合併、回傳合併後的模板網頁給Controller，該模板網頁內容如下：

  ```
    <h2>Hello world! 這是 index</h2>

    <a href="./todos/new">Create</a>
    <ul>
      {{#each todos}}
      <li>
        {{this.name}}
        <a href="./todos/{{ this._id }}">detail</a>
        <a href="./todos/{{ this._id }}/edit">edit</a>
        <form action="./todos/{{ this._id }}/delete" method="POST" style="display: inline;">
          <button type="submit">delete</button>
        </form>
      </li>
      {{/each}}
    </ul>

  ```

## todolist 瀏覽特定todo的實作
1. 滿足該瀏覽特定todo的路由會是如下，其中會以/todos/為篩選條件來取得後面的字串作為特定todo的id
```
GET /todos/:id
```
2. 實際下MVC架構會是如何做：
  - 客戶端發出 GET /todos/:id 的請求
  - 伺服器收到請求，並到路由清單找尋對應 GET /todos/:id 的 Controller 來處理請求
  - 該Controller 解析請求並取出特定todo的id
  - Controller 向 Model - todoModel 發出找尋符合id的資料之請求Query
  - Model 透過 Mongoose 的幫助下，向MongoDB資料庫發出Query請求
  - 當資料庫回傳指定結果給Model時，Model會將資料轉交給Controller
  - Controller 再向View引擎(handlebars)發出對應請求的渲染畫面，並加入至Model轉交過來的資料
  - View收到資料後，便將資料和對應模板網頁做合併，最後將合併後的模板網頁傳至Controller
  - Controller 再將合併後的模板網頁傳至客戶端。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1638283689/blog/srello/viewAtodo_mhculs.png)


3. 在這裡MVC負責以下工作：
  - Controller：路由清單尋找與 GET /todos/:id 請求相符的路由，執行該路由的Controller、獲取請求中的id、該Controller 向 Model-todoModel 發出獲取對應collection且能夠滿足id的資料之請求、Controller 向View引擎(handlebars)要求渲染對應請求的模板網頁、回傳View轉交過來的模板網頁至客戶端

  ```
    /* 載入程式模組，取得對應collection的document constructor */
    const todoModel = require('./models/todoModel')
    app.get('/todos/:id', (req, res) => {

    // 則是從/todos/:id取得:id來作為特定todo的Id
    const reqId = req.params.id
    // 藉由document constructor所擁有的方法-find來對對應的collection建立特定資料搜查的請求(Query)
    //，而由於沒有參數會直接從collection取得所有資料，最後todoModel.findById(reqId)本身回傳為代表
    // 搜查請求Query

    /* Controller 向 Model - todoModel 發出找尋符合id的資料之請求Query */
    todoModel.findById(reqId)
    
    /* 要求Query最後執行的結果格式轉化成handlebars能夠接收的格式 */
      .lean()
    // 以Promise形式來執行Query，換言之，Model 透過 Mongoose 的幫助下，向MongoDB資料庫
    // 發出Query請求.exec()
    // 當執行完Promise之後，當搜尋成功，其todo會是Query執行後的搜尋結果，接著用它來對detail
    /* 
       當資料庫回傳指定結果給Model時，Model會將資料轉交給Controller，Controller 再向View引擎
       (handlebars)發出對應請求的渲染畫面，並加入至Model轉交過來的資料
    */
      .then(todo => res.render('detail', { todo }))
    /* 當搜尋失敗，就以eror物件來回傳搜尋失敗後得到的錯誤訊息 */
      .catch(error => console.log(error))

    })
  ```
  
  - Model：在這裏負責接收Controller的請求來在對應集合中找到滿足id資料、向 MongoDB 資料庫發出 query 請求、當MongoDB 資料庫回傳資料時，再由Model把資料回傳給Controller，該Model設定內容如瀏覽所有todo的內容相同，但實際上執行的地方是在Controller的路由系統中，由Controller調用著Model。

  - View：在這裏負責接收Controller的請求和資料來將資料和對應請求的模板網頁做合併、回傳合併後的模板網頁給Controller，該模板網頁內容如下：

  ```
    <p>{{ todo.name }}</p>
    <a href="/todos/{{ todo._id }}/edit">edit</a>
    <a href="/">back</a>
    <form action="/todos/{{ todo._id }}/delete" method="POST" style="display: inline;">
      <button type="submit">delete</button>
    </form>
  ```


## todolist 建立todo的實作
1. 要完成這個實作，會有兩個請求方向，一種是導向負責建立todo頁面的請求，接著第二種是接續著第一種來定義在建立頁面下發送增加資料的請求：
  - 導向負責建立todo頁面的請求：通常會有這請求是因為使用者所處的頁面並非是建立頁面，所以使用者必須發送導向其頁面的請求：
  ```
    GET /todos/new
  ```

  - 在建立頁面下發送增加資料的請求：當使用者處在建立頁面上，而該頁面上通常會是由表格來接收使用者所輸入的內容來新增資料，在這裡並不會有任何:id的請求，因此直接設定成如下：
  ```
    POST /todos
  ```
### 導向負責建立todo頁面的請求
1. 實際下MVC架構只會有Controller和View兩者間的互動，其互動會是如下：
  - 客戶端發出 GET /todos/new 請求
  - 伺服器收到請求，並從路由系統中挑出對應的 Controller 來處理請求
  - Controller收到請求後便直接向View發出對應請求的模板網頁
  - View收到Controller，便回傳對應的模板網頁給Controller
  - Controller 把View回傳過來的模板網頁回傳給客戶端
  - 客戶端收到模板網頁，瀏覽器就便按照其網頁內容來刷新畫面
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1638284242/blog/srello/direct2CreatePage_bshlqy.png)

2. 在這裡MVC負責以下工作：
  - Controller： 路由清單尋找與 GET /todos/new 請求相符的路由，執行該路由的Controller、Controller 向View引擎(handlebars)要求渲染對應請求的模板網頁、回傳View轉交過來的模板網頁至客戶端

  ```
    // Controller 內容
    // 定義Controller 接收到 GET /todos/new所要做的處理
    app.get('/todos/new', (req, res) => {

      // 向View引擎(handlebars)要求渲染對應請求的模板網頁、回傳View轉交過來的模板網頁至客戶端
      res.render('new')
    })
  ```

  - View：接收Controller的渲染畫面指示、負責產生對應請求的模板網頁、回傳模板網頁至Controller，其模板網頁內容如下：
  ```
    // 定義GET /todos/new 請求所要獲取的畫面，並以名為new的渲染檔案來儲存。
    // 其內容包含一個提交按鈕和一個輸入欄的表格，當使用者按下提交按鈕就發送POST請求至/todos
    <form action="/todos" method="POST">
      <input type="text" placeholder="name" name="name">
      <button type="submit">Create</button>
    </form>
  ```


### 在建立頁面下發送增加資料的請求
1. 實際下MVC架構只會有Controller和Model兩者間的互動，其互動會是如下：
  - 客戶端發出POST /todos請求
  - 伺服器接收請求，並從路由清單中挑取對應的Controller來處理請求
  - Controller收到請求後，會先從POST封包內取出內容並把它當作新清單項目的內容
  - Controller將內容轉交給Model，請求Model去生成基於該內容的Document Instance (todo)
  - Model向MongoDB資料庫發出新增todo至資料庫的請求
  - 資料庫新增資料便回傳結果給Model
  - Model 將新增資料的調查結果回報給Controller
  - Controller接收到調查結果，若成功的話，會發出請求讓客戶端導向首頁，若失敗的話就顯示錯誤訊息。
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1638284242/blog/srello/createATodo_rxv3k8.png)
2. 在這裡MVC負責以下工作：
  - Controller： 路由清單尋找與 POST /todos 請求相符的路由，執行該路由的Controller、獲取POST封包內容並將其內容轉送給Model，接著向Model發出資料增加的請求、根據Model的增加結果來判斷是否發出請求讓客戶端導向首頁

  ```
    // Controller 內容，在這裏todoModel即為Model所提供，當使用它便是代表向它發出請求來處理資料增加
    // add a new todo
    app.post('/todos', (req, res) => {
      // 解析POST封包內容
      const name = req.body.name
      // Controller將內容轉交給Model，請求Model去生成基於該內容的Document Instance (todo)
      // 然後由Model來建立該Instance。
      const newTodo = new todoModel({ name })

      // Model 向 MongoDB資料庫發出新增todo(Instance 轉換至文件，然後將文件插入至指定集合)的請求，
      return newTodo.save()
      // Model 將新增資料的調查結果回報給Controller，Controller接收到調查結果，若成功的話，
      // 會發出請求讓客戶端導向首頁，若失敗的話就顯示錯誤訊息。
        .then(res.redirect('/'))
        .catch(error => console.log(error))

    })
  ```


## todolist 編輯特定todo的實作
  1. 要完成這個實作，會有兩個請求方向，一種是導向負責建立todo頁面的請求，接著第二種是接續著第一種來定義在建立頁面下發送增加資料的請求：
  
  - 導向負責編輯todo頁面的請求：通常會有這請求是因為使用者所處的頁面並非是編輯頁面，所以使用者必須發送導向其頁面的請求：
  ```
    GET /todos/:id/edit
  ```

  - 在編輯頁面下發送編輯資料的請求：當使用者處在編輯頁面上，而該頁面上通常會是由表格來儲存先前的資料，這些資料可以進一步在表格來修改和編輯，當使用者提交按鈕時，便會發送該請求 
  ```
    POST /todos/:id/edit
  ```

  
### 導向負責編輯todo頁面的請求
1. 實際下MVC架構只會有Controller和Model兩者間的互動，其互動會是如下：
  - 客戶端發出 GET /todos/:id/edit 請求
  - 伺服器收到請求，並從路由清單中找尋對應的Controller
  - Controller收到請求後，解析請求並從中取出特定todo的id
  - Controller將id轉送給Model，並請它幫忙找尋對應id的資料
  - Model向資料庫發出找尋對應id之資料的請求
  - 資料庫找尋完後，便把對應資料轉送給Model
  - Model再把轉送過來的資料轉交給Controller
  - Controller把資料轉交給View，請求它回傳對應todo的編輯頁面
  - View引擎將對應的模板網頁和資料合併，接著回傳合併後的模板網頁給Controller
  - Controller收到View轉送過來的網頁，就將網頁轉送給客戶端
  - 客戶端收到請求後，便按照網頁內容來渲染
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1638345814/blog/srello/viewEditPage_oqvnlb.png)

2. 在這裡MVC負責以下工作：
  - Controller：從路由清單中找尋對應的Controller，並執行對應的Controller、Controller解析請求並從中取出特定todo的id、Controller將id轉送給Model，並請它幫忙找尋對應id的資料、Controller把Model傳過來的資料轉交給View，請求它回傳對應todo的編輯頁面、Controller收到View轉送過來的網頁，就將網頁轉送給客戶端
  ```
    app.get('/todos/:id/edit', (req, res) => {

      // Controller收到請求後，解析請求並從中取出特定todo的id
      const id = req.params.id
      // Controller將id轉送給Model，並請它幫忙找尋對應id的資料
      todoModel.findById(id)
      // Model進一步設定輸出的內容格式要符合handlebars能夠接受的格式
        .lean()
      // Model再把轉送過來的資料轉交給Controller，Controller把資料轉交給View，
      // 請求它回傳對應todo的編輯頁面
        .then(todo => res.render('edit', { todo }))
        .catch(error => console.log(error))
    })
  ```

  - Model：接收Controller向資料庫存取的請求、Model向資料庫發出找尋對應id之資料的請求、將資料庫回傳至Model的資料轉交給Controller
  - View：接收Controller的對應請求和資料、負責將對應請求的模板網頁和資料合併成一個網頁、將其網頁回傳給Controller，對應請求的模板網內容如下：
  ```
    <form action="/todos/{{ todo._id }}/edit" method="POST">
      <input type="text" placeholder="name" name="name" value="{{ todo.name }}">
      <button type="submit">Save</button>
      <a href="/">back</a>
    </form>
  ```
### 在編輯頁面下發送編輯資料的請求
1. 實際下MVC架構只會有Controller和Model兩者間的互動，其互動會是如下：
  - 使用者發出 POST /todos/:id/edit
  - 伺服器接受請求，從路由清單中找尋對應路由的Controller
  - Controller 接收到請求，解析網址上的識別碼以便取得對應todo的id 以及 從POST封包中獲取修改內容
  - Controller 轉送id 和 修改內容 至Model，並向它發出搜尋並修改對應id之todo 的請求，
  - Model 收到請求便轉成資料庫指令向MongoDB資料庫發出對應請求
  - 資料庫執行完畢後，便回傳處理結果至Model
  - Model收到結果便轉送結果至Controller
  - Controller根據收到的結果來判別是否發出請求客戶端導向首頁的請求，若結果是成功的話，就發出導向請求，失敗的話，就印失敗訊息
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1638345814/blog/srello/modifyEditPage_lmbjzh.png)

2. 在這裡MVC負責以下工作：
  - Controller：從路由清單中找尋對應路由的Controller、Controller 轉送id 至Model，並向它發出搜尋並編輯對應id之todo 的請求、接收Model的執行結果來判定是否向客戶端發出請求轉向首頁

  ```
    app.post('/todos/:id/edit', (req, res) => {

      // Controller 解析網址上的識別碼以便取得對應todo的id
      const id = req.params.id
      // Controller 從POST封包中獲取修改內容
      const name = req.body.name

      // Controller 轉送id 和 修改內容 至 Model，並向它發出搜尋並修改對應id之todo 的請求，
      // 資料庫執行完畢後，便回傳處理結果至Model，Model收到結果便轉送結果至Controller。
      todoModel.findById(id)
        .exec()
        .then(todo => {
          // 這裡的todo會是Document Instance
          // console.log(todo instanceof todoModel)         result: true
          // console.log(todo instanceof mongoose.Model)    result: true
          // console.log(todo instanceof mongoose.Document) result: true
    
          todo.name = name
          return todo.save()
        })
      // Controller根據收到的結果來判別是否發出請求客戶端導向首頁的請求，若結果是成功的話，
      // 就發出導向請求，失敗的話，就印失敗訊息
        .then(() => res.redirect('/'))
        .catch(error => console.log(error))
    })
  ```

  - Model：接收Controller的對應路由之請求、Model 收到請求便轉成資料庫指令向MongoDB資料庫發出對應請求、接收資料庫的執行結果並轉送結果至Controller


## todolist 刪除特定todo的實作
1. 滿足該刪除特定todo的路由會是如下，其中會以/todos/為篩選條件來取得後面的字串作為特定todo的id
```
  POST /todos/:id/delete
```

2. 實際下MVC架構會是如何做：
  - 使用者發出 POST /todos/:id/delete
  - 伺服器接受請求，從路由清單中找尋對應路由的Controller
  - Controller 接收到請求，解析網址上的識別碼以便取得對應todo的id 
  - Controller 轉送id 至Model，並向它發出搜尋並刪除對應id之todo 的請求
  - Model 收到請求便轉成資料庫指令向MongoDB資料庫發出對應請求
  - 資料庫執行完畢後，便回傳處理結果至Model
  - Model收到結果便轉送結果至Controller
  - Controller根據收到的結果來判別是否發出請求客戶端導向首頁的請求，若結果是成功的話，就發出導向請求，失敗的話，就印失敗訊息

  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1638367084/blog/srello/deletePage_vn7aep.png)

3. 在這裡MVC負責以下工作：
  - Controller：從路由清單中找尋對應路由的Controller、Controller 轉送id 至Model，並向它發出搜尋並刪除對應id之todo 的請求、接收Model的執行結果來判定是否向客戶端發出請求轉向首頁
  ```
    app.post('/todos/:id/delete', (req, res) => {
      // Controller 接收到請求，解析網址上的識別碼以便取得對應todo的id 
      const id = req.params.id

      // Controller 轉送id 至Model，並向它發出搜尋並刪除對應id之todo 的請求，
      // Model 收到請求便轉成資料庫指令向MongoDB資料庫發出對應請求，資料庫執行
      // 完畢後，便回傳處理結果至Model，Model收到結果便轉送結果至Controller
      todoModel.findById(id)
        .exec()
        .then(todo => todo.remove())
      // Controller根據收到的結果來判別是否發出請求客戶端導向首頁的請求，若結果是
      // 成功的話，就發出導向請求，失敗的話，就印失敗訊息
        .then(() => res.redirect('/'))
        .catch(error => console.log(error))
    })
  ```
  - Model：接收Controller的對應路由之請求、Model 收到請求便轉成資料庫指令向MongoDB資料庫發出對應請求、接收資料庫的執行結果並轉送結果至Controller