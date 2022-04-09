---
sidebar_position: 3
---

# 紀錄：如何設計API


## 使用協議
客戶端和伺服器之間的互動使用**HTTPS 協議**，該HTTPS協議是基於：
  - 客戶端和伺服器端的應用層會是使用 **HTTP協議** 來建立內容
  - 進行傳輸時會使用著SSL/TLS 協定來替應用層的資料內容加密、解密、保證資料完整性，該協定在廣義來說是在TCP/IP四層中的應用層，但嚴格來說會是傳輸層和應用層之間的層級
![UNDERSTANDING SSL/TLS: PART 1 - WHAT IS IT?](https://blog.eleven-labs.com/assets/2016-12-21-understanding-ssltls-part-1/tls-in-osi.png)

Note: 
  - SSL (Secure Sockets Layer) ， TLS (Transport Layer Secret)
  - SSL/TLS 協定 本身是用來加密傳輸時的內容和保證資料完整性(如不被篡改)，任何在他之上層級的服務皆能夠使用，比如SSH，本身並不限定於HTTP
  - SSL和TLS之間的關係為：兩者皆指加密傳輸和保證資料完整性的同個協定，只是該協定早期是非標準的形式來提供服務，從1.0至3.0，3.0之後就被IETF組織給標準化，從SSL變更為TLS，隨後又出TLS 1.0、TLS 1.1、TLS 1.2等等版本。
  - TLS 1.0 和 SSL 3.0 版本在功能上差不多。
參考資料：
[UNDERSTANDING SSL/TLS: PART 1 - WHAT IS IT?](https://blog.eleven-labs.com/en/amp/understanding-ssltls-part-1/)

## 域名
1. 盡量將提供API服務的伺服器域名定義為專用域名下
```
https://api.example.com
```
2. 如果確定API服務本身不會出現版本或者重大變化的話，可考慮以主域名為主
```
https://example.org/api/
```

## 版本命名方式
1. 利用域名來區分
```
https://api.example.com/<version>

e.g., 

https://api.example.com/v1
https://api.example.com/v2
```

2. 使用HTTP請求封包-header來標記客戶端想要的版本是什麼


## 端點
1. 端點原本是指某個人事物的最後部分或者想試著抵達的終點，在API設計中會是指URI的最後部分(包含URL本身)，而那部分剛好會是定義資源的部分，因此會將端點與資源劃上等號
> the end of something or something that you are trying to achieve
> a point at which a line segment ends
![](https://oscimg.oschina.net/oscnet/up-92a47264770be1741992adc4e1cddfd71b7.png)
## 端點設計要點
### 短小便於輸入的URI
### 人類可以馬上讀懂的URI
### 沒有大小寫混用的URI
### 修改方便的URI
### 不會暴露伺服器架構的URI
### 規則統一的URI：
規則會是指URI所用的詞彙和URI的結構這兩者的命名規則，若不一致容易造成開發者或者客戶端在使用上的混亂，比如獲取特定朋友資訊的API是設計成如下：
```
GET /friends?id=100
```
而對特定好友發送訊息的API是設計成如下：
```
POST /friend/100/message
```

從這兩個例子中發生兩個不一致的問題：
  - 指定特定對象的命名方式：獲取朋友資訊是使用Query string來指定，發送訊息則是用/friend/100來指定
  - 兩者對應的對象本該是一致的，但擁有單複數的形式：獲取是使用friends，發送則是用friend

解法為：
  - 統一成用/xxxx/id的形式來指定特定對象
  - 將指定對象的命名方式統一成複數
  ```
  // 獲取特定朋友資訊
  GET /friends/100

  // 對特定好友發送訊息
  POST /friends/100/message
  ```

### 需要連接的單字細節：域名和端點

當端點或者域名裡需要連接兩個以上的單字時，會有以下選擇：
  - 脊柱命名(Spinal Case)：以單字為單位，並使用連字號來連接單字們，如下
  ```
  profile-image
  ```
  - 蛇形命名(Snake Case)：以單字為單位，並使用下底線來連接單字們，如下
  ```
  profile_image
  ```
  - 駝峰命名(Camel Case)：以單字為單位，並使用大小寫來區分單字們，如下
  ```
  // 小駝峰
  profileImage
  // 大駝峰
  ProfileImage
  ```
但考慮實際上URI的域名限制：
  - 不允許使用下底線
  - 使用點字號.會被以DNS尋找域名的方式去處理
  - 不區分域名的大小寫，一律全都以小寫或者大寫來看待

所以變得只剩脊柱命名能夠成為唯一選擇，蛇形會因為下底線而無法正常被解析，駝峰則是因為不區分大小寫而失去單字分隔的作用，當然也可以試著將兩個以上的單字拆分成用/來連接，比如
```
profile_image => profile/image
```

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
[RESTful API 设计指南](https://www.ruanyifeng.com/blog/2014/05/restful_api.html)
