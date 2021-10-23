---
sidebar_position: 3
---

# .bashrc 和 .bash_profile

.bashrc 或 .bash_profile 是bash shell所使用的設定檔，是當使用者以shell介面進行登入時，便會以檔案內容來為目前使用者設置專屬的shell執行環境(使用者所能使用的資源、指令、權限、介面等)，每一種支援bash shell的作業系統會從兩者選一個來當作設定檔的名稱或者以不同形式來同時使用這兩個檔名，整體來說，不一定就是固定使用特定的檔名。

## .bash_profile
一般來說當使用者以bash shell介面(含以該介面來遠端登入)登入時，會在出現登入成功畫面出現前來載入.bash_profile來設定目前登入的使用者所用的執行環境

### macOS環境下
macOS 環境比較特別，macOS本身是以Z shell來當作Shell環境，但實際上預設上會是以bash shell為主，並且使用.bash_profile來初始化每一次執行終端機的shell環境。

## .bashrc
若在X window 系統(圖形化系統)打開bash shell介面的話，會在系統呈現bash shell介面之前，會先載入.bashrc來設定

## bashrc中的rc
.bashrc 中的 rc 全名為 run commands，意思是指用來設定如何執行某些東西的指令，通常會以檔案的形式來封裝這些程式碼來定義執行其他程式所需的初始化

### 參考資料
1. [What is the difference between .bash_profile and .bashrc?](https://medium.com/@kingnand.90/what-is-the-difference-between-bash-profile-and-bashrc-d4c902ac7308)