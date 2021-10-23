---
sidebar_position: 1
---

# shell 介紹

## terminal
最早以前的電腦主機是需要一個由螢幕和鍵盤所構成的實體終端機(Terminal)來連接的，比如下圖中的終端機，而這個

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1634978075/blog/shell/Televideo925Terminal_t0n7ic.jpg)
一個名為終端機的實體裝置來當使用者可以使用的介面，使用者透過這個介面就能對電腦主機進行一些系統操作，而這個終端機是由螢幕和鍵盤所構成，所以使用者可以透過鍵盤來輸入電腦能看得懂的指令，而電腦可以透過螢幕來輸出執行

## Terminal vs Shell



### 終端機
1. 最早終端機是以實體裝置來呈現，只有螢幕和鍵盤的裝置，而這個裝置會另外連接至電腦主機，透過這個裝置可以輸入電腦主機可辨識的指令以及透過螢幕來顯示整體執行過程。
2. 現在則以軟體來替代終端機來替代，這個軟體會模擬成一個輸入介面以及可顯示執行過程的畫面


### Shell
1. Shell如其名，原意為外殼，在這裡指的是系統的外殼，使用者可透過這個外殼來與系統進行互動
2. Shell是由一組其本身所定義的指令以及一個負責將指令轉換至系統能解析並執行的解釋器所構成的系統，這系統會直接在終端機介面上執行。
3. 當使用者一登入終端機介面時，系統會指定一種Shell環境賦予使用者來與系統進行互動，只要在這個環境下輸入對應的指令便會透過其解釋器轉換成系統能執行執行的程式碼。

### Read–eval–print loop
1. 另一種Shell環境，只是所使用的指令會是某種程式語言的語法，而解釋器則是負責進行解析特定程式語言的解釋器來將使用者輸入的程式碼轉化為系統能讀懂的指令並執行
2. 在該介面可以允許使用者輸入並讀取(read)，接著解析並執行(eval)其內容，最後再印出(print)執行後的結果至Shell環境中

### 參考資料
1. [你明白 shell、bash 和 zsh 等词的真正含义吗？](https://zhuanlan.zhihu.com/p/34197680)


## .bashrc 和 .bash_profile
1. 是配置使用者以bash shell介面進行登入時所要載入的檔案內容，通常會在一登入該shell後就載入，並為目前使用者設定專屬的shell執行環境(使用者所能使用的資源、指令、權限、介面等)
2. 每一種作業系統會根據自己需求來決定要載入.bashrc或者.bash_profile，不一定就是固定某一種。
3. .bash_profile：一般來說當使用者以bash shell介面(含以該介面來遠端登入)登入時，會在出現登入成功畫面出現前來載入.bash_profile來設定目前登入的使用者所用的執行環境
4. .bashrc ：若在X window 系統(圖形化系統)打開bash shell介面的話，會在系統呈現bash shell介面之前，會先載入.bashrc來設定
5. macOS 環境比較特別，macOS本身是以Z shell來當作Shell環境，然而預設上會先以bash環境會用來初始化的檔案.bash_profile來進行初始化並設定在.zshrc中，
6. .bashrc 中的 rc 全名為 run commands，意思是指用來設定如何執行某些東西的指令，通常會以檔案的形式來封裝這些程式碼來定義執行其他程式所需的初始化

### 參考資料
1. [What is the difference between .bash_profile and .bashrc?](https://medium.com/@kingnand.90/what-is-the-difference-between-bash-profile-and-bashrc-d4c902ac7308)