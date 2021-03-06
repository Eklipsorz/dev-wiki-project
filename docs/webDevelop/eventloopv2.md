---
sidebar_position: 2
---

# Event loop 與 在JavaScript中的它


## JavaScript 背景
由網景和微軟掀起瀏覽器大戰中，瀏覽器的網頁在早期只能透過HTML檔案內容將一些文字和內容顯示在瀏覽器上，而這樣的內容就只是在電腦上顯示一張大海報，網景為了近一步打敗微軟而開發出能與網頁互動的程式語言-JavaScript，讓瀏覽器去執行網頁上的JavaScript來讓使用者的互動藉由該語言來實現特定效果，但後來微軟憑藉著作將瀏覽器綁定自家的作業系統來壟斷整個瀏覽器市場，最後打贏了網景，但網景所衍生出的JavaScript並未就此消失，而是進一步以Open Source的形式存活著並且成長為一種很成熟的語言。

## JavaScript 的執行緒
JavaScript的出生是因應想要讓使用者對網頁內容進行某些互動來執行某些任務，甚至可以透過改變DOM來進一步改變網頁，其中透過DOM來改變網頁是它的最大特點，但DOM的節點本身在多個執行緒下的執行環境是一個很容易被改變且會發生預期外的結果或者使網頁無法正常，比如說有多個執行緒，每一個執行緒都想對同一個DOM節點做寫入、刪除、讀取的動作，若讓負責刪除的執行緒先執行，那麼剩餘做讀取、寫入的執行緒會因為找不到該節點而出錯。

為了避免這樣子的問題發生，JavaScript 本身就被設定成只會有一個執行緒來執行任務且必須是個直譯語言，而在瀏覽器執行它的時候，其負責解析語言的元件只會建立執行一個主執行緒來負責執行JavaScript，同一個時間點只會有一個任務被執行。

當然這只是建立在JavaScript本身的限制，實際上來說執行環境(如瀏覽器、Node.js)會因應HTML5所提出的Web Worker標準而開放額外的API來讓JavaScript去調用並在執行環境上建立除了主執行緒以外的執行緒，而這些執行緒會受到主執行緒來控制，並且不能夠操作DOM節點，這樣子的提供不僅增加效能，也保證DOM節點會是單個執行緒(也就是主執行緒)來執行。

## 電腦處理任務的方式

一般來說，一個程式碼若要確實被執行，過去一定得將程式碼封裝成Process這基本形式來讓Scheduler去分配實體CPU執行，另外在這裏會用任務來稱呼特定功能的程式碼，並非真的是指能被當排程的基本單位，當封裝成Process形式之後，就會被放入一個準備佇列(Ready Queue)等待Scheduler排到他，最前面的任務會被Scheduler按照任務性質來將任務分配至特定的佇列(Queue)，每一個佇列都對應著實體CPU，等待Scheduler分配完最前面的任務後，再由次先的任務進行分配，後面以此類推，當然對應CPU的佇列也都是如此，會先從自己對應的佇列中挑第一個任務執行，然後執行完隨後挑次先，一直到佇列為空。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636632652/blog/event/eventloop/scheduling_szgjwp.png)

而現代作業系統不再以Process形式來當基本排程形式，而是以執行緒(Thread)為基本排程形式來給Scheduler，具體實現方式是將一個程序X上的功能切分成分成好幾份子功能並按照份數來建立執行緒，每個執行緒會從中拿到獨立功能來執行並且共享(可能會共享著程序X的變數、函式)著同一個程序X來代表著程序X執行某些功能，而這樣子的切分會由於同個程序X上的Thread為單位來佔用多個CPU資源執行，進而使一個程序X所發揮出來的效率會比單純以Process來排程來得好，畢竟後者最多只能拿到一個CPU資源來執行。在這裡我們皆用任務來稱呼每個要被排成的執行緒。


而電腦處理任務的方式，同步處理和非同步處理，理論上，同步處理下的任務們只會在同一個時間點內執行一個任務，而且每一個任務皆必須等待上一個的任務結束才能執行，而非同步處理下的任務們可以各自被執行，而且每一個任務都不用等待任何一個任務就能繼續執行。


