---
sidebar_position: 2
---

# passport.js
1. 一個提供各種登入策略(Strategies)來進行使用者驗證的第三方套件
2. 主要功用為：
  - 提供各種登入策略來進行使用者驗證
  - 將使用者資訊存放在server的session，並設定對應session位置至client的cookie
3. 驗證策略為如下：
  - 本地驗證：以伺服器所提供的帳號密碼來驗證使用者是否輸入正確
  - FB 驗證
  - Google 驗證
  - JWT 驗證
  - 諸如此類



## 補充知識：credentials
1. 英文如下，中文是指能夠證明某些事物擁有某種特性或者能力的物件，意指為身份驗證資訊
> the abilities and experience that make someone suitable for a particular job or activity, or proof of someone's abilities and experience

2. 在passport.js中，credential則是強調一位想要驗證身份的使用者所攜帶的身分驗證文件，在這裡的想要驗證身份的使用者即為客戶端，而身份驗證文件則是指客戶端的帳號密碼資訊或者其他證明它身份的資訊，如OAuth。
> Authentication mechanisms define how to encode a credential, such as a password or an assertion from an identity provider (IdP), in a request. They also specify the procedure necessary to verify that credential. If the credential is successfully verified, the request is authenticated.
[what is credential in passport.js](https://www.passportjs.org/concepts/authentication/strategies/)

3. 使用者若透過身份驗證文件X通過passport.js的第一次身份驗證，通常passport.js會於server 建立一份session來儲存對應文件X在server對應的身分證資訊，並給予session id至client 的cookie中，客戶端會根據server域名來將 對應session id 儲存在server域名相關的cookie中，使用者在隨後第二次身份驗證時會**根據域名找尋對應的cookie，給予對應的session id來給passport.js做驗證**，若passport
在電腦科學中，而在passport.js中，指的是第一次使用者用來登入的帳密資訊，該資訊只需要第一次登入時才需要請求附帶，隨後等成功登入後，伺服器會賦予session id給客戶端並請求它存在cookie，讓伺服器只需要驗證cookie內容是否為伺服器中的session id就能讓它通行



## 登入功能的認證實作
1. 登入功能在實作上會分成兩段：
 - 認證使用者：根據 session 資訊，判斷 request 是哪一個使用者，如passport.js
 - 儲存認證結果：生成session來儲存認證結果，並要求客戶端儲存對應的session-id至cookie，如express-session

## Express-Session
1. 一種第三方套件，非官方套件。
2. 可以幫開發者從伺服器中擷取cookie資訊、並從中替客戶端建立儲存對應id的cookie和對應的session在伺服器內部的套件。
3. 具體來說，當載入該模組時，客戶端和伺服器端只要處於request/response cycle的話，就會建立session來紀錄兩者的連線過程，同時間會賦予session id 給該session並讓客戶端建立cookie去儲存session id，而到時cookie只要拿著這session id發送請求至伺服器，伺服器收到便拿該id獲取對應先前的連線過程是什麼。 
4. 安裝方式：
```
npm install express-session
```
5. 載入以及設定方式：其套件的回傳內容是本身是一個middleware function，所以若要讓其他的middleware能夠使用其功能，必須先將套件以app.use來使用其midddleware function並放置在其他middleware之前，如下例子：首先先載入express-session回傳的middleware function至session變數，然後用options物件去設定該middleware function，然後後面的app.method或者其他middleware function就能因為use的特性而使用express-session功能。
```
// load express-session
const session = require('express-session')
app.use('/', seession( options ))

// other routes (other middleware functions)
app.method(path, callback)
app.method(path, callback)
.
.
```

5. 延續上個例子，當某個請求是可以對應某一個route時，在伺服器以那個route之前來處理請求，會先經過app.use所載入的session middleware來對req生成對應的屬性來代表著session和cookie的內容

```
app.use('/', seession( options ))

// other routes (other middleware functions)
app.method(path, callback)
app.method(path, callback)
.
.
```



## Express-Session Options
1. Options 是藉由物件來設定Express-Session所提供的middleware
2. 該物件常用的屬性為：
  - secret：一組字串，用來簽署每個儲存session id的cookie以及用該字串來解開每個被簽署過後的cookie是否由該伺服器所建立的cookie，比如說一個儲存session id的cookie會是如下，中間"."前面為session id，而後者則是伺服器用secret套入至加密演算法來生成的字串內容或者稱呼為簽章(signature)，換言之，cookie內容會夾雜session id和前面提到的字串內容，以簽章來辨明該session id是特定伺服器的。
  ```
  s%3ATR0HRHZCS4JQadtvhIJMrQ0lPwKPggXm.Ec668MuqLt%2Fz2FXI7EJqxq85VVXrSPljKa%2Bj7tmaxcU
  ```
  - name：字串，設定由模組建立的cookie 所擁有的名字。
  - saveUninitialized：布林值，原本當客戶端與伺服器開始進行連線時，就會開始建立session物件來紀錄兩者在連線時的狀態，且剛建立的session物件的屬性未在伺服器中被任意值來寫入，此session就會被當作未初始化的session，而若saveUninitialized被設定為true時，就便會將未初始化的session儲存在伺服器的session store，而saveUninitialized被設定為false時，就便不會將未初始化的session儲存在伺服器session store。 true是會強制儲存未初始化的session至伺服器中的session store以及替客戶端建立儲存對應session id的cookie，false是不會強制儲存。
  > When an empty session object is created and no properties are set, it is the uninitialized state. So, setting saveUninitialized to false will not save the session if it is not modified.
  
  - resave：布林值，若設為true，每一次客戶端和伺服器之間只要出現連線互動，所對應的session都會強制繼續存在session store，在請求過程中，無論對應session內容是否有被伺服器修改，也都會繼續存至session store，若為false，則不會只因為出現連線互動而強制保留對應的session至session store

3. 需要注意的點：
  - 伺服器要求客戶端儲存session id至cookie時，此cookie會被稱之為session id cookie
  

## express-session的安全解法
1. 由於客戶端的cookie很容易被篡改或者惡意修改成其他資料，讓伺服器對應不合法的資源，為了避免這樣，伺服器當發憑證給客戶端時，也就是下圖中的Step2，伺服器會用一個私鑰(private key)和session id產生一組亂碼，而這樣過程被稱之為簽名(sign)，而亂碼本身就是signature，最後會將session id和亂碼合併成一個字串給客戶端讓它做認證，另外其亂碼會存在伺服器中。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1639932196/blog/loginSystem/implementationOfCertificating_gvmhfx.png)

