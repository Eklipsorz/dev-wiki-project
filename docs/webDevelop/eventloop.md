---
sidebar_position: 1
---

# Event loop 與 在JavaScript中的它

## Event loop 背景
電腦上會有許多種類的工作或者任務會如同生產線一般上的產品那樣，由生產線的滾輪帶動這些工作至負責執行處理的工作人員(實體CPU)，實際上生產線會以Queue來實現，每一個Queue會對應一個實體CPU來處理，而分配任務至Queue則是由Scheduler來處理，這些實體CPU碰到這些工作或者任務時，會執行並做些處理，然後再換下一個工作或者任務來處理，直到所有任務被完成，

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636632652/blog/event/eventloop/scheduling_szgjwp.png)

然而當有一個工作本身是依賴於I/O設備且在生產線正被實體CPU所執行處理時，由於這個工作必須等到I/O設備回應它，它才能讓實體CPU繼續處理，在回應之前，它後頭的任務(如下圖中的Task 1 至 Task N)全部會被它一個任務(I/O Task)給延宕，且在CPU等待該任務的期間是空閒的。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636557741/blog/event/eventloop/beforeEnqueue_kzf6yl.png)
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636557741/blog/event/eventloop/afterEnqueue_qc5xr0.png)

為了進一步利用CPU空閒時間來處理後續的Task 1 至 Task N，作業系統會將I/O 種類的任務另外放進另一個Queue來處理，等到CPU處理完一段時間再回過頭依序檢查看看每一個I/O任務上是否已經等到對應I/O設備的回應，若等到回應時，會將任務重新排進對應CPU的Queue或者當場執行，通常來會為了增加效能而重新讓Scheduler根據任務性質來排進哪一個CPU的Queue。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636632653/blog/event/eventloop/reEnqueue_o24jrc.png)

### 事件
然而這種可以延宕實體CPU的任務並不侷限於上述的I/O種類任務，還有由I/O本身或者某些程式被某些行為給觸發出來的信號，然後再由藉由攔截信號來執行的特定任務X，在這裏的信號會當作是 "事件(event)" ，而為了執行特定工作X，勢必會產生出一個機制來監聽某種信號/事件和產生對應的特定任務X，而通常會使用兩種方式來實現：
1. pull (polling)：由系統在固定時間內產生詢問I/O設備和某些程式是否要產生事件/信號，若詢問成功便會直接執行對應I/O設備，但詢問失敗會容易造成不必要的效能損益，尤其是I/O設備和某些程式不止於1種，可能總加起來的數量會是N個，那麼詢問的任務數量在固定時間內也是N個
2. push：當事件發生時，由發送信號來源的I/O設備和某些程式主動通知系統事件發生了，要系統處理一下事件，而在事件發生前，系統可以做自己的事情，而發生的當下就會去處理事件，系統可以挑時間去處理，隨後處理完系統再回到當時處理前的工作狀態。

另外值得一提的是當特定任務X本身是由非系統本身的事件所觸發時，其(比如滑鼠點擊事件)事件會變得不可預知且有可能影響著其他任務所存取的內容和結果而使系統變得更不穩定(這問題被稱之Critical-Section Problem，其中Critical-Section是指多個任務共享的資源)，比如說有一個任務1想要存取某個資源下的X值來處理自己的任務內容，而唯獨只有X=1的時候才能讓任務1正常運作。


![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636642604/blog/event/eventloop/criticalSectionExample1_zflvd8.png)

然而突如其來的特定任務X想要修改該資源下的X值，使值從1轉換為2，由於兩個任務同時都各由獨立的CPU資源來執行，先後順序會無法從中定義，但若是先從特定任務X執行或者在執行任務1的存取X值的同時下任務X就已經修改的話，任務1會讀取到X=2，而這樣的讀取結果會根據前面所定下的規則-唯獨只有X=1的時候才能讓任務1正常運作，讓任務1呈現的結果是不正常或者無法正常執行任務1。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636642604/blog/event/eventloop/criticalSectionExample2_jnecq7.png)

