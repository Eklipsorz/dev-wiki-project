---
sidebar_position: 10
---

# REST 風格

## REST
1. 全名為REpresentational State Transfer，較完整的說法是Resource Representation State Transfer，
是一種 **以資源為中心，用HTTP方法操作資源，並且符合HATEOAS(Hypermedia as the engine of application state)的網頁軟體開發風格之ㄧ**
  - 背景：當時背景下的軟體開發和網路發展皆分別以單機環境下進行以及制定/實現某些系統之間的通信協定，兩者很少有交集
  - 目的：如何讓不同軟體方便網路互相傳遞資訊來達到其功能性
  - 應用於：REST 的狀態轉換是基於HTTP/HTTPS協議。
  - 最終目標：當存取到特定資源X時，回應就會包含動態產生出來的一組與特定資源X相關連的API連結，而非以寫死的方式來表達能對特定資源做哪些進一步的操作
  - 備註：此為風格，並非需要嚴格遵守的標準。
2. 在這裡選擇完整說法來解釋名詞：
  - Resource：會是指網路上的實體、資源，實體/資源可以是指某組資料、服務、圖片、歌曲，但實際上該字本質上仍停留在用文字概念去描述該實體、資源的存在，在網路上難以用具體的形式去呈現資源(如看、摸、聽)，在這裡會是以 **以URI** 這文字概念來 **描述其實體資源**。
  - Representation：是指以某種形式來呈現某人事物，**在這裏會以某種形式來呈現Resource這概念**，換言之就是以某種形式來具體化資源，比如用HTML格式、JSON格式等等

  - State 由於客戶端和伺服器之間的互動勢必會涉及到不同時間點的資源內容，或許是在某段時間的同份資源A內容會變更，或許是某段時間點的同份資源A內容會移除，這些將會是資源A在不同時間點的內容或者說狀態，最後在這裡會以狀態來描述同份資源在不同時間點下的資源內容。
  > 狀態是指某物件在特定時間所會有的情況、資訊。 The particular condition that something is in at a specific time
  且由於**該概念是建立在無狀態的HTTP協議上，所以所有的資源都會永久性保留在伺服器，客戶端只會拿到某一個時間點的資源然後就被移除。**
  
  - Transfer 是指某事物在某方面從A轉換至B，而經由前面三個字的形容會是指這些具體化資源之間的狀態切換，每當客戶端向伺服器端發出資源相關請求時，勢必會轉換伺服器的資源目前狀態(如URI->明確清單JSON)，來讓伺服器傳遞資訊給客戶端：從狀態A轉換至轉換B，在這裡為了統一將請求轉換以資源為中心的請求，客戶端所發出的請求得必須透過現有且較明確的HTTP方法來發出，比如GET、POST、PUT、DELETE等HTTP方，而這種轉換會是在Resource Representation的基礎上進行轉換。 換言之，客戶端會發出HTTP動詞＋資源名稱 請求來讓目前在伺服器的資源進行狀態切換，以達到目的。
  - 簡化後的REpresentational State Transfer 的 REpresentational 是指著Resource Representation 這件事

3. 整體概念是描述著如何對在網路上的任意具體化資源進行轉移或者獲取，在這裡會有兩大課題：
 - 定義每個具體化資源在網路上的位置
 - 定義如何對網路上的具體化資源進行切換、獲取

4. 通常應用於HTTP/HTTPS協定的網頁服務，那麼在這裡的 Resource Representation State 或者 Representational State 所指的具體化資源會是以PDF、HTML、CSS、JS、圖片檔、影片檔等能在網頁服務呈現的具體內容。

5. 關於第三點所提到的兩大課題在網頁服務上所要做的實現：
 - 定義每個具體化資源在網路上的位置：透過URI來標明具體化資源在網路上所在的位址，而URI會包含著URL和URN(以序號來區分每個具體化資源)、換言之，URI只會是在網路上的每一個具體化資源本身。
 - 如何對網路上的具體化資源進行切換、獲取：定義明確的HTTP/HTTPS協定標準方法來做狀態切換(如何獲取該狀態或者該具體化資源)，而標準方法會是GET、POST、PATCH、DELETE等方法。
 - 在這樣環境下，網頁開發下的客戶端和伺服器端之間的URL只會是具體化資源本身，不會額外夾雜不能夠構成資源的資訊，而對於狀態切換的標準方法會是以HTTP封包的形式來進行實現。

