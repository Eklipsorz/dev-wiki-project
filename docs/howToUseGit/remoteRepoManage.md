---
sidebar_position: 2
---

# 遠端倉庫常用指令



## git remote
該指令適用來管理一組遠端倉庫，這些倉庫所有分支皆是你所追蹤的，常見管理方法有查看所有已設定的遠端倉庫、新增要存取的遠端倉庫、刪除要存取的遠端倉庫。

### 查看所有已設定的遠端倉庫
主要是查看遠端倉庫有哪些，當使用者下達以下指令便會跑出對應訊息
```
git remote -v
```

### 新增要存取的遠端倉庫
主要是新增一個遠端倉庫好讓開發者能從本地端更新或者下載指定遠端倉庫的內容，name是指對應遠端倉庫位置url的別名，而url則是對應著遠端倉庫的url，另外name的預設別名為origin，若以origin來設定，跟遠端倉庫相關的指令只要沒特別指定遠端倉庫，預設上會直接使用origin對應的遠端倉庫。
```
git remote add <name> <url>
```

比如說：設定一個遠端倉庫，其位置為url1，而對應名稱為name1，以後要存取時便使用name1就能使用對應的遠端倉庫

```
git remote add name1 url1
```

### 刪除要存取的遠端倉庫
主要是移除一個已設定好的遠端倉庫設定(對應名稱和對應url)，在這裡的name是指對應url的別名

```
git remote remove <name>
```

比如說：移除一個已設定好的遠端倉庫設定，下達後便移除對應別名為name1的遠端倉庫之相關設定。

```
git remote remove name1
```


## 當如果取得遠端倉庫的內容時
你在一個分支取得遠端倉庫的內容後，git系統會為你添加兩個指標，一個是repository/branch，另一個則是repository/HEAD，前者會指向儲存取得內容的所有版本紀錄之節點連接圖，而另一個則是指向取得內容的最新版本。這兩個如同正常的分支和HEAD來運作，只是在這裡用repository來區分它們是用來控管從遠端倉庫取得的內容。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632995576/blog/git/remote/remoteBranch_d8tfzg.png)

## git push
根據git remote所設定的遠端倉庫來將本地端倉庫內容推送(push)/更新至指定遠端倉庫，主要分為三種，第一種語法：主要功用為推送本地端內容至遠端倉庫，在這裡的repository是指遠端倉庫(由提供git服務的伺服器所提供的倉庫)，可由git remote設定的別名來代表repository，而src則是指要推送哪一個本地端分支，而dst則是推送的目的地，整體的語法就是推送本地端src分支的內容至遠端倉庫repository下的dst分支。

```
git push <repository> <src>:<dst>
git push -u <repository> <src>:<dst>
git push -u <repository> <dst>
```

第二種語法：主要功用為推送本地端內容至遠端倉庫並設定上游分支，整體語法除了-u參數的存在以外，其餘功能與第一種語法的功能一樣，而-u相當於以下語法，首先會先以執行第一種語法，接著透過checkout切換本地端分支至src，去設置上游分支名支，形式上會是repository/其repo所擁有的分支。
```
git push <repository> <src>:<dst>
git checkout <src>
git branch -u <repository>/<dst>
```

第三種語法上：主要功用為推送本地端內容，主要是若src對應的分支名和dst對應的分支名是一樣的名稱，就能簡化成dst，但原則上系統會以第一種語法的形式來push。


### git push 簡化和限制

理論上若設定上游分支B給指定分支A時，你在分支A所下達的push指令可以進一步簡化，原本是要添加遠端倉庫的別名、要推送哪個本地端分支、推送目的分支，但經由簡化後，只需要下達：
```
git push
```

就能讓git系統從上游分支替你解析這個分支是對應哪個遠端分支，並且自動幫你填入完整指令，

```
git push <repository> <src>:<dst>
```

但根據git官方文件顯示，會出於安全政策，而不允許與上游分支名稱進行綁定的本地端分支名稱取不一樣名稱，上游分支名稱和本地分支名稱必須是一樣的。

> the current branch is pushed to the corresponding upstream branch, but as a safety measure, the push is aborted if the upstream branch does not have the same name as the local one


若堅持使用不一樣的名稱，會跑出以下的錯誤訊息，並告知你必須親自設定完整git push指令，不能使用簡化版的push指令。

```
fatal: The upstream branch of your current branch does not 
match the name of your current branch.
```

## git fetch
主要會從遠端倉庫的分支下載最新內容(對於本地端而言所沒有的內容)至本地端形成另一個分支，而這個分支會是repository/branch，由於該分支會有新內容，所以repository/HEAD或者FETCH_HEAD會移動至最新版本內容。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632996015/blog/git/remote/gitFetchResult_nlv9wa.png)


語法主要有三種：repository是遠端倉庫的別名、dst則是遠端倉庫的分支名稱
1. 第一種 只有在設定上游才有效，會取得該遠端倉庫的所有分支，
2. 第二種 會取得該遠端倉庫的所有分支 
3. 第三種 會取得該遠端倉庫的對應分支，其中dst為目標分支
```
git fetch
git fetch <repository>
git fetch <repository> <dst>
```

## git pull
主要會從遠端倉庫的分支下載最新內容(對於本地端而言所沒有的內容)至本地端形成另一個分支A，並且利用git merge來將分支A的內容與目前分支的內容進行合併。換言之，git pull是由以下指令所組合而成的：

```
git fetch <repository> <dst>
git merge <repository>/<dst>
```

## git clone
主要從遠端倉庫以倉庫(含有.git的目錄)形式下載回本地端，語法會是以下，其中repository會是遠端倉庫的URL，

```
git clone <repository>
```




