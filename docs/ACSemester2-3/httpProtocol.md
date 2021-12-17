---
sidebar_position: 34
---

# http 協定特點


## HTTP 特點
1. 支持客戶/伺服器模式 
2. 簡單快速
3. 靈活
4. 無連接
5. 無狀態


## 連接(Connection)
1. 原文為如下，中文意思為連線在TCP中是種資訊，它融合socket資訊、序列號(sequence number)、窗口大小(window sizes)。
>The reliability and flow control mechanisms described above require that TCPs initialize and maintain certain status information for each data stream. The combination of this information, including sockets, sequence numbers, and window sizes, is called a connection.
2. 起源於
 - [what is connection](https://datatracker.ietf.org/doc/html/rfc793)
 - [为什么 TCP 建立连接需要三次握手](https://draveness.me/whys-the-design-tcp-three-way-handshake/)


 ## 網路插座(Socket)
 1. 原文是指某種裝置的插槽，作用是用來與其他裝置相互連接並傳遞/接收訊號
 2. 在電腦科學裡，會是指電腦網路中的網路節點所具有的軟體結構，其結構會如同字面上的意思而構成插槽，並允許其他裝置透過插槽連接來相互傳遞/接收訊息，而每台電腦都代表著網路節點，換言之，這些電腦上具有由軟體建立出來的插槽，插槽之間會透過有線/無線來相互連接，而虛構出來的插槽會負責從中接收和傳送資料至特定節點上的網路插槽。
 3. 由於插槽實際上實現是主機之間的網路連接，而主機本身也允許提供額外的應用層的服務，只是若放在同一個插槽勢必多耗成本來檢查每一個接收進來的封包，為此每個服務都會有各自的網路插槽來與其他裝置來連接，以此辨明封包上的種類。
 4. 由於前者，每個虛構出來的網路插槽都具有port和ip來綁定以此辨明插槽是屬於哪個主機以及主機上哪個服務上。
