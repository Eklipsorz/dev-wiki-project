---
sidebar_position: 2
---

# OSI 以及 TCP/IP


## 實體層 
1. 是OSI 7層中的第一層，英文是Physical Layer
2. 負責定義實體主機之間要如何透過媒介來傳遞資料，定義當中會包含著網卡的定義和規範、實體主機能用上的媒介是什麼(信號、網路線)、實體主機間能如何透過媒介進行單純的資料傳輸(1對1的傳輸)、資料用什麼表示或者如何表示資料(由0/1所組成的數位信號來表示，而這也是封包的原型)，比如有兩個主機分別為主機1和主機2，兩者實體主機之間夾雜著媒介(可以是信號、實體)，主機1可以透過媒介傳遞資料(淺藍色圓球代表資料，且由多個0/1所組成的信號)至主機2，而主機2再透過媒介接收由主機1傳遞的資料：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633431377/blog/network/macaddrNetwork/exampleOfPhysicalLayer_ll1whw.png)

3. 但留下了一個問題，這個問題是網路的傳遞速度會使得資料傳送和資料接收之間存在著延遲，進而使資料出現傳送或者接收的不完整的問題




## 資料鏈結層
1. 是OSI 7層中的第二層，英文是Data Link Layer
2. 此層主要著手於資料傳輸上的完整性以及進一步擴大資料傳輸的範圍
3. 實現方式為，首先會定義獨特且唯一的實體位址(Media Access Control Address, MAC Address)並賦予給每台電腦上的網卡來進一步擴大未來能傳輸的範圍，再來就是將第一層的封包封裝成好幾等份的資料形式，其資料形式稱為資料訊框(Data Frame)，每一個訊框都擁有三個部分：Header、Frame Data、 Trailer，Header會紀錄著傳遞該封包的主機所擁有的mac adddress以及接收該封包的主機所擁有的mac address，而Frame Date則是儲存原始封包被拆分後的資料，而Trailer則是儲存用來處理資料傳輸完整性的資料和參數。提供一個介面來讓來實現網路層的真實資料傳輸，換言之，網路會由這一層來實現資料傳遞。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633433037/blog/network/macaddrNetwork/exampleDataFrame_xiivfw.png)

4. 如何運用mac address來傳遞資料：透過網卡的mac address來決定資料從哪裡傳到哪裡，在這樣結構下，能建立以下網路，這個網路會由多個實體主機，每一台實體主機都配戴有mac address的網卡，並用各自的媒介與一個轉接器上的port(負責進行連接的地方)進行連接，從而構成一個小型網路，而轉接器會記錄著(如下圖中的右表)所有連接它的主機所擁有的mac address以方便未來的資訊傳遞，所以當網路上的任一主機當發送資料給同一個網路下的另一個主機時，會先將資料傳遞轉接器，由轉接器來找到他所想要傳遞的目的主機，進而將資料轉接給他。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633422190/blog/network/macaddrNetwork/macaddressNetwork1_fzo0li.png)

基於這個基礎，我們可以構建出另一個小網路，然後再彼此依靠著另一台轉接器連接，當網路1的主機想要傳遞資料給網路2下的某台主機時，便直接將資料轉接給兩個網路之間的轉接器，而轉接器由於本身沒辦法透過mac address來區分這哪一個網路而使用廣播(向所有連接者傳遞)將這個資料給其他網路，在這裡只會有網路2的轉接器接收到，當它收到時便會根據目的的mac address來傳達資料給指定主機
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633422197/blog/network/macaddrNetwork/twoMacAddressNetworks_macsec.png)

當隨著越來越多的網路或者群體出現時，連接各個網路的轉接器要廣播的對象就越來越多，
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633422195/blog/network/macaddrNetwork/multipleMacAddressNetworks_uwxlte.png)

若在這個情況下，有個主機想要跨網路傳遞資料給另一個網路下的主機時，就比如網路1的主機1想傳遞信號至其他網路下，那麼當網路1的轉接器收到資料便會轉接給連接其他網路的轉接器，由它來進行廣播，然後再讓真正擁有目的主機所擁有mac adddress之網路來負責最後資料的轉接，在這樣個情況下，每一次的傳送都要對所有網路進行一次轉接，甚至若網路情況比用一台來連接所有網路還要複雜的話，或者要傳遞的目的主機突然不在或者轉移至其他網路時，其效能都會花在廣播和轉接，

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633422195/blog/network/macaddrNetwork/sendASignalOnNetwork_lnxyuf.png)


## 網路層 
1. 是OSI 7層中的第三層，英文是Network Layer
2. 由於前兩層只能提供mac address來建立網路，每一次要進行跨網路的資訊傳遞時，便會受限於獨特且無法變更的mac address，這只能透過廣播至每個小型網路或者群體，來讓擁有目的mac address的轉接器進行資料的傳遞，完成主機之間的資訊傳遞。
3. 網路層為了盡可能避免廣播以及如何決定端點間的路由(電腦之間的資料傳遞路徑)，而改由一種虛擬網路位址來取代mac address作為傳遞資料的來源地和目的地，首先它會賦予每台主機一個虛擬網路位址，每一次傳遞只需要透過其位址來傳達，並且位址可以依照主機所屬的網路以及用途而有所區分，進而負責轉接的機器更了解資料要往哪裡傳，而非完全依賴著廣播來解決，並在這樣的基礎下，此層也被定義在虛擬位址的基礎下提供特定路徑來讓資料從端點A傳送至端點B。
4. 整體而言，網路層負責定義虛擬位址的建立、維護、管理以及端點之間在虛擬位址的基礎下的特定路徑決定。


### 額外知識
1. 端點(End node)是指連接至網路(由許多電腦和路由器構成的大型網路)的一台電腦，由於該電腦座落於整體網路的邊緣地帶，因此被稱呼為end node。
2. 其虛擬網路位址在網路層中是由IP (Internet Protocol) 協定所定義，該協定主要定義虛擬位址給主機以及封裝一些定址資訊在封包，該虛擬網路位址在這個協定下又稱之為IP Address或者IP。

### 參考資料
1. [为什么用ip不用mac寻址](https://blog.csdn.net/a13602955218/article/details/108311560)
2. [what is end node](https://en.wikipedia.org/wiki/End_system)


## 傳輸層
1. 是OSI 7層中的第四層，英文是Transport Layer
2. 網路層只負責定址、在定址的基礎下決定路徑，但資料上的可靠性(傳送方和接收方都能正常接收和傳送資訊給彼此)、封包流量管理、封包傳遞效能。
2. 由傳輸層負責解決資料傳輸上的可靠性、封包流量管理、封包傳遞效能等等


## 會話層
1. 是OSI 7層中的第五層，英文是Session Layer
2. 其Session

## 表現層
1. 是OSI 7層中的第六層，英文是Presentation Layer
2. 負責將封包組裝成應用層能夠讀取的資料形式，也可以運用在內容上加解密。

## 應用層
1. 是OSI 7層中的第七層，英文是Application Layer
2. 為了進一步運用下面幾層(傳輸層、網路層、網路介面層)的功能而額外提供一層來給予特定應用服務產生出只屬於該應用服務的封包內容，這個應用層會預設幾個常見特定服務來定義，比如HTTP，當應用程式應用事先定義好的HTTP，就能讓應用程式發送HTTP形式的封包來進行傳遞，而HTTP的封包也會間接運用到傳輸層、網路層、網路介面層的功能。