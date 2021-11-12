---
sidebar_position: 2
---

# Event loop 與 在JavaScript中的它


## JavaScript 背景
由網景和微軟掀起瀏覽器大戰中，瀏覽器的網頁在早期只能透過HTML檔案內容將一些文字和內容顯示在瀏覽器上，而這樣的內容就只是在電腦上顯示一張大海報，網景為了近一步打敗微軟而開發出能與網頁互動的程式語言-JavaScript，讓瀏覽器去執行網頁上的JavaScript來讓使用者的互動藉由該語言來實現特定效果，但後來微軟憑藉著作將瀏覽器綁定自家的作業系統來壟斷整個瀏覽器市場，最後打贏了網景，但網景所衍生出的JavaScript並未就此消失，而是進一步以Open Source的形式存活著並且成長為一種很成熟的語言。

## JavaScript 運作方式
JavaScript的出生是因應想要讓使用者對網頁內容進行某些互動來執行某些任務，甚至可以透過改變DOM來進一步改變網頁，其中透過DOM來改變網頁是它的最大特點，但DOM的節點本身在多個執行緒下的執行環境是一個很容易被改變且會發生預期外的結果或者使網頁無法正常，比如說有多個執行緒，每一個執行緒都想對同一個DOM節點做寫入、刪除、讀取的動作，若讓負責刪除的執行緒先執行，那麼剩餘做讀取、寫入的執行緒會因為找不到該節點而出錯。

為了避免這樣子的問題發生，JavaScript 本身就被設定成只會有一個執行緒來執行任務且必須是個直譯語言，而在瀏覽器執行它的時候，其負責解析語言的元件只會建立執行一個主執行緒來負責執行JavaScript，同一個時間點只會有一個任務被執行。

當然這只是建立在JavaScript本身的限制，實際上來說執行環境(如瀏覽器、Node.js)會因應HTML5所提出的Web Worker標準而開放額外的API來讓JavaScript去調用並在執行環境上建立除了主執行緒以外的執行緒，而這些執行緒會受到主執行緒來控制，並且不能夠操作DOM節點，這樣子的提供不僅增加效能，也保證DOM節點會是單個執行緒(也就是主執行緒)來執行。

## 任務執行方式

一般來說，一個程式碼若要確實被執行，過去一定得將程式碼封裝成Process這基本形式來讓Scheduler去分配實體CPU執行，在這裡會以任務(Task)來稱呼這些形式，當封裝成Process形式之後，就會被放入一個準備佇列(Ready Queue)等待Scheduler排到他，最前面的任務會被Scheduler按照任務性質來將任務分配至特定的佇列(Queue)，每一個佇列都對應著實體CPU，等待Scheduler分配完最前面的任務後，再由次先的任務進行分配，後面以此類推，當然對應CPU的佇列也都是如此，會先從自己對應的佇列中挑第一個任務執行，然後執行完隨後挑次先，一直到佇列為空。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636632652/blog/event/eventloop/scheduling_szgjwp.png)

而現代作業系統不再以Process形式來當基本排程形式，而是以執行緒(Thread)為基本排程形式來給Scheduler，具體實現方式是將一個程序X上的功能切分成分成好幾份子功能並按照份數來建立執行緒，每個執行緒會從中拿到獨立功能來執行並且代表著相同程序X來執行，而這樣子的切分會由於同個程序X上的Thread為單位來佔用多個CPU資源執行，進而使一個程序X所發揮出來的效率會比單純以Process來排程來得好，畢竟後者最多只能拿到一個CPU資源來執行。在這裡我們皆用任務來稱呼每個要被排成的執行緒。


而電腦處理任務的方式，同步處理和非同步處理，理論上，同步處理下的任務們只會在同一個時間點內執行一個任務，而且每一個任務皆必須等待上一個的任務結束才能執行，而非同步處理下的任務們可以各自被執行，而且每一個任務都不用等待任何一個任務就能繼續執行。


然而，實際上會按照實體CPU的數量以及是否支援非同步處理，若CPU數量只有一個的話，只能夠支援同步處理，也就是一次只能執行一個任務(如下圖)。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636720649/blog/event/eventloop/singleCPU_xvyctj.png)

但若CPU數量多於一個的話，就能支援非同步處理，在這情況下就同時執行執行多個任務，但不能夠說完全不用等待，因為實體CPU也就N個，若要排進CPU的任務大於N個時，勢必會有任務需要等待前面的任務完成才能做。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636720649/blog/event/eventloop/multipleCPU_w9o1hc.png)

