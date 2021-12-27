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
  - Authorization Grant：用來證明Resource Owner賦予應用程式一定程度的授權、同意應用程式去做某些處理和存取一定範圍內的資源，通常會由Authorization Server根據Resource Owne的同意情況來將Grant給予Web Application
  - Authorization Code：主要功用如同Authorization Grant的功能一樣，只是差別在於該Code會是透過Authorization Server來傳送至使用者，然後再由使用者傳送給Web Application
  - Redirect URI（Callback URL）：指定在使用者驗證完使用者身份並獲得Grant後會導向的頁面，該頁面會是由第三方來提供
  - Access Token：Authorization Server在確定Authorization Client本身是合法且其擁有的Grant是合法後所簽發給Authorization Client的Token，本身由於安全問題，該Token只會維持一段時間能夠使用，過期就無法使用
  - Scope：使用者許可給Authorization Client/第三方使用的資源範圍，通常會是以清單形式來表示scope，而內容則為可以取得使用者的名稱、可以編輯使用者的大頭貼、可以刪除我的某則貼文


## OAuth Authorization Code Flow 流程
1. 首先當使用者點擊Sign in with xxxx按鈕時，就表示使用者正要使用xxxx服務(如Google、FB)所授權的資訊認證來當做目前網站的會員資料認證，同時也表示著使用者向Web Application發送GET /xxxx/loginAuth服務要求xxxx服務的資訊認證來做登入認證
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1640538440/blog/OAuth/mediumExample_bqqckm.png)
2. 當Web Application收到請求後，便回傳ClientID以及要求使用者導向Authorization Server下的/authorization
3. 使用者收到導向請求便向Authorization Server發送GET /authorization請求以及附帶ClientID
4. Authorization Server收到後便檢驗ClientID是否合法並辨識為該使用者為OAuth協定下的Resource Owner
5. 辨識為合法後，Authorization Server要求使用者填寫帳密
6. 使用者提交帳密至Authorization Server
7. Authorization Server檢驗帳密，看是否合法，若不是合法就要求重新輸入，合法的話就繼續下一步
8. Authorization Server會顯示Web Application想要存取何種資料並詢問願不願意，若拒絕的話，OAuth認證就結束
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1640541206/blog/OAuth/ScopeExample_mcoxbj.png)
9. 使用者向Authorization Server表示願意
10. Authorization Server會生成一個Authorization code並請求使用者導向至/RedirectURI?code=xxx，RedirectURI會是指Web Application
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1640626647/blog/OAuth/AuthFlowPart1_mkitpy.png)
11. 使用者向Web Application發送GET /RedirectURI?code=xxx
12. Web Application收到請求便向Authorization Server發送POST /token以及傳送使用者傳遞過來的Authorization Code、自己所擁有的Client ID和Client Secret
13. Authorization Server收到請求便檢驗Authorization Code、Client ID、Client Secret是否合法，若不合法就中斷，若合法就繼續下一步
14. Authorization Server核可後便發送Access Token(這裡的值會是aaa)和Refresh Token至Web Application，此時Web Application才有權利向Resource Server索要合法範圍的使用者資料
15. Web Application發送GET /Resource?access_token=aaa至Resource Server
16. Resource Server檢驗access_token是否合法，若不合法就中斷，若合法就繼續下一步
17. Resource Server核可後便傳遞對應的使用者資料
18. Web Application收到後便確定帳號驗證成功，同時許可使用者可以登入該Web Application
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1640628119/blog/OAuth/AuthFlowPart2_hhxhnp.png)

## OAuth Refresh Token流程
1. 由於每一份Access Token都有時限，超過一定時間就會過期變得無法讓任何一方透過它來要求資源，Authorization Server為了讓Web Application能夠再申請另一份短期的Access Token而提供一個Refresh Token至Web Application，而當Web Application所擁有的Access Token是過期的話，那麼其Application就會向Authorization Server重新申請一份新的Access Token，提交的資料有Refresh Token和Grant，Authorization Server收到這些資料並審核成功的話，並會給予新的Access Token至Web Application
2. 好處是：不須經過使用者來重新同意來延長Access Token使用
3. 流程為如下，以Access Token過期後為主：
  - Web Application拿過期的Access Token去向Resource Server索要對應使用者資料
  - Resource Server回報錯誤並告知Web Application目前所擁有的Access Token已經過期，無法再有權利索要資料
  - Web Application向Authorization Server發送索要新的Access Token之請求，並傳送Refresh Token和Code至該Server
  - Authorization Server收到並檢驗Refresh Token和Code是否合法，若合法就繼續做下一步，若不合法就不允許發放新的Access Token
  - Authorization Server審核成功並發送新的Access Token至Web Application
  - Web Application以新的Access Token來向Resource Server索要對應使用者資料
  - Resource Server檢驗新的Access Token是否合法，若合法就繼續做下一步，若不合法就不允許發放對應資料
  - Resource Server審核成功就回傳對應使用者資料至Web Application
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1640628454/blog/OAuth/RefreshAuthFlow_oscggl.png)