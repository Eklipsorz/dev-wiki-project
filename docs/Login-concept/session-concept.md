---
sidebar_position: 3
---
# 探討：Session 本身和設定


鑑於 [探討：http 協議上的 session 概念](/session-concept-in-rfc)所提到stateful session的概念，在這裡會基於概念的實現來說明session，此外，客戶端的cookie和伺服器的session皆為 **實作stateful session概念的產物**，不過這裡只會探討到實現層面，並且為了特別區分出哪一個是實作以及哪一個才是session概念，會特別強調session物件來表示實作

## 伺服器的session

cookie放在客戶端上，會是由瀏覽器負責實現其概念和如何管理cookie，而伺服器上的session則是依據基於RFC規範的第三方套件來負責產生session物件來實現session概念、使用名為Session store的空間來集中管理著這些session物件、每個session物件本身的設定(如安全、持續時間)，所以實際上session物件本身的好壞會由第三方套件來決定

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650528404/blog/network/session/session-example_broxec.png)

通常情況下，每個session物件是由key-value所組成的hash table結構(或者近似於hash table結構)，每一個session都有對應的key值或者使用session ID作為key值，其value會是該物件實際存下的內容-與客戶端連線時的互動狀態


在客戶端與伺服器端建立連線後，客戶端就會向伺服器發送請求，伺服器收到請求，隨後為了紀錄請求和回應的過程而建立了名為session的物件來儲存過程中所會有的資訊(PS.這些資訊正是代表著連線時的互動狀況)，而這個物件實際上會佔用伺服器的記憶體、硬碟、資料庫系統空間(PS. 該空間也會從記憶體或者硬碟兩者之一獲取)。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650439921/blog/network/session/building_a_session_ommsjq.png)

## 存了哪些資訊
具體來說，session主要會是用來代替客戶端來儲存它原本要儲存的資料以及伺服器單方面能夠維持 "整體連線時的互動狀況" 正常運作的資訊：
1. 代替客戶端cookie那一端來所儲存較為龐大或者較為敏感的資料
2. 延續第二點，但不同的是伺服器委託其他系統X來儲存cookie的原始資料，並讓伺服器的session物件儲存原始資料位於系統X的索引所在
3. 除了委託用途以外，能夠維持客戶端和伺服器兩者於連線的正常互動狀態之參數

第一點所提到的資料具體來說，由於客戶端的cookie本身受限於沒辦法放多資料且因面向前端而容易遭受到
惡意使用者讀取並使用，所以才會選擇將資料改放在專門處理資料的伺服器上，且比起客戶端而言，比較不容易受到惡意使用者從外部惡意讀取到資料，通常伺服器的實現上會直接存放cookie的原始資料，如下圖中的Session那樣儲存cookie的原始資料

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650529849/blog/network/session/data-inside-session_lxazph.png)


最後客戶端每一次要利用cookie原始資料與伺服器互動時，只需要向伺服器發送請求時附加原始資料位於session store的索引位置，當伺服器收到請求便會讀取索引位置來找到對應的session讀取客戶端委託伺服器儲存的原始資料，接著就是利用者原始資料處理業務，最後回傳結果給客戶端
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650531966/blog/network/session/index-inside-cookie-example1_mscr6s.png)


第二點是延續第一點，伺服器學客戶端那樣委託其他系統X來儲存cookie上的原始資料，然後再由session物件儲存原始資料於其他系統X的索引位置，這系統X的型態會是使用資料庫的某一個區塊或者檔案本身來儲存cookie的原始資料，如下圖中的左右兩個例子：左邊是將原始資料以檔案形式儲存在硬碟，並讓session儲存原始資料位於在硬碟的索引位址，右邊是將原始資料以紀錄或者符合資料庫表格格式的形式儲存在伺服器，並讓session儲存原始資料位於在資料庫的索引位址。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650529850/blog/network/session/index-inside-session_op9oon.png)

最後同樣地，客戶端每一次要利用cookie原始資料與伺服器互動時，只需要向伺服器發送請求時附加原始資料位於session store的索引位置，伺服器收到請求封包中的索引位置來找到對應session，若伺服器還委託資料庫系統來儲存cookie原始資料的話，那麼session的內容勢必對應著資料庫區塊的位址，到此時，伺服器會利用資料庫區塊的位址向對應資料庫系統發送讀取對應資料的請求，麻煩它回傳放在資料庫系統的cookie原始資料至伺服器，而資料庫系統讀取到這請求就回傳資料，獲取到原始資料的伺服器就直接處理資料並回傳結果至客戶端。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650533730/blog/network/session/index-inside-cookie-example2_dgmqhf.png)

但若伺服器委託的系統並不是資料庫的話，而是處於硬碟下的檔案，那麼就其session內容勢必對應著硬碟位址，到此時，伺服器會利用硬碟位址來向對應的檔案系統發送讀取原始資料的請求，其系統收到後便回傳資料至伺服器本身，最後伺服器獲取原始資料就便處理，隨後回傳結果至客戶端。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1650533729/blog/network/session/index-inside-cookie-example3_gfegar.png)

另外在這裡也可以將硬碟替換成記憶體來儲存cookie原始資料，只是與持續性來說，記憶體雖可以憑藉記憶體階層以及本身效能來達到快速存取的，但如果途中客戶端和伺服器的連線中斷，那麼很有可能因爲為了節省記憶體而直接釋放掉，或者說整體應用程式因爲崩潰被迫重開，導致記憶體被刷新；硬碟在這替代的話，則可以免除這問題，只是會因為記憶體階層以及本身效能不像記憶體那樣而無法達到快速存取的效果。

第三點所提到的參數具體會是伺服器單方面讓伺服器和客戶端連線時的互動狀態-session(非物件)繼續運作的參數，或者說，在這個session中，伺服器所要負責儲存的參數，比如說電商網站，，前者只不過是說客戶端將自己儲存的參數全都丟給伺服器來做，在第三點中，一樣也可以將伺服器所要儲存的參數存在資料庫、檔案系統、記憶體等等。

### 安全問題：偽造cookie和session對應關係

## 儲存在哪？ && 何時釋放？
伺服器的session物件主要放置於名為session store的空間，該空間通常會是由記憶體或者硬碟所構成，至於session物件的持續留在伺服器的時間就決定了何時釋放該物件所佔用的記憶體空間，然而這部分得依據實際的第三方套件來決定。

若是由express-session套件內建預設的MemoryStore模組來製作和管理session物件，MemoryStore模組本身只是為了測試session而開發，所以不會提每個session提供持續時間設定或者對沒用到的session物件進行記憶體釋放，所以會有記憶體洩漏問題-**某一些記憶體區塊無法被正常釋放，而這些記憶體區塊還保留值並且不被系統看待成可用的記憶體，造成記憶體上的浪費**，所以MemoryStore模組並不適合真正的上線後環境
> Warning The default server-side session storage, MemoryStore, is purposely not designed for a production environment. It will leak memory under most conditions, does not scale past a single process, and is meant for debugging and developing.

不過，若選擇其他針對
不過可替代的方案有：
  - 以硬碟為主的資料庫來管理session：MongoDB
  - 以記憶體為主的資料庫來管理session：Redis
  - 以記憶體空間來管理session：Memcached
參考資料：
[一次 node.js 内存泄漏排查](https://zhuanlan.zhihu.com/p/257854926)

安全設定有哪些？
## session

## 誤解
1. cookie刪了，session就跟著刪了
2.

經典登入案例：？

鑑於


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