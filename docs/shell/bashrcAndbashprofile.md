---
sidebar_position: 3
---

# .bashrc 和 .bash_profile
1. 是配置使用者以bash shell介面進行登入時所要載入的檔案內容，通常會在一登入該shell後就載入，並為目前使用者設定專屬的shell執行環境(使用者所能使用的資源、指令、權限、介面等)
2. 每一種作業系統會根據自己需求來決定要載入.bashrc或者.bash_profile，不一定就是固定某一種。
3. .bash_profile：一般來說當使用者以bash shell介面(含以該介面來遠端登入)登入時，會在出現登入成功畫面出現前來載入.bash_profile來設定目前登入的使用者所用的執行環境
4. .bashrc ：若在X window 系統(圖形化系統)打開bash shell介面的話，會在系統呈現bash shell介面之前，會先載入.bashrc來設定
5. macOS 環境比較特別，macOS本身是以Z shell來當作Shell環境，然而預設上會先以bash環境會用來初始化的檔案.bash_profile來進行初始化並設定在.zshrc中，
6. .bashrc 中的 rc 全名為 run commands，意思是指用來設定如何執行某些東西的指令，通常會以檔案的形式來封裝這些程式碼來定義執行其他程式所需的初始化

### 參考資料
1. [What is the difference between .bash_profile and .bashrc?](https://medium.com/@kingnand.90/what-is-the-difference-between-bash-profile-and-bashrc-d4c902ac7308)