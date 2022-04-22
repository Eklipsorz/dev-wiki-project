---
sidebar_position: 6
---

# jwt-concept


JWT(JSON Web Token)是一種內容形式以JSON為原型且附加簽名值(signature)的token，
其token的驗證方式是讓伺服器和客戶端之間只需要做signature比對來驗證就能達成，而非是傳統的API token得讓伺服器事先儲存正確的token來比對接收過來的token是否為合法，進而讓伺服器驗證成本平均分擔在每個客戶端和伺服器。


## JWT 本質和內容格式
JWT本質就如同發給客戶端一把合法的通關密語，在使用上客戶端只需要向伺服器說出通關密語就行，在這裡伺服器只會 **認誰擁有合法的通關密語就是合法使用者**，而非驗證哪個使用者才是真正的合法使用者，雖說仍有安全疑慮，但只要讓能夠存取到的服務不會危害到真正的系統，仍不失為一個好方法，其驗證機制會是驗證token是否被竄改為主，JWT形式為如下：
```
base64UrlEncode(Header).base64UrlEncode(Payload).Signature
```
其中base64UrlEncode是指 **內容不被url編碼系統轉譯的base64編碼**，而Header、Payload、Signature則是撇開該編碼後的JWT 原始資料，其中Header、Payload會以JSON格式來存放，而Signature則是由前兩者和自己密鑰計算出來並附加上去，本身並不是以JSON來存放。

JWT主要有三個部分所組成：
1. Header：說明算法、token型別，用途為 **方便告知對方伺服器以什麼型別token和演算法來重現signature並且比對**。
2. Payload：說明該token要包含的資料是為何，用途為 **由payload提供客戶端對應的資訊，並盡可能不用透過伺服器來存取資料庫**。
3. Signature：由Header、Payload、伺服器的Secret來進行雜湊而產生的簽名值，大概計算公式如下，用途為 **藉由驗證是否竄改來驗證使用者是否為合法**。
```
Alg(base64UrlEncode(Header) +  base64UrlEncode(Payload) + secret)
```
Alg則是依據著Header所指定的hash算法(產生Signature值)而決定的算法，base64UrlEncode 是指 base64url，原形為base64編碼，因為Signature很有可能會是以url形式的一部分來傳遞給對方，而base64編碼下會出現讓url編碼誤會的字元，如=、+、/，而base64url是基於這問題來解決，其本身仍是base64編碼，但會注意編碼是否有url編碼誤會的字元，若有的話，就轉換，若沒有的話就不轉換。


參考資料：
[Why do you use base64 URL encoding with JSON web tokens?](https://stackoverflow.com/questions/56711129/why-do-you-use-base64-url-encoding-with-json-web-tokens)

一旦這些Header、Payload存放完資訊，就會以base64url來編碼，來構成以下資訊
```
base64UrlEncode(Header).base64UrlEncode(Payload)
```
接著再搭配產生密鑰公式來產生密鑰Signature，並附加至前者結果字串
```
base64UrlEncode(Header).base64UrlEncode(Payload).Signature
```



## JWT 特點 & 缺點

特點：
1. 在分散式系統中，可以透過同一個secret值來維持每一個伺服器對於驗證資料的一致性：由於是以signature以及secret來認證的，所以在多個後端伺服器的情況下，只要每個伺服器共享著同樣的secret，那麼任何一個能在伺服器認證成功的使用者所獲取到的token，都能在其他伺服器的認證系統下認證成功，而且會是同一個使用者
2. 減少伺服器對於驗證結果的儲存成本：將驗證結果分給客戶端去儲存，當要驗證時，伺服器就拿客戶端給定的結果來判斷客戶端是否持有合法的JWT

缺點：
1. 只要有人拿到token，就能變成那個人，即使那個token本就不屬於他的。
2. 由於JWT本身不需要伺服器以session來儲存JWT儲存驗證結果和對應其JWT，這導致伺服器很難控管token本身的時效性、安全性、能否主動撤銷
3. Header和Payload是由base64url編碼格式所構成，這編碼由於可以反解，所以某種情況下等同於明文，所以不建議將敏感性資訊丟進Header和Payload


## 如何控管JWT 的過期
由於JWT本身很難對每一個客戶端下的token進行控管，如每個token的時限上控管，若不給予token時效，會增加這token被惡意使用的時間和機率，解法為**賦予每個客戶端一組token和refresh token，並設定每個token和refresh token何時過期，token為短期有效，refresh token為長期有效，並用於token過期時來產生新的token，而refresh token本身過期的話，就重新從主要獲取token和refresh token的路徑請求**
![](https://pic4.zhimg.com/80/v2-0452e08eef898d1a4f1dbdda7628fb6f_1440w.jpg)

實現流程為：
1. 直接將 **何時過期的時間** 存放在每一個token下的payload 或者 header上，並且每接收到token時，伺服器就去比對目前時間和token上的過期時間是否過期，若過期就駁回；若不過期就允許

2. 在每一次給予主要token的同時間，也新增一個refresh token來給予客戶端，並設定較長的時效性


https://cloud.tencent.com/developer/article/1193280

參考資料
https://ithelp.ithome.com.tw/articles/10199102
https://github.com/WebDevSimplified/JWT-Authentication/blob/master/authServer.js
https://medium.com/@solomonrock13/nodejs-authentication-using-jwt-and-refresh-token-and-sequelize-and-postgres-from-scratch-2031bd9a815e
https://cloud.tencent.com/developer/article/1193280

## 補充知識：credentials
1. 英文如下，中文是指能夠證明某些事物擁有某種特性或者能力的物件，意指為身份驗證資訊
> the abilities and experience that make someone suitable for a particular job or activity, or proof of someone's abilities and experience

2. 在電腦科學裡中，身份驗證資訊會是像帳密那樣或者能夠證明使用者是哪個使用的資訊，通常token實作上是以credential來封裝的，那麼其token就會是credential。

3. JWT 本身由於內部的payload攜帶著credential資訊，所以本質上JWT就能代表著credential。
> Warning: JWTs are credentials, which can grant access to resources. Be careful where you paste them! We do not record tokens, all validation and debugging is done on the client side.