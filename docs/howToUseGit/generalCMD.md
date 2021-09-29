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
## git branch
1. 查看、建立、刪除分支，分支是指另一條時間線的版本紀錄。
2. 查看分支語法如下，這兩個指令二選一，當執行後，會顯示遠端和本地端的分支
```
git branch -a
git branch --all
```
3. 建立分支語法如下，其中branchName可由使用者自行決定，當使用者下達之後，便會以目前分支A的最新版本上做一個分支B，而另一個分支B一開始會有分支A的最新紀錄，而在那可以建立獨立於分支A的內容。
```
git branch branchName
```

4. 刪除分支語法如下，其中branchName可由使用者自行決定，當使用者下達之後，便會刪除名為branchName的分支。

```
git branch -d branchName
```

## git checkout
1. 用作於切換分支
2. 使用語法為如下，當使用者下達之後，便會從目前分支轉移至名為branchName的分支上。

```
git checkout branchName
```


## git revert
1. 將指定版本撤回並自由選擇是否提交一份版本內容說明目前版本為已撤回指定版本
2. 撤回的意思如同字面上的意思，會取消、反轉對於指定版本的動作，當提交內容是新增內容A，那麼經由revert會轉變為刪除同樣內容A；而若是提交內容為刪除內容B，那麼經由revert會轉變為增加內容B
3. 語法主要有三種，第一種為會撤回sha-12對應的版本內容跳進編輯畫面來寫提交內容並提交，第二種為撤回但不用跳進編輯畫面，採用預設提交訊息內容並提交，第三種則為撤回但不用提交

```
git revert <sha-1>
git revert --no-edit <sha-1> 
git revert --no-commit <sha-1>
```
4. 通常適用於已經推送出去的 Commit，或是不允許使用 Reset 或 Rebase 等會修改歷史紀錄的指令的場合。

## git reset
1. 將HEAD回溯至先前版本內容
2. 語法形式為，第一種形式是將HEAD指向對應sha-1的版本，第二種形式則是以對應sha-1版本的前幾個版本，多少個^代表著多少前幾個版本，若為1個^，則以對應sha-1版本的前一版本來回溯，若為2個^，則以則以對應sha-1版本的前二版本來回溯，後面以此類推，第三種是用~n來指定前n個版本，比如&lt;sha-1&gt;~2就以對應該sha-1版本的前兩個版本進行回溯，最後一種形式則以HEAD所在來回溯，剩下符號的應用如同第二三種。


```
git reset <sha-1> 
git reset <sha-1>^ / git reset <sha-1>^^ /  git reset <sha-1>^^^  
git reset <sha-1>~n
git reset HEAD^ / git reset HEAD^ / git reset HEAD^
```

3. 另外會搭配soft、Mixed、Hard:
 - soft：回溯指定版本，程式碼會退回staged狀態，在這個狀態下的版本內容會回歸在staging area並處於等待commit的狀態。
 - mixed：回溯指定版本，程式碼會退回Unstaged，在這個狀態下的版本內容並不會在staging area且處於untracked的狀態，這是預設的git reset設定，換言之，即使不加soft、mixed、hard，預設上會以mixed為主。
 - hard：回溯指定版本，回溯前的當前版本內容會全刪掉，但只針對內容，不針對檔案。
 

4. 每次下達git reset時，會自動將reset前的版本紀錄儲存至ORI_HEAD，以方便開發者藉由以下指令再回到reset前的版本紀錄。

```
git reset ORI_HEAD
```

5. 通常適用於尚未推出去的 Commit。



 