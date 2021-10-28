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

