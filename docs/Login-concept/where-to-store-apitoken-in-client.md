---
sidebar_position: 8
---

# 前後端分離：客戶端如何存API Token？
當前端成功從API伺服器獲取API Token時，往後若要使用API伺服器獲取特定服務必須將對應Token傳給伺服器以此證明自己是合法使用者來獲得使用服務，而客戶端勢必要有一個空間要能夠存放API Token，主要有兩種方式：
  - 使用cookie來存放API Token：由於前後端分離出來，所以會容易因為兩者所處的Domain不同，所以容易因為同源政策關係，而無法從隸屬於後端伺服器的cookie取出token來從目前所在的網域發出合法的請求
  - 使用Bearer Token：將Token放在HTTP請求封包上的標頭，尤其是Authorization標頭

## 使用cookie來存放API Token

## 使用Bearer Token


## 補充知識：JWT Extractor && Bearer Token
1. 當使用者獲得token並向同一個伺服器使用token去索要該平台需要認證的服務時，使用者會發送一個封包，該封包上的Authorization Header會是Bearer字串 和 token 所合併的字串，而這樣子合併的字串又被稱之為bearer token
> Whenever the user wants to access a protected route or resource, the user agent should send the JWT, typically in the Authorization header using the Bearer schema. The content of the header should look like the following:
```
Authorization: Bearer <token>
```
> The server's protected routes will check for a valid JWT in the Authorization header, and if it's present, the user will be allowed to access protected resources. If the JWT contains the necessary data, the need to query the database for certain operations may be reduced, though this may not always be the case. 

2. bearer token 實現方法會是依據OAuth2.0規範來實現的token存放方式

3. passport.js所提供的JWT Extractor為ExtractJwt，它具有一些專門提取JWT的方法，其中一個方法會是以Bearer Token來提取
```
// 
const JWTExtractor = require('passport-jwt').ExtractJwt
// 以Bearer Token的方式來提取JWT
JWTExtractor.fromAuthHeaderAsBearerToken()
```
> fromAuthHeaderAsBearerToken() creates a new extractor that looks for the JWT in the authorization header with the scheme 'bearer'


4. bearer token 中文意思為不記名令牌或者不記名字串，在這裡會是只用特定字串token來代表每個被認證的對象，所以才叫不記名。
> a person whose job is to carry something, or a person who brings a message



### 補充知識：Bearer Token
0. Bearer Token，原本取自於銀行界的可兌現之支票，誰擁有這張支票誰就有擁有權利來獲取對方的錢，從這引申為誰擁有token誰就有權利去獲取伺服器方的服務
```
最近研究第三方支付系统接入，设计中有许多设计到Bearer的概念，我觉得Bearer Token的设计概念也应当与类似支票兑付的场景模型有关，我来说一点自己的理解。字面翻译Bearer，指的是持票人，用支票使用场景来解释的话，就是指手持支票坐等兑付的受款人（这里假定是最普通的银行汇票）。那么银行就作为承兑人，以票面上出票人的签名为凭证行使保障受款人收款的义务。

转回Bearer token的场景，这里的“票”就指的是token。token作为一种凭证，server端必须承认持有token的用户拥有token用户的对应权利，此时持有token的用户就是一个“bearer”，应当作为“受款人”获取server的“付款”，即可以访问token所允许的资源。所以由此来看，所谓Bearer token，其实就是token。毕竟token本身就是令牌的意思，表示持有者拥有了权限，可以以此享受权利，获取资源。（当然也不排除Token在其他场景具备其他使命，毕竟互联网使用场景千千万）当然，我还是觉得Bearer token这种“Bearer”多余了，造成不必要的理解负担。
```
由於在前後端分離的情況下，勢必前端客戶端所在的網域和後端伺服器所在的網域是不同的，而Cookie

1. 原本JWT給予使用者方式是讓使用者將JWT存在自己cookie，下次再使用相同伺服器的認證服務時，就會讓伺服器去讀取cookie中的JWT是否和伺服器存下的JWT等同一致，若是一致就驗證成功並給予權利使用資源，若不一致就不給予權利使用資源
2. 但將JWT純粹放在Cookie會受到客戶端Cookie不能夠跨域使用的問題而受限 - 原本token是存放在客戶端cookie中的網域Y類別或者IPY類別，但只要相同的伺服器變更網域X或者IPX，客戶端就只能按照對應網域X或者IPX來尋找cookie內的資料，但對應資源是空的，故此得重新認證
3. 承繼第二個問題，為了很好避免掉重新認證的問題，所以就有人提出OAuth2.0來解決並讓瀏覽器、程式模組、伺服器能夠支援-而這方法就是讓瀏覽器或者程式模組能在發送HTTP請求前將JWT存放在HTTP請求下的Header Authorization區塊下，格式會是如下，同時間伺服器也必須能夠識別出HTTP請求中的Header Authorization區塊是有夾雜著JWT
```
Authorization: Bearer <token>
```

4. 原文：
```
经过一系列加密和签名算法之后，JWT变成了这样的结构的一个具有有效期的字符串（是否有效服务端会根据expires判断）Base64URL(header).base64UrlEncode(payload).HMACSHA256(Base64URL(header).base64UrlEncode(payload), secret )这个字符串。你可以把它放在Cookie里面自动发送，但是这样不能跨域，所以更好的做法是放在HTTP请求的头信息Authorization字段里面。Authorization: Bearer <token>
```
5. 參考資料：
[What's the difference between JWTs and Bearer Token?](https://stackoverflow.com/questions/40375508/whats-the-difference-between-jwts-and-bearer-token)
[https://www.zhihu.com/question/305585277](https://www.zhihu.com/question/305585277)

