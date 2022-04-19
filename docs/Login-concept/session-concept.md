---
sidebar_position: 2
---
#  探討：Session 本身和設定

## session 本身的含義

根據字典所描述的session，會是指特定活動下的一段時間或者特定活動下的一場會議，在這裡可以稍微轉換一下說法，一段時間可以用一場會議來替代描述session，那麼實際上專注在會議上的定義就能明瞭session的意涵，而meeting代表著一個計畫好的時間點，能夠與其他人討論著什麼東西的時間點，或者說計畫好的互動情形、互動狀態，添加至原句就 **使session的意思變成特定活動下的所預計好之互動情形** ，
```
session: a period of time or meeting arranged for a particular activity
meeting: a planned occasion when people come together to discuss something
```

## session 在http/https協議上的含義
從上述轉換成網路連線的說法，**特定活動下的所預計好之互動情形** 的特定活動下就是意旨**伺服器和客戶端之間的連線活動下**，所預計好之互動情形則是意旨 **伺服器和客戶端之間在連線時的互動會根據RFC規範所定義的內容來進行**，換言之預計就是指著根據RFC規範下所會有的互動，簡單點就是指 **伺服器和客戶端之間在連線時的互動狀態**


## session 在http/https協議的存在目的
由於http/https協議本身為了簡化客戶端和伺服器之間的連線結構，而定義客戶端和伺服器之間在連線時互動情況並不會紀錄下來-無狀態(stateless)，但隨著越來越多服務需要客戶端和伺服器之間的過去互動情況，比如首次登入後就直接透過互動情況而跳過、使用者可以透過過去的購物車內容來直接購買，就有人於RFC規範提出session概念以及如何運用session概念來允許客戶端和伺服器在連線過程中 **分別讓客戶端單方面紀錄與伺服器之間的互動情形、讓伺服器單方面紀錄與客戶端之間的互動情形、更或者讓兩者紀錄雙方的互動情形**。

而具體實作方式則是客戶端和伺服器各自從自己的系統中分配空間來儲存能夠代表與彼此進行連線時會有的互動情形之資料，而這樣的資料會是用session來稱呼，session 裡頭的內容則是稱之為session information，比如客戶端曾與某個電商伺服器進行連線並從中將產品放進購物車，但過程中客戶端先暫時關閉伺服器連線，而為了讓客戶端還能回憶起購物車的產品，會紀錄著這產品的相關資訊，而這資訊本身就會是session，其內容會是產品的主要內容，也就是session information。

總結：
1. session概念 本身是為了 **在不違反http/https無狀態下的環境能夠滿足伺服器和客戶端對於狀態的需求**
2. session 本身是概念，會有相對應的實現方法，如:
  - Cookie：賜福器透過Set-Cookie來幫助客戶端添加Cookie資訊至客戶端本地端，以方便下次與同樣伺服器延續之前處理狀態下的處理過程，其中Cookie資訊本身會因為用途而會是session的具體實體之一。
  - 
  
  下的互動狀態會被稱之為session，

