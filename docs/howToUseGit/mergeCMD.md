---
sidebar_position: 6
---

# 進階指令 - git merge
git上的合併主要分為兩種，第一種為


![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633013783/blog/git/merge/mergeType2_lwmagq.png)

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633013783/blog/git/merge/mergeType1_lks8cy.png)



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


### 參考資料
1. [如何按照提交時間排序來生成合併結果](https://zlargon.gitbooks.io/git-tutorial/content/branch/merge.html)



## 快轉合併