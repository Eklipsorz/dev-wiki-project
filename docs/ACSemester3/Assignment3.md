---
sidebar_position: 22
---

# 作業3獲得的新知和技術

## ``效果
1. ``是製作一個字串模板的特殊符號，內部肯定會有模板上的固定字元、可變動字元(模板上的凹槽形狀)，而可變動字元會是由表達式所構成，而這表達式會是JS的表達式
```
`${expression}`
```

2. 當系統遇到表達式時，會先執行表達式來得到對應內容，最後呼叫類似toString的形式來轉換成字串，當形成字串之後，就會串連其他固定字串或者其他可變動字串來合併成一個大字串輸出
```
`${(expression).toString()}
```

3. 添加``和沒添加之間的差別：  
```
console.log(expression)           // 系統會執行expression並取出回應值來印出
console.log(`${expression}`)      // 系統會執行expression並取出回應值，接著轉換成字串做最後的印出
```


## === 對於物件的處理
1. 對於非物件的資料型別，會檢查雙方(a和b)的型別是否一致，若一致就繼續檢查兩者的值是否一樣
```
a === b
```
2. 對於物件的資料型別，會檢查雙方的型別是否都為物件，若都是物件的話就透過參照位址檢查雙方是否為同一個物件


```
const obj1 = {
  property1: 123
}

let obj2 = {
  property1: 123
}

console.log(obj1 === obj2) // return false
```

```
const obj1 = {
  property1: 123
}

let obj2 = {
  property1: 123
}

obj1 = obj2

console.log(obj1 === obj2) // return true
```


## mongoose.Types.ObjectId
1. 實際透過mongoose和其schema所建立的objectId類別下之實例，跟專門定義mongoose schema下的objectId不同(如Mongoose.ObjectId或者Shema.Types.ObjectId)
2. 換言之，它是每一筆資料下的_id所擁有的原型prototype，每當透過schema建立資料時，其資料下的_id會被賦予了該原型。

3. 該原型的new 建構式會是如下，其id為實際的Object ID數值，只是用著mongoose.Types.ObjectId來包覆著

```
new ObjectId(id)
```

4. new ObjectId() and new ObjectId and ObjectId() 皆為代表著ObjectId原型下的實例
[What is the difference between new ObjectId() and new ObjectId and ObjectId()?](https://stackoverflow.com/questions/44265981/what-is-the-difference-between-new-objectid-and-new-objectid-and-objectid)

```
  // Duck-typing to support ObjectId from different npm packages
  if (id instanceof ObjectID) return id;
  if (!(this instanceof ObjectID)) return new ObjectID(id);
