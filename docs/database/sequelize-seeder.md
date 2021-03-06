---
sidebar_position: 8
---

# sequelize 種子資料
種子資料產生器和schema migration機制擁有up和down函式，up函式是負責定義如何產生種子資料，而down函式事負責刪除對應的種子資料已恢復到產生前的狀態，seeder雖然有up和down函式，但本質上沒像schema migration檔案那樣標記著up狀態和down狀態，只會按照指令種類來執行對應的up函式或者down函式，所以每一個seeder的up函式/down函式能被重複執行，主要指令種類為：產生seeder、執行seeder的up函式、執行seeder的down函式
1. 建立種子資料產生器seeder，指令為如下，種子資料產生器名字會是以name為主
```
sequelize seed:generate --name name
```
2. 執行指定的sequelize seeder下的up 函式，指令為如下，需添加--seed來指定
```
// 格式：
npx sequelize db:seed --seed filename(需添加副檔名)

// 範例：
npx sequelize db:seed --seed 20220122173045-default-data2.js
```
3. 執行指定的sequelize seeder下的down函式，指令為如下：
  - 若沒添加--seed指定的話，會預設執行最近建立的seeder下的down函式
  - 若添加 --seed和對應檔案 就執行對應檔案的down函式
```
// 格式1: 執行最近建立的seeder下的down函式
npx sequelize db:seed:undo

// 格式2： 執行指定seeder檔案下的down函式
npx sequelize db:seed:undo --seed filename(需附加副檔名)

// 格式2範例：
npx sequelize db:seed:undo --seed 20220122165046-default-data.js
```

4. 執行所有seeder的up函式：會依照建立時間先後來執行，檔名上的時間越前面就越先被執行
```
npx sequelize db:seed:all
```

5. 執行所有seeder的down函式：會依照建立時間先後來執行，檔名上的時間越前面就越先被執行
```
npx sequelize db:seed:undo:all
```