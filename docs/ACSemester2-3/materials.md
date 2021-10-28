---
sidebar_position: 14
---


# 2-3 S2補充知識


## .gitignore
1. 指定不用追蹤/紀錄的檔案，並且讓git系統忽略掉他們
2. 會設定在對專案毫無幫助、容易產生巨大版本衝突、隱私資料，比如為node_modules 和 DS_Store，前者是存放目前專案所需要的套件，通常會存放大量目錄和檔案，若其他專案開發者安裝不同版本的套件，很有可能產生巨大的版本衝突，但又為了專案所需不得不折中以package.json 和package-lock.json來替代，然後再讓其他開發者根據這兩個檔案來更新或變更套件，而後者的DS_Store則是因爲該檔案只是macos 用來記錄當前目錄所潛藏的屬性，比如文件的圖標位置，對專案來說沒任何意義。
3. .gitignore 形式如以下，若以相對位置來表示會以.git所在的目錄為主。

```
Filename
Path/filename
```

## 良好GitHub 專案習慣為：
1. 設定.gitignore 來避免恐會造成大量版本衝突、對專案毫無幫助、隱私資料外洩，比如node_modules目錄、.DS_Store，那麼.gitignore內容為：
```
node_modules
.DS_Store
```
2. 若使用npm來安裝模組，請在package.json設定npm script來幫助其他開發者方便使用你的專案
3. 建立一份readme來告知專案的用途、版本、套件、平台等 


## npm run 
1. 是npm的指令之一
2. 用來執行package.json的scripts物件所擁有的指令屬性，形式會是如下，command會是scripts物件在package.json上的屬性
```
npm run <command>
```
3. 而scripts物件在package.json屬性會是：其中command1會是由字串構成的指令，而對應值會是commandValue1，當npm run command1時，便會間接呼叫本機端上的shell程式來執行command1的對應值，也就是讓shell程式去執行commandValue1

  "scripts": {
	command1: commandValue1, 
	command2: commandValue2,
				.
				.
				.
				.
   }

## bootstrap.js所需的函式庫
1. bootstrap.js 會依賴於 jquery.js 和 popper.js 這兩個函式庫，因此載入bootstrap.js前，要先載入那兩個函式庫
，形式會是如下，path1-3代表著存放這些函式庫的地方，瀏覽器或者負責執行的程式會從前面先執行。
```
<script src=/path1/jquery.js>
<script src=/path2/popper.js>
<script src=/path3/bootstrap.js>
```

## 函式物件的方法和屬性
1. 在JS中，由於函式本身會被當成物件來看待，所以嚴格來說可以像物件那樣替函式添加屬性和方法，然而不允許直接在函式內部添加屬性和方法，如：

```
function functName() {

	/* do something */
	parameter1: value1
}
```

2. 取而代之的方法為透過 pass by reference 或者以 exports 的形式來為函式物件增加屬性和方法，具體有三種方法：

 - 方法1：在同一檔案內，將函式物件以reference形式指派給其他變數，再透過該變數對於物件的屬性/方法增加方式來間接為函式物件增加屬性/方法，如下所示

 ```
 function functName() {
  	/* do something */
  }

 const testFunction = functName
 testFunction.parameter1 = value1

 ```

 - 方法2：不同檔案的情況下，其中一個檔案A要把函式輸出給另一個檔案B當模組內容使用，那麼檔案A會透過 exports 這內建物件來將函式物件的reference儲存並傳遞給檔案B的require之回傳值，由負責接收回傳值的變數來儲存，接著由於是reference的因素，所以透過變數對於物件的屬性/方法增加方式來間接為函式物件增加屬性/方法，如下所示： 

 ```
  /* Inside one.js */
  function functName() {
	  /* do something */
  }

  exports = module.exports = functName

  /* Inside two.js */

  const mod = require('./one')
  mod.parameter1 = value1

  console.log(mod)   
  /* 
    顯示結果為 [Function: functName] {
	      parameter1: value1
    } 
  */

 ```
 

 - 方法3：與方法2類似會是以模組內容的形式輸出給另一個檔案，不同的是先在輸出前於提供模組的檔案A間接對函式物件進行屬性/方法的增加，然後輸出其最後內容給另一個檔案B，形式如下：

```
  /* Inside one.js */
  function functName() {
	    /* do something */
  }

  exports = module.exports = functName
  exports.parameter1 = value1

  /* Inside two.js */

  const mod = require('./one')
  console.log(mod)   
  /* 
    顯示結果為 [Function: functName] {
	      parameter1: value1
    } 
  */
 ```