```
查明誰是this


## Mongoose find 細節
1. Mongoose find 無法直接使用objectId("....")來代表_id搜尋，必須是以_id.toString()才能對應
2. ObjectId.equals(ObjectId) 可比對兩個ObjectId物件上所代表的ID是否一樣，若一樣的話，會回傳true，不一樣就會回傳false
```
item._id.equals(record.categoryId)
```
## regex 抓不到Query String
1. 若將regex應用至express上的路由上，會因為系統會自動獲取Query String內而在設定上無法透過regex來撈取Query String
2. 案例：拿下面來說，預計會抓取到GET locathost/?category=xxx這請求，但由於設定上不允許對應，所以這路由處理會是失效的，不論請求是否如預期那樣時，皆無法觸發以下請求
```
router.get(/^\/\?category\=.*$/, (req, res) => {
  .....
})
```

3. 參考資料
[ObjectID()](http://mongodb.github.io/node-mongodb-native/api-bson-generated/objectid.html#equals)

## handlebars parent context
1. handlebars 會依據block helper的切分而將渲染檔案切割成parent context和child context，其中child context會是每個block helper能夠處理的內容，當這些helper執行時，往往只能夠辨識它們能理解的關鍵字(如this、this相關的變數、root、../等)，若遇到變數名稱，會直接視為空值或者不存在。

2. parent context則是這些child context的上一層內容或者說包含block helper 的內容，其結構可以如下，首先最外層的context由於沒被任何block helper包含著，所以可以直接存取response物件所賦予的locals變數，第一個被block helper包住context A則只能夠使用該helper能夠辨識或者存取的變數，不能夠存取locals變數，context A也有部分內容被block helper包含著，該context也同樣地被限制在helper 所能夠辨識的

```
context               // parent of parent context A
{{#each test}}        
  context             // parent context A
  {{#each comments}}
    context          // child context of parent context A
  {{/each}}
{{/each}}
```

3. 由這些來區分內容的話，最外層的context會被稱之為root context或者initial context，而剩下就是某某context的child context來區分。

4. 若要在child context存取parent context所擁有的locals變數，可以使用以下方法：
  - 使用@root.指明要存取最上層的initial context所擁有的變數
  - ../ 使用parent context所能夠辨識的變數內容，../可以重複疊加，比如../../就指名要parent context的parent context。


5. 舉例：

```
{ 
   rootPath: '../../../', 
   navigation: { all: { 'a': { sublinks: [Object] }
}

Here is your rootPath variable: {{rootPath}}

{{#each navigation.all}}

    top-level rootPath 1: {{../rootPath}}      <!-- works -->
    top-level rootPath 2: {{@root.rootPath}}   <!-- works -->

    {{#if sublinks}}

            {{#each sublinks}}
                sub-level rootPath 1: {{@root.rootPath}}  <!-- works -->
                sub-level rootPath 2: {{../../rootPath}}  <!-- works -->
            {{/each}}

    {{/if}}

{{/each}} 
```

結果為如下：
```
Here is your rootPath variable: ../../../ 

top-level rootPath 1: ../../../ 
top-level rootPath 2: ../../../ 

sub-level rootPath 1: ../../../ 
sub-level rootPath 2: ../../../
```

首先最外圍的context是inital context，所以能直接讀取rootPath的變數值來印出，而下面則是由block helper所組成的child context，第一個block helper是迭代著 navigation物件下的all下屬性值對應的內容，目前是擁有屬性a的物件，而屬性a對應的值是擁有sublinks屬性的物件，在這裡的top-level rootPath 會透過root和../來分別讀取到最外圍context所能存取的變數以及上一層所能讀取的變數，並於最後印出，接下來的each helper讀取到{{#if sublinks}}時，會是以this.sublinks來解析，而this正是指著屬性a所對應的物件，最後的sub-level rootPath 1則是透過root和../..來分別讀取到最外圍的context所能存取的變數以及上一層的上一層所能夠讀取的，也就是兩層each以外的最外層變數
```
'a': { sublinks: [Object] }
// 相等於 a: { sublinks: [Object] }
```


6. 細節：
  - {{#if variable}} 和 {{#each variable}}的variable皆屬於同一層
  ```
  {{#if variable}}
    {{#each variable}}
        context
    {{/each}}
  {{/if}}
  ```

 - each helper內使用另一個each helper時，系統會預設使用this.property來判定另一個each helper所假定的variable以及each helper面對物件時會是以屬性來指派給this，示意如下

 ```
  // route
  const array = {
      subarray: {
        testvariable: {
          var1: 12
        }
      }
    }

  // inside template file

  {{#each array}} // 讀取array內容
  // 這裡的this會是以property來讀取，所以在這裡會是對應至array.subarray這物件
  {{#each testvariable}} // 會以this.testvariable來讀取
  {{this}}  // 這裡的this會是以property來讀取，所以會是對應至testvariable.var1的內容
  {{/each}}
  {{/each}}
 ```


- {{#each variable}} ：當variable是陣列時，就依照索引值順序來引入至this，當variable是物件時，就依照屬性值先後順序來引入至this。
```
// route
  const array = [
    1, 2, 3, 4, 5, 6
  ]

// inside template file
{{#each array}}
  start
  {{this}}
  end
{{/each}}
```

結果為：
```
start 1 end start 2 end start 3 end start 4 end start 5 end start 6 end
```



```
// route
  const array = {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: 5,
    f: 6
  }

// inside template file
{{#each array}}
  start
  {{this}}
  end
{{/each}}
```

結果為：
```
start 1 end start 2 end start d end start 4 end start 5 end start 6 end
```


### parent context and child context 參考資料
1. [https://handlebarsjs.com/api-reference/data-variables.html#root](https://handlebarsjs.com/api-reference/data-variables.html#root)
2. [Handlebars Scope Issue: Can't access template variable from embedded `each`](https://stackoverflow.com/questions/32796559/handlebars-scope-issue-cant-access-template-variable-from-embedded-each)


## Bad Request 
1. 一種狀態碼所表示的狀態，描述著伺服器無法處理由客戶製造的錯誤請求(請求內出現錯誤的語法、格式、錯誤路由、錯誤的輸入內容)，對應狀態碼為400
2. passport.js 本地端的Bad Request 案例(如下)
  - 使用者未輸入完帳密就請求驗證

3. 設定passport.js本地端對應的Bad Request的flash message為badRequestMessage屬性值，要在路由那邊來設定，並且該訊息會使用flash所建立的error種類陣列來存放內容。
  ```
    router.post('/login', passport.authenticate('local', {
      failureFlash: true,
      failureRedirect: '/users/login',
      successRedirect: '/',
      badRequestMessage: 'You need to input account and password'
    }))

  ```
  [passport js missing credentials](https://stackoverflow.com/questions/34511021/passport-js-missing-credentials)

## Mongoose Query 執行方式
1. 靈感來源：透過mongoose官網對於Query執行過程的stack trace而發現幾件事情 
 
 
2. Query本身：Query 雖然本身不是原生promise，只是單純如同字面上的意思儲存資料庫操作指定和資料庫設定的物件，本身被單獨解析執行並不會真的執行Query內容，在這裡，我設定一個一定會報錯的Query來給予系統執行，

```
db.on('open',  () => {
 
// 什麼事情都不會發生，但理論上，這個Query一被執行起來就會報錯
  UserModel.findOne({ _id: 'User1' })	
 
})
```
若被執行的話，會報出以下的錯誤，但結果什麼事情都沒發生
```
UnhandledPromiseRejectionWarning: CastError: Cast to ObjectId failed for value "User1" (type string) at path "_id" for model "User”
```
3. 若添加await、then/catch/finally至Query時：可如果添加.then、.catch、await的話，mongoose就會偷偷在處理.then、.catch、await所要求的動作之前，系統會先透過Query.exec()來執行Query，而exec()本身會回傳著夾帶著Query執行結果的promise，好讓讓它應對後面的.then、catch、await。

若繼續延伸上一個例子時，添加await、catch、then在Query身旁，
```
// await:
db.on('open',  () => {
 
// mongoose 會先以exec()來執行Query
// 接著以exec()回傳後的promise來處理await
await UserModel.findOne({ _id: 'User1' })	
})


// then:
db.on('open',  () => {
 
// mongoose 會先以exec()來執行Query
// 接著以exec()回傳後的promise來處理then
 UserModel.findOne({ _id: 'User1' })	
    .then(....)
})

// catch:
db.on('open',  () => {
 
// mongoose 會先以exec()來執行Query
// 接著以exec()回傳後的promise來處理then
 UserModel.findOne({ _id: 'User1' })	
    .catch(....)
})
```


當系統解析Query時，mongoose會偷偷執行exec()產生而出現以下錯誤訊息
```
UnhandledPromiseRejectionWarning: CastError: Cast to ObjectId failed for value "User1" (type string) at path "_id" for model "User”
```

同時StackTrace也呈現著"系統會為了處理then、catch、await而先額外執行著exec()來執行Query"，此時的await、then、catch所出現的stacktrace，都有先經過exec()來執行Query

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1641919675/blog/promise/thenExample_o1cxlo.png)

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1641919675/blog/promise/catchExample_rv4k0d.png)

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1641919675/blog/promise/awaitExample_uldezm.png)