6. 若符合REST對於網頁服務的實作風格之軟體架構可以用RESTful來形容。


### 參考資料
1. [How I Explained REST to My Wife](http://www.looah.com/source/view/2284)
2. [怎样用通俗的语言解释REST，以及RESTful？](https://www.zhihu.com/question/28557115)
3. [什麼是REST跟RESTful?](https://ihower.tw/blog/archives/1542)
4. [Representational State Transfer (REST)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
5. [理解RESTful架构(原作者阮一峰）](https://www.ruanyifeng.com/blog/2011/09/restful.html)



## REST 網頁開發風格
0. 由REST所定義出的風格
1. 若使用REST風格來開發API給予客戶端使用，其API的root位址可以是兩種中的其中之一，第一種的hostname是提供API的主機域名或者IP，api只是單純指這URL是指向API資源的關鍵字，version則是指API的版本，可以是v1、v2等等來代表版本一、版本二之類的，而\*代表著後續任意字元，通常會是對應某個API服務，第二種root位址中的hostname跟第一種的hostname一樣，只是差別就在於在原hostname前面添加api.來代替後續的api/，而version和\*則是和第一種的version和\*一樣。

```
hostname/api/version/*
api.hostname/version/*
```

舉例來說：第一、二種的可以是：
```
example.org/api/v1/*
api.example.org/v1/*
```
2. 若使用REST風格來開發API給予客戶端使用，其API的版本可以在URI上呈現或者呈現在HTTP傳遞的封包
3. URI只會是名詞，不會有任何一種能讓人類辨別出是動詞、形容詞等其他詞性的詞，且推薦使用複數，錯誤範例為
```
GET /getProducts
GET /listOrders
GET /retrieveClientByOrder?orderId=1
```

而正確範例為：
```
GET /products
GET /orders
GET /orders/1
```
4. 保證HEAD方法和GET方法是安全的請求，不會對資源狀態有所改變(污染/違背GET、HEAD的本質)，比如説：以下請求方式會表示說以GET形式來對產品id為1的產品執行DELETE的動作
```
GET /deleteProduct?id=1
```
5. 資源的地址若很複雜可以用巢狀結構(Nested Structure)，比如要指向朋友ID為10375923的個人資料，就從大到小去切分這位址，朋友清單(friends)->朋友ID(10375923)->個人資料（profile)
```
GET /friends/10375923/profile
```
6. 小心每個回傳結果的大小，如果結果太大，及時利用HTTP協議上的分頁技術(Pagination)或者做些限制，而分頁技術則是將所有項目數按照特定數字來分頁，並且將指定頁數頁面連結放在Header來告知客戶端，另外也可以使用一些參數來從龐大資料獲取只想要的部分：從訂單中的第五十一筆開始擷取資料，並取出25筆，這樣參數的使用會將orders轉化指向代表特定25筆訂單的資源。
```
/orders?limit=25&offset=50
```
7. 使用正確的HTTP Status Code來回傳傳送和接收的狀態。
8. 在回傳結果時使用簡單好讀的內容(包含錯誤詳細訊息、註釋)，可以使用JSON檔案來進行回傳
9. 若使用REST架構開發API給予客戶端使用，其提供API的一方使用HTTPS來保證傳遞接收之間的安全性


### 參考資料
[RESTful web API 設計](https://docs.microsoft.com/zh-tw/azure/architecture/best-practices/api-design)


## REST 等級模型
1. 目的在於：以REST約束來評估一個Web服務設計的好壞並做個分級
2. 等級模型起草於Leonard Richardson在2008年 QCon Talk中，並於2010年由Martin Fowler進一步詮釋模型，並使該模型更加完整
3. 等級大致上分為：
  - Level 0：將整個Web服務規劃成一個URI，並只用一個HTTP動詞來進行客戶端和伺服器之間的互動方式，通常HTTP動詞會是POST
  - Level 1：將整個Web服務依據資源來分成多個URI，但只用一個HTTP動詞來進行客戶端和伺服器之間的互動方式
  - Level 2：將整個Web服務依據資源來分成多個URI，並分成多個HTTP動詞來進行客戶端和伺服器之間的互動方式
  - Level 3：基於Level2的概念下使用HATEOAS概念。
  ![](https://devopedia.org/images/article/252/1821.1579540894.jpg)
參考資料：
[你的REST不是REST？](https://www.ithome.com.tw/voice/128528)
[Richardson Maturity Model](https://devopedia.org/richardson-maturity-model)
## HATEOAS
1. 全名為Hypermedia As The Engine Of Application State，主要是用來區分REST 架構和非REST 架構的條件
> Hypermedia as the Engine of Application State (HATEOAS) is a constraint of the REST application architecture that distinguishes it from other network application architectures.

2. HATEOAS如同其名，**以超媒體(Hypermedia)來表示整個網頁應用程式來表示目前的狀態以及可用的狀態是什麼**，只要客戶端一向應用伺服器向某個資源X發出變更請求時，伺服器就會回傳一組超媒體以及對應資料，其超媒體(一組連結)會是根據使用者對於其資源相關聯的資源是否有權限而生成對應資源的API連結(超連結)或者對應資源的連結，兩者皆含有目前狀態所在的URI位址
> With HATEOAS, a client interacts with a network application whose application servers provide information dynamically through hypermedia. A REST client needs little to no prior knowledge about how to interact with an application or server beyond a generic understanding of hypermedia.
簡單來說，當使用者透過初始API URI就能獲取接下來能夠做的操作以及資源的對應連結，接著使用者想要對其中一個資源操作下達對應的請求，就能讓伺服器再次回傳其資訊以及接下來還能夠做啥操作和資源的對應連結。
  這可以帶來：
  - 在前後分離的架構，前端可以不必事先知道伺服器的處理邏輯以及對應的API是什麼而寫死API URI，因為每個請求回應都會包含著接下來能夠執行的操作以及資源是什麼，而且當伺服器更動API時，前端也不需要跟著改動，只需要等待伺服器回傳對應較新的API URI
  - 伺服器能在客戶端不中斷的情況下根據與客戶端的API互動而進行對應能操作的資源之URI

3. 案例：伺服器和客戶端都支援著HATEOAS，那麼只要客戶端向伺服器發出查詢12345這帳戶的資料

```
GET /accounts/12345 HTTP/1.1
Host: bank.example.com
```
就會呈現該帳號的資料以及一組連結、包含目前所在的狀態資訊-self資訊，連結會是伺服器根據客戶端對於其帳號相關聯的資源是否有權限而動態生成對應的連結，self資訊是用來告知彼此目前狀態或者目前處理的請求是什麼，在這是允許12345帳號是擁有存款(deposits)、提款(withdrawals)、轉帳(transfers)、關閉請求(close-requests)的請求，所以會生成四個連結，而self則是指的是/accounts/12345

```
HTTP/1.1 200 OK

{
    "account": {
        "account_number": 12345,
        "balance": {
            "currency": "usd",
            "value": 100.00
        },
        "self": {
            "href": "/accounts/12345 HTTP/1.1"
        }
        "links": {
            "deposits": "/accounts/12345/deposits",
            "withdrawals": "/accounts/12345/withdrawals",
            "transfers": "/accounts/12345/transfers",
            "close-requests": "/accounts/12345/close-requests"
        }
    }
}
```

假使12345帳戶沒錢的話的話，伺服器給定的連結會只剩下存款和關閉請求
```
       "links": {
            "deposits": "/accounts/12345/deposits",
            "close-requests": "/accounts/12345/close-requests"
        }
```

4. 參考資料
[架构之:REST和HATEOAS](https://zhuanlan.zhihu.com/p/393221998)
[淺談 REST API 的設計和規劃](https://marco79423.net/articles/淺談-rest-api-的設計和規劃)