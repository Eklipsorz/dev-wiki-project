---
sidebar_position: 7
---


# Sequelize 的 Database migration
在Sequelize 中主要會有
由哪些指令來提供？
這些指令做什麼功用？如何使用





3. 在Sequelize中，Migration會是由儲存其內容的檔案來定義如何將內容轉換至某個時間點的內容或撤銷內容轉換，其檔案別名為Migration file。
> Just like you use version control systems such as Git to manage changes in your source code, you can use migrations to keep track of changes to the database. With migrations you can transfer your existing database into another state and vice versa: Those state transitions are saved in migration files, which describe how to get to the new state and how to revert the changes in order to get back to the old state.

4. 能在盡量不破壞原有紀錄的情況下是指若遷徙內容指定新增原本沒有欄位或者原本不存在的內容的話，會保有其他紀錄的欄位值，但若遷徙內容是指刪除原本存在的屬性，那麼復原之後(增加原本屬性後)，該屬性所存在的值全都因為刪除而永久性遺失，因此在這裏不一定完全能夠保證原本紀錄不會出現資料遺失的問題

5. 其技術在網頁開發框架中會是以ORM、ODM來產生對應Migration檔案來從而實現，最主要的作用是有兩者：
  - 使用ORM、ODM的Migration檔案可以憑藉著ORM和ODM所提供的語法來轉換成它們能夠支援的資料庫語法，進而產生對應資料庫效果，換言之，可以跨資料庫平台來產生/刪除/變更對應的資料表格上之schema
  - ORM、ODM所提供的Migration服務會根據其檔案的建立時間來做基本的版本管理以及方便回到指定版本下的資料庫schema內容





## SequelizeMeta 表格
1. 儲存所有已被執行過的Migration 檔案之檔名
2. 主要用途為：
  - 輔助Undo來挑選最近執行過的Migration檔案，當對某個Migration檔案執行完Undo之後便從SequelizeMeta刪除該Migration檔案名稱
  - 比對目前目錄下的Migration 檔案是否為未執行過
3. 儲存Migration 檔案名稱的方式為，最近的Migration 檔案會放到表格最下面，然後執行Undo時，會從表格最下面挑選，近似於Stack的方式來決定，越後越先處理

## npx sequelize db:migrate
1. 功能為執行對應時間點的遷徙檔案內容(紀錄)來得到其遷徙結果，具體而言會執行遷徙檔案內容中的up函式來獲取結果
2. 流程會有：
  - 確定sequelize連線到的資料庫是否存在Sequelizemeta表格，若不存在就先建立名為Sequelizemeta的表格
  - 將目錄下所有處於down狀態的migration 檔案與SequelizeMeta表格所列出的檔名比對，看哪個檔案名稱不存在，就確定要執行migrate是他們這些不存在SequelizeMeta表格內
  - 從第二步驟確定的遷徙檔案來建立對應的表格：先獲取被確定執行的遷徙檔案，執行檔案數可以大於1，接著執行這些遷徙檔案內的up函式，執行完畢後，將對應的遷徙檔名紀錄在SequelizeMeta表格內以及標記為up狀態，以此標記完系統已執行完畢並定義其undo的順序，
  
