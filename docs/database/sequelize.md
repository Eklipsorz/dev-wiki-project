---
sidebar_position: 5
---

# sequelize 簡介


1. 安裝sequelize 本身和其所依賴的套件sqlite3，另外為了很好地從CLI能夠控制sequelize套件，而安裝sequelize-cli
```
npm install sequelize sqlite3 sequelize-cli
```


## sequelize-cli 指令集合
該套件所提供的指令集合是輔助開發者/使用者能透過CLI方式以及.sequelizerc設定檔內容來設定/控制sequelize的執行狀態。若目前執行指令的所在有出現名為.sequezlizerc的檔案，那麼每當執行sequelize的指令集合時就會先讀取其檔案的內容來執行。
### .sequelizerc
.sequelizerc 是一份sequelize-cli用來參考使用的參數清單，清單下可以設定以下內容：
```
* env: The environment to run the command in
* config: The path to the config file
* options-path: The path to a JSON file with additional options
* migrations-path: The path to the migrations folder
* seeders-path: The path to the seeders folder
* models-path: The path to the models folder
* url: The database connection string to use. Alternative to using --config files
* debug: When available show various debug information
This is a special configuration file. It lets you specify the following options that you would usually pass as arguments to CLI:
```

通常若不存在.sequelizerc檔案的話，會按照sequelize預設設定來執行，所以若預設設定並不符合自己對於sequelize的執行方式需求時，使用時機點會是如下:
    - 你想要更改預設的migration路徑、model路徑、seeder路徑或者conifg路徑位置
    - 你想要將config.json更改成更名或者以js形式來進行，如config.js
```
Some scenarios where you can use it:
* You want to override default path to migrations, models, seeders or config folder.
* You want to rename config.json to something else like database.json
```

### sequelize-cli: init 指令
init 指令是主要會按照sequelize預設設定或者.sequelizerc設定來建立sequelize所需要的初始設定檔案和目錄，主要會有以下設定檔和目錄：
    - 連線設定檔，如conifg/config.json
    - 定義資料庫對應在ORM上的物件和設定資料庫在ORM對應物件上所會有的設定，如models/index.js
    - 定義資料
    - migrations目錄：資料庫遷徙設定檔的位置，預設會是空目錄
    - seeders目錄：資料庫的種子資料設定檔位置，預設會是空目錄
```
sequelize init
``` 
1. 對應sequelize的指令，而init則是指定的參數
2. sequelize init 主要會自動建立sequelize所需要的設定檔和目錄
3. 主要會建立的設定檔和目錄為：
    - config/config.json：資料庫設定檔
    - models/index.js：model 設定檔案
    - migrations目錄：資料庫遷徙設定檔的位置，預設會是空目錄
    - seeders目錄：資料庫的種子資料設定檔位置，預設會是空目錄

## sequelize config/config.json
1. 裡面存放三種開發時機點對於資料庫操作的設定檔，三種模式分別為Development 、Test、Production這三種
2. Development mode: 你自己在本地開發的模式，此時的資料庫通常就安裝在你的電腦裡，而且裡面幾乎都是假資料
3. Test mode：測試時候用的環境，資料庫需要配合測試案例的設計，來配套獨特的資料組合，把操作變因降到最小。
4. Production mode：產品上線以後，資料庫會在正式平台 (如 Heroku、AWS、GCP 等) 的模式，此時資料庫裡是真正的使用者數據
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1642693447/blog/database/sequelize/config.json_ydf3g3.png)
5. 每一種設定檔都可以定義不同的資料庫、使用者、密碼、其他設定
6. Sequelize 會透過config/config.json來與對應的資料庫做自動連線，不像mongoose都要開發者親自下達以及設定資料庫連線才能完成
7. 關於 operatorsAliases 這個設定，其實它在 Sequelize v5 以後就被棄用了，但在 sequelize init 腳本裡目前還是會自動帶入。如果沒有刪掉它，會在 server 上跳出 warning message。

