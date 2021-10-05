---
sidebar_position: 1
---


# HTTP：客戶端如何對伺服器發送請求

當客戶端在自身的瀏覽器上輸入伺服器的對應路由時，會透過DNS對應IP來將對應伺服器URL轉換成IP，接著再透過IP來找到伺服器，接著發送要求至伺服器來要求對應路由的網頁，接著伺服器的controller部分會接收客戶端的請求並將對應資料從資料庫撈出來對應至伺服器上的view區塊中先將要給予使用者的網頁進行（資料上的對應和渲染），最後再把網頁以封包的形式傳遞至客戶端，隨後客戶端的瀏覽器再把網頁進行解析以及渲染，

當客戶端需要從伺服器獲取資料時，便會利用對應路由從伺服器端獲取資料並讓其回傳資料並於自身瀏覽器進行渲染或著由伺服器回傳帶有對應資料的網頁（也就是由伺服器利用客戶想要的資料進行網頁的改造並回傳之）



基本問題：
1. Protocol、packet、router、IP、TCP、HTTP、Client & Server端是什麼
2. 它們能做些什麼？
3. 你能夠用他們做什麼？
4. 資料為什麼可以成功傳輸到目的地？(以現有平台提供的線索來回答)
5. 設備之間究竟為什麼能互相溝通


Protocol：
1. 一種能表達參與方皆能有共識(同意相同的事情)的形式
2. 以第一點來衍生成一種能讓電腦彼此連線並傳遞封包的電腦語言，或者說一種能讓電腦讀懂並讓他們能夠有彼此連接傳遞封包這共識的表達形式。

一本把電腦網路講得很基本且扎實的聖經書，使用方式就按照你所不懂的部分來看就好，當作字典使用(x)，除非你想考中華電信公職或者真的很想了解網路在做什麼
https://github.com/QSCTech/zju-icicles/blob/master/计算机网络基础/教材/Computer%20Networks%20(Fifth%20Edition).pdf

TCP分為四層，應用層、傳輸層、網路層、網路介面層

網路介面層->網路層->傳輸層->應用層

參考資料：
[TCP/IP四层模型（协议簇）各层作用](https://segmentfault.com/a/1190000022946409)


網路介面層(Network interface layer)：原分別對應OSI 7層中的實體層(Physical Layer) 和 鏈接層 (Link Layer)，實體層是負責定義實體連接(包含網卡、網路線、網路信號、資料如何在這些媒介上面進行單純的傳輸，也就是機器A放資料至實體媒介中，而機器B就直接從該實體媒介中撈到該資料)的規範，

## 實體層 
1. 是OSI 7層中的第一層，英文是Physical Layer
2. 負責定義實體主機之間要如何透過媒介來傳遞資料，定義當中會包含著網卡的定義和規範、實體主機能用上的媒介是什麼(信號、網路線)、實體主機能如何透過媒介進行單純的資料傳輸，比如也就是機器A放資料至實體媒介中，而機器B就直接從該實體媒介中撈到該資料，傳輸過程沒特別的處理，就只是單純的傳輸。
3. 但留下了一個問題，這個問題是網路的傳遞速度會使得資料傳送和資料接收之間存在著延遲，進而使資料出現傳送或者接收的不完整的問題

## 資料鏈結層
1. 是OSI 7層中的第二層，英文是Data Link Layer
2. 此層主要著手於資料傳輸上的完整性，基本上會提供因傳遞速度產生出來的資料不完整問題處理、控管資料傳送和接收的順序，使用慢速接收者不至於會被快速傳送者的封包給淹沒/佔據著。
3. 提供一個介面來讓來實現網路層的真實資料傳輸，換言之，網路會由這一層來實現資料傳遞。





網路層(Network Layer)：
1. 由於前兩層只專注於實體媒介中的傳輸(就一條線和兩台機器的傳輸，或者就專注mac address上的傳輸)以及其傳輸完整性，傳遞資料必須要讓主機們擁有判定資料是從那個主機發送以及發送至哪個主機才能辦法實現，而前兩層頂多就固定不可更動的mac address來進行資料傳輸，建構出來的網路會受限於不可更動的缺點，
2. 網路層為了進一步控管主機在傳遞網路上的位址而額外提供虛擬網路位址給每台主機當作網路上的地址來控管網路上面的主機、利用虛擬網路位址來將部分主機分為好幾個群體，有些群體上的所有電腦可以構成與世隔絕的網路，有些則是與世界連接的群體，並在這樣的基礎下，此層也被定義在虛擬位址的基礎下提供特定路徑來讓資料從端點A傳送至端點B
3. 整體而言，網路層負責定義虛擬位址的建立、維護、管理以及端點之間在虛擬位址的基礎下的特定路徑決定

端點(End node)是指連接至網路(由許多電腦和路由器構成的大型網路)的一台電腦，由於該電腦座落於整體網路的邊緣地帶，因此被稱呼為end node


參考資料：
[为什么用ip不用mac寻址](https://blog.csdn.net/a13602955218/article/details/108311560)
[what is end node](https://en.wikipedia.org/wiki/End_system)

IP (Internet Protocol) 
1. 主要是網路層上的協定，是負責定義虛擬位址給主機以及封裝一些定址資訊在封包
2. 虛擬位址被稱之為IP Address，其位址會被簡寫為IP


傳輸層(Transport Layer)：
1.  網路層只負責定址、在定址的基礎下決定路徑，但資料上的可靠性(傳送方和接收方都能正常接收和傳送資訊給彼此)、封包雍塞處理、封包傳遞效能仍出現問題
2.  由傳輸層負責解決資料傳輸上的可靠性、封包傳遞效能等等



應用層(Application Layer)
1. 為了進一步運用下面幾層(傳輸層、網路層、網路介面層)的功能而額外提供一層來給予特定應用程式發送只屬於該應用程式的封包內容，這個應用層會預設幾個常見特定服務來定義，比如HTTP，當應用程式應用事先定義好的HTTP，就能讓應用程式發送HTTP形式的封包來進行傳遞，而HTTP的封包也會間接運用到傳輸層、網路層、網路介面層的功能。