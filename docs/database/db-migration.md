---
sidebar_position: 6
---

# Database Migration 
1. 別名為schema migration 、database change management
2. Database migrate是指Database下某個schema內容能在盡量不破壞原有紀錄的情況下遷徙(變更)至其他時間點的schema內容，而Database migration則是對應其他時間點的schema內容
3. 在Sequelize中，Migration會是由儲存其內容的檔案來定義如何將內容轉換至某個時間點的內容或撤銷內容轉換，其檔案別名為Migration file。
> Just like you use version control systems such as Git to manage changes in your source code, you can use migrations to keep track of changes to the database. With migrations you can transfer your existing database into another state and vice versa: Those state transitions are saved in migration files, which describe how to get to the new state and how to revert the changes in order to get back to the old state.

4. 能在盡量不破壞原有紀錄的情況下是指若遷徙內容指定新增原本沒有欄位或者原本不存在的內容的話，會保有其他紀錄的欄位值，但若遷徙內容是指刪除原本存在的屬性，那麼復原之後(增加原本屬性後)，該屬性所存在的值全都因為刪除而永久性遺失，因此在這裏不一定完全能夠保證原本紀錄不會出現資料遺失的問題

5. 其技術在網頁開發框架中會是以ORM、ODM來產生對應Migration檔案來從而實現，最主要的作用是有兩者：
  - 使用ORM、ODM的Migration檔案可以憑藉著ORM和ODM所提供的語法來轉換成它們能夠支援的資料庫語法，進而產生對應資料庫效果，換言之，可以跨資料庫平台來產生/刪除/變更對應的資料表格上之schema
  - ORM、ODM所提供的Migration服務會根據其檔案的建立時間來做基本的版本管理以及方便回到指定版本下的資料庫schema內容