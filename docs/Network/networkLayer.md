---
sidebar_position: 1
---

# 網路模型


## OSI (Open System Interconnection Model) 模型

端點和端點之間的資料傳遞基本上會涉及到資料傳遞媒介、網卡、資料該如何發送、資料如何與應用程式有著緊密的關係等等複雜性的問題，而往往要實現資料傳遞都針對這些因素來開發，若某些因素的實際開發出了差錯，還有可能會讓代表其他因素的實際開發也跟著出現問題，因此為了讓一切的資料傳遞都能夠很好地維護以及管理，會將資料傳遞過程分割數個階層，每個階層都有各自獨立的功能且不會彼此干擾，因此可以開發者在不同階層進行開發，當某一層出現問題時，只要針對那一層來解決就行了，而這就是OSI 模型，而該模型下的七層階層分別為以下，並且雙方端點都擁有這七種階層：
  - 應用層 (Application Layer)
  - 呈現層 (Presentation Layer)
  - 會話層 (Session Layer)
  - 傳輸層 (Transport Layer)
  - 網路層 (Network Layer)
  - 資料鏈結層 (Data Link Layer)
  - 實體層 (Physical Layer)

一般來說，當一台主機A上的應用程式要傳遞封包給另一台主機B的應用程式時，會使用先從應用層往下傳遞，首先在應用層時會添增Header(以AH做為代表)至原本封包的前面來進行封裝，而這種形式的封包只有另一台主機的應用層能讀懂且能夠知道要怎處理這封包，封裝後再繼續往呈現層傳遞，呈現層也同樣把自己的Header(PH)添加至從上一層拿到封包(包含著AH和封包)，同樣地，呈現層這樣子的封裝只能夠被另一台主機的呈現層能讀懂並知道要做什麼，接著從呈現層傳遞至會話層之後的層級，最後到資料鏈結層前都是做同樣的封裝動作爛傳遞，當到資料鏈結層時，封包並不是單純拿另一個Header來封裝，而是會以Header(包含著目的和來源的MAC位址)和CRC(檢測此層級的硬體封包完整性)來放在封包內容的前後兩端，此時的封包內容會是從軟體封包轉換成硬體封包(硬體能讀懂的封包形式)，當然地另一台主機的資料鏈結層會讀懂此層封裝後的內容並知道如何做，最後再透過實體層，將資料鏈結層的封包轉換成一連串的位元串(0/1組合)並轉成類比信號由媒介發送這位元串給另一台主機的實體層。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633537945/blog/network/networkModel/osiModel_ezrzcl.png)

當另一台實體主機B的實體層讀到時，便會將類比信號轉換成數位信號，還原當時要傳送的位元串並且從實體層往上傳遞至資料鏈結層，由此層將位元串轉換成資料鏈結層能讀懂的硬體封包或者稱硬體訊框(Frame)並傳遞至資料鏈結層，當資料鏈結層讀取到往上傳遞的硬體封包時，就會按照Header和Trailer來進行相關動作，接著做完便直接拔除這兩個區塊，讓封包變成網路層能看懂的資訊並往上層的網路層進行傳遞，當網路層接收時，便會直接以當前封包內容來讀取網路層的封裝形式，然後處理完便直接把網路層的Header移除，使它變成傳輸層能看懂的內容，這個動作會一直持續到應用層處理好自己的Header之後，這個時候的應用層會直接拔除自己那層的Header來將封包轉化原本主機A的應用程式要傳遞的原始資料給主機B的應用程式。

## 各層主要功用

### 應用層
1. 是OSI 7層中的最後一層，英文是Application Layer
2. 該層級定義一個介面，允許應用程式能透過網路上的同一層與另一個應用程式溝通，以將資料收收或者傳送給應用程式，最終展示給使用者


### 呈現層
1. 是OSI 7層中的第六層，英文是Presentation Layer
2. 由於應用程式透過應用層傳遞的資料不一定是網路傳輸接收的標準編碼格式，所以這層會負責：
  - 網路服務(或程式)之間的資料格式轉換
  - 傳送方必須要將要傳送(從同機下應用層傳遞下來的)的資料轉化成網路的標準格式
  - 接收方必須要將接收到的資料轉化成(同機下應用層)應用程式能讀懂的格式
3. 可以透過這層進行資料的加解密

### 會話層
1. 是OSI 7層中的第五層，英文是Session Layer

2. 在電腦科學裡，Session 本身是指兩個端點之間的暫時性資訊交換過程，換言之，會是指端點間傳輸過程下的狀態和內容
> a session is a temporary and interactive information interchange between two or more communicating devices, or between a computer and user 
3. 在這裏的話，Session 會是一種封裝底層(會話層以下的層級)的資料結構，存放至各個端點內並允許其內部的應用程式在只考慮高層級的情況下，只需要透過對該結構內的寫入和讀取就能與其他端點下的應用程式進行訊息交換
> The name of this layer tells you much about what it is designed to do: to allow devices to establish and manage sessions. In general terms, a session is a persistent logical linking of two software application processes, to allow them to exchange data over a prolonged period of time. 
4. 在這層級，主要負責：
  - 定義Session的具體概念：Session 是什麼？ Session 種類？ Session如何幫助應用程式與其他端點下的應用程式進行交流？ 
  - 定義如何讓應用程式使用這概念：具體會定義作業系統如何提供一種API來給予應用程式調用並從中建立Session
  - 管理/維護端點內的所有Session

