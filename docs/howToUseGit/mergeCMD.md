---
sidebar_position: 6
---

# 進階指令 - git merge
一般來說，git上的合併對象會是兩個分支，其中一個分支會從另一分支衍生而來，若經過合併，其結果會是這兩個分支上的版本紀錄根據合併結果重整紀錄，根據合併的難易度，可以分為兩種，第一種為：分支Branch1從另一個分支Branch的最新紀錄衍生，並且做一種名為快轉(fast-forward)合併的處理來解決。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633013783/blog/git/merge/mergeType2_lwmagq.png)

第二種為分支Branch1是從另一個分支Branch的最新紀錄衍生，但隨後分支Branch又提交一些版本紀錄，這時得做合併提交，然而提交過程中會有同個內容的修改衝突，這部分會在隨後再補充。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633013783/blog/git/merge/mergeType1_lks8cy.png)

## 快轉 (Fast-forward)
若我們有兩個分支，分支Branch2從另一個分支Branch1的最新紀錄衍生，
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633015120/blog/git/merge/simpleMerge_hdy9sw.png)

當我們在Branch1下達merge Branch2的話，合併結果會直接使負責指向Branch1和Branch1最新節點的兩個指標(Branch1和HEAD)直接指向Branch2的最新版本紀錄，而這種指標的轉移過程稱之為快轉。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633015120/blog/git/merge/simpleMergeResult_ja3cfv.png)


## 合併提交 (Merge commit)
若我們有兩個分支，一個主分支(Branch1)，另一個是從主分切分出來的分支(Branch2)，而HEAD目前指向主分支(Branch1)，

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633013862/blog/git/merge/mergeBefore_y77jhm.png)


在那下達以下指令的話，其中Branch2會是以分支Branch2的最新版本紀錄作為以下指令的commit
```
git merge Branch2
```

由那個commit來決定被合併的版本紀錄有哪些，在這裡會是從主分支的Commit 1一直到Branch2下的最新版本紀錄Commit N'，以這些來合併，接著會和目前分支的版本紀錄比較，哪些是重複的，哪些是不重複的，重複就不考慮合併，接著挑選出這些不重複，在這裡會是以紅框作為挑選對象的範疇，

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633013862/blog/git/merge/mergeSelection_guqfbc.png)


由這些節點來按照提交時間來重新排列成新的版本紀錄，提交時間較早先放入，其次較早的紀錄則被前者連接著，後頭的紀錄就按照這樣規則排下去形成從早到晚的版本紀錄(如下圖)，
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633010765/blog/git/merge/commitTimeline_zitpfe.png)


然而最後產生的節點圖並不會以上頭的排序結果來重新或者合併一個分支來呈現，而是將兩個分支上的需要重排(原本在橘框的)節點重新按照提交時間在原分支上進行排序，最後會在主分支Branch1產生一個額外的commit N"來表示合併已經完成，並且分支Branch2會連接至commit N"  
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633013861/blog/git/merge/mergeAfter_ogfylj.png)




## 如何還原合併結果
首先先在放置合併結果訊息(或者當時下達git merge時的分支)當前的分支利用reflog找出合併前的最新commit紀錄對應的sha-1，找到後下達以下指令，就能相關的指標和分支調回合併前的節點圖。
```
git reset --merge <sha-1>
```



### 例子
當我們有兩個分支，分別為master和testbranch，master最新提交的紀錄是增加test2這個檔案，而testbranch最新提交的紀錄是提交testbranch3這個檔案，
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633016090/blog/git/merge/mergeExampleBefor_ombtig.png)

若我們透過checkout切換master分支，並且下達merge testbranch來與testbranch分支合併
```
git checkout master
git merge testbranch
```

其結果會是如下圖，你可以看到每一個節點的提交時間從晚到早來排序，
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633016090/blog/git/merge/mergeExampleAfter_legfy0.png)

若我們想要將合併結果還原至合併前的節點圖(也就是第一張圖那樣)，必須使用reflog去尋找git merge之前提交的最新版本紀錄所對應的sha-1碼是什麼，

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633016336/blog/git/merge/reflogExample_gqlqzk.png)

在這裡，我們可以很快找到第一筆被找到的commit訊息，其對應節點是6a1529d，所以只需要下達以下指令就能還原成合併前的節點圖

```
git reset --merge 6a1529d
```

## 參考資料
1. [如何按照提交時間排序來生成合併結果](https://zlargon.gitbooks.io/git-tutorial/content/branch/merge.html)
2. [如何還原合併後的結果](https://careerkarma.com/blog/git-undo-merge/)


