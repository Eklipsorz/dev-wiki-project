---
sidebar_position: 1
---

# 常見指令
在這裡會列幾個比較常用的指令，剩餘的指令會以進階指令作為新章節來說明。

## git init
1. 建立一個空的repo以及初始化相關的repo設定或者重新建立repo以及重設其設定，
2. 下達後便會以目前目錄為repo的根目錄，並建立.git等隱藏目錄，該目錄會存放git版本紀錄和設定檔
3. 語法形式為：

```
git init
```

## git add
將檔案/目錄設定成staged，並放入staging area中

## git commit
將目前staging area的內容以一個版本的形式儲存至repo，通常會搭配commit message來標明這次版本做了什麼，提交完之後，該版本會搭配訊息、相關資訊，最後在以sha-1編碼來生成對應該commit的數字，來方便系統辨識(以數字來代表commit本身)，而sha-1的生成規則則是將程式碼內容送進SHA-1的演算法來生成，結果會是40個16進制的亂數，只要程式碼有些許不同，其亂碼也會跟著改變，同時也能用前7碼來縮寫。

## git status

## git log

## git diff