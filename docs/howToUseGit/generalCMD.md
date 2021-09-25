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
1. 將檔案/目錄設定成staged，並複製至staging area中
2. 當對內容下達git add指令後會是如下，其中兩個content會被複製至staging area中，準備被提交至repo。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632562545/blog/git/anExample2OfGitStorage_s1tpcw.png)
3. 語法形式為以下，第一種是對目前所在目錄下達git add，第二種則是對某個特定檔案進行git add
```
git add .
git add filename1
```


## git commit
1. 將目前staging area的內容以一個版本的形式儲存至repo
2. 通常會搭配commit message來標明這次版本做了什麼，提交完之後，該版本會搭配訊息、相關資訊，最後在以sha-1編碼來生成對應該commit的數字，來方便系統辨識(以數字來代表commit本身)，
3. sha-1的生成規則則是將程式碼內容送進SHA-1的演算法來生成，結果會是40個16進制的亂數，只要程式碼有些許不同，其亂碼也會跟著改變，同時也能用前7碼來縮寫。
4. 當對工作目錄下達git commit時會是如下，兩個content會以一個版本的形式去存放至repo作為最新的版本紀錄。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632562545/blog/git/anExample3OfGitStorage_gqcrun.png)

5. 語法形式為以下，其中m代表要將訊息賦予這次提交內容，後面會接著字串，這裡的message並非是一定要填，只是顯示用途。

```
git commit -m 'message'
```


## git status
1. 查看working tree下的檔案/目錄狀態(只針對git能辨識的狀態)，會顯示的狀態在狀態篇有提到
2. 語法形式為以下：
```
git status
```

## git log
1. 查看目前repo的所有提交過的版本和相關訊息
2. 語法形式為以下，第一種會顯示每一個版本的完整紀錄，第二種則是以圖形來顯示每一個版本之間的關係，第三種則是以將每一個版本的訊息濃縮成一行來顯示。

```
git log
git log --graph
git log --oneline
```

## git diff
1. 查看指定版本之間的內容差異
2. 會以+和-來顯示內容增加和內容減少
2. 語法形式為以下，第一種會拿最新版本的內容與目前工作目錄下的內容作比較，第二種則是以sha-1碼來找到對應的版本來比較，這兩個sha-1碼對應的版本會是不一樣的。

```
git diff
git diff <sha-1> <sha-1> 
```