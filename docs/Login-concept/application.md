---
sidebar_position: 5
---


# cookie 和 session 之間的應用
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



## session
1. 原意為為從事某項活動的互動或者從事某項活動的會議，在這裡會將活動設定為每一次伺服器和客戶端之間的連線活動，而會議/互動則本身引申為代表兩者間在連線時的互動紀錄，前兩者合再一起就是伺服器和客戶端之間的連線活動時紀錄。
2. 其紀錄由伺服器端進行，可儲存客戶端和伺服器連線時的互動狀態
3. 由於伺服器本身會處理大量的請求，也意味著有大量的Session儲存在伺服器，所以為了提供穩定的服務，會替每個session安排超時時間，當session紀錄到達時間時，就從伺服器釋放
4. session store 是伺服器存放session 資料的地方，通常會是伺服器的記憶體、某個資料庫服務(如redis)