## JavaScript 如何實現同步執行
首先電腦本身只能看懂由0和1所構成的機械碼，它不會明白JavaScript的語法在講些什麼，而當時設計者為了讓開發者能夠快速開發來強占瀏覽器的市場而捨棄較嚴謹的編譯語言，改由直譯語言，其中編譯語言是一種事先將程式的語法(偏易於人類看懂的語法)轉譯成機械能看懂的形式，並直接讓執行環境去執行的，但由於可以事先轉譯，基本會要求開發者寫出的程式碼語法要先滿足一定程度的規則才能成功轉譯成機械能看懂的形式，比如要求開發者告知每個資料和變數的資料型別是什麼。而直譯語言則是不用事先編譯成機械碼，而是透過一個程式X來一行一行邊讀取程式碼邊轉譯成機械碼來執行，而這個程式X會是解釋器，通常執行成本會比編譯語言來得高，但透過不煩瑣的規則來讓開發者能夠快速開發。

實際上來說，瀏覽器會提供JavaScript解釋器(常以一個專門解析的JavaScript引擎來代表)以及讓它在瀏覽器建立一個Main Thread來滿足JavaScript的直譯和單一執行緒這兩個先決條件，當瀏覽器讀取JavaScript程式碼的時候，瀏覽器會先丟給JavaScript引擎去解析當前程式碼並轉譯成機械碼至Main Thread來執行，而執行方式就是將機械碼以另一個thread形式去丟給Scheduler給實體CPU執行。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636724367/blog/event/eventloop/simpleBrowserSystem_kt2jov.png)

在這裏由於瀏覽器本身就只有Main Thread這一個能夠執行JavaScript，而JavaScript本身是依照一行一行來讀取並轉譯執行，也就是說同一個時間內只會有一個任務(夾帶著轉譯後的JavaScript)能夠被Main Thread執行，而後一個任務(夾帶著轉譯後的另一個JavaScript)必須等它被Main Thread完成才能被執行，從這樣子來看，就是個典型的同步處理，只是Main Thread儼然對他們而言就是個實體CPU

### 同步處理的常見問題 - Blocking
根據同步處理的特性：
>同步處理下的任務們只會在同一個時間點內執行一個任務，而且每一個任務皆必須等待上一個的任務結束才能執行

當目前執行的任務一直使用著CPU資源或者佔用著CPU並向外部請求時，會阻塞(block)後續的任務(程式碼)沒辦法及時執行，而這個現象叫做阻塞(Blocking)，以下用程式碼來示範，下面分別會從foo.com、bar.com、qux.com取得資料，並列印出來，過程中會因為getSync佔用著CPU資源去索求外部資源，並等待外部發送回應給getSync，其結果就是整個程式的執行會因為前面三段getSync而產生阻塞現象。


```
let foo = $.getSync('//foo.com')
let bar = $.getSync('//bar.com')
let qux = $.getSync('//qux.com')

console.log(foo)
console.log(bar)
console.log(qux)
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636733654/blog/event/eventloop/BlockingExample_z4gh1m.gif)

## Call Stack
當遇到函式呼叫時，會先將目前呼叫的函式X紀錄下來並放進(Push)一個呼叫堆疊(Call Stack)，而當在目前函式X中遇到了return 或者 沒東西可讓函式X執行，就表示該函式X要回傳或者結束執行，此時就會從堆疊移出(Pop)最上面的函式X(也就是目前函式X)，而通常檔案本身也可以視作為一個大函式-main function，當瀏覽器執行這個檔案時就便是呼叫了大函式，

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636727784/blog/event/eventloop/CallStackDiagram_akoh7f.png)


而當大函式/main function執行完畢時，便也會從Call Stack移出它。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636727969/blog/event/eventloop/popMainFunction_op58q2.png)

### Call Stack 例子
在這裏我們建立三個函式分別為funct1()、funct2()、funct3()，然後在定義他們之後呼叫他們去執行來觀察他們與Call Stack之間發生什麼事情，範例程式碼如下：
```
function funct1() {
    console.log('funct1')
    funct2()
}

function funct2() {
    console.log('funct2')
    funct3()
}

function funct3() {
    console.log('funct3')
}

