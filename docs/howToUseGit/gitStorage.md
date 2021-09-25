---
sidebar_position: 2
---


# git 有哪些儲存空間
git系統本身是以分散式系統來儲存和處理，本身存放的資料會透過網路而被分散存放，以保證資料不會因為集中存放而易毀損，git分散存放的方式是將本地端的git Repository上傳至伺服器，再由伺服器去發送其Repository給予任何一個想要存取其資料的人或者電腦，每個人都可以透過伺服器而擁有一份Repository，當其中一人或者伺服器出現問題無法正常存取Repository，可藉由其他人在本地端下的repo來進行修復或者正常存取。

而git系統所擁有的儲存空間可藉由本地端和伺服器端而分為兩種，第一種為本地端的儲存空間，第二種則為伺服器端的儲存空間，在這個小文章中，將會介紹這兩種儲存空間。

## 本地端的git系統
當在本地端透過git init或者利用git clone來產生一個新repo時，會以當下的目錄作為repo的根目錄，而這個根目錄和其下檔案/目錄皆稱為工作目錄(working directory/working tree)，每當在目錄下對某些內容下達git add時，會將內容存放暫存區(Staging area)，接著當下達git commit時，便會將暫存區的內容以一個版本的形式去提交/上傳至倉庫(Repository)。 而工作目錄、暫存區、倉庫會是本地端的git 系統所會擁有的儲存空間，在這小節中，會以上面的描述為基礎來稍微詳細說明每種的用途以及他們是什麼。


### Working directory



### Staging area


### Repository