---
sidebar_position: 7
---

# Three-way handshake

## 前置知識
### Socket 是什麼？
1. 原文是指某種裝置的插槽，作用是提供介面來給予其他裝置插入該插槽獲取對應功能
> an opening or hollow that forms a holder for something
2. 在電腦科學裡，是指：
  - 由軟體形式所構成的插槽，該插槽會以目前插槽所在的網域和特定埠號等資訊來識別，其對應功能為與網路連接的功能，該插槽可由應用程式透過插入該插槽這類比概念來將應用程式與插槽綁定，使應用程式擁有能夠與網路連接的能力
  - Process 透過網路來進行通訊的方式之一

3. 其概念定義源自於OSI七層中的會話層(Session Layer)，主要目的透過建立一個抽象資料結構(該結構即為Session)來讓應用程式的開發只需要顧慮到會話層透來快速與另一個端點下的應用程式建立連接，而不需要顧慮到底層(傳輸層、網路層、資料鏈結層、實體層)，該資料結構會封裝著底層資訊和對應技術。

4. 在這裏使用了Socket這資料結構來當作Session的形式之一，主要定義以下課題：
  - 如何建立Socket：由作業系統來提供Socket相關的API給予應用程式建立和管理，一旦應用程式呼叫對應API來建立Socket時，其Socket形式會是以檔案形式來表示，並以應用程式目前所在的主機網域和使用埠號作為識別
  - 如何透過Socket傳遞/接收訊息：只要讓應用程式以檔案形式來對該Socket進行寫入/讀取檔案操作就能讓作業系統幫忙封裝底層網路資訊進行端點下的應用程式之間的訊息傳遞或者接收訊息
  > A socket is an abstraction through which an application may send and receive data,in much the same way as an open file allows an application to read and write data to stable storage. A socket allows an application to "plug in" to the network and communicate with other applications that are also plugged in to the same network. Information written to the socket by an application on one machine can be read by an application on a different machine, and vice versa.
