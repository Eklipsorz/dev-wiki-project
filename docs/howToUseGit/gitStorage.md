---
sidebar_position: 2
---


# git 有哪些儲存空間
git系統本身是以分散式系統來儲存和處理，本身存放的資料會透過網路而被分散存放，以保證資料不會因為集中存放而易毀損，git分散存放的方式是將本地端的git Repository上傳至伺服器，再由伺服器去發送其Repository給予任何一個想要存取其資料的人或者電腦，每個人都可以透過伺服器而擁有一份Repository，當其中一人或者伺服器出現問題無法正常存取Repository，可藉由其他人在本地端下的repo來進行修復或者正常存取。

而git系統所擁有的儲存空間可藉由本地端和伺服器端而分為兩種，第一種為本地端的儲存空間，第二種則為伺服器端的儲存空間，在這個小文章中，將會介紹這兩種儲存空間。

## 本地端的git系統
當在本地端透過git init或者利用git clone來產生一個新repo時，會以存放.git目錄作為repo的根目錄，而這個根目錄和其下檔案/目錄皆稱為工作目錄(working directory/working tree)，每當在目錄下對某些內容下達git add時，會將內容存放暫存區(Staging area)，接著當下達git commit時，便會將暫存區的內容以一個版本的形式去提交/上傳至倉庫(Repository, repo)。 而工作目錄、暫存區、倉庫會是本地端的git 系統所會擁有的儲存空間，在這小節中，會以上面的描述為基礎來稍微詳細說明每種的用途以及他們是什麼。


### Working directory
1. 當我們下達git init 來建立repo或者利用git clone來獲取repo時，不論是哪一種都會有.git這隱藏目錄，而git系統會以這個隱藏目錄所在的目錄作為它的工作目錄
2. 在這個工作目錄下，可以允許存放未被git追蹤的檔案，換言之，即使沒下達git add，任意的檔案都能存放在這個目錄下


### Staging area
1. 英文意思為(軍人的)集結區域，在這裡同樣地，這區塊也是集結著一些要準備提交/上傳的內容，提交/上傳的目的會是本地端的repo，而這個區域也可已被稱之為暫存區
2. 每一個工作目錄下都擁有各自的暫存區、倉庫，換言之，n個工作目錄下的暫存區和倉庫都是彼此獨立的，不會是一樣的內容，更別說彼此會相互影響。
3. 當在工作目錄對某些內容下達git add時，系統會先檢查這些內容是不是目前最新版本的內容，若不是的話，才會被複製並存放至暫存區，否則就不會放入該暫存區。


### Repository
1. 英文意思為倉庫，會存放不同時期下的版本內容或者提交內容
2. 每一個工作目錄下都擁有各自的暫存區、倉庫
3. 當對工作目錄下達git commit時，便會把該目錄下所擁有的暫存區以版本的形式來提交/上傳至倉庫，而這個倉庫會是這個工作目錄所擁有的倉庫。此時的暫存區會被清空。


### 例子
在這裡我們以一個小例子來描述這三種主要儲存空間，同樣地，當我們下達git init 來建立repo或者利用git clone來獲取repo時，此時repo會是以目錄來表示，這時我們只需要進去該目錄，而這個目錄會是一個工作目錄，裡頭放了.git隱藏目錄以及一些內容(如下圖所示)
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632562546/blog/git/anExample1OfGitStorage_nqnzun.png)

若我們對著這些檔案下達git add時，系統會先去拿倉庫的最新版本內容來比對這些要加入至暫存區的內容，比對結果是不一致的話，才會複製其內容並放入暫存區，若一致則不會複製以及不會做放入暫存區的動作。在這裡，我們就先以不一致的情況下來說明
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632562545/blog/git/anExample2OfGitStorage_s1tpcw.png)


當對這個工作目錄下達git commit時，系統會以一個版本的形式來將暫存區的內容搬移至repo，而此時的暫存區會被清空。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632562545/blog/git/anExample3OfGitStorage_gqcrun.png)