---
sidebar_position: 5
---

# git flow
將版本紀錄分為五種分支，每個分支都負責不同的功能，其中Main/Master和Develop因長期活在整個git flow裡頭而被稱作為長期分支，剩下大部分的分支都會因為任務結束而被刪除。

## Master (main)
1. 主要存放穩定、隨時可上線的版本
2. 版本紀錄的commit來源皆源自其他分支合併過來的紀錄，如release分支、hotfix分支

1. 用途：主要儲存下一次軟體釋放所要做的最新開發變動，並確保產品隨時上線的狀態
2. 該分支會存於開發者本地端的分支和遠端倉庫那邊的分支
a production-ready state.

main branch where the source code of HEAD always reflects a state with the latest delivered development changes for the next release



Hotfixes

Release branches
1. 用途：當develop分支已經達到下一次

May branch off from:
develop
Must merge back into:
develop and master

The key moment to branch off a new release branch from develop is when develop (almost) reflects the desired state of the new release. At least all features that are targeted for the release-to-be-built must be merged in to develop at this point in time. All features targeted at future releases may not—they must wait until after the release branch is branched off.

至少所有

所有目標功能



Develop
1. 用途：主要儲存軟體開發階段時的程式碼變動，當該分支下的程式碼發展一定程度時，就會合併至主分支。
2. 
3. 該分支會存於開發者本地端的分支和遠端倉庫那邊的分支

Feature branches
1. 從Develop 分支分出來的額外分支，主要會以開發特定功能而建立的分支
2. 等到該分支的特定功能已完成就會合併至Develop分支
3. 該分支基本上會存在於開發者本地端的分支，不會是遠端倉庫那
May branch off from:

develop
Must merge back into:
develop

Feature branches (or sometimes called topic branches) are used to develop new features for the upcoming or a distant future release.
Feature branches typically exist in developer repos only, not in origin.