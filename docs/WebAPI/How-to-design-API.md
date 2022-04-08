---
sidebar_position: 3
---

# 紀錄：如何設計API


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
