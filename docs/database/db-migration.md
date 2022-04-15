---
sidebar_position: 6
---

DB migration
1. 是指ORM層面的migration
2. 誤解：通常增量是指增加新欄位(?)

實際上被incremental所形容的名詞會是代表改變的差異(changes)所擁有的增長性，這差異是指兩個版本之間的差異，可以是增加某個欄位，也可以是移除某個欄位，不論內容是否為增加、編輯、刪除，只要有所差異，皆為差異本身，不會因為內容為增加就變成incremental changes 或者因為內容為減少就變成decremental changes或者不是incremental changes。
> Migrations manage incremental, often reversible, changes to data structures in a programmatic way.
> In software engineering, schema migration (also database migration, database change management) refers to the management of incremental, reversible changes and version control to relational database schemas.
3. 用途：在保留原始資料的情況下進行migration(由開發者自行定義)

資料庫：
1. 完整性
2. 一致性

[What are database migrations?](https://www.prisma.io/dataguide/types/relational/what-are-database-migrations#what-are-database-migrations)


# Database Migration 
Database migration (別名為schema migration 或者 database change management)，目前是由ORM/ODM來實現，並非由資料庫管理系統本身來實現，其概念主要目的是為主要為了在 **盡量不破壞(PS. 只是盡量，並非完全能夠避免不破壞)原有表格資料的情況下**，來將每一個時間點下的表格架構進行版本控制來讓這些表格架構能夠重複使用、可根據執行環境來進行測試、共享於整個團隊。
> The goals of database migration software are to make database changes repeatable, shareable, and testable without loss of data


## 具體版本形式
在這裡的ORM/ODM會透過migration檔案來定義每一個版本所擁有的架構內容，根據內容形式可以進一步區分為
  - State based : 每一個版本是以狀態來描述，狀態會是以某些資訊或者類似形式的內容來代表每一個版本，狀態描述其版本下所具有的完整架構內容為何，接著實際做切換時，會以目前版本的架構和指定切換的架構之間的差異是如何來更改
  - Change based ：每一個版本是以版本之間的差異，當前版本內容會是前一個版本和目前內容之間的差異，所以通常會以連續的版本內容來構成最終版本下所對應的完整架構，近似於git版本中的Snapshot機制，
  
  
### 具體版本形式-例子 
  
  比如説一開始會使用檔案1來表示初始版本的架構，隨後下一個版本的實際內容是多出欄位4，所以檔案2的內容就是該差異-增加欄位4，最後再下一個版本中會與第二版的內容相差一個欄位3，也就是說第三版只有欄位1、欄位2、欄位4，那麼檔案3就會以差異來紀錄-移除欄位3
  ```
  migration 檔案1:
    欄位1、欄位2、欄位3
  migration 檔案2：
    增加欄位4
  migration 檔案3：
    移除欄位3
  ```

## 具體實現概念
不論其形式如何，具體實現概念會是允許開發者以偏好程式語言來開發對應的migration檔案，當執行對應其migration檔案會經由ORM/ODM而轉換成對應資料庫語法(如SQL)，接著再向對應的資料庫管理系統發送對應的資料庫語法，來建立對應的架構，其對應的語法會根據對應資料表格還是否擁有著資料而變動：
  - 若沒資料的話，就會以migration檔案

首先在這裡會假設資料庫會有N個資料表格(Table)，而ORM/ODM則有M個有關於表格2的版本，每一個版本都紀錄不同時期下的表格2架構內容，若ORM/ODM此時決定將表格

// 切換回寫例子
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650021326/blog/database/migration/db-migration-build-table1_vjnkjf.png)

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650021326/blog/database/migration/db-migration-update-table1_puwlsx.png)
由於ORM/ODM本身輔助原有資料庫的表格、紀錄來對應至某個程式語言下的物件之中間層軟體(負責轉換和對應)，本質上會有以下這兩種不同變動方向：
  - 第一個方向：ORM/ODM 和 原有資料庫管理系統 之間的對應和轉換
  - 第二個方向：某個程式語言下的物件 和 ORM/ODM 之間的對應和轉換

然而在這裡的Database migration會是以ORM/ODM來實現，而非第一個方向中的資料庫管理系統，很大的可能是為了要讓資料庫管理系統更好地專注在資料的管理，而改讓面向於實際開發者的ORM/ODM去專注在偏向於測試、協作的業務上-Database migration。


## ORM/ODM 破壞原有資料？

在這樣子的情況下，每一次由ORM/ODM進行migration時，

之所以只能以增量相關來管理表格架構，並非是因為實作上是不可行的，而是因為若以內容減量來管理的話，其包含的資料會因此而損失，比如假設Database Migration可以管理增減量相關的變更狀態，那麼拿以下架構來進行欄位上的減量：減少price這欄位的話
```
name  price   date
apple 10    4/13
orange 12 4/14
```
那麼就會是以下結果，接著再回到減少price欄位前的狀態：price欄位還在時的狀態，price欄位的所有資料有可能會永久遺失，當然在這情況下，可以**替Migration增加還原原有資料的功能**，但那時勢必會**需要每一個版本都儲存著那時原有資料的內容，甚至像git系統那樣：每一個版本都儲存前一版的差異，但不管如何，資料庫所需要的空間大小並不像git所需要的空間較小，除此之外，還要考量到資料同步上的問題會不會因為過於龐大、非同步、網路問題而產生分歧**，
```
name  date
apple 4/13
orange 4/14
```

因此為了避免不必要同步、永久遺失、永久遺失所需要做的補償，只允許Data migration控管表格架構(schema)下的增量相關管理。

## Database migration 的版本控管功能
基本概念會是以時間戳來紀錄每一個時間點下的表格架構，同時會區分成三種開發情景來設定不同情景下的連線設定，分別為：
  - 開發環境(development environment)：通常會以本地端的資料庫作為連線設定的基礎
  - 測試環境(test environment)：通常會以遠端的測試用資料庫作為連線設定的基礎
  - 線上環境(production environment)：通常會以正式上線用的資料庫作為連線設定的基礎
這三種環境下所能使用的版本通常都是共同的，這些版本的切換通常運用在開發環境和測試環境，因為這兩個環境所儲存的資料皆為彰顯是否實現功能開發以及其功能的效能是如何，資料本身並沒有太大的意義，但若應用於線上環境下，會因為儲存的資料皆為使用者相關的而使這些資料具有較大意義而無法輕易切換版本。


## Sequelize 的 Database migration
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