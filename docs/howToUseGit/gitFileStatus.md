---
sidebar_position: 3
---

# git 系統中的檔案狀態
當設定git的repo之後，系統便以repo的根目錄所在來檢測當下目錄所擁有的檔案和目錄是在哪一個檔案狀態，而這個檔案狀態是由git系統所決定，基本上會分為五種：Untracked、Tracked、Unmodified、Modified、Staged，在本文章會分別描述這四種狀態以及狀態之間的轉換。

## untracked
未被git追蹤、版控(能夠透過git來控管版本)的狀態，通常在這個狀態下是不會出現在repo的最新版本紀錄以及Staging Area中。

## Tracked
被git所追蹤、能被版控的狀態，通常會是能在repo的最新版本紀錄找到或者已經被放入Staging Area中。

## Unmodified
能被git所追蹤、能被版控的狀態，只是內容與最新版本紀錄一樣。

## Modified
能被git所追蹤、能被版控的狀態，內容與最新版本紀錄不一樣，顯示變更。

## Staged
被放入Staging Area且準備提交的狀態。

## Relationship betweeen them
在這裡我們以Untracked、Unmodified、Modified、Staged這四種狀態來說明四種間是如何轉換狀態，當目標的狀態是untracked，可以透過git add指令將狀態轉為staged，而Unmodified和Modified只有在目標已在最新版本紀錄中才會有的狀態，前者只要內容有做變更就會轉移至Modified，而若目標被移除的話，狀態就直接從版本紀錄中移除，未來若以相同目標名稱顯示的話，則會以Untracked來顯示，而Modified狀態只有在git add或者被放入staging area中才會轉移至Staged，而Staged狀態則是只有git commit才會轉移成Unmodified狀態。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632583252/blog/git/lifecycle_kxzc2i.png)

## 若使用git status來調查
如題，其顯示狀態會有四種：

1. Untracked file：沒被git版控、追蹤的狀態
2. Changes not staged for commit (modified)：有被git追蹤、版控，但內容與最新的版本內容不一致的狀態
3. Changes to be committed (staged) ：被提交至staging area並準備被提交至repo的狀態 
4. Committed：被提交至repo的狀態