然而，實際上會按照實體CPU的數量以及是否支援非同步處理，若CPU數量只有一個的話，只能夠支援同步處理，也就是一次只能執行一個任務(如下圖)。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636720649/blog/event/eventloop/singleCPU_xvyctj.png)

但若CPU數量多於一個的話，就能支援非同步處理，在這情況下就同時執行執行多個任務，但不能夠說完全不用等待，因為實體CPU也就N個，若要排進CPU的任務大於N個時，勢必會有任務需要等待前面的任務完成才能做。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636720649/blog/event/eventloop/multipleCPU_w9o1hc.png)

## JavaScript 如何處理任務
首先電腦本身只能看懂由0和1所構成的機械碼，它不會明白JavaScript的語法在講些什麼，而當時設計者為了讓開發者能夠快速開發來強占瀏覽器的市場而捨棄較嚴謹的編譯語言，改由直譯語言，其中編譯語言是一種事先將程式的語法(偏易於人類看懂的語法)轉譯成機械能看懂的形式，並直接讓執行環境去執行的，但由於可以事先轉譯，基本會要求開發者寫出的程式碼語法要先滿足一定程度的規則才能成功轉譯成機械能看懂的形式，比如要求開發者告知每個資料和變數的資料型別是什麼。而直譯語言則是不用事先編譯成機械碼，而是透過一個程式X來一行一行邊讀取程式碼邊轉譯成機械碼來執行，而這個程式X會是解釋器，通常執行成本會比編譯語言來得高，但透過不煩瑣的規則來讓開發者能夠快速開發。

實際上來說，JavaScript本身就實現著同步處理，瀏覽器會提供JavaScript解釋器(常以一個專門解析的JavaScript引擎來代表)以及讓它在瀏覽器建立一個Main Thread來滿足JavaScript的直譯和單一執行緒這兩個先決條件，當瀏覽器讀取JavaScript程式碼的時候，瀏覽器會先丟給JavaScript引擎去解析當前程式碼並轉譯成機械碼至Main Thread來執行，而執行方式就是將機械碼以另一個thread形式去丟給Scheduler給實體CPU執行。
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

## 什麼是事件

當對特殊元件做出某些行為時，這些元件會發射對應行為的信號，然後再藉由攔截信號來執行的特定任務X，在這裏信號正是被稱作為 "事件(event)"，而特定任務X則是這個事件發生時所要做的事情，事件和特定任務X之間的互動可以實現使用者對於軟體上的元件互動，比如點擊某個按鈕就跑出一系列的清單、滑動卷軸能夠往下看後續內容。而為了實現整體事件系統的處理，勢必會得要有一個機制在軟體Y上來監聽某種信號/事件以及產生對應的特定任務X。

### 如何實現事件？

在這裏會有三種方式來實現：
1. pull (polling)：由軟體Y在固定時間內產生任務去詢問對應元件是否要產生事件/信號，若詢問結果是要產生，便直接執行對應的特定任務X，但詢問一直失敗很容易造成不必要的效能損益，尤其是關心的元件可能不止於1種，可能總加起來的數量會是N個，那麼負責詢問的任務數量在固定時間內也是N個。
2. push：當事件發生時，由負責發送信號來源的元件主動通知軟體Y，要軟體Y處理事件，而在事件發生前，系統可以做自己的事情，而發生的當下就會以執行特定任務X來處理事件，隨後處理完系統便再回到當時處理前的工作狀態，但實際上來說這樣會把所有特定任務X的處理集中在軟體Y的主要執行緒上，而非由元件產生執行緒處理，這會使主要執行緒的效能逐漸變差。
3. 軟體Y以同步去處理：軟體Y會保持等待，直到對應元件產生事件，便會執行對應的特定任務X，但這個方法很有可能把大部分的時間都耗在等待事件發生，而實際上那段時間若用上的話，卻能夠處理完後續任務。

