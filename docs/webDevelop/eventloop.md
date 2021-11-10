---
sidebar_position: 1
---

# Event loop 與 在JavaScript中的它



## Event loop 背景
電腦上會有許多種類的工作或者任務會如同生產線一般上的產品那樣，由生產線的滾輪帶動這些工作至負責執行處理的工作人員(實體CPU)，實際上生產線會以Queue來實現，每一個Queue會對應一個實體CPU來處理，而分配任務至Queue則是由Scheduler來處理，這些實體CPU碰到這些工作或者任務時，會執行並做些處理，然後再換下一個工作或者任務來處理，直到所有任務被完成，然而當有一個工作本身是依賴於I/O設備且在生產線正被實體CPU所執行處理時，由於這個工作必須等到I/O設備回應它，它才能讓實體CPU繼續處理，在回應之前，它後頭的任務(如下圖中的Task 1 至 Task N)全部會被它一個任務(I/O Task)給延宕，且在CPU等待該任務的期間是空閒的。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636558109/blog/event/eventloop/CPUQueue_heh0gr.png)
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636557741/blog/event/eventloop/afterEnqueue_qc5xr0.png)

為了進一步利用CPU空閒時間來處理後續的Task 1 至 Task N，作業系統會將I/O 種類的任務另外放進另一個Queue來處理，等到CPU處理完一段時間再回過頭依序檢查看看每一個I/O任務上是否已經等到對應I/O設備的回應，若等到回應時，會將任務重新排進對應CPU的Queue或者當場執行，通常來會為了增加效能而重新讓Scheduler根據任務性質來排進哪一個CPU的Queue。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636561209/blog/event/eventloop/reEnqueue_gdhp2t.png)

## 事件
然而這種可以延宕實體CPU的並不侷限於上述的I/O種類任務，還有由I/O本身被某些行為給觸發出來的信號，然後再由信號來執行的特定任務X，在這裏的信號會當作是事件(event)，而為了執行特定工作，勢必會產生出必須要監聽某種信號/事件的任務，以及事件被監聽到的所要做的任務(也就是上述提到的特定任務X)，監聽事件任務本身和等待I/O回應的任務一樣，必須要一直等待著事件發生才有所進展，而特定任務X本身就只是隨機且不可預期什麼時候會執行，但會因此造成一些執行結果上的同步問題。





而當有一個工作本身是依賴於I/O設備並且在生產線中正被實體CPU所執行處理，而這個工作必須等到I/O設備回應它，它才能讓實體


被實體CPU所執行，而當一個工作本身是依賴於I/O設備並且給予實體CPU執行時，有可能一大半的執行時間都在等待I/O設備回應該工作，而排


Event loop 本身是個分發事件何時執行的設計概念或者程式設計
說明event loop 是電腦科學名詞，它為了實現輪詢而會建立一個空間來存放....

它具有多種event loop和空間來進行


## Event Loop 誕生



## Event loop 運作程序
說明event loop如何運作

## 實現 Event loop 的重點 

## 在JavaScript中的它
為了進一步了解它，會先從JavaScript如何執行、如何面對任務、在Node.js又是如何


## JavaScript 簡介
主要說明JavaScript 是單執行緒和原因、瀏覽器執行它


## JavaScript 工作模式
JavaScript 單執行

### JavaScript 面對同步任務

### 那 JavaScript 如何面對異步任務
event loop 

## Event Loop 在 Node.js 上


## 補充資料

### 如何允許 JavaScript 產生多個執行緒


### Stack 

### Callback
1. 在名詞上是指回電行為，而要稱之為回電行為必須要滿足以下幾個條件：a. 限定於兩個對象之間，其中一方為對象A，另一方則為對象B.  b. 對象A呼叫對象B，而對象B呼叫對象A，若滿足條件，其中對象B呼叫對象A就即為回電。
2. 若名詞套用在電腦科學的話，會是套用在程式模組之間的呼叫，根據以上條件來描述，兩個對象都皆為程式模組，對象A擁有著用來 ”可以呼叫自己(對象A)” 來使用對象A程式模組的小程式模組X，而對象B就只是程式模組，當對象A呼叫對象B時，為了讓對象B使用自己的功能或者呼叫告訴他(對象A)我(對象B)已經完成工作，可以過來檢查而提供小程式模組X給對象B呼叫對象A，而對象B當需要用上對象A的功能或者呼叫他(對象A)並告知自己(對象B)的工作狀態時，會透過小程式模組X去呼叫了對象A，請他幫忙執行某些功能或者檢查對象B的工作狀態。而對象B用上小程式模組X來呼叫對象A這行為就是回電(callback)的行為，而在這裡的小程式模組會因為協助回電而被取名為callback function，也可以被簡稱為callback
3. 簡而言之，Callback function就是給予其他程式模組呼叫自己(程式模組)的function，透過呼叫自己來調用自己才有的功能或者其他程式模組用來告知自己並說明他們的工作狀態是如何。
4. Call = call then back ，即為對象B被對象A呼叫，而對象B又呼叫對象A來返回為對象A來執行


### Polling


### 參考資料
1. [Callback](https://en.wikipedia.org/wiki/Callback_(computer_programming))
2. [Callback functions](https://docs.microsoft.com/en-us/dotnet/framework/interop/callback-functions)