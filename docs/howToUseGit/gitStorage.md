---
sidebar_position: 2
---


# git 有哪些儲存空間
git系統本身是以分散式系統來儲存和處理，本身存放的資料會透過網路而被分散存放，以保證資料不會因為集中存放而易毀損，git分散存放的方式是將本地端的git Repository上傳至伺服器，再由伺服器去發送其Repository給予任何一個想要存取其資料的人或者電腦，每個人都可以透過伺服器而擁有一份Repository，當其中一人或者伺服器出現問題無法正常存取Repository，可藉由其他人在本地端下的repo來進行修復或者正常存取。

而git系統所擁有的儲存空間可藉由本地端和伺服器端而分為兩種，第一種為本地端的儲存空間，第二種則為伺服器端的儲存空間，在這個小文章中，將會介紹這兩種儲存空間。


## git 儲存檔案的方式

### File Snapshot
File snapshot是針對一個檔案來紀錄它在不同時間點下的內容紀錄，每一個時間點下的檔案內容都會被紀錄，而這些內容又如同紀錄某個時段的相片(snapshot)，故稱之為snapshot，但由於這項技術是紀錄特定檔案在不同時間點的內容，所以若只是單純採用複製的方法來實現，會造成系統很多負擔，而且這些內容大部分都是重複的，所以會採取內容共享的手段來大幅度減少系統負擔，我們以一個例子來說明，在這裡我們建立一個名為File 1的檔案內容，其內容會直接以類似by reference或者指標的形式指向
在硬碟(Disk)的實際內容。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632575615/blog/git/anExample_createFile1_l14ho0.png)

而當我們對File 1進行所謂共享來獲取snapshot時，會生成另一個名為File 2檔案(名稱可以是同名)，其內容會直接以類似於by reference方式或者以指標的形式來指向File 1檔案內容的記憶體位址所在，

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632575634/blog/git/anExample_snapshot_lm3qcm.png)

讓File 2不用再向系統要一塊硬碟空間去存放File1 內容，而是直接像上圖那樣，兩個檔案都共享著同個內容，對於File 1而言， File 2 是以File 1的內容作為snapshot來進行相關的修改

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632575959/blog/git/anExample_copyfile_iovmhc.png)

當File 2想要對共享內容做變更時，系統會以copy on write的形式來向系統要塊硬碟空間去存放這些變更內容，而非整塊檔案內容，在這裡File 2內容還是會仍指向存放File 1內容的硬碟位址，但會增加名為Changes的指標去指向存放變更內容的硬碟區塊，同樣地，File 1本身也能做這樣子的更改。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632576922/blog/git/anExample_copyonwrite_scoduu.png)

若還有第三個檔案File 3的話，那就要考慮它以什麼檔案內容來當snapshot，若還是把File 1的檔案內容當作snapshot的話，原則上會如同File 2那樣共享File 1內容並存放變更內容，但若是以File 2為主的話，File 3的內容會指向File 2所指向的內容以及它所修改的內容，而File 3的變更內容則是指向存放對File 2進行變更的內容之硬碟區塊。若還有其他檔案如同File 3那樣延續著以前一個檔案來當作snapshot來修改的話，會如同File 3來指向。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632578989/blog/git/anotherExample_copyonwrite_uexl15.png)

### git snapshot
git在repo存放的每一個版本的資料形式會是以類似於file snapshot的形式來進行，每一個版本會是以前一個版本作為snapshot來修改，我們以一個例子來說明，首先我們先建立一個repo並放入三個檔案，分別為File 1、File 2、File 3，接著對他們進行add和commit來提交第一次的版本，儲存形式會是以完整檔案來存放，接著當我們對著三個檔案做了一些小修改時，並且add以及commit來提交第二次的版本，這時第二次的版本內容將不會是以完整內容來存放，每個檔案有分為snapshot以及Changes((snapshot 和 Changes 並非git真以這名稱來說明，在這裡是方便解釋，但概念上是一樣的)，而snapshot會直接指向前一個版本內容的位置，也就是指向第一個版本的內容，而每個檔案都會指向第一個版本的對應檔案，而Changes則是存放著這次版本修改的內容。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632579687/blog/git/anExampleOfGitsnapshot_pjz1z2.png)

若有檔案沒在第二次版本做任何修改的話，會如同下圖中的第二個檔案那樣，只會有指向前一個版本內容的snapshot。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632580141/blog/git/Gitsnapshot_file2nochange_y1ixl8.png)


接著若我們繼續對三個檔案進行修改的話，並且提交第三次的版本，這時的版本內容將會以第二個版本內容為主，每個檔案的snapshot
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632580274/blog/git/Gitsnapshot_threeVersions_yg2es2.png)


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