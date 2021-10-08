---
sidebar_position: 4
---

# 網站封包
由於網站普遍皆使用應用層的http協定或者https協定來實現，所以客戶端和伺服器端之間的封包傳遞皆使用前兩者的其一個協定來規劃封包形式、封包內容、如何傳遞，在這裏會以http的封包來說明，而https的封包則留到後續再補充。


## HTTP封包形式


當client端想透過HTTP協定去使用server端的網站時，client端跟server端這兩者間會做TCP/IP中的三向交握中建立連結，之後client便會正式發送請求的HTTP封包給server端，而server端收到請求後便會回傳另一個HTTP封包來回應client。

未增加封包後的版本：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1631431354/blog/how2useAPI/client2server_rzjbgc.png)

增加封包後的版本：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1631433951/blog/how2useAPI/detail_client2server_k7juuv.png)


由於應用層的HTTP封包都會直接分為Header和Body，Header會存放告訴系統如何處理應用層或者HTTP協定，而Body則是應用程式-網站所要傳遞/接收的正式內容，client端和server端對於Header和Body的內容皆不一樣，client端封包的Header部分會有Request Method、Request URL、Request Headers，而Body則是Message body，其中Resqest Method是指想要對目標資源做什麼樣請求，Request URL是具體的目標資源，其URL是用來定義資源在網路環境下的的位置格式，使用這個格式可以在網路上找到對應的目標資源，Request Headers則是進一步定義請求的設定、格式、要找的伺服器位址(與URL不同，只列出對應伺服器域名或者IP)，最後一個Message body是該請求封包的具體詳細請求內容。

server端封包的Header部分會有Status code、Response Headers，而Body則是Response Body，其中Status code則是以數字表示回應client請求的結果/狀態，Response Headers則是進一步定義回應"請求"的設定、內容格式(Content-Type，定義回傳資料是什麼格式)，而Response Body則是具體詳細的回應內容。

### Resquest Method
Resqest Method是指Client端想要對目標資源做什麼樣請求，具體請求有：
1. GET: 主要向目標資源請求讀取某些資料。
2. POST: 主要告訴對方我要傳東西給你，請看我在Message body儲存的內容來新增。
3. PATCH: 主要告訴對方我要傳東西給你，請看我在Message body儲存的內容來更新。
4. PUT: 透過特定內容來取代目標資源上的被取代內容來達到更新，若被取代內容不存在的話，會增加特定內容至目標資源上。
5. DELETE: 會請求對方刪除某些資源。

如果一個請求被重複發送好幾次，其最後結果會像是下達一個同種請求後的結果，該請求就會是冪等(Idempotent)，而冪等名字是源自於數學的一元運算式的冪等，如果一個請求不會改變伺服器上的狀態和資源，該請求為safe，根據這兩種請求，我們將上述五種請求歸類：

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1631438998/blog/how2useAPI/safeAndIdempotent_yonpjf.png)

差別：
1. PATCH vs. PUT ： 前者允許更新一部分內容，後者得必須完整的內容來替代
比如說我們想要修改某個網頁的第一隻動物的名字，而且只修改名字而已(如下)，
{
  "id" : 1,
  "name" : "小白" ,
  "type" : "小型犬" ,
  "birthday" : "2019/1/29"

}
我們可以使用以下兩種方法之一：
```
PATCH http://127.0.0.1:3000/api/animal/1
PUT http://127.0.0.1:3000/api/animal/1
```

當我們使用第一種來將名字修改成大白的話，並將更新內容(如下)填入至Message Body，

```
{
  "name":"大白"
}
```

其結果回如同預期那樣，只修改名字屬性

```
--結果--
{
  "id" : 1,
  "name" : "大白" ,
  "type" : "小型犬" ,
  "birthday" : "2019/1/29"
}
```

但若採用二種方式，把同個更新內容填入至Message Body，

```
{
  "name":"大白"
}
```

其結果會是如下，所有沒被設定的屬性值皆會用預設值，比如null或者容易猜到的，就只有名字大白正確地修改

```
--結果--
{
  "id" : 1,
  "name" : "大白" ,
  "type" :  null ,
  "birthday" : null
}
```
所以若要採取PUT的話，得把全部的屬性值都寫上一輪來做取代，比如：
```
{
  "id" : 1,
  "name":"大白" ,
  "type" : "小型犬" ,
  "birthday" : "2019/1/29"
}
```


參考資料：
https://ithelp.ithome.com.tw/articles/10224134
https://stackoverflow.com/questions/31089221/what-is-the-difference-between-put-post-and-patch