一般來說，軟體Y為了效率而可能會採用push這個異步處理來實現事件，甚至在當元件上的特定事件發生時，就會產生特定的執行緒來執行每個事件的特定任務，雖然效率上，可以藉由這樣而不把事件處理全給軟體Y的主要執行緒上，但實際上來說，若這些執行緒使用某些共用資源時，會產生一種名為Critical-Section Problem 或者 Race Condition，且由於事件對應的特定任務會因為事件的不可預知性而使這個問題更難解決，另外事件就算給予軟體Y主執行緒來執行，還是因爲共享資料的關係而不能保證這些執行緒不會有這些問題。


## 事件會發生什麼問題

不論是否為Critical-Section Problem 或者 Race Condition，執行緒本身是著共享同一份程序(Process)的資源並從中分出一個子功能來執行，所以共享資源是難以避免，而每一個要使用共用資源的執行緒肯定存在著存取共用資源的程式碼區塊，而這些區塊被稱之為Critical-Section，當有多個執行緒在這個區塊是存取(Access)同一個資源時，而存取可以是修改、刪除、讀取、寫入等一系列操作，這些執行緒很有可能因為彼此都想要存取同一個資源而使每個執行緒的最後執行結果變得不如預期，有些可能因為讀取到其他執行緒修改的值、也有些則是因為讀取到的資源被刪除了，而這種彼此競相存取/修改同一個資源的行為就叫做競爭情況(Race Condition)，此時發生該情況的Critical Section就稱之為Critical-Section Problem。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636791025/blog/event/eventloop/raceConditionDiag_vp78kk.png)


### 問題例子
假設說有一個任務1想要存取某個資源Y下的X值來處理自己的任務內容，而資源Y本身對於系統而言是可以共享存取的，所以任務1的Critical Section會是存取資源Y的X值之程式碼(如下)，首先會透過id來取得資源Y這個物件，然後透過其資源Y的getXValue來取得X值，預期這個任務1會拿到X=1來做處理，

```
// Critical Section
resource = getResourceById(id)
result = resource.getXValue()
// Critical Section
```

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636642604/blog/event/eventloop/criticalSectionExample1_zflvd8.png)

然而突如其來的特定任務X想要修改該資源Y下的X值，使值從1轉換為2，任務2的Critical Section會是如下，會透過id來取得資源Y這個物件，然後透過其資源Y的modifyXValue(2)來將X值修改成2，
```
// Critical Section
resource = getResourceById(id)
result = resource.modifyXValue(2)
// Critical Section
```

若這兩個任務同時都各由獨立的CPU資源來執行，執行先後順序會無法從中定義，但若是先從特定任務X執行或者在執行任務1的存取X值的同時下任務X就已經修改的話，任務1只會讀取X=2，而非是讓任務1讀取X=1，這樣子的存取很有可能會讓任務1的執行結果是不如預期，甚至無法正常執行
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636642604/blog/event/eventloop/criticalSectionExample2_jnecq7.png)

## 系統如何解決問題
面對這樣子的Critical-Section Problem，系統主要有兩種方式，第一種使用鎖(lock)機制，讓先存取資源的人能夠鎖住資源，不讓其他人在同時間使用資源，第二種使用事件迴圈(event loop)，這種會強制將要存取Critical Section的任務轉換為同步處理，透過同步處理的特性-同一時間只允許一個任務執行，不過這兩種只是阻止同時間的修改，若先修改同一份資源的任務先執行的話，還是會讓後面執行的任務會出現不如預期的問題，拿先前的問題例子中的任務1和任務X，任務1只要先存取資源Y，就能皆能透過這上述任意方式來保證任務1再存取的同時間不會有任務X來存取或者修改，但是如果任務X先於任務1的話，那麼任務1到最後還是會取到X=2，為此，就有人提出另一種event loop來解決。