3. 若是執行由model:generate而產生的migration 檔案，則是會獲取檔案對應的model名稱並強制轉換小寫加複數型態的名稱當被建立的表格名稱，比如model為Todo，而被建立的表格名稱會是todos。
```
npx sequelize db:migrate
```
4. 參考資料
[db:migrate](https://sequelize.org/master/manual/migrations.html#creating-the-first-model--and-migration-)



## npx sequelize db:migrate:undo
1. 功能為執行對應時間點的遷徙檔案內容(紀錄)來復原其遷徙結果，具體而言會執行遷徙檔案內容中的down函式來復原結果
2. 主要執行方式有三種：
  - 系統自行決定哪個處於up狀態的migration檔案來執行其down函式，在這裏會從Sequelizemeta資料表格中挑選一個最近執行的migration檔案，而該檔案名稱會在其表格之最下面，執行完該檔案的down函式後，便從Sequelizemeta資料表格刪除對應的migration檔案以及設定該檔案為down的狀態。
  ```
  npx sequelize db:migrate:undo
  ```
  - 系統自行決定哪個處於up狀態的migration檔案來執行其down函式，在這裏會從Sequelizemeta資料表格中挑選所有處於up狀態的migration檔案來執行他們的down函式，執行完之後就便從Sequelizemeta資料表格刪除所有對應的migration檔案以及設定所有檔案為down的狀態。
  ```
  npx sequelize db:migrate:undo:all
  ```

  - 由開發者決定哪個處於up狀態的migration檔案(由name來決定)來執行其down函式，只要系統一執行完down函式，就會從Sequelizemeta資料表格刪除對應的migration檔案以及設定對應檔案為down的狀態。
  ```
  npx sequelize db:migrate:undo:all --to name
  ```

## npx sequelize db:migrate:status
1. 顯示sequelize 對於目前資料庫的遷徙狀態，具體來說會顯示每個migration檔案的執行狀態，主要會有up和down狀態：
  - up 狀態：若檔案為這個狀態，代表系統已經執行該檔案的up函式並獲取對應的遷徙狀態
  - down 狀態：若檔案為這個狀態，代表系統已經執行該檔案的down函式並復原至對應的遷徙狀態或者檔案本身沒被以up或者down的形式來執行，只是呈現的遷徙狀態很像是執行該遷徙檔案內的down函式所會有的狀態。
2. 舉例：假設有三個migration檔案分別為migration1、migration2、migration3，其中migration1和migration2已經被db:migrate給執行，而migration3未被執行，那麼db:migrate:status的結果會是顯示為
  ```
  up migration1
  up migration2
  down migration3
  ```
  



## migration 檔案產生方式
1. 建立Model時，會自動建立對應的migration檔案來在資料庫系統產生對應的資料表格，格式如下：
```
npx sequelize model:generate --name name --attributes attribute1:type1,attribute2:type2,...
```
2. 手動建立migration檔案，格式如下：migration:generate指定產生migration檔案，而對應的檔名為時間戳記以及name。

該檔名會是以ORM背後對應的資料表格名稱之單數形式，之所以單數形式是因為ORM物件只不過是對應特定表格的物件且肯定是在某個特殊程式語言下的物件，那麼貿然用複數來像**資料庫用一個複數來形容包覆著多筆同樣資料的表格**這行為可能會違反團隊的開發風格。
[表名應該複數化嗎?是用Person, Persons, People還是People?](https://kknews.cc/zh-tw/code/n6ark83.html)
```
npx sequelize migration:generate --name name 
```
3. 每個migration 檔案只會被執行一次，若要再被執行，只有執行Undo或者執行完Undo緊接著執行相同的migration檔案
```
// 從Sequelizemeta資料表格取出最近的migration檔案來執行其對應的undo函式/對應程式碼模組
npx sequelize db:migrate:undo

// 執行migration檔案的undo並使其檔案為down狀態(未被執行的狀態)，接著在執行同個migration檔案
npx sequelize db:migrate:undo
npx sequelize db:migrate
```



## migration 檔案內容
1. 每一個migration內容主要具有兩個輸出函式，可由開發者自行定義：
  - up : 定義如何執行migrate
  - down : 定義如何從當前migrate的執行結果復原至執行migrate之前。
  > A Migration in Sequelize is javascript file which exports two functions, up and down, that dictate how to perform the migration and undo it
  - up和down舉例：假設up為新增一個欄位X、更改欄位Y的名字等，那麼down就是刪除欄位X、恢復欄位Y的原本名字。假設up 的指令是 createTable，那麼down就為dropTable。
  - 主要migrate的內容會是修改model對應資料表格的schema
  - 這兩個函式會由sequelize系統自行呼叫，不能夠手動呼叫

2. migration 檔案內容如下：
```
'use strict';

module.exports = {

  // 定義up function內容
  // 其中queryInterface 物件是Sequelize中負責提供資料庫存取API來給開發者使用的物件
  // ，由它來具備修改資料庫的方法
  // Sequelize 物件就是Sequelize本身
  async up(queryInterface, Sequelize) {
    // ....
  },
  // 定義up function內容
  // 其中queryInterface 物件是Sequelize中負責提供資料庫存取API來給開發者使用的物件
  // ，由它來具備修改資料庫的方法
  // Sequelize 物件就是Sequelize本身
  async down(queryInterface, Sequelize) {
  }
}
```
3. 範例： 實際定義up和down的行為會以await形式來執行，其中addColumn是向指定表格增加指定欄位，而removeColumn則是向指定表格刪除指定欄位
```
'use strict';

module.exports = {
  async up(queryInterface, Sequelize) {
    /**
     * Add altering commands here.
     *
     * Example:
     * await queryInterface.createTable('users', { id: Sequelize.INTEGER });
     */

    // addColumn(table, key, option)
    // table 指定表格名稱(要以實際出現資料庫的表格名稱為主)
    // key 指定新增的欄位名稱
    // option 物件為指定欄位帶有什麼類別/性質、限制、預設值
    await queryInterface.addColumn(
      'users',
      'phone',
      {
        allowNull: false,
        type: Sequelize.STRING
      })
  },

  async down(queryInterface, Sequelize) {
    /**
     * Add reverting commands here.
     *
     * Example:
     * await queryInterface.dropTable('users');
     */
    // removeColumn(table, key)
    // table 指定要刪除欄位的資料表格
    // key 指定要刪除的欄位
    await queryInterface.removeColumn(`users`, `phone`)

  }
};

```


## 補充：model.init 設定
1. model.init 是sequelize 是透過初始化設定來產生特定model類別，而特定model類別會繼承於通用model類別，而在這裡的設定會是以通用model類別會有的屬性來初始化，具體會定義：
  - 定義特定model類別下對應的資料表格會有哪些欄位或者屬性
  - 定義特定model類別下的建構式所擁有的屬性和設定
  - 建構式則是用來構成對應資料表格的實例
2. model.init形式會是：
  - attributes是定義對應的資料表格會有哪些欄位或者屬性
  - options是定義對應的建構式所擁有的屬性和設定
```
 init(attributes: object, options: object): Model
```

3. 常見的options值如下：
  - options.modelName: 為字串，定義model的名稱，**用途為：用model類別和model類別實例來操作資料時所要用識別資料**
  > Set name of the model. By default its same as Class name.
  - options.tableNmae： 為字串，定義model所對應的資料表格名稱是為何，預設下若沒設定，會將options.modelName當作options.tableName，**用途為：用model類別和model類別實例來操作資料時所要用識別資料**
  - options.underscored: 為布林值，true代表啟用某功能；false代表不啟用某功能，啟用是指**啟用自動將該model下的屬性名轉換換成snake case命名法的形式**，但除了queryInterface是sequelize直接用SQL與資料庫連接以外，該介面會提供一系列的API，但這些API主要是直接對應著SQL，不像一般model使用方法那樣，還要經由sequelize本身的model來處理，才能轉換成對應SQL與資料庫處理。
  > Add underscored field to all attributes, this covers user defined attributes, timestamps and foreign keys. Will not affect attributes with explicitly set field option
  - options.freezeTableName：為布林值，true代表啟用某功能；false代表不啟用某功能，啟用是指**強制將取得的model name轉換成複數形式來表示，若不啟用就不轉換**
  > If freezeTableName is true, sequelize will not try to alter the model name to get the table name. Otherwise, the model name will be pluralized
4. 參考資料
[model.init](https://sequelize.org/master/class/lib/model.js~Model.html#static-method-init)



## Sequelize: 設定欄位的型別：


### Sequelize.DataTypes 和 Sequelize 在指定型別上？
Sequelize.DataTypes 和 Sequelize 這兩者本身可以設定欄位本身特定型別，且都皆為一樣
```
Sequelize.DataTypes.STRING
Sequelize.STRING
```

實驗：若載入sequelize並儲存至Sequelize物件，並比對Sequelize.DataTypes上的STRING 和 Sequelize.STRING是否一樣以及印出兩者內，結果都是一樣
```
const Sequelize = require('sequelize')
console.log(
  'test result:',
  Sequelize.DataTypes.STRING === Sequelize.STRING,
  Sequelize.DataTypes.STRING,
  Sequelize.STRING
)
```

結果為如下：
```
test result: true [class STRING extends ABSTRACT] {
  types: {
    postgres: [ 'varchar' ],
    mysql: [ 'VAR_STRING' ],
    mariadb: [ 'VAR_STRING' ],
    sqlite: [ 'VARCHAR', 'VARCHAR BINARY' ],
    mssql: [ 231, 173 ],
    db2: [ 'VARCHAR' ],
    snowflake: [ 'VAR_STRING' ]
  },
  key: 'STRING'
} [class STRING extends ABSTRACT] {
  types: {
    postgres: [ 'varchar' ],
    mysql: [ 'VAR_STRING' ],
    mariadb: [ 'VAR_STRING' ],
    sqlite: [ 'VARCHAR', 'VARCHAR BINARY' ],
    mssql: [ 231, 173 ],
    db2: [ 'VARCHAR' ],
    snowflake: [ 'VAR_STRING' ]
  },
  key: 'STRING'
}
```

另外DataTypes的新增原因是為了在使用習慣上能夠很好地使用才添加的。

> DataTypes is: A convenience class holding commonly used data types.

[Sequelize: difference of DataTypes and Sequelize](https://stackoverflow.com/questions/47563038/sequelize-difference-of-datatypes-and-sequelize)