相關的 RFC規範 和 參考文獻：
[HTTP State Management Mechanism](https://tools.ietf.org/html/rfc2109)
> This document specifies a way to create a stateful session with HTTP requests and responses.  It describes two new headers, Cookie and Set-Cookie, which carry state information between participating origin servers and user agents.  The method described here differs from Netscape's Cookie proposal, but it can interoperate with HTTP/1.0 user agents that use Netscape's method.  (See the HISTORICAL section.)

>  This document describes a way to create stateful sessions with HTTP requests and responses.  Currently, HTTP servers respond to each client request without relating that request to previous or subsequent requests; the technique allows clients and servers that wish to exchange state information to place HTTP requests and responses within a larger context, which we term a "session"
在這裡客戶端單方面與伺服器之間的互動情形紀錄會是稱作為cookie，而伺服器負責單方面與客戶端互動情況紀錄會是session，若兩者同時紀錄，那麼就會是雙方紀錄彼此的互動情形。


[淺談 Session 與 Cookie：一起來讀 RFC](https://blog.huli.tw/2019/08/09/session-and-cookie-part2/)



Session機制是一種服務器端的機制，服務器使用一種類似於散列表的結構（也可能就是使用散列表）來保存信息

保存這個Session id的方式也可以採用Cookie，但似乎很多人都有一種彷彿瀏覽器關掉Session也會消失的錯覺，但本質上並非如此，就像會員卡，除非你主動提出銷卡，否則店家不會刪除顧客資料，這件事情對Session來說也如出一轍，除非通知Server刪除Session，否則Server端會一直保留來保持會話暢通，但瀏覽器從來不會主動在關閉前通知服務器要關閉

而關閉瀏覽器後因為Cookie消失造成 Session id也消失了，但只要把原來的Session ID再發送給Server，那還是能夠找到原來的Session 。

Cookie和Session得結合，到底還會不會有被竄改的問題，這個時候就要靠"簽章"來驗證資料的真實性，在我使用Cookie請求時，可以加一個簽章，也就是在我傳輸的資料後面加上一個對應的秘密字串，當伺服器回傳時，可以回應該字串，若是其他使用者偷偷串改的話，由於串改的資料和我的秘密字串無法相符，當然也無法作偽造，這就是所謂的 SignedCookie，但記



## session
1. 原意為為從事某項活動的互動或者從事某項活動的會議，在這裡會將活動設定為每一次伺服器和客戶端之間的連線活動，而會議/互動則本身引申為代表兩者間在連線時的互動紀錄，前兩者合再一起就是伺服器和客戶端之間的連線活動時紀錄。
2. 其紀錄由伺服器端進行，可儲存客戶端和伺服器連線時的互動狀態
3. 由於伺服器本身會處理大量的請求，也意味著有大量的Session儲存在伺服器，所以為了提供穩定的服務，會替每個session安排超時時間，當session紀錄到達時間時，就從伺服器釋放
4. session store 是伺服器存放session 資料的地方，通常會是伺服器的記憶體、某個資料庫服務(如redis)


## cookie 和 session 之間的應用
1. 應用於伺服器和客戶端之間的憑證系統，主要分為：
  - Cookie-based Authentication ：由客戶端提供的cookie內容來認證其對象在伺服器是否為合法對象
  - Token-based authentication：由客戶端發送的請求封包裡夾雜著憑證文字(token)來讓伺服器判斷該請求是否為合法對象。
2. Cookie-based Authentication : 由於cookie是存放客戶端和伺服器端的連線互動時的狀態，很容易被其他電腦獲取其cookie，通常會在cookie內容上加密或者不在cookie儲存敏感性資料，取而代之的是，伺服器本身會以類似的陣列形式來儲存敏感性資料，而cookie就儲存其資料在陣列上的位置，而當伺服器收到請求後，會從cookie取得對應server的session id(在session陣列上的位置)，並透過其id來在伺服器內部存取對應的session內容，最後從內容獲取資料庫的內容。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1639747105/blog/http/cookie_session_tgjczs.png)

## 使用者登入認證機制
1. 當使用者進入一個需要帳號密碼來進行登入的頁面時，輸入完帳密並正確導入對應頁面，可如果使用者將頁面關掉，其服務會因為無狀態的關係而沒把帳號密碼的相關狀態存下，而當使用者在進入相同頁面時，使用者會被要求輸入帳密做後續的認證


2. 為了讓使用者更方便登入服務，每當使用者向伺服器成功登入服務時，伺服器就給予使用者一個憑證，讓它能夠在關閉頁面情況下還是能夠依據著憑證直接使用相同服務，不必重新登入
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1639985704/blog/loginSystem/Certificate_System_hc5u2n.png)

3. 憑證認證機制具體實現，有四個步驟：在這裡先假設是第一次登入，
  - Step1. 使用者輸入帳密並發送POST /users/login 請求伺服器認證帳密
  - Step2. 伺服器確認帳密是否正確，若正確就建立session來儲存對應帳號的主要屬性並給予session id至客戶端，讓客戶端以cookie儲存
  - Step3. 客戶端收到session id後，就便儲存在cookie中，並且每次對同一個伺服器發送請求時都會讓伺服器去存取cookie內容
  - Step4. 伺服器會去檢查cookie內容的session id是否存在於伺服器中，若存在，就讓客戶端去存取對應的帳號資料
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1639932196/blog/loginSystem/implementationOfCertificating_gvmhfx.png)

4. 憑證認證上的安全性：由於是利用儲存在cookie上的session id來做認證，而瀏覽器本身可允許修改cookie內容，因此很容易讓人透過修改session id來偽裝其他人的身份，比如說客戶端原本拿到session-id為a的資料，對應到伺服器的使用者資料就是id為1的使用者，若把session-id更改為b，就可以讓伺服器誤以為是id為2的使用者，讓客戶端直接使用該使用者的資源。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1639933781/blog/loginSystem/CertificationStore_a7t3ou.png)


## 憑證認證上的安全性
1. 在session-id上增添簽署的功能來避免他人從中竄改cookie中的session-id




  ## Set-Cookie 標頭
  1. 是伺服器回應給客戶端請求的回應封包所含有的標頭，其標頭會要求客戶端建立一個cookie去儲存標頭內的內容。


  ## Cookie 標頭
  1. 是客戶端向伺服器端發送請求的封包所含有的標頭，其標頭內容會是之前客戶端被要求儲存cookie的內容，該內容會被伺服器做認證並做後續處理


  
## Set-Cookie 和 Cookie 之間的互動
1. 伺服器回應客戶端上一個請求
```
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```
2. 現在隨著每個請求，瀏覽器會使用 Cookie 標頭將所有先前儲存的 cookies 傳給伺服器。
```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```