### lock 
由於資源本身的存取是透過Critical Section來達成，因此可以被視作為資源的代言人，而 lock 這一個機制會允許任何一個先存取到Critical Section的程式替這個Critical Section或者對應的資源上鎖，使得後面想要存取同一個資源的任務都會被鎖給擋住(Blocked)，而當上鎖該資源並存取資源的任務只要存取完畢，便會把鎖解開讓後面的任務去存取，當然先搶到的任務可以擁有先鎖住資源的權利。除了上述之外，鎖本身會阻塞(Block)想要存取同一個資源的任務，因此若想要存取同一個資源的任務一多的時候，效能很有可能會因為阻塞而衰減。


以問題例子上的任務1和任務X為例，具體實現會是宣告兩個任務都能辨識的lock變數以及基於lock的while來實現lock，將這些分別放置在兩個任務對於同份資源的Critical Section中(內容如下)，而一開始的lock值會是false，當其中一方改變了lock值，另一方能夠看到變動。

```
// 任務1的Critical Section
while (lock) {}
  lock = true
  resource = getResourceById(id)
  result = resource.getXValue()
  lock = false

// 任務X的Critical Section
while (lock) {}
  lock = true
  resource = getResourceById(id)
  result = resource.modifyXValue(2)
  lock = false
```

所以在這裏只要讓任務1先去執行下面程式碼就能透過lock設定為true來鎖住資源Y，而當任務X要去執行的時候會因為lock為true而一直跑無限迴圈，直到任務1存取完資源Y時，
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636795235/blog/event/eventloop/lockExample_y0f60k.png)

，直到任務1存取完資源時，就便將lock設定為false，使任務X能夠從無限迴圈跳開去修改資源Y的X值為2

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636795564/blog/event/eventloop/lockExample2_nt7vjj.png)


### simple event loop
event loop 是指著負責分發適當任務執行的流程迴圈，也就是會重複做相同分發任務執行，只是任務會隨著分發而有所不同，本身會運用一個佇列(Queue)、從佇列中挑出一個最前面的元素、挑出的元素只給一個執行緒來執行這三個因素來將原本設定為異步處理的任務轉換成以同步處理的任務，從而解決Critical Section Problem 以及 鎖帶來的Blocking問題，具體方式如下，每一次由事件觸發而衍生出事件處理任務皆放入任務佇列(Task Queue)，而負責轉交給Scheduler或者執行任務的執行緒只會有一個，當中會從佇列的最前面任務挑出來放入至執行緒去做執行或者分派給Scheduler去執行，當執行完之後便會挑選下一個在佇列的任務，直到清空。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636798774/blog/event/eventloop/simpleEventLoop_jdc87i.png)

所有放在佇列中的任務全都會如同步處理那樣：
>同步處理下的任務們只會在同一個時間點內執行一個任務，而且每一個任務皆必須等待上一個的任務結束才能執行

而這樣子的處理也避免使用鎖以及鎖帶來的阻塞現象，當然也得要考量每一個被執行的任務是否本身就是個阻塞其他任務的任務。這方法本身效能會因受限於只有單執行緒能執行以及像鎖那樣無法完全避免Critical Section Problem帶來的不可預期之執行結果。

### complex event loop
為了進一步提升效能和緩解Critical Section Problem帶來的不可預期之執行結果，就額外添加多執行緒以及額外Queue來延遲真正造成不可預期的任務，在這裏由一個主要執行緒根據哪些任務是真正容易對共用資源造成不可預期的任務來分發至不同地方來執行：擁有多個執行緒的空間(Thread Pool)、暫緩執行用的空間，通常會是對某些內容進行寫入、刪除、變更的功能會被歸納成容易對共用資源造成不可預期的任務，讀取、修改某副本內容、沒涉及共用資源的功能則是歸納成較安全的任務(Thread-safe)。


首先一開始還是會由每一次由事件觸發而衍生出事件處理任務都放入任務佇列，由主要執行緒(Main Thread)從佇列中挑選第一個任務並判別其性質進行分發，若是不會對共用資源造成任何修改的任務，則會被放入Thread Pool，在這裏會分配多個執行緒去執行任務，而若主要執行緒判別任務為會對共用資源造成任何修改的任務，則放入另一個Queue等待適當時機回到Main Thread執行

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636803337/blog/event/eventloop/complexEventLoop_ocm5jl.png)



