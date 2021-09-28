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

不論你壓扁了多少個，最後需要你填寫最新版本的提交訊息，這時你在按照自己所需來填寫版本，並且最後結果


### 取消方法

若你後悔這次合併後的結果，可以試著利用reflog找出先前在

```
git reflog
```


### 例子

## 將分支複製轉移至另一個分支


### 取消方法

### 例子