面對這樣子的任務X之下，勢必得讓其他任務們透過一些機制來保證存取的內容不受到影響，比如 lock 這一個機制，前者會允許其他程式鎖住要存取容易被特定任務X改變的資料，而只要被鎖住並不會讓其他任務去存取或者變更，拿上面的例子來說明的話，就是用以下類似的代碼來實現，在任務1和特定任務X的程式碼塞入while、適當的lock、存取資源的方式(如下程式碼)，其中兩方都能夠共享存取資源resource，而lock會是他們之間能夠辨識的變數，兩者執行之前的lock值會是false，當其中一方改變了lock值，另一方能夠看到變動，所以在這裏只要讓任務1先去執行下面程式碼就能透過lock設定為true來鎖住資源，
```
while (lock) {}
  lock = true
  // access resource 
  lock = false
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636645008/blog/event/eventloop/lockExample_hpjnkk.png)

而當任務X要去執行的時候會因為lock為true而一直跑無限迴圈，直到任務1存取完資源時，就便將lock設定為false，使任務X能夠從無限迴圈跳開去存取資源，但這只是建立在其他任務會先於任務X存取內容之下，沒辦法無法完全保證任務X不會去修改內容，需要一個手段能夠控制任務X
### 事件系統

事件本身和事件的特定任務X之間的互動會構成一種事件系統，主要大致上分為兩種，但在這文章中只探討於第二點。
1. 由硬體和作業系統所內建的事件系統：當事件發生時，會以 "中斷(interrupt)" 目前系統的執行並讓系統保存目前執行狀態X轉由處理事件，隨後處理完就藉由先前執行狀態X還原當時的狀態去繼續執行著當時的任務，而這樣子的好處是避免系統一直產生詢問事件是否發生的任務在Queue。

// 上圖

在這個事件系統中，由於中斷本身是出自於硬體和作業系統所內建的事件，而這些事件本身只專注於I/O設備和某些特定程式，所以不太容易去改變除了這些以外的資料或者資源，但還是不保證這些特定任務X不去改變，所以系統和硬體本身會提供額外內建的手段來保證在這個事件系統下的資料正確性。

2. 建立在作業系統的基礎軟體層面上事件系統：作業系統為了充分讓應用層級的程式運用事件觸發而將 "每個I/O裝置以及特定程式的發送信號以及如何攔截到它們來做特定處理" 封裝成事件處理API，而應用程式只需要調用這些API就能將 "事件處理內容綁定某個裝置下的點擊事件"、"攔截事件並按照事件處理內容"，但至於如何操作和管理這些事件，就要看應用層級的程式所使用的事件系統是如何處理事件，而事件系統通常會由程式語言、執行環境來提供。


## Event Loop 誕生
在基礎軟體層面上的事件系統，由於作業系統所提供


## Event loop 運作程序
說明event loop如何運作

## 實現 Event loop 的重點 
queue
單執行緒
解決的問題

## 在JavaScript中的它
為了進一步了解它，會先從JavaScript如何執行、如何面對任務、在Node.js又是如何
1. 簡介 JavaScript 簡介、JavaScript 工作模式 皆為在瀏覽器處理。

## JavaScript 簡介
主要說明JavaScript 是單執行緒和原因、瀏覽器執行它
1. JavaScript的本質 - 單執行緒
2. 為啥是單執行緒
3. 它能夠產生多執行緒嗎？ 

## JavaScript 工作模式
JavaScript 單執行緒
介紹任務有哪種？同步任務/異步任務
簡介如何處理

### JavaScript 面對同步任務
介紹JavaScript如何處理

### 那 JavaScript 如何面對異步任務
介紹JavaScript如何處理
1. 如何在瀏覽器運作Event loop的形式


## Event Loop 在 Node.js 上
如何在Node.js實現Event Loop

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