Thread Pool本身是由固定數量的執行緒組成，平時會保持著可幫助任務執行的狀態，但由於數量有限，所以會用佇列來存放被放進Thread Pool的任務，等著哪些執行緒是可幫忙執行的狀態再來從佇列中挑選前面的任務來執行，當沒有執行緒可用時，便會停止挑選直到有可用的執行緒。所有被執行緒執行完的任務會回傳至主執行緒做最後的處理以及回傳結果。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636804100/blog/event/eventloop/threadPoolOnEventLoop_rmfugm.png)

而被真正容易對共用資源造成不可預期的任務會被放進一個Queue，當然這個佇列也是等待著執行緒去執行，只是負責執行的執行緒和執行方式都跟上述有所不同，由於這些任務很容易對共用資源造成不必要且不可預期的結果，所以會試著等其他要使用共用資源的任務執行完再來從這個佇列挑出來至主執行緒來執行。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636804544/blog/event/eventloop/queueOnEventLoop_ilrszi.png)

整體來說，這個event loop架構會盡可能透過開放多執行緒讓任務們的執行效率提高以及藉由佇列來分離出一些很容易對共用資源造成不必要且不可預期的任務並放進另一個佇列等待時機在主執行緒執行。

## JavaScript 的事件

在這裏的JavaScript由於本身時常應用於瀏覽器，所以會以瀏覽器的事件來進行說明，首先當瀏覽器將網頁上所有元件轉換成適當的實體模型以及計算每個元件的實體位置、大小等資訊時，便賦予網頁上的所有元件在執行環境(瀏覽器)上是具有事件接收和事件觸發的物件，而所有對事件/信號的發送會由瀏覽器(瀏覽器還調用著作業系統的API來接收信號)負責轉送。 

也就是說當使用者對著元件X進行點擊且設定名為onElementXClicked函式來當作事件發生所要做的處理時，addEventListener本身會先建立專屬於元件X的EventListener物件，該物件會以清單的形式來紀錄元件X上所有綁定的事件種類和對應事件處理內容，而瀏覽器實際上會在利用先前提到的資訊計算、實體模型去判斷這個點擊信號是屬於哪個DOM物件，然後鎖定該DOM物件就檢測其物件是否有相關事件物件(比如EventListener物件)，若有的話，就根據事件種類來找尋對應的函式來當作事件處理，在這裏函式會是onElementXClicked，所以點擊元件後會顯示hi這字串
```
elementX.addEventListener('click', function onElementXClicked(event) {
  console.log('hi)
})
```

### Event Flow 
然而，如果使用者對著巢狀結構下的子元件來進行互動時，比如類似於程式碼中的元素3(element3)，那麼瀏覽器該如何判定這次的互動/事件是屬於哪個元素呢？瀏覽器大可直接根據事件是源自於哪裡來將事件歸類於element3，並且由它的事件處理來處理
```
<element1>
   <element2>
   	<element3>
		content
	  </element3>
   </element2>
</element1>

```

可由於是巢狀關係(如下圖)，元素3(element3)被元素2(element2)所包含著，而元素2(element2)被元素1(element1)包含著，嚴格來說，當使用者對元素3(element3)做互動，同樣地也對著另外兩個元素進行互動，更甚至說包含這些元素的元素(比如body)也跟著互動。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1630587482/blog/event/threeElements_lohr6c.png)


這時，瀏覽器可以有幾種選擇去決定事件是屬於哪個元件，第一種選擇是按照之前的規則，直接將事件歸類於元素3(element3)並發送信號給元素3，並對元素3的EventListener來找尋對應的事件處理，另外一種則是使用事件流(event flow)將訊號傳遞給每個包含元素3的元素，並試著對這些元素的EventListener找尋對應的事件處理，只是傳遞方向又可以細分兩種，一種是從最外圍的元素1開始往內傳遞的事件捕獲(event capture)，也就是信號會先傳遞至元素1，接著在傳遞元素2，最後傳遞元素3，另外一種則是從元素3往外傳遞信號的事件冒泡(Event bubbling)，也就是信號會先傳遞至元素3(發生事件的來源處)，接著在傳遞至元素2，最後傳遞元素1。

