---
sidebar_position: 5
---

# sequelize 簡介


1. 安裝sequelize 本身和其所依賴的套件sqlite3，另外為了很好地從CLI能夠控制sequelize套件，而安裝sequelize-cli
```
npm install sequelize sqlite3 sequelize-cli
```


## npx sequelize init  
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

