---
sidebar_position: 4
---

# 網站封包
由於網站普遍皆使用應用層的http協定或者https協定來實現，所以客戶端(Client)和伺服器端(Server)之間的封包傳遞皆使用前兩者的其一個協定來規劃封包形式、封包內容、如何傳遞，在這裏會以http的封包來說明，而https的封包則留到後續再補充。


## HTTP封包形式


當Client端想透過HTTP協定去使用server端的網站時，Client端跟server端這兩者間會做TCP/IP中的三向交握中建立連結，之後Client便會正式發送請求的HTTP封包給server端，而server端收到請求後便會回傳另一個HTTP封包來回應Client。

未增加封包後的版本：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1631431354/blog/how2useAPI/client2server_rzjbgc.png)

增加封包後的版本：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1631433951/blog/how2useAPI/detail_client2server_k7juuv.png)


由於應用層的HTTP封包都會直接分為Header和Body，Header會存放告訴系統如何處理應用層或者HTTP協定，而Body則是應用程式-網站所要傳遞/接收的正式內容，Client端和server端對於Header和Body的內容皆不一樣，Client端封包的Header部分會有Request Method、Request URL、Request Headers，而Body則是Message body，其中Resqest Method是指想要對目標資源做什麼樣請求，Request URL是具體的目標資源，其URL是用來定義資源在網路環境下的的位置格式，使用這個格式可以在網路上找到對應的目標資源，Request Headers則是進一步定義請求的設定、格式、要找的伺服器位址(與URL不同，只列出對應伺服器域名或者IP)，最後一個Message body是該請求封包的具體詳細請求內容。

server端封包的Header部分會有Status code、Response Headers，而Body則是Response Body，其中Status code則是以數字表示回應Client請求的結果/狀態，Response Headers則是進一步定義回應"請求"的設定、內容格式(Content-Type，定義回傳資料是什麼格式)，而Response Body則是具體詳細的回應內容。

### Resquest Method
Resqest Method是指Client端想要對目標資源做什麼樣請求，具體請求有：
1. GET: 主要向目標資源請求讀取某些資料。
2. POST: 主要告訴對方我要傳東西給你，請看我在Message body儲存的內容來新增，注重於建立內容和資源。
3. PATCH: 主要告訴對方我要傳東西給你，請看我在Message body儲存的內容來按照現有資料內容進行部分內容更新。
4. PUT: 透過特定內容來取代目標資源上的所有內容來達到更新，若被取代內容不存在的話，會增加特定內容至目標資源上。
5. DELETE: 會請求對方刪除某些資源。


### 冪等請求
英文為Idempotent，源自於數學的一元運算式的冪等，也就是若進行任一個元素進行多次的冪等運算式，其結果會是該元素進行一次的冪等運算式之結果，套用在HTTP請求上，如果任一個資源被一個請求被重複發送好幾次，其最後結果會像是下達一個同種請求後的結果，該請求就會是冪等(Idempotent)。根據這個定義，主要的五種請求方法會是：

1. GET 請求方法：屬於冪等請求，因為當對一個資源進行多次同樣的GET請求方法，都會是得到同樣的資源內容，這對於下達一次的GET請求方法而言，結果會與多次同樣的方法之結果是一樣。
2. POST 請求方法：不屬於冪等請求，因為當對一個資源進行多次同樣的POST請求方式時，會不斷建立好幾塊內容或者資源，這對一次的POST請求方法而言，結果會與多次同樣的方法之結果是不一樣，因為單次的POST請求方法就只是一塊內容或者資源。
3. PATCH 請求方法：不屬於冪等請求，然而該請求方式可以是冪等請求或者非冪等請求，一切取決於開發者如何運用PATCH以及運用方式是否具有冪等特性，但在這裡會為了嚴謹性而直接設定成不屬於冪等請求，不過當我們指定Patch為冪等請求時，比如下面的Patch request，這個請求會直接修改名為Tito的歲數，但多次下次同樣的Patch request，其結果會如同指下達一次的Patch reqeust之結果，也就是都皆為33歲
```
// Original resource
{
  name: 'Tito',
  age: 32
}

// PATCH request
{
  age: 33
}

// New resource
{
  name: 'Tito',
  age: 33
}
```

