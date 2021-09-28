---
sidebar_position: 6
---

# 進階指定 - git rebase
在這裡base會是指以什麼做為根基，而在git系統是指以什麼commit紀錄(節點)作為分支的起始點，拿下圖來說的話，分支1(Branch 1)是以Master/Main分支上的commitM來作為它的base，而re是重新、再一次，re和base組合在一起就是重新定義目前分支的起始點，換言之就是把某個分支的base改換另一個commit節點來作為新的base。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632815679/blog/git/rebase/anExampleOfigtbase_ybh3kf.png)

在這裡根據它所定義的意義來看，它擁有兩種主要功能：1. 將同一個分支上的多個commit節點合併成一個commit節點，2. 將分支A複製至另一個分一個分支B的頂端紀錄(分支B的最新提交紀錄)上。

## 合併多個commit節點
當想要讓同一分支上的多個commit節點合併成一個commit節點時，可以下達以下指令：
```
git rebase -i <sha-1>
```
接著它會以當前分支上的最新提交紀錄至sha-1對應版本的後一版作為處理範圍，接著以這個範圍的提交紀錄顯示在vim編輯器，內容格式如下，第一列會顯示sha-1對應版本的後一版，第二列則是後二版，依此類推至最後一行為最新版本，每一個版本的第一欄command是表示rebase對於該版本所要做的處理，後面欄位則是顯示對應版本的sha-1碼、提交訊息。

```
command <sha-1> commit message 1    
              .
              .
              .
command <sha-1> commit message N-1
command <sha-1> commit message N
```

command部分預設會是pickup，代表rebase不會變動該提交紀錄，但如果要合併的話，可以改成s或者壓扁(squash)，這指定會將前一個版本合併成同一個版本，比如說在版本N的第一欄填上squash，那麼rebase會將版本N和版本N-1壓扁成一塊提交紀錄。

```
pickup <sha-1> commit message N-1
squash <sha-1> commit message N
```

而壓扁可以不只可以壓2個版本，還能按照squash數量以及能被壓扁的量來盡可能增加可壓的數量，比如說在版本N至版本M+1前面填上squash，壓扁版本N至版本M成一個提交紀錄。

```

pickup <sha-1> commit message M
squash <sha-1> commit message M+1
              .
              .
              .
squash <sha-1> commit message N-2
squash <sha-1> commit message N-1
squash <sha-1> commit message N
```

不論你壓扁了多少個，最後需要你填寫最新版本的提交訊息，這時你在按照自己所需來填寫版本，並且最後結果會以你壓扁後的版本紀錄為主，但實際上原本壓扁前的版本紀錄也會存在git系統直到被git回收機制被回收，在這裡根據壓扁的位置我們可以大致區分為壓扁最新幾筆紀錄以及壓扁中間幾筆紀錄，以這兩個情況下我們試著用一些簡單例子說明當壓扁後來說明會是怎麼樣的情形，接著簡化成一種通用概念來表示。


### 壓扁最新幾筆版本紀錄
我們以一個簡單例子來說明，首先我們先提交五個版本紀錄，每一個版本都附加著sha-1碼，並且提交順序是以序號1的大小來決定，數字越大代表提交順序是越後面，HEAD因此指向第五個版本來表示目前分支最新版本是第五個版本，

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632837434/blog/git/rebase/simpleExample_zymlon.png)

在這裡我們想要讓最新的三筆版本紀錄壓扁成(合併成)一個版本，在下達git rebase相關指令後，其節點圖會是如下圖所示，實際能看到會是由原來第三版本至第五版本壓扁而成的最新第三版本，也就是目前HEAD所指向的，而它的sha-1碼會因為版本內容不同以及壓扁的關係而改變，然而git系統會保留壓扁前的節點圖，也就是內部隱藏的部分，這部分可以幫助開發者回溯到壓扁前的節點圖，這邊會由後續的取消方法來說明。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632837434/blog/git/rebase/simpleExampleAfterSquash_lq8zpa.png)

若我們將這個具體例子轉換成通用概念的話，會考慮著有N個版本紀錄，第N版本原本是HEAD所指向的，第M版本至第N版本則是指定被壓扁的對象，那麼在壓扁後的節點圖會像是如下圖，最新的N-M+1個版本會壓扁成一個新的版本，也就是下圖中的第M'個版本，同樣地，這只是實際上呈現的節點圖且它的sha-1碼會因為版本內容不同以及壓扁的關係而改變，而內部隱藏著原本壓扁前的節點圖。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632838124/blog/git/rebase/generalExampleAfterSquash_vjwxk4.png)

### 壓扁中間幾筆版本紀錄
同樣地，我們再次拿上面的簡單例子來壓扁，順序和HEAD都一樣，但這次是壓扁第二～三的版本紀錄。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632837434/blog/git/rebase/simpleExample_zymlon.png)

當我們下達git rebase的指定後，其節點圖會是如下圖所示的實際呈現那邊，在那裡會是由原第二～三個版本節點壓扁後的版本節點2'，接著再從原節點圖複製第四～五個版本節點並與新節點2'進行連接，所以下面的每個版本節點所擁有sha-1都經由複製和壓扁而改變，當然，原本還沒壓扁的節點圖會留起來等待git回收機制進行回收或者讓開發者回溯到壓扁前的節點圖
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632838538/blog/git/rebase/simpleExample2AfterSquash_dtibn0.png)

若我們將這個具體例子轉換成通用概念的話，考慮著N個版本紀錄以及第N版本原本是HEAD所指向的，第X至X+Y個版本節點則是被壓扁成一個版本節點的對象，那麼在壓扁後的節點圖會像是如下圖，第X至X+Y個版本節點被壓扁成第X'個版本節點，而原節圖點上第X+Y個節點之後的節點會被複製至下面與新節點X'進行連接，這些節點同樣地因為壓扁和複製而被更改sha-1碼。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632839767/blog/git/rebase/generalExampl2eAfterSquash_lp9ufa.png)

### 取消方法

若你後悔這次合併後的結果，可以試著利用reflog找出先前在

```
git reflog
```


### 例子

## 將分支複製轉移至另一個分支


### 取消方法

### 例子




