---
sidebar_position: 6
---

# jwt-concept


Jwt

目的為一種由JSON形式為原型的token種類，其中包含了以下三個部分
Header：說明算法、token型別，用途為方便告知對方伺服器以什麼型別token和演算法來計算signature
Payload：說明該token要包含的資料是為何，用途為由payload提供客戶端對應的資訊，並盡可能不用透過伺服器來存取資料庫
Signature：由Header、Payload、伺服器的Secret來進行雜湊而產生的簽名值，用途為驗證竄改。

JWT形式為：
base64(Header, Payload).Signature

Signature:
Hash( base64UrlEncode(header) +  base64UrlEncode(payload) + secret)
  base64UrlEncode => base64url 編碼，原形為base64編碼，因為signature很有可能會是以url形式的一部分來傳遞給對方，而base64編碼下會出現讓url編碼誤會的字元，如=、+、/，而base64url是基於這問題來解決，其本身仍是base64編碼，但會注意編碼是否有url編碼誤會的字元，若有的話，就轉換，若沒有的話就不轉換。

https://stackoverflow.com/questions/56711129/why-do-you-use-base64-url-encoding-with-json-web-tokens

其中header和payload是由base64編碼所組成，這編碼由於可以反解，所以等同於明文，所以不建議將敏感性資訊丟進header和payload


特點：
由於是以signature以及secret來認證的，所以在多個後端伺服器的情況下，只要每個伺服器共享著同樣的secret，那麼任何一個能在伺服器認證成功的使用者所獲取到的token，都能在其他伺服器的認證系統下認證成功，而且會是同一個使用者


缺點：
1. 只要有人拿到token，就能變成那個人，即使那個token本就不屬於他的。
2. 由於JWT本身沒有過期的控管，容易會讓token的時效性是永遠


面向第二個缺點的解法為：
1. 賦予token的過期時間，並且新增另一個token-refresh token，用途為用來申請產生新的token，並給予使用者兩個token，第一個為正式使用的token，時效性較為短期，第二個token為refresh token，時效性較為長期，




```
app.post('/login', (req, res) => {
  // Authenticate User

  const username = req.body.username
  const user = { name: username }

  const accessToken = generateAccessToken(user)
  const refreshToken = jwt.sign(user, process.env.REFRESH_TOKEN_SECRET)
  refreshTokens.push(refreshToken)
  res.json({ accessToken: accessToken, refreshToken: refreshToken })
})

function generateAccessToken(user) {
  return jwt.sign(user, process.env.ACCESS_TOKEN_SECRET, { expiresIn: '15s' })
}
```


jwt.sign(payload, secret, option)

Option 為指定header的部分，且是設定如何產生jwt和屬性，其中可以透過expriesIn來設定過期時間

expiresIn: expressed in seconds or a string describing a time span zeit/ms.
Eg: 60, "2 days", "10h", "7d". A numeric value is interpreted as a seconds count. If you use a string be sure you provide the time units (days, hours, etc), otherwise milliseconds unit is used by default ("120" is equal to "120ms").

就能讓每個後端伺服器都能驗證成功

由於原本將需要驗證的結果是給伺服器來儲存的，每一個對應的token都必須要有正確結果才能驗證，但JWT本身包含著不可解的簽名值，因此每個拿到token的客戶端都擁有簽名值，
致使伺服器端不需要額外儲存的成本，如session，


https://5xruby.tw/posts/what-is-jwt

jwt.verify(token, secretOrPublicKey, [options, callback])
(Asynchronous) If a callback is supplied, function acts asynchronously. The callback is called with the decoded payload if the signature is valid and optional expiration, audience, or issuer are valid. If not, it will be called with the error.
(Synchronous) If a callback is not supplied, function acts synchronously. Returns the payload decoded if the signature is valid and optional expiration, audience, or issuer are valid. If not, it will throw the error.
token is the JsonWebToken string


Refresh token：
難點在於1. 會需要伺服器建立一個空間來存放每個已獲取token的使用者對應的refresh token，來驗證其refresh token
若一般token有效時間過長，會使驗證refresh token 頻率變低；有效時間過短，會使驗證refresh token 頻率變高


// 驗證refresh token並產生新的token
app.post('/token', (req, res) => {
  const refreshToken = req.body.token
	// 預防沒有refreshtoken
  if (refreshToken == null) return res.sendStatus(401)
	// 預防不是真正的refresh token
  if (!refreshTokens.includes(refreshToken)) return res.sendStatus(403)
  jwt.verify(refreshToken, process.env.REFRESH_TOKEN_SECRET, (err, user) => {
    if (err) return res.sendStatus(403)
    const accessToken = generateAccessToken({ name: user.name })
    res.json({ accessToken: accessToken })
  })
})

// 產生refresh token
app.post('/login', (req, res) => {
  // Authenticate User

  const username = req.body.username
  const user = { name: username }

  const accessToken = generateAccessToken(user)
  const refreshToken = jwt.sign(user, process.env.REFRESH_TOKEN_SECRET)
  refreshTokens.push(refreshToken)
  res.json({ accessToken: accessToken, refreshToken: refreshToken })
})



伺服器驗證一個 jwt 的過程也很簡單：
1. 收到 jwt
2. 將 header 和 payload 用金鑰和對應的演算法簽名
3. 判斷生成的簽名和 jwt 第三部分是否一致
4. 不一致則返回錯誤，一致則表示 payload 內的資料可信



# 實作 token 登入機制
安裝JWT套件，並且透過本地端驗證(通過帳密驗證)而獲取的使用者資料來產生對JWT至客戶端

## 安裝套件
安裝負責產生JWT以及支援用JWT做驗證的passport套件，套件有：
  - 產生JWT的工具 (jsonwebtoken)
  - 支援JWT做驗證的passport套件 (passport-jwt)
```
npm install jsonwebtoken passport-jwt
```



```
const jwt = require('jsonwebtoken')
const userController = {
  signIn: (req, res, next) => {
    try {
      // 簽發 JWT，效期為 30 天
      const token = jwt.sign(req.user, process.env.JWT_SECRET, { expiresIn: '30d' }) 
      res.json({
        status: 'success',
        data: {
          token,
          user: req.user
        }
      })
    } catch (err) {
      next(err)
    }
  }
}
module.exports = userController

```


疑問：
1. Refresh token 可以放進表格、redis、資料庫? 專門存已拿到token的使用者資訊
2. Redis 能否設定refresh token過期時間?
3. Refresh token 過期的話？得重新索求token和refresh token


https://cloud.tencent.com/developer/article/1193280

參考資料
https://ithelp.ithome.com.tw/articles/10199102
https://github.com/WebDevSimplified/JWT-Authentication/blob/master/authServer.js
https://medium.com/@solomonrock13/nodejs-authentication-using-jwt-and-refresh-token-and-sequelize-and-postgres-from-scratch-2031bd9a815e
https://cloud.tencent.com/developer/article/1193280



