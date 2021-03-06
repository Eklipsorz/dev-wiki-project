---
sidebar_position: 11
---


# sequelize ：model目錄下的index.js
1. 載入套件以及取得資料庫設定檔，套件載入主要會有fs、path、sequelize
```
// 載入fs、path、sequelize模組
const fs = require('fs')
const path = require('path')
const Sequelize = require('sequelize')

// 定義basename為目前執行檔案的名稱
const basename = path.basename(__filename)
// 取得目前環境為何？是開發階段的環境？還是部署上線階段的環境
const env = process.env.NODE_ENV || 'development'
// 在這裏__dirname為目前檔案所在的目錄路徑，另一個路徑為../config/config.json
// resolve會因為後者的..參數而將__dirname內的目前目錄跳過，在這原本是/xxx/xx/project/models
// 跳過後，就變成/xxx/xx/project/config/config.json，這剛好會取得config.json檔案內容
// 其內容會是一個陣列，分別為代表不同開發階段的development、production、travis這三個屬性
// 在這裏會依照env變數來決定其對應階段的設定檔內容
const config = require(path.resolve(__dirname, '../config/config.json'))[env]

// 定義我們目前進行連線的資料庫是為何
// 它會擁有各個表格對應的model
// 它會擁有sequelize本身的方法-操作model對應表格的方法
const db = {}
```



2. 建構一個綁定資料庫連線資訊的Sequelize實例，可藉由它來與對應資訊的資料庫進行連線以及資料操作
```
let sequelize
// 若設定config檔案有透過use_env_variable來告知有設定環境變數的話，就能使用指定環境變數下
// 的內容來取下連線的資料庫、資料庫使用者帳號、資料庫使用者密碼
if (config.use_env_variable) {
  sequelize = new Sequelize(process.env[config.use_env_variable], config)
} else {
  // 若沒有設定的話，就用config.json定下的的設定來建立Sequelize實體
  sequelize = new Sequelize(config.database, config.username, config.password, config)
}
```
若有設定use_env_variable，那麼其use.env.variable內容會是如下：
```
{
    "production": {
	  // 告知使用CLEARDB_DATABASE_URL這環境變數儲存production環境下的資料庫連線設定
    // 只需要存取該環境變數的內容就能知道
        "use_env_variable": "CLEARDB_DATABASE_URL"
    }
}
```
而CLEARDB_DATABASE_URL內容會是如下：  
  - username：使用者帳號
  - password：密碼
  - host：主機名稱
  - schema：要使用哪個資料庫/要使用哪個命名空間(包含哪些可用表格)
```
mysql://<username>:<password>@<host>/<schema>
```


3. 載入models下的所有對應資料庫表格的model至db變數，經由這步驟處理後，db的屬性名稱將會以model名稱為主，比如說model有user、todo，那麼db就會有user、todo這兩個屬性
```
fs
// 讀取目前被執行檔案之目錄所在，在這裏會是指models目錄，其結果會是該目錄的所有model和index.js
  .readdirSync(__dirname)
// 過濾出擁有.js副檔名的檔案，在這裏只會有對應特定資料表格的model.js
  .filter(file => {
    // 過濾條件為不為.filename 以及 file不為index.js本身 以及必須是.js檔案
    return (file.indexOf('.') !== 0) && (file !== basename) && (file.slice(-3) === '.js')
  })
// 載入每個對應model的類別並透過require來初使化它們於類別的設定
  .forEach(file => {
    // 透過model物件帶有的name屬性名稱來替db增加其model名字為主的屬性名字以及對應的model
    // path.join中的__dirname會是目前index.js所在的目錄(絕對路徑)，而file則是檔案名稱
    // 所以path.join實際上是將目前所在的目錄與所有model對應檔名做合併，然後透過require
    // 而拿到對應函式，model的name會依據該model下的modelName來決定
    // 其函式會是建構特定model的類別，會綁定存放資料庫設定的sequelize instance以及設定
    // 每個model下的屬性具有什麼樣的資料型別。
    // 這使得每次對model進行CRUD都會透過sequelize instance來找到對應的資料庫表格來操作
    const model = require(path.join(__dirname, file))(sequelize, Sequelize.DataTypes)
    db[model.name] = model
  })

```

4. 迭代db物件所擁有的model名稱來呼叫各model的associate來替每個model定義自己與其他model之間的關係。
```
// 由於db的屬性皆由model名稱所構成，所以在這裡會是迭代所有model
Object.keys(db).forEach(modelName => {
  // 檢測其model是否有設定關聯，若有就執行
  if (db[modelName].associate) {
    // 呼叫對應model的靜態方法-associate來定義自己model與其他model之間的關係
    db[modelName].associate(db)
  }
})
```

5. 將可以與資料庫連線的設定物件-sequelize和擁有對應資料庫的資料操作API的Sequelize套件綁定在db變數的屬性，並輸出其db，好讓db能
```
// 匯出需要的物件




// 定義我們目前進行連線的資料庫是為何
// 它會擁有各個表格對應的model
// db.sequelize物件夾雜著queryInterface、config
// queryInterface 是主要讓應用程式對應SQL的介面
db.sequelize = sequelize


// 它會擁有sequelize本身的方法-操作model對應表格的方法
// 也擁有著DataType、Validator、Model這三屬性
db.Sequelize = Sequelize

module.exports = db
```



補充知識：
1. __filename：
  - Node.js 內建的變數，會是指目前被執行的檔案所在(以絕對路徑呈現檔案位置)
2. __dirname：
  - Node.js 內建的變數，會是指目前被執行的檔案所在目錄(以絕對路徑呈現目錄位置)
3. path.basename(path)：
  - 為path模組的方法之一
  - 回傳路徑中的最後一個部分
  - 比如/dir1/dir2/file，basename('/dir1/dir2/file')會回傳file，
4. path.resolve(path1, path2, path3,....)
  - 為path模組的方法之一
  - 主要將path1、path2、path3..等路徑依照特定方式來解析成一個絕對路徑，該方式會從最右邊至左來處理路徑，首先會先在最左邊取得兩個路徑，處理完會得到結果字串，再與剩下還在右邊的路徑做處理，直到處理完畢
5. path.join(path1, path2, path3,....)
  - 為path模組的方法之一
  - 主要將path1、path2、path3..等路徑依照特定方式來解析成一個合併路徑，該方式會從最右邊至左來處理路徑，首先會先在最左邊取得兩個路徑，處理完會得到結果字串，再與剩下還在右邊的路徑做處理，直到處理完畢
6. fs.readdirSync(path[, options])
  - 為fs模組的同步方法之一
  - 以同步的形式來讀取path所在的目錄有什麼內容，並以陣列形式回傳該目錄所擁有的檔案和目錄
  - 舉例：該fs.readdirSync是放在index.js，並且執行index.js之後，
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1643045879/blog/forumProject/config/sequelize_apy9ki.png)
  其結果會是：
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1643045879/blog/forumProject/config/readdirSyncCmd_c4vxgd.png)