合併成的字串格式會是如下，以.為區隔，前半段是session id，後半段為簽名後的亂碼
```
s%3Ax6O_y8GbzQZ83IhRCNiX9Qt-VeeHAlwl.EC9XKGB72koXfNeergC8uPKSO9L19Te%2FN1yrgjYiwJE
```

2. 當客戶端從cookie中拿著session id和亂碼組成的合併字串來向同一個伺服器發出請求時，伺服器會做一系列檢查，比如：
  - 檢查亂碼是否存在session store中，若有的話，就繼續做下一步，沒有就要求對方輸入帳密
  - 其session id和伺服器的私鑰(private key)用同個方法來產生亂數，並且比對該亂數是否和cookie內的亂數一致，若一致就允許使用服務，若不一致就要求對輸入帳密
  - 正常使用服務





## passport.js 設定方式
1. 首先，passport會使用express-session所建立的session來進行使用者驗證，本身並不負責建立/管理session和定義cookie內容，所以會先載入express-session，接著設定其套件，設定完之後，再來設定passport本身套件的載入，會從middleware來著手，主要會是passport.initialize和passport.session，接著再來定義passport在這能夠使用的驗證策略(如本地驗證、FB驗證等)，然後定義著passport在express-session所建立的session上進行存放/存取，最後就是在路由器或者應用程式層級來調用passport的驗證功能，大致上分為幾個步驟：
  - 載入express-session並設定
  - 載入passport.js套件並引用passport.initalize和passport.session至express server來當middleware
  - 定義passport在這能夠使用的驗證策略
  - 定義passport在express-session所建立的session上進行的存放/存取(序列化/反序列化)
  - 在路由器或者應用程式層級來調用passport的驗證功能，並指定何種驗證策略(本地驗證、FB驗證)