5. 例子：在這裡會有三個主機分別為Host A、Host B、Host C，每台主機的作業系統會提供session的API給應用程式，其中Host A有兩個應用程式分別為A1、A2會想需要和其他端點下的應用程式做訊息交換，而Host B有和Host C各分別有一個擁有相同需求的應用程式，分別名為B1、C1。
  - 建立Session：Host A、Host B、Host C 透過作業系統的API來建立對應的Session，每一個Session都綁定特定訊息來方便識別，如IP、Port等資訊，且每個Session的形式上都有可能不同，如用檔案表示Session
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647823542/blog/network/OSI/build-session-example_pomq4u.png)
  - 與其他端點下的Session進行連接：Host A的A1會想與Host B的B1進行訊息交換，所以這兩個應用程式對應的Session會相互連接，而Host A的A2會想與Host C的C1進行訊息交換，所以這兩個應用程式對應的Session會相互連接。
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647823519/blog/network/OSI/session-example_oz9iev.png)
  - 當對Session進行寫入/寫入：當Host A的A1要傳遞訊息至Host B的B1時，就會在Host A內尋找對應的Session並從中進行內容上的寫入並通知作業系統傳遞訊息，隨後它收到就便把訊息傳遞至與該Session連接的Host B之B1對應的Session，Host B對應的Session收到對應內容時，作業系統就把內容轉換至對應的應用程式B1，來讓B1讀取其內容。
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647824291/blog/network/OSI/send-receive-data-inside-session-example_mptjex.png)

6. 參考資料：
 - [What exactly is Socket](https://stackoverflow.com/questions/16233193/what-exactly-is-socket)
 - [A TCP/IP Refeerence You Can Understand](http://www.tcpipguide.com/free/t_SessionLayerLayer5.htm)
 - [tcp-port-and-osi-model](https://stackoverflow.com/questions/52775730/tcp-port-and-osi-model)


### 傳輸層
1. 是OSI 7層中的第四層，英文是Transport Layer
2. 定義發送端和接收端這兩者所使用的傳輸連接技術(使用UDP？還是TCP？封包格式、資料封包如何傳送以及傳送順序、傳輸過程所需要做的檢測、(丟失封包)錯誤復原、重新傳送)以確保各個資料封包可以正確無誤到達目的端


### 網路層
1. 是OSI 7層中的第三層，英文是Network Layer
2. 定義來源地和目的地這兩者間的位址形式(IP位址)、這層級的連接建立、終止、維持、來源處至目的地之間的傳輸路徑為何


### 資料鏈結層
1. 是OSI 7層中的第二層，英文是Data Link Layer
2. 定義如何將偏向軟硬體這兩者間進行互轉，也負責定義此層級的訊息交換、流量控制、失誤問題
  - 定應 LLC (Logical Link Control)如何將偏向軟體的資料形式轉化Frame(約莫1500 bytes)的形式，最後透過第一層將資料轉化成位元串來傳遞
  - 定義 MAC (Media Access Control)如何將偏向硬體的資料形式轉化成偏向軟體的資料形式
  - 兩者間需要存取傳遞媒介(網路線有關的)硬體資源


### 實體層
1. 是OSI 7層中的第一層，英文是Physical Layer
2. 定義所使用媒介設備上的電壓和訊號並傳送/接收對應信號：
  - 傳遞方：將數位信號轉換為電壓這類比信號來傳遞信號
  - 接收方：將電壓這類比信號轉換為數位信號
  - 會定義多少電壓會是代表數位信號1和0
3. 負責將Frame轉換成位元串以及位元串轉換至Frame的編碼形式




## TCP/IP Protocol
1. 除了OSI模型以外，另一種描述資料傳遞的網路模型
2. 由於OSI模型本身的部分網路階層在應用程式實作上沒完全使用或者發揮出來，但仍必須依層級來進行多餘的封裝和解封裝，因此有人提出將部分階層合併一個階層來使用，來解決這樣多餘的過程，而這個合併後的結果就是TCP/IP Protocol。
3. TCP/IP Protocol因OSI的前面三層之間相關性而合併成應用層來處理，而實體層則是和資料鏈結層合併成鏈結層，整體來說TCP/IP只有四層，而封裝和解封裝的流程仍如同OSI 模型那樣處理。
4. 因為合併後的網路階層比較能夠滿足一般應用程式的開發需求以及能夠減少不必要的解封裝而受到歡迎，而OSI模型本身會被當作暸解網路架構的教材。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633511952/blog/network/networkModel/osi2tcp_jn4i6a.png)