不過實際上來說，瀏覽器傳遞信號會一次分三種階段來傳遞信號，當對一個目標元件X產生互動時，第一個階段會先從包含元件X的最外面元件到內部元件來傳遞信號的捕獲階段(Capturing Phase)，第二個階段為從目標元件X的父元件傳遞信號至目標元件X的目標階段(Target Phase)，最後一個階段會先從目標元件X往外傳遞信號的冒泡階段(Bubbling Phase)，當使用者對某個元件X進行互動時，瀏覽器會先透過第一個階段來傳遞信號，接著就是第二個階段：將信號傳遞至實際發生事件/互動的元件X，最後就是第三個階段傳遞信號。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636879992/blog/event/currentPropagationPath_rj9x5j.png)

### JavaScript 的 Event Loop
當瀏覽器按照event flow傳遞事件/信號的時候，若單純由JavaScript負責接收事件/信號與事件處理的話，會因爲只有執行環境(瀏覽器)為它提供的Main Thread只有一個而容易發生Blocking的現象，讓後續的任務無法繼續做。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636816028/blog/event/eventloop/baseBrowserModel_akh3kj.png)

因此瀏覽器為了補足這塊而提供一些Web API讓JavaScript能夠額外建立thread去處理接收事件/信號與事件處理，但這些thread本身並不夠直接執行這些事件處理，因為若由它處理就有可能違背JavaScript的設計初衷，比如只允許單個執行緒來對DOM節點做操作，必須讓這些thread將事件處理轉交給Main thread，由JavaScript引擎親自執行，但直接轉交又會打亂Main thread對於其他任務的執行，因此在這裏會引用event loop的分發概念來試著將事件處理轉交給Main thread，在這裏所採用的event loop架構會是上述提到的simple event loop，會建立一個Task Queue或者CallBack Queue(由於事件綁定的函式是屬於callback類型函式，而該Queue又是專門存放這類型，所以得名)來接收所有被觸發的事件處理，若Task Queue不為空的話，event loop會一直檢測Call Stack是否為空，接著等Call Stack為空時，便從Queue分發事件處理至Stack來執行。


在這裏我們從上述描述建構瀏覽器所提供的WebAPI、Task Queue以及JS本身的Call Stack、Main thread，首先當還沒有任何事件綁定和沒有事件的話，瀏覽器會繼續按照JavaScript的特性來一行又一行轉譯並放入Main Thread來執行，其中Call Stack也塞滿了一些函式以及目前所讀取的檔案本身-Main Function。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636820736/blog/event/eventloop/eventLoopinJS_eftuh5.png)


若JavaScript檔案中的程式碼包含以下並且讓瀏覽器讀取到時，這段程式碼會間接呼叫WebAPI中的DOM來將onElementXClicked當作是特定元件X的點擊事件所要做的處理，並建立額外Thread去負責管理每個元件的事件接收和事件處理。
```
elementX.addEventListener('click', function onElementXClicked() {
  // do something
})
```

當特定元件X被點擊一次的時候，會將對應點擊事件的函式-onElementXClicked放到Task Queue，接著點第二次，就放入第二個相同函式，最後點第三次，就放第三個相同函式，
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636821626/blog/event/eventloop/generateEventHandler_npw18g.png)

這時會觸發event loop本身的檢測，它會檢查Call Stack是否為空，但Task Queue為空的時候，就不會特意檢查Call Stack，當Call Stack為空時，就便從Task Queue抽出第一個函式-onElementXClicked，執行完就換下一個任務，直到清空Task Queue
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636821626/blog/event/eventloop/dispatchEventHandler_uibmen.png)

### Event Loop 例子

