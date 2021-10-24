---
sidebar_position: 10
---

# REST 風格

## REST
1. 全名為REpresentational State Transfer，實際上可以用Resource Representation State Transfer，它是一種以資源以及如何對資源進行切換為中心的網路軟體開發風格，並非標準。
2. 名詞解釋：
  - Resource 是指概念，概念又是指一堆對於某件人事物的描述，這些描述會有其特徵、功能、形狀、顏色等等
  - Representation 是指以某種形式來呈現某人事物，在這裏會以某種形式來呈現Resource這概念，換言之就是以某種形式來具體化資源。
  - State 是指狀態，在這裏會被前兩個字給形容來表示狀態是由具體化的資源所構成，換言之，每個狀態都會指著不同的具體化資源，只是用狀態來形容這件事，而用State來描述是為了充分描述這些具體化資源之間的切換、從哪獲取具體化資源
  - Transfer 是指某事物在某方面從A轉移至B，而經由前面三個字的形容會是指這些具體化資源之間的切換(將A、B想成具體化資源)、從哪裡獲取具體化資源(將A想成外部，而B就是具體化資源)
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