funct1()
console.log('end')
```

在這裏是採用Loupe平台來觀測程式碼和Call Stack之間的互動，在這個平台中並不會把main function特意顯示，程式碼結果如下，首先系統會先呼叫funct1()，執行funct1後便將funct1()推入在Call Stack中去紀錄，然後去印funct1這字串，接著funct1()又呼叫funct2()，這時會將funct2()推入在Call Stack中紀錄，funct2()會在funct1()之上，然後去印funct2這字串，最後再由funct2()呼叫funct3()，這時會將funct3()推入在Call Stack中紀錄，funct3()會在funct2()之上，然後去印funct3這字串。


![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636728641/blog/event/eventloop/CallStackExample_kay9ld.gif)

而funct3執行完畢後，也沒呼叫什麼以及執行什麼，此時Call Stack會移出目前最上面的函式-funct3()，而系統會在移出後自動抓最上面的函式-funct2()來返回funct2()呼叫funct3()的地方，接著funct2()也沒呼叫什麼以及執行什麼，此時Call Stack會移出目前最上面的函式-funct2()，而系統會在移出後自動抓最上面的函式funct1()來返回funct1()呼叫funct2()的地方，接著funct1()也沒呼叫什麼以及執行什麼，就便讓Call Stack移除目前最上面的函式-funct1()，最後回到main function呼叫funct1()的地方，隨後在印個end這字串

### Call Stack 極端例子 - 不斷地呼叫
當函式一直不斷地呼叫時，會不斷往Call Stack推入更多函式，這會使得Call Stack本身被佔用，以下面的程式碼做為例子，程式碼會先定義funct1()，其內部會是呼叫funct1()，接著在呼叫funct1()，最後結果會是讓funct1()不斷被自己呼叫，直到被開發者中斷、系統自己中斷才停止。
```
function funct1() {
    funct1()
}

funct1()
```

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636732264/blog/event/eventloop/infExample_boi9hh.gif)

## JavaScript 如何實現異步執行

1. Stack 
2. Task Queue
3. webAPI

### setTimeOut

## 什麼是事件
然而這種可以延宕實體CPU的任務並不侷限於上述的I/O種類任務，還有由I/O本身或者某些程式被某些行為給觸發出來的信號，然後再由藉由攔截信號來執行的特定任務X，在這裏的信號會當作是 "事件(event)" ，而為了執行特定工作X，勢必會產生出一個機制來監聽某種信號/事件和產生對應的特定任務X，而通常會使用兩種方式來實現：
1. pull (polling)：由系統在固定時間內產生詢問I/O設備和某些程式是否要產生事件/信號，若詢問成功便會直接執行對應I/O設備，但詢問失敗會容易造成不必要的效能損益，尤其是I/O設備和某些程式不止於1種，可能總加起來的數量會是N個，那麼詢問的任務數量在固定時間內也是N個
2. push：當事件發生時，由發送信號來源的I/O設備和某些程式主動通知系統事件發生了，要系統處理一下事件，而在事件發生前，系統可以做自己的事情，而發生的當下就會去處理事件，系統可以挑時間去處理，隨後處理完系統再回到當時處理前的工作狀態。


### 事件是同步執行？還是異步執行？
 
## 事件會發生什麼問題
另外值得一提的是當特定任務X本身是由非系統本身的事件所觸發時，其(比如滑鼠點擊事件)事件會變得不可預知且有可能影響著其他任務所存取的內容和結果而使系統變得更不穩定(這問題被稱之Critical-Section Problem，其中Critical-Section是指多個任務共享的資源)，比如說有一個任務1想要存取某個資源下的X值來處理自己的任務內容，而唯獨只有X=1的時候才能讓任務1正常運作。


![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636642604/blog/event/eventloop/criticalSectionExample1_zflvd8.png)

然而突如其來的特定任務X想要修改該資源下的X值，使值從1轉換為2，由於兩個任務同時都各由獨立的CPU資源來執行，先後順序會無法從中定義，但若是先從特定任務X執行或者在執行任務1的存取X值的同時下任務X就已經修改的話，任務1會讀取到X=2，而這樣的讀取結果會根據前面所定下的規則-唯獨只有X=1的時候才能讓任務1正常運作，讓任務1呈現的結果是不正常或者無法正常執行任務1。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636642604/blog/event/eventloop/criticalSectionExample2_jnecq7.png)



## 系統如何解決問題

### lock 
lock 這一個機制會允許其他程式鎖住要存取容易被特定任務X改變的資料，而只要被鎖住並不會讓其他任務去存取或者變更，拿上面的例子來說明的話，就是用以下類似的代碼來實現，在任務1和特定任務X的程式碼塞入while、適當的lock、存取資源的方式(如下程式碼)，其中兩方都能夠共享存取資源resource，而lock會是他們之間能夠辨識的變數，兩者執行之前的lock值會是false，當其中一方改變了lock值，另一方能夠看到變動，所以在這裏只要讓任務1先去執行下面程式碼就能透過lock設定為true來鎖住資源，
```
while (lock) {}
  lock = true
  // access resource 
  lock = false
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636645008/blog/event/eventloop/lockExample_hpjnkk.png)

而當任務X要去執行的時候會因為lock為true而一直跑無限迴圈，直到任務1存取完資源時，就便將lock設定為false，使任務X能夠從無限迴圈跳開去存取資源，但這只是建立在其他任務會先於任務X存取內容之下，沒辦法無法完全保證任務X不會去修改內容，需要一個手段能夠控制任務X


### event loop


## JavaScript 的事件處理
1. 是採用event loop
2. 實際運作方式

## Node.js 的事件處理
1. 運作方式