2.  載入express-session並設定
```
const session = require('express-session')
app.use(session(options))
```

3. 載入passport.js套件並引用passport.initalize和passport.session至express server來當middleware，在這裡有幾個小細節：
  - passport.session() 本身負責從session中驗證跟改寫session
  - passport.session()本身是一種middleware，可以改變req物件以及藉由session id來找到對應的session 或者 deserialized user object
  - passport 本身不會建立/管理session，所以passport.session本身並不會提供這些功能，而是定義在express-session所建立的session上進行存放/存取，所以passport的載入也必須在express-session載入之後，否則無法正常使用passport.session功能
  - passport.initialize() 本身是初始化passport.js套件
```
const app = express()
app.use(session(options))

// 主要設定
app.use(passport.initialize())
app.use(passport.session())
```

4. 定義passport在這能夠使用的驗證策略，在這裏只定義本地端的驗證策略，細節如下：
  - passport.use 是 passport套件能夠用的方法之一，主要是定義這裡能夠用的本地端的驗證策略為何以及其具體內容，其方法只有一個參數，這個參數是存放代表驗證策略的物件，其驗證策略的物件正是具體策略內容
  - 若要在某個專案下使用本地端驗證，必須是passport.use(new LocalStrategy(..))，其LocalStrategy是本地端驗證策略的類別之一，也是其類別的建構式，具體建構式內參數要求填入一個函式物件來當做callback，而函式物件是定義該策略的內容

  ```
  // define passport-local strategy
  passport.use(new LocalStrategy(
    function(username, password, done) {
      User.findOne({ username: username }, function (err, user) {
        if (err) { return done(err); }
        if (!user) { return done(null, false); }
        if (!user.verifyPassword(password)) { return done(null, false); }
        return done(null, user);
      });
    }
  ));
  ```

5. 定義passport在express-session所建立的session上進行存放/存取，主要會透過名為serialize和deserialize技術來將完整使用者實例轉換為以數字來代或者透過數字來找到對應的完整使用者實例，其中以下方法會透以下來定義serialize和deserialize這兩件事，而每個方法都有callback這參數來定義，這callback都會搭配著兩個參數
```
passport.serializeUser(callback)
passport.deserializeUser(callback)
```
serialize的callback所擁有的參數為兩個，分別為完整使用者實例和done方法，而deserialize的callback所擁有的參數也為兩個，分別為對應實例的id和done方法，而這些由於涉及驗證過程，所以都有done來進行，而done本身是passport內建函式物件。而passport本身有提供額外的sample來給予我們使用，在這裏的serialize會使用該user的id來代表user實例，而deserialize會使用id和資料庫查詢來找到對應的user實例，當驗證成功時，會執行serialize來將user實例轉換為id並存於session，而當要驗證使用者給予的cookie對應的user-id時，會使用deserialize來試圖從資料庫找到是否有對應的資料。
```
passport.serializeUser(function(user, done) {
  done(null, user.id)
})

passport.deserializeUser(function(id, done) {
  User.findById(id, function(err, user) {
    done(err, user)
  })
})
```
6. 在路由器或者應用程式層級來調用passport的驗證功能，並指定何種驗證策略(本地驗證、FB驗證)，passport.js提供一個middleware(如下)來讓路由器和應用層級來調用該驗證功能，比如POST /users/login，authenticate的type是指定驗證種類(若為'local'，就指定本地端驗證)，options則是指定驗證設定，而callback則是passport認證後會執行的函式，其參數會是(err, user, info)，而這些參數會是由done函式回傳的引數。
```
passport.authenticate(type, options, callback)
passport.authenticate(type, callback)

// callback:
// (err, user, info) => {
//  do something
// }
```


