---
sidebar_position: 11
---

# Parallel 和 Concurrent 之間的差別

## 原文

### Parallel 

第一段為韋伯字典所定下的定義，意思是一種可以允許多個操作或者多個任務被同時執行的安排或者狀態，而非一個接一個執行

> an arrangement or state that permits several operations or tasks to be performed simultaneously rather than consecutively

第二段為劍橋字典所定下的定義，意思是一個事件所發生的時間會跟另一個事件的發生時間一樣或者極度近似。

> used to describe an event or situation that happens at the same time as and/or similar to another one

從這兩段來看，parallel 會特別強調"多個人事物"同時這件事情，也就是 "多個人事物" 同時做某種事情，每件人事物都做著不同或者相同的事情。


### Concurrent 


韋伯字典和劍橋字典皆是如此定義concurrent，同時發生什麼或者同時執行什麼，沒像parallel特別強調於發生事物的數量多寡是否多於1個
> happening or operating at the same time


## 比較結果

### 已知資訊
1. 從翻譯字面上來看 parallel 和 concurrent 這兩個字皆為同時
2. 但 parallel 比起 concurrent 又更為嚴謹，會強調發生的人事物是大於1個，而 concurrent 只是簡單描述同時這件事情，沒特別強調

### 最後
結合前面兩者，我們可以將parallel 定義為 "多個人事物" 可以同時做的情形，而concurrent則是指排除掉parallel所定義的同時，也就是指單個事物可以同時做的能力。


### 同時的意涵

根據觀察者可看到的結果，來將同時分成兩種：
1. 不論是宏觀或者微觀上，某些事物都會是同時的
2. 宏觀上，某些事物會是同時的，但微觀上是不同時，比如說在某種程式花了1秒同時完成n個任務，而這件事就是我們人類所能觀察到的角度，但實際上在1秒內，這種程式只能一次執行一個任務，因此會不斷地切換任務來做，只是做到1秒的時候，就剛好完成n個任務，而這個角度就是微觀角度。
## 參考資料
1. [一起了解什麼是高併發](https://www.itread01.com/content/1547164817.html)
2. [劍橋字典對於parallel的定義](https://dictionary.cambridge.org/us/dictionary/english-chinese-traditional/parallel)
3. [韋伯字典對於parallel的定義](https://www.merriam-webster.com/dictionary/parallel)
4. [劍橋字典對於concurrent的定義](https://dictionary.cambridge.org/us/dictionary/english/concurrent)
5. [韋伯字典對於concurrent的定義](https://www.merriam-webster.com/dictionary/concurrent)