然而若Patch request改成非冪等請求，則會像是以下的Patch request，當下達一次的Patch request就會替名為Tito的歲數增加歲數，這會讓每一次的Patch結果皆不一樣，換言之，多次的Patch request之結果跟單次Patch request的結果是不一樣的。
```
// Original resource
{
  name: 'Tito',
  age: 32
}

// PATCH request
{
  $increment: 'age'
}

// New resource
{
  name: 'Tito',
  age: 33
}

```

這兩個範例皆從本小節的參考資料得來的，若要仔細了解可以去閱讀參考資料中的[Why PATCH method is not idempotent?](https://softwareengineering.stackexchange.com/questions/260818/why-patch-method-is-not-idempotent)


4. PUT 請求方法：屬於冪等請求，該功能會先找到它想更改的內容，並用其他內容來完整取代，所以若對一個資源進行多次同樣的PUT請求方法，會因為第一次就已經成功取代，而並不會讓後續相同的PUT請求方法找到同樣想改的內容進行取代，整體結果會跟第一次的PUT請求方法一樣。

5. DELETE 請求方法：屬於冪等請求，該功能會先找到它想要刪除的內容，並且找到就刪除，所以若對一個資源進行多次同樣的DELETE請求方法，會因為第一次就已經完成刪除，而並不會讓後續相同的DELETE請求方法找到同樣想改的內容進行刪除，整體結果會跟第一次的DELETE請求方法一樣。


參考資料：
1. [Why PATCH method is not idempotent?](https://softwareengineering.stackexchange.com/questions/260818/why-patch-method-is-not-idempotent)

## 安全請求

如果一個請求不會改變伺服器上的狀態和資源，該請求為safe，根據這兩種請求，我們將上述五種請求歸類：

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1631438998/blog/how2useAPI/safeAndIdempotent_yonpjf.png)

### PATCH vs. PUT
這兩者的共同點是可以更新資料，就前者而言，可以直接指定部分資料範圍來更新資料，而後者則是透過取代來更新資料，然而在更新方面上，PUT得必須添加更新內容和其餘內容來替代，比如說我們想要修改某個網頁的第一隻動物的名字，

```
{
  "id" : 1,
  "name" : "安安" ,
  "type" : "狒狒" , 
  "birthday" : "2019/01/01"
}
```

而且只修改名字而已(如下)，我們可以使用以下兩種方法之一：
```
PATCH http://127.0.0.1:3000/api/animal/1
PUT http://127.0.0.1:3000/api/animal/1
```

當我們使用第一種來將名字修改成 "你好" 的話，並將更新內容(如下)填入至Message Body，

```
{
  "name":"你好"
}
```

其結果回如同預期那樣，只修改名字屬性

```
--結果--
{
  "id" : 1,
  "name" : "安安" ,
  "type" : "狒狒" ,
  "birthday" : "2019/01/01"
}
```

但若採用二種方式，把同個更新內容填入至Message Body，

```
{
  "name":"你好"
}
```

其結果會是如下，所有沒被設定的屬性值皆會用預設值，比如null或者系統預設值，實際上就只有名字被正確地修改

```
--結果--
{
  "id" : 1,
  "name" : "你好" ,
  "type" :  null ,
  "birthday" : null
}
```
所以若要採取PUT的話，得把全部的屬性值都寫上一輪以及要更新的內容合起來寫來做取代，比如：
```
{
  "id" : 1,
  "name":"你好" ,
  "type" : "狒狒" ,
  "birthday" : "2019/01/01"
}
```


參考資料：
1. [PUT、PATCH 差別](https://ithelp.ithome.com.tw/articles/10224134)
2. [What is the difference between PUT, POST and PATCH?](https://stackoverflow.com/questions/31089221/what-is-the-difference-between-put-post-and-patch)


The difference between PUT and POST is that PUT is idempotent: calling it once or several times successively has the same effect (that is no side effect), whereas successive identical POST requests may have additional effects, akin to placing an order several times.