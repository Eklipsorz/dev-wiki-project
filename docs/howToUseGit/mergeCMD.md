---
sidebar_position: 6
---

# 進階指令 - git merge

以指定的版本紀錄(commit)為終點並往前延伸至連接至該版本紀錄的起點，這些版本紀錄將會和目前所在的分支做一個大合併，相同節點不會重複合併，不同節點將會合併

Commits, usually other branch heads, to merge into our branch. Specifying more than one commit will create a merge with more than two parents 


1. 將指定版本紀錄(commit)相關的版本紀錄([詳見請看這邊](./mergeCMD))按照提交時間來合併，也就是說相關的版本紀錄和目前分支上的版本紀錄按照時間來排序成新的版本紀錄，顯示上仍以分支為主，若指定的版本是某個分支的HEAD，就表示該分支與目前分支做合併，合併後，兩個分支都還存在。

2. 語法為如下，其中commit為指定版本紀錄，將會以這個為標準將相關的紀錄來進行合併
```
git merge <commit>
```