---
sidebar_position: 6
---

# Database Migration 
Database migration (別名為schema migration 或者 database change management)，目前是由ORM/ODM來實現，並非由資料庫管理系統本身來實現，其概念主要目的是為主要為了在 **盡量不破壞(PS. 只是盡量，並非完全能夠避免不破壞)原有表格資料的情況下**，來將每一個時間點下的表格架構進行版本控制來讓這些表格架構能夠重複使用、可根據執行環境來進行測試、共享於整個團隊。
> The goals of database migration software are to make database changes repeatable, shareable, and testable without loss of data


## 具體版本形式
在這裡的ORM/ODM會透過migration檔案來定義每一個版本所擁有的架構內容，根據內容形式可以進一步區分為
  - State based : 每一個版本是以狀態來描述，狀態會是以某些資訊或者類似形式的內容來代表每一個版本，狀態描述其版本下所具有的完整架構內容為何，接著實際做切換時，會以目前版本的架構和指定切換的架構之間的差異是如何來更改
  - Change based ：每一個版本是以版本之間的差異，當前版本內容會是前一個版本和目前內容之間的差異，所以通常會以連續的版本內容來構成最終版本下所對應的完整架構，近似於git版本中的Snapshot機制，
### 具體版本形式-例子 
  
1. State based migration：在這裡假設直接以白話來當作狀態描述每一個版本的完整架構內容，而每一版即為每一個檔案，一開始我們有欄位1、欄位2、欄位3，那麼就會以檔案1來代表，若第二版會是增加個欄位4，那就會以檔案2來代表，若第三版會是移除欄位3，那麼就會以檔案3來代表，說完這三個migration檔案的緣由，那麼若我們想自由切換版本的話：
  - 若我們想切換成第一版的話，會拿目前版本和代表第一版的檔案內容相比差異是為何，並根據差異來切換
  - 若我們想切換成第二版的話，會拿目前版本和代表第二版的檔案內容相比差異是為何，並根據差異來切換
  - 若我們想切換成第三版的話，會拿目前版本和代表第三版的檔案內容相比差異是為何，並根據差異來切換
  - 後面版本依此類推
```
migration 檔案1:
  欄位1、欄位2、欄位3
migration 檔案2：
  欄位1、欄位2、欄位3、欄位4
migration 檔案3：
  欄位1、欄位2、欄位4
```

2. Change based migration：假設有三個migration檔案，一開始會使用檔案1來表示初始版本的架構，隨後第二版的實際內容是欄位1、欄位2、欄位3、欄位4，第二版與第一版相差了欄位4，所以代表第二版的檔案2的內容就是該差異-增加欄位4，最後第三版的實際內容是與第二版內容相差一個欄位欄位3，也就是說第三版只有欄位1、欄位2、欄位4，那麼檔案3就會以差異來紀錄-移除欄位3，說完這三個migration檔案的緣由，那麼若我們想自由切換版本的話：
  - 若我們想切換成第一版的話，就會直接拿檔案1來代表
  - 若我們想切換成第二版的話，就會拿檔案1和檔案2來代表切換成第二版並給予系統去轉換成第二版的內容
  - 若我們想切換成第三版的話，就會拿檔案1、檔案2、檔案3來代表切換成第三版並給予系統去轉換成第三版的內容
  - 後面版本數依此類推
```
migration 檔案1:
  欄位1、欄位2、欄位3
migration 檔案2：
  增加欄位4
migration 檔案3：
  移除欄位3
```

3. 總結一下：
  - State based migration：是代表每一個版本的檔案都儲存該版本的完整內容，當要切換指定版本，就直接透過對應獨立檔案來切換成對應的版本
  - Change based migration：是代表每一個版本的檔案都會儲存前一版和目前版本之間的差異，當要切換指定版本，就必須透過一連串的檔案合併才能切換成對應的版本

## 具體實現概念
不論其形式如何，ORM/ODM具體實現概念會是以**版本內容增量為目標來實現版本切換**，在這裡會是允許開發者以偏好程式語言來開發對應的migration檔案，當執行對應其migration檔案會經由ORM/ODM而轉換成對應資料庫語法(如SQL)，接著再向對應的資料庫管理系統發送對應的資料庫語法，來建立對應的架構，其對應的語法會根據對應資料表格是否在資料庫且是否擁有著資料而變動：
  - 若沒資料的話，ORM/ODM就會以目前所拿到的migration檔案來構建對應的表格X'來存在暫存空間，並檢查資料庫是否有對應資料庫和是否有資料，若都沒有的話，會取代掉在資料庫上同名的表格X或者直接建立
  - 若有資料的話，ORM/ODM就會先以目前所拿到的migration檔案來構建對應的表格X'來存在暫存空間，並檢查資料庫是否有對應資料庫和是否有資料，若有的話，就盡可能從資料庫找到對應的表格X來獲取可以填入新表格X'的欄位資料，並將資料填入X'，隨後用X'來替代資料庫上的表格X。


