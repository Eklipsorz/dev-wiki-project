---
sidebar_position: 19
---

# browser 新知和技術


## 瀏覽器的process
1. 主要分為四種：
  - Browser  Process：主要負責處理瀏覽器本身能提供的功能(如書籤、我的最愛、UI功能)以及負責處理一些不可見且需要獲取作業系統權限才能做的任務，如網路請求和檔案存取，
  - Renderer Process：負責處理網頁畫面渲染的工作和事件處理
  - Plugin Process：負責處理網頁所使用的插件或者負責執行插件功能的工作
  - GPU Process：負責調用GPU資源來支援其他工作，主要會支援HTML、CSS等圖形化元素的渲染

### 參考資料
1. [What is the GPU process in Chrome's task manager and why does it hog up so much RAM?](https://www.quora.com/What-is-the-GPU-process-in-Chromes-task-manager-and-why-does-it-hog-up-so-much-RAM)
2. [Inside look at modern web browser (part 1)](https://developers.google.com/web/updates/2018/09/inside-browser-part1)


## Renderer Process
1. 負責處理：
  - Parsing：將一份網頁解析成DOM Tree、CSSOM Tree、Render Tree
  - Layout： 並根據前者資訊來做相對應的網頁元素間之位置計算、大小計算、如何擺放
  - Paint： 最後按照指定顏色和位置在指定的pixel位置畫出指定畫面
  - 同時也會負責執行同份網頁下的JavaScript
2. 該Process本身內含著Main Thread、Worker Threads等執行緒來執行，而Process最主要的執行會是由Main Thread來執行，而其Thread會是每一個網頁(tab)都各一個。

* 為啥Renderer Process會負責執行同份網頁下的JavaScript呢？這是因為JavaScript本身被允許改變DOM Tree、CSSOM Tree、Render Tree的結構，比如document.write()，而這些資訊若被改變的話，那麼其畫面也會跟著改變，所以為了更加有效率地使用這樣子的變化才將執行JavaScript任務分配給Renderer Process

### 參考資料
1. [Inside look at modern web browser (part 3)](https://developers.google.com/web/updates/2018/09/inside-browser-part3)



## Call stack
1. 每一次要執行某種內容區塊的程式碼，會先該區塊存放在stack上，等到執行完畢才從stack移出該區塊，主要存放所有目前被呼叫卻沒執行完畢的內容區塊
2. 內容區塊種類主要分為：Global Execution Context 、Function Execution Context
3. 由於第一點的特性，也可以代表著哪些區塊還需要執行以及定義執行順序，換言之，所有要被執行的程式碼區塊必須先於call stack出現才能夠被執行


### 參考資料
1. [JS 原力覺醒 Day03 - 執行環境與執行堆疊](https://ithelp.ithome.com.tw/articles/10216450)


## GPU (Graphics Processing Unit)
1. 原本設計於處理畫面渲染的處理實體單位，一個單位具有多個核心(主要負責處理/執行工作的部分)
2. 與CPU的核心相比而言，每一個GPU的核心被設計成只能夠處理較為簡單的運算，但卻可以透過核心數量來合併一個大型處理單位來處理較為複雜的運算，比如多個GPU核心一起解決同一個問題
3. 除了畫面渲染之外，也能透過自身特性來處理較為複雜的問題和運算
4. 參考資料
[What is the GPU process in Chrome's task manager and why does it hog up so much RAM?](https://www.quora.com/What-is-the-GPU-process-in-Chromes-task-manager-and-why-does-it-hog-up-so-much-RAM)