5. 例子：在這裡會有兩個主機分別為Host A、Host B，每個主機分別有Application A1和Application B1這兩個應用程式在運作，這兩個都想與網路連接並與彼此傳遞/接收訊息，在這裡主要會以下步驟：
  - 每台主機透過作業系統所提供Socket的API來建立對應的Socket來讓該應用程式與網路連接，每個Socket都用Socket所在的IP和特定Port當作識別用的資訊
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647832161/blog/network/OSI/build-socket-example_ftpefh.png)
  - 接著使用A1使用API來指定與Host B的B1進行連接，並且當連接建立完畢後，接著透過API來將傳遞內容寫入至Socket A1這檔案，Host A上的作業系統會利用封裝後的底層資訊來將檔案內容傳遞至Host B的Socket B1上，並通知該Host B的作業系統去讀取Socket B1，接著Host B的作業系統收到通知就會讀取Socket B1並將內容轉換給Application B1上。
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647831617/blog/network/OSI/send-receive-data-inside-socket-example_ypmu7m.png)
6. 參考資料：
- [What exactly is Socket](https://stackoverflow.com/questions/16233193/what-exactly-is-socket)
- [A TCP/IP Refeerence You Can Understand](http://www.tcpipguide.com/free/t_SessionLayerLayer5.htm)
- [socket编程到底是什么？](https://www.zhihu.com/question/29637351)

### Sequence number 是什麼？
1. 出自於TCP/IP 傳輸層的Sequence number，主要用途為藉由賦予連續號碼至每個所傳遞的封包來對封包進行排序、去除重複性(eliminate duplicates)或者讓每個封包都有各自的序號，來與ACK一同方便驗證封包傳遞的可靠性、驗證資料是否因封包丟失而不完整
2. 細節：
  - Sequence number並不是TCP/IP的識別號碼
  - Sequence number並不代表會如同資料庫的主鍵號碼那樣：每個封包要擁有不同號碼，被使用過的號碼就不能夠重複使用
3. 參考資料：
 - [TCP 序列號 (Sequence Number, SEQ)](https://notfalse.net/26/tcp-seq)
### Acknowledgment number 是什麼？
1. 出自於TCP/IP 傳輸層的Acknowledgment number，主要用途為藉由目前連線的序號來方便讓對方驗證該序號是否為過去錯誤請求的序號，比如因網路速率問題而導致的重複性發送初次連接建立申請。

### ACK? SYN? RST?
1. 出自於TCP/IP 傳輸層 Header的Control bit，該Control bit/TCP Flag本身在Header都用來告知接收方和傳送方該封包內容是屬於哪一種類型或者表明其封包的用途為何
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1631374337/blog/TCP_IP/tcpflag_wumylp.png)

2. 在這裏的ACK、SYN、RST主要用途分別為如下：
  - SYN (Synchronize sequence numbers)：若為1的話，則告知對方 "傳送方想透過序號同步的方式來申請與對方建立初次連接(我方對於對方的初次連接建立請求)，並於封包內的Sequence Number欄位寫入序號以及寫入暫定的相關連接資訊"
  - ACK (Acknowledgment field significant)：若為1的話，則告知對方 "接收方已經收到先前傳送方發送過來的封包並表示回應，接著在封包內的Acknowledgment Number欄位寫入前一個封包的Sequence Number欄位值+1"
  - RST (Reset the connection): 若為1的話，則告知對方 "目前我方與你的連線狀態有問題，建議對方刪除我方與你的連線，並重新建立連線"，發生在三向交握的話，會是指接收方所傳遞的ACK欄位值並不是目前傳遞方所擁有的SEQ值+1所發生過期問題，這時傳送方會告知對方目前建立的連線是有問題，請接收方刪除目前連線並重新等待是否有其他SYN封包。
  ![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1631371279/blog/TCP_IP/tcp_ipHeader_rwqwcl.png)


### window size 是什麼？做什麼用？

1. 出自於TCP/IP 四層中的傳輸層 Header，主要用途為宣告接收者在特定時間點所能接收到的最大資料大小，以此來控制流量。
2. 當主機A的封包1會從傳輸層封裝至實體層並傳送至另一個主機B時，其封包1若指定TCP/IP Header中的window size為size A：
  - 那麼當主機B收到封包1時，會從window size讀取到size A，進而指定下一次傳遞封包給主機A的最大封包大小為size A
  - 當主機B傳遞封包2至主機A時，就會按照size A來製作符合其大小的封包傳遞至主機A，另外主機B所傳遞的封包2也指定著window size為size B
  - 當主機A從主機B收取封包時，會從Header讀取到size B，這指定下一次傳遞給主機B的最大封包大小會是size B。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1647839246/blog/network/OSI/window-size-diag_ypolsp.png)


3. 參考資料
 - [What is the window size in TCP?](https://www.quora.com/What-is-the-window-size-in-TCP)
 - [TCP series #4: TCP receive window and everything you need to know about it](https://accedian.com/blog/tcp-receive-window-everything-need-know/)
 - [tcp傳輸窗口解析——藉助wireshark深入分析](https://kknews.cc/zh-tw/news/8ge5n9q.html)


### 什麼是連線？
1. 根據[RFC 793 - Transmission Control Protocol ](https://tools.ietf.org/html/rfc793)以及[为什么 TCP 建立连接需要三次握手](https://draveness.me/whys-the-design-tcp-three-way-handshake/)所提出的連線定義為，連線會是由序號、視窗大小、一對sockets所構成。

> The reliability and flow control mechanisms described above require that TCPs initialize and maintain certain status information for each data stream. The combination of this information, including sockets, sequence numbers, and window sizes, is called a connection.

2. 連線目的為何？ 主要確保正式傳輸前後的時期是分別能夠保證以及驗證資料傳輸的可靠性 以及 實作傳輸過程的流量控制
  - 正式傳輸前的可靠特性： 傳輸前的連線目的是讓雙方的資料傳遞都是可以成功傳遞訊息至對方/能成功接收到對訊息，接著進而產生對應的資料來代表連線
  - 正式傳輸後的可靠特性：傳輸後的目的可透過代表連線的資料來幫助驗證後續的資料傳遞是否為合法*
  - 流量控制：

3. 連線中的資料各代表著：
  - 序號、一對sockets：保證可靠性
  - 視窗大小(Window Size)：流量控制

### http 協定特定-無連接(Connectionless)
1. Connection 是指http協定下的客戶端和伺服器端為了確保資料傳遞能夠成功傳遞至對方/接收對方訊息而建立的資訊和初次傳遞的可靠性驗證，資訊則是用驗證往後傳遞是否可靠，

2. Connectionless的less則是表示著Connection在某方面的程度或者數量較少，甚至到沒有Connection，在這裡的某方面程度則是指連線持續時間方面或者拿來驗證可靠性的資料持續存在時間，所以連線持續時間較短/資料持續存放時間較短或者甚至到沒有，都可稱作為Connectionless或者無連接。


3. http協定的無連接特性是出自於當時概念背景是： **早期向伺服器發送的請求都因為索取的資料較小而具有同個客戶端不連續發送同個連線的請求，會隔一段時間發送的特性，所以當伺服器面對巨大請求量時，往往請求都會因為前面特性而發散，使得連續的請求們都不是從同一個連線中發送**，為了讓伺服器能夠服務更多請求，所以該協定以連線持續較短的連線為目標來解決，

4. 在第三點提出的背景下，http協定下的無連接實作概念為：每一次連接都只處理一個請求，當伺服器處理完一個客戶請求並回傳結果給客戶端，接著等客戶端回應它已收到回應的訊息給伺服器或者不等，此時伺服器和客戶端就相互刪除代表連線的資訊以此來中斷連線，使連線成為連線持續較短的連線

5. 現如今的背景-**會因為連續數量請求的頻繁出現而使http協定下的無連接實作概念變成累贅**：隨著時間的推移，每一個請求不再只是索求大小較小的資源，而是會因為所要的資源太大，而必須將請求或者回應分割好幾份小請求或小回應來發送或者回應，然而每當小回應回傳客戶端時，可能會因為無連線的規則而斷開連線，但由於請求/回應還未完全做完，所以又會為了發送請求或者接收回應而再次讓客戶端和伺服器建立連線，並重新接收，然後回應完又斷開，然後再繼續建立連線的迴圈，讓連線建立的成本逐漸成了累贅，甚至進而使得在巨大的請求量中，連續的請求們會出現較多為同一個連線的情況。

6. 為了解決上述問題而提出的解法：提出了Keep-Alive機制在HTTP的實作上，來讓連線的持續時間延長至伺服器或者客戶端本身認為傳送/接收過程已經結束才停止連線，換言之，由他們(伺服器和客戶端的其中一方)決定何時中斷連線，在Keep-Alive的情況下，每個請求所建立的連線會延長，這可以盡可能緩解原本需要一直重複建立連線的傳送/回應的過程，但由於連線延長，所以容易讓連線本身佔據著系統資源。

7. 其餘細節：
  - 這裡的無連線通常形容著網路協議(Protocol)，也就是無連線協議，而無連線協議只是不會維持連線時間較長的連線，也並非沒能力提供連線時間較長的連線。

### 重複歷史連接初始化是什麼？
當客戶端對伺服器進行資料傳輸，剛開始若沒有對方的連接資訊的話，會向伺服器來申請初次連接資料的請求，然而客戶端發送出去的請求很有可能因為網路速率問題、系統問題而導致客戶端發送出多次重複性的請求，且客戶端無法直接撤回這些處於網路環境下的請求，而這些請求正是重複歷史連接初始化。

### 連接狀態
為了進一步讓主機了解該主機與其他主機下的連接情況，RFC793文件就定義了一系列狀態來表示目前連接處於哪種狀況並允許作業系統根據這些狀態來表達自己與其他連接的狀況，具體狀態有：
  - LISTEN：目前主機處於可接收源自於任意TCP和PORT的請求並進行處理
  - SYN-SENT：目前主機已經發送SYN封包來申請連接，接著正等待著對應的連接請求
  - SYN-RECEIVED：目前主機已經接收到其他主機X對於目前主機的SYN連接請求並發送另一個對應SYN連接請求至其他主機X，接著正等待著其他主機X發送至目前主機的連接確認請求
  - ESTABLISHED：目前主機已確定建立連接，可以正常傳送和接收
  - CLOSED：目前主機沒有任何連接資訊

參考資料：
  - [TCP/IP State Transition Diagram (RFC793)](https://users.cs.northwestern.edu/~agupta/cs340/project2/TCPIP_State_Transition_Diagram.pdf)
  - [RFC 793 - Transmission Control Protocol ](https://tools.ietf.org/html/rfc793)
## 正文
### 三向交握的目的
根據[RFC 793 - Transmission Control Protocol ](https://tools.ietf.org/html/rfc793)所定義的三向交握，其提出的原因為如下：
  - 客戶端在向伺服器發出初次申請連接請求的時候，因為速率或網路問題而發送出多個相同請求
為了避免相同請求的發生，而提出三向交握這特定的連接建立方法來解決

> The principle reason for the three-way handshake is to prevent old duplicate connection initiations from causing confusion.




### 為什麼要叫three-way handshake？
handshake 本身是帶有達成一致的含義，而在這裏handshake就是以三個傳遞資訊的方向來達成一致，方向分別為：
  - 方向1：從client -> server
  - 方向2：從server -> client
  - 方向3：從client -> server
### tcp 三次握手侷限於於特定協定？
TCP 三次握手本身是TCP/IP 傳輸層建立連接的方法，且三次握手本身專注於傳輸上的連線方面，所以不會特定侷限於某個協定來實現。

 - [what is connection](https://datatracker.ietf.org/doc/html/rfc793)
 - [为什么 TCP 建立连接需要三次握手](https://draveness.me/whys-the-design-tcp-three-way-handshake/)