### 具體實現概念-例子
首先在這裡先假設資料庫沒有存在任何資料表格，開發者為了建立表格1而用偏好語言X來定義migration檔案，也就是第一版的表格1，隨後給予ORM/ODM執行並轉換實際對應的資料庫語法來在資料庫管理系統執行對應的操作，其對應操作會是從下圖中的第二步驟開始，會先依照對應的migration檔案來建構對應的表格1'，並檢查資料庫上的有沒有表格1是有資料的，若沒有的話，會將表格1‘寫入至資料庫上並構成表格1
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650021326/blog/database/migration/db-migration-build-table1_vjnkjf.png)


若接下來開發者想建立版本2的表格1，就會建立新的migration檔案來對應該版本的表格，並給予ORM/ODM執行和轉換對應的資料庫語法來向資料庫管理系統發送對應操作，其對應操作也會是從下圖中的第二步驟開始，首先一開始會依照對應的migration檔案來建構對應的的表格1'，並檢查資料庫上的有沒有表格1是有資料的，若有的話，會將表格1的資料盡量填入表格1'，在這裡會將apple、orange、noodle等資料寫入至表格1'，等寫入完成之後，就將表格1'去取代資料庫上的表格1。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650021326/blog/database/migration/db-migration-update-table1_puwlsx.png)

## Database migration 的版本控管功能
基本概念會是以時間戳來紀錄每一個時間點下的表格架構，同時會區分成三種開發情景來設定不同情景下的連線設定，分別為：
  - 開發環境(development environment)：通常會以本地端的資料庫作為連線設定的基礎
  - 測試環境(test environment)：通常會以遠端的測試用資料庫作為連線設定的基礎
  - 線上環境(production environment)：通常會以正式上線用的資料庫作為連線設定的基礎
這三種環境下所能使用的版本通常都是共同的，這些版本的切換通常運用在開發環境和測試環境，因為這兩個環境所儲存的資料皆為彰顯是否實現功能開發以及其功能的效能是如何，資料本身並沒有太大的意義，但若應用於線上環境下，會因為儲存的資料皆為使用者相關的而使這些資料具有較大意義而無法輕易切換版本，原因在於**ORM/ODM來處理欄位時會因為移除欄位而損失資料**。

### ORM/ODM 破壞原有資料？
在切換版本的過程，難免會移除掉某些欄位X，在這樣子的情況下，ORM/ODM若切換至欄位X還在時的版本內容，其對應的欄位的資料並不會跟著復原，比如說拿以下架構來進行欄位上的減量：減少price這欄位的話
```
name  price   date
apple 10    4/13
orange 12 4/14
```
那麼就會是以下結果，
```
name  date
apple 4/13
orange 4/14
```
接著再回到減少price欄位前的狀態：price欄位還在時的狀態，price欄位的所有資料會因為**ORM/ODM只支援增量上的版本而永久遺失**。
```
name   date price
apple  4/13
orange 4/14
```

## Why 探討:版本內容增量為目標來實現版本切換?
從上述例子可以窺見 **版本內容增量為目標來實現版本切換的ORM/ODM** 在處理欄位移除時會有損失資料的可能。

在這裡這些ORM/ODM之所以只能以增量相關來管理表格架構，並非是因為實作上是不可行的，而是因為若以內容減量來管理的話，ORM/ODM或者資料庫管理系統是必須要替每一個版本額外備份或者花費CPU成本去計算哪些欄位資料要額外備份，來方便返回至資料移除前的版本，不論是選擇哪一樣，那時勢必會**需要每一個版本都儲存著那時原有資料的內容，甚至像git系統那樣：每一個版本都儲存前一版的差異，但不管如何，資料庫所需要的空間大小並不像git所需要的空間較小，除此之外，還要考量到資料同步上的問題會不會因為過於龐大、非同步、網路問題而產生不一致**。

而內容增量管理的話，由於本身就是會拿原本在資料庫的資源來附加至對應表格，所以會需要空間來存放需要附加的資料以及預設

並不需要像內容減量那樣需要額外成本來儲存和計算需要附加的資料，當然若還原至資料附加前的版本，也只需要表格上的替代(附加前的版本是沒資料)或者從其他欄位獲取其他資料就能完成(附加前的版本是有其他資料)，而資料同步問題的話，難免會因為會與原表格的資料有所出入，比如說在搬移過程，原表格有被添加幾筆紀錄，但比起減量而言，由於本身不需要額外成本去儲存附加資料(換言之，資料轉移至目的地(目標表格)的距離比起減量而言還要短)，所以不一致問題比較輕微。

總結一下：
  - 減量的話，除了基本的暫存空間來存放未來要切換的版本架構以外，會需要額外(時間和空間)成本來定義需要還原的資料是哪些以及儲存該資料；然後由於需要從另外空間將資源還原至原始表格上，多多少少會有不一致的問題，甚至由於還原資料過於龐雜而使這問題更加嚴重
  - 增量的話，除了基本的暫存空間來存放未來要切換的版本架構以外，並不需要(時間和空間)額外成本來存放需要(處理)附加的資料，取而代之的是從對應表格取出對應資料來放在暫存空間的表格上；然後由於需要從另外空間將資源還原至原始表格上，多多少少會有不一致的問題。


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