若要在應用層級添加該驗證功能，如POST /users/login的話，會是如下，authenticatec會是用'local'來指定本地端驗證，而options則是用failureRedirect: '/login'來指定當驗證失敗時，就轉回登入頁面，而最後的callback則是描述著驗證成功就轉回首頁
```
app.POST('/usrs/login', passport.authenticate('local', { failureRedirect: '/login'}), 
  function(req, res) {
    res.redirect('/')
  }
)
```

## LocalStrategy 建構式簡介
1. 該建構式具體參數為兩個，分別函式物件-callback和parameters物件，callback為該函式物件且是主要定義本地端驗證策略的內容，
```
new LocalStrategy(callback)
new LocalStrategy(parameters, callback)
```
parameters物件是指定passport.js要把哪些屬性視為帳號和密碼(credential)，預設情況下是會以username和password這兩個屬性(attribute)名稱來驗證帳密，假若要驗證其他屬性名稱且這些屬性接代表著帳密的話，可以設定parameters物件來改寫，如下所示，usernameField是指定哪個屬性為帳號/使用者名稱，而passwordField則是指定哪個屬性為密碼。
```
{
    usernameField: attribute1,
    passwordField: attribute2,
}
```
舉例，若屬性名稱為email才是帳號，而passwd才是密碼的話，其建構式會是如下：
```
new LocalStrategy({
  usernameField: 'email',
  passwordField: 'passwd'
}, callback)
```

2. 在這裡的callback是指定三種參數的函式，其中三種參數分別為：
  - username：第一個參數，代表著使用者名稱
  - password：第二個參數，代表著使用者名稱對應的密碼
  - done：第三個參數，本身為callback，是用來通知passport.js目前驗證結果並給予對應的處理，該callback有三個參數，第一個參數是代表著錯誤訊息的物件，第二個參數則是成功符合某條件(比如找到對應資料和密碼正確)時所要傳入req物件的內容，第三個參數為附加資訊來說明驗證結果是如何。
3. LocalStrategy 的 callback 應用於資料庫的查找，利用callback的username參數來從資料庫找看看有沒有對應的使用者資訊，過程中可能會遇到四種狀況：
  - 資料庫讀取失敗/資料庫執行失敗：當遇到這情況時，由於資料庫執行失敗後會有錯誤訊息的物件err，此時將它傳入至done使它變成done(err) 來告知passport.js情況
  - 找不到指定的使用者資訊：當遇到這情況時，由於資料庫是可以正常運行的，但就是找不到對應的使用者資料，所以沒錯誤訊息的物件和沒對應使用者資料，而done的呼叫會是done(null, false)，其中false事告知passport.js找不到對應的使用者資料
  - 找到使用者資訊，但密碼是錯誤的：當遇到這情況時，由於資料庫是可以正常運行的且能夠找到對應的使用者資料，只是輸入密碼是錯的，因此done的呼叫會是done(null, false)
  - 找到使用者資訊，密碼是對的：在這情況下，資料庫是可以正常運行，也找到使用者資訊，密碼也是對的，為了更一步更新session的內容會將對應資訊傳入至done裡，而done的呼叫會是done(null, user)




## Serialize 和 Deserialize
1. Serialize 英文為如下，主要是將某個事物以序列化(分配數字來代表某個事物)來轉化或者排列，在passport.js中會是將完整使用者實體轉換為數字來代表其實體，並以這個數字存放在session中

>  to arrange or publish in serial form

2. Deserialize則是相反於Serialize，主要是將序列後的產物轉換為原本序列化前的型態，在passport.js中會是將數字轉換為完整使用者實體。

