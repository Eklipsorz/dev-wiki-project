---
sidebar_position: 2
---

# 網路模型(待紀錄)


## OSI 模型
將端點之間的傳送接收過程分數個階層，每個階層都有各自獨立的功能且不會彼此干擾，因此可以開發者在不同階層進行開發，當某一層出現問題時，只要針對那一層來解決就行了，而這就是OSI 模型，而該模型下的七層階層分別為以下，並且雙方端點都擁有這七種階層，
  - 應用層（Application Layer)
  - 呈現層 (Presentation Layer)
  - 會話層 (Session Layer)
  - 傳輸層 (Transport Layer)
  - 網路層 (Network Layer)
  - 資料鏈結層 (Data Link Layer)
  - 實體層 (Physical Layer)


![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633455135/blog/network/macaddrNetwork/osimodel_s84kdv.png)
### 應用層
1. 是OSI 7層中的最後一層，英文是Application Layer
2. 定義應用程式如何進入此層的溝通介面，以將資料收或者傳送給應用程式，最終展示給使用者

### 呈現層
1. 是OSI 7層中的第六層，英文是Presentation Layer
2. 由於應用程式透過應用層傳遞的資料不一定是網路傳輸接收的標準編碼格式，所以：
  - 傳送方必須要將要傳送的資料轉化成網路的標準格式
  - 接收方必須要將接收到的資料轉化成應用程式能讀懂的格式
3. 可以透過這層進行資料的加解密

### 會話層
1. 是OSI 7層中的第五層，英文是Session Layer
2. 定義兩個位址(兩個端點位址)的連線通道(port)之連接管理(建立連線、中斷連線等)
3. 用來確定網路服務(應用層所提供的服務)建立連線的確認

https://stackoverflow.com/questions/52775730/tcp-port-and-osi-model


### 傳輸層
1. 是OSI 7層中的第四層，英文是Transport Layer
2. 定義發送端和接收端這兩者所使用的連接技術(使用UDP？還是TCP？封包格式、資料封包如何傳送以及傳送順序、傳輸過程所需要做的檢測、(丟失封包)錯誤復原、重新傳送)以確保各個資料封包可以正確無誤到達目的端




### 網路層
1. 是OSI 7層中的第三層，英文是Network Layer
2. 定義來源地和目的地這兩者間的位址形式(IP位址)、這層級的連接建立、終止、維持、來源處至目的地之間的傳輸路徑為何


### 資料鏈階層
1. 是OSI 7層中的第二層，英文是Data Link Layer
2. 定義如何將偏向軟硬體這兩者間進行互轉：
  - 定應 LLC (Logical Link Control)如何將偏向軟體的資料形式轉化Data Frame的形式，最後透過第一層將資料轉化成位元串來傳遞
  - 定義 MAC (Media Access Control)如何將偏向硬體的資料形式轉化成偏向軟體的資料形式
  - 兩者間需要存取傳遞媒介(網路線有關的)硬體資源
  - 負責定義此層級的訊息交換、流量控制、失誤問題


### 實體層
1. 是OSI 7層中的第一層，英文是Physical Layer
2. 定義所使用媒介設備上的電壓和訊號並傳送/接收對應信號：
  - 傳遞方：將數位信號轉換為電壓這類比信號來傳遞信號
  - 接收方：將電壓這類比信號轉換為數位信號
  - 會定義多少電壓會是代表數位信號1和0
3. 負責將Data Frame轉換成位元串的編碼形式










