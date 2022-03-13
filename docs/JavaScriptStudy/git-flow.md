---
sidebar_position: 5
---

# git flow
將版本紀錄分為五種分支，每個分支都負責不同的功能，其中main/master和develop因長期活在整個git flow裡頭而被稱作為長期分支，剩下大部分的分支都會因為任務結束而被刪除。

## master (main)分支
1. 主要存放穩定、隨時可上線的版本，並使用git tag功能來標記每一次commit的紀錄所屬的版本號為何
2. 版本紀錄的commit來源皆源自其他分支合併過來的紀錄，如release分支、hotfix分支，一般來說並不會從master分支來進行commit，如下圖：當從release分支合併至master分支時，版本數會大幅度提升，而從hotfix分支合併至master分支，會因為其分支作用而使版本數微幅度提度。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647155176/backend/git-flow/master-commit_mbk6ww.png)
3. 重點：
  - 只允許從release分支、hotfix分支合併來新增master上的commit紀錄
  - 不允許從master分支上進行任何提交(commit)
  - 每一個commit紀錄會搭配tag來標記所屬的版本號是為何
4. 該分支會存於開發者本地端的分支和遠端倉庫那邊的分支
## hotfix 分支
1. hotfix的hot指的是緊急的、重要的，而fix則是指修復，不會特定新增功能。在這裡組合起來的分支就是緊急修復用的分支。
2. 主要用途為：當線上版本或者master分支上出現緊急問題時，就會從master目前紀錄上切分出hotfix分支來解決此問題，而當問題解決後，就從hotfix分支合併至master分支和develop分支，以確保兩者保持一致
3. 版本紀錄的commit來源皆源自：
  - 從master切分出來的hotfix分支並修復bug而從hotfix分支上提交的commit
4. 主要流程為：
  - 當master出現緊急問題時，就以master分支目前紀錄切分出hotfix分支
  - 從hotfix分支修正問題並提交一份commit
  - 將hotfix分支的修正commit合併至master分支和develop分支
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647164444/backend/git-flow/hotfix-commit_kdthln.png)
5. 其他細節：
  - hotfix屬於臨時分支，確定修正緊急問題後就即可選擇刪除該分支
  - 命名規則與feature一樣，建議為：hotfix/bug_hotfix，bug為緊急問題名稱，比如hotfix/user_createtime_hotfix就是專門修正使用者建立時間的hotfix分支


## release 分支
1. 用途：當develop分支內容已經足夠成熟或完成指定的release功能時，就會負責儲存從develop分支合併過來的紀錄並進行上線前的測試，根據測試結果來從release分支修正bug並於該分支提交
2. 本紀錄的commit來源皆源自：
  - 從develop分支合併過來的紀錄
  - 從develop分支合併過來的紀錄並做上線前的測試，根據測試結果來修正測試問題，並以develop分支來提交commit
3. 當完成release分支下的測試時，就會被合併至develop分支和master分支，來保持兩者都不會遇到相同的測試問題。
4. 整體會有的流程如下：
 - 當develop分支下的目前完成下次release的目標時，就會合併至release分支並提交
 - 在release分支下進行第一輪的測試，若遇到測試問題，則從release分支進行修正並提交commit
 - 從release分支下的最新內容分別合併至develop分支和master分支，來保證兩者都不會遇到相同的測試問題
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647164443/backend/git-flow/release-commit_ovpp5o.png)
5. 其他細節：
  - 此分支為臨時分支，產品成功上線後可選擇刪去該臨時分支
  - 該分支主要用來測試以及根據測試結果來修正
  - 分支命名方式為release/version_publishtime，其中version為版本數，而publishtime則是發布時間，比如說release/v2.1.1_20201018，意思為2020年10月18日發布的版本，版本數是2.1.1。
  

## develop 分支
1. 主要存放所有開發的主分支，不會有第二個develop分支，並盡可能保持最新功能開發和bug修復後的版本
2. 版本紀錄的commit來源皆源自：
  - 當要新增單一功能時，就會從develop分支切出feature分支，每一種功能都各一個feature分支，而當該feature分支的功能開發完成後便會合併至develop分支增加commit
3. 當develop分支上完成下一次release所要求的功能或者進度時，便會將develop分支的目前內容合併至release分支，在那進行測試。

4. 整體會有的流程如下：
  - 當develop分支要開發新功能時，就切分feature分支並在那進行開發，等到該分支的開發程度完成就將該分支的目前內容合併至develop分支
  - 當develop分支上已經完成下一次release分支所要求的功能或者目標時，就將develop分支合併至release分支上，在那進行相關測試
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647162556/backend/git-flow/develop-commit_uyythk.png)

5. 其他細節：
  - 該分支會存於開發者本地端的分支和遠端倉庫那邊的分支
  - 該分支不能夠從Develop本身來提交。


## feature 分支
1. 主要會以另外以develop分支來切分出feature，其內容為存放以develop目前版本為主的單一功能開發，每一種單一功能的開發會有各自的feature分支，不會全由單一的feature分支來控管所有單一功能的開發
2. 版本紀錄的commit來源皆源自：
  - 從feature本身所提交的commit紀錄
3. 當feature分支的目前內容已經完成功能開發就會將目前內容合併至develop分支
4. 整體流程為在這為了要開發feature 1和feature 2這兩個功能
  - 從develop目前分支分別切分出feature 1和feature 2 這兩個分支，接著分別進入這兩個進行開發
  - 只要其中一個feature 分支完成功能開發就將feature分支的目前內容合併至develop分支
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647162556/backend/git-flow/feature-commit_alosee.png)
5. 其他細節：
  - 該分支為暫時性分支，主要會存在開發者本地端的分支，不會存在於遠端分支
  - 建議命名方式為：feature/功能名(特性名稱)，比如開發使用者建立，就會是feature/user-create