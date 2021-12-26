---
sidebar_position: 13
---

# OAuth 介紹



## Authorization
1. 原文為如下：
> official permission for something to happen, or the act of giving someone official permission to do something
2. 一種給予他人官方權利做某件事的行為


## OAuth
1. OAuth是Open Authorization的縮寫
2. 是一種將資訊和資訊認證授權給第三方使用的開放標準(開放意旨授權給外面)，在這裏第三方可以將授權過來的資訊認證當作服務上的登入認證機制，而資訊可以作為第三方的使用者基本資料來建立。
3. 驗證流程會有的角色名詞：
  - Resource Owner：有能力授權他人存取受保護之資源的實體物件，受保護之資源會是實體物件所擁有的，而若實體物件本身是人，那麼會被稱之為使用者(user)
  - OAuth Client/Authorization Client：為Resource Owner所交流的Application Server，想要存取Resource Owner所擁有的私人資源來處理，在OAuth流程中會被使用者/Resource Owner委派去和Authorization Server作驗證獲取token 並拿著token去和Resource Server索要對應的私人資源，也就是Resource Owner的私人資源
  - OAuth Server/Authorization Server：負責單方面面向Resource Owner驗證它是否為合法的使用者以及哪些資源是可以被第三方使用，最後根據結果來產生出Authorization grant給Authorization Client 或者拒絕、驗證Authorization Client是否合法以及它所擁有的Authorization grant是否合法，最後根據結果來產生出Authorization Token
  - Resource Server：存放Resource Owner所擁有的私人資源之伺服器，同時會驗證Authorization Client的Authorization Token是否合法。

4. 其他用詞：
  - Authorization Grant：用來證明Resource Owner賦予應用程式一定程度的授權、同意應用程式去做某些處理和存取一定範圍內的資源，通常會由Authorization Server根據Resource Owne的同意情況來給予Grant
  - Redirect URI（Callback URL）：指定在使用者驗證完使用者身份並獲得Grant後會導向的頁面，該頁面會是由第三方來提供
  - Access Token：Authorization Server在確定Authorization Client本身是合法且其擁有的Grant是合法後所簽發給Authorization Client的Token
  - Scope：使用者許可給Authorization Client/第三方使用的資源範圍，通常會是以清單形式來表示scope，而內容則為可以取得使用者的名稱、可以編輯使用者的大頭貼、可以刪除我的某則貼文


## OAuth 一般流程
1. 首先當使用者點擊Sign in with xxxx時，就表示使用者正要使用xxxx服務(如Google、FB)所授權的資訊認證來當做目前網站的會員資料認證
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1640538440/blog/OAuth/mediumExample_bqqckm.png)
2. 當點擊之後，便是發送請求至對應目前網站的伺服器，而伺服器的路由處理就是向xxxx服務的認證伺服器(Authorization Server)並提供對應的認證資料1給對方伺服器
3. 伺服器會檢驗認證資訊1中的Client ID、Client Secret是否合法，若合法就設定Redirect URI和Response Type，其中這兩個分別為指定當使用者在xxxx服務的登入認證成功時所要導向的網址以及Authorization Server所要回應給Web Application的Grant種類是為何
4. 將使用者頁面導向xxxx服務的登入頁面
5. 使用者在登入頁面輸入帳密並提交至Authorization Server做身份驗證
6. Authorization Server檢驗帳密是否合法，若合法就繼續下一步，否則就停留或者中斷
7. Authorization Server詢問使用者哪些資料要提供給網站
8. 使用者提交資料授權範圍(Scope)至Authorization Server後，就會依照Redirect URI來導向另一個頁面，若授權範圍影響著認證或者不同意的話，會直接回報錯誤訊息
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1640541206/blog/OAuth/ScopeExample_mcoxbj.png)
9. Authorization Server給予Web Application一份Authorization Grant來表示使用者同意Web Application來代替使用者去獲取對應範圍的資料以及進一步的資料認證
10. Web Application傳遞認證資訊2來索要Access Token
11. Authorization Server檢驗認證資訊2是否合法，若合法就繼續做下一步，若不合法就不允許發放Access Token
12. Authorization Server發放Access Token至Web Application
13. Web Application透過Access Token來索要對應範圍(Scope)的使用者資料做認證
14. Authorization Server 會檢驗Access Toke，若合法就繼續做下一步，若不合法就不允許發放對應資料
15. Authorization Server發放對應使用者資料

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1640540290/blog/OAuth/AuthFlow_djhfdf.png)