在這裏以HTML形式設定兩個按鈕元件，它們的id分別為testbtn1、testbtn2，而JavaScript則是為這兩個按鈕添加點擊事件，而對應的事件處理皆為onClick，接著我輪流點testbtn1、testbtn2，一直到點擊第五次，也就是點擊完testbtn1就停下了。

```
// inside js 

$.on('#testbtn1', 'click', function onClick() {
    console.log('button1 is clicked')
});
$.on('#testbtn2', 'click', function onClick() {
    console.log('button2 is clicked')
});

// inside html 

<button id="testbtn1">button1</button>
<button id="testbtn2">button2</button>
```

影片可以點擊下方來看，首先瀏覽器上的JavaScript遇到事件綁定時會呼叫瀏覽器下的Web API去替那個兩個按鈕增加點擊事件，接著當點擊testbtn1時，瀏覽器負責管理事件接收和處理的執行緒會將testbtn1點擊事件時對應的函式-onClick放入至Callback Queue，接著再點擊testbtn2時，瀏覽器會把testbtn2點擊時對應的函式-onClick放入至Callback Queue，然後隨著後續的點擊來把對應的函式放入至Queue，過程中，負責管理事件接收和處理的執行緒會把Queue的第一個函式放入至Call Stack來執行，隨後執行完就被移出，再挑下一個在Queue的函式，直到Queue被清空。
[點擊我看例子](https://youtu.be/50zxZ7GawmA)


### setTimeout
setTimeout 本身是瀏覽器的WebAPI所提供的定時器，當瀏覽器在JavaScript檔案讀取到以下格式時，便會呼叫WebAPI下的setTimeout來執行，此時瀏覽器會建立一個thread去按照secs秒數來等待，等到時間到的時候會把指定函式-funct 放入至Task Queue或者CallBack Queue，而指定函式會跟其他上述提到的事件處理函式一樣，等到Call Stack為空才會將Queue的funct移到Main Thread來執行，而Call Stack會放上funct。 
```
setTimeout(function funct() {
    // do something
}, secs)
```

### setTimeout 例子1
設定一個等待1秒才執行的計時器，首先當瀏覽器的JavaScript引擎讀取到setTimeout和1000時，就便呼叫瀏覽器的WebAPI-setTimeout來建立一個thread來實現計時器，一開始會按照1000ms來等待，接著時間一到就把對應的函式-timeout放入至Callback Queue，然後等到Call Stack為空時，就把Queue中的timeout函式放到Main Thread來執行。
```
setTimeout(function timeout() {
    console.log('delay 1 sec');
}, 1000);

console.log('Hello!');
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636826224/blog/event/eventloop/setTimeout1sec_s5m432.gif)

### setTimeout 例子2
設定一個等待0秒才執行的計時器，雖然看形式上會是先於顯示Hello，但實際上還得瀏覽器還得呼叫、對應函式放入Queue、等待Call Stack為空才能正式執行，所以基本上會慢於顯示Hello。 
```
setTimeout(function timeout() {
    console.log('delay 0 sec');
}, 0);

console.log('Hello!');
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636826230/blog/event/eventloop/setTimeout0sec_dkuv8i.gif)


## 參考資料
1. [Event dispatch and DOM event flow](https://www.w3.org/TR/DOM-Level-3-Events/#event-flow)
2. [event: pull vs. push](https://softwareengineering.stackexchange.com/questions/363397/how-does-an-event-listener-work)
3. [Concurrency vs Event Loop vs Event Loop + Concurrency](https://medium.com/@tigranbs/concurrency-vs-event-loop-vs-event-loop-concurrency-eb542ad4067b)
4. [Event Queue](https://gameprogrammingpatterns.com/event-queue.html)
5. [Event loop、Call Stack 與 Task Queue](https://gist.github.com/librarylai/de189c3b53a3c6a24d40f2f6a10ee08c)
6. [Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
7. [jserv 對於event loop + Thread pool的課程作業](https://hackmd.io/@jwang0306/sehttpd)