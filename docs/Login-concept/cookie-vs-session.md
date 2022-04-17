---
sidebar_position: 1
---

# cookie vs. session
由於http/https協議本身為了簡化客戶端和伺服器之間的連線結構，而定義客戶端和伺服器之間在連線時互動情況並不會紀錄下來-無狀態(stateless)，但隨著越來越多服務需要客戶端和伺服器之間的過去互動情況，就有人提出讓客戶端、伺服器根據情況來從自己的系統索要空間來**分別讓客戶端單方面紀錄與伺服器之間的互動情形、讓伺服器單方面紀錄與客戶端之間的互動情形、更或者讓兩者紀錄雙方的互動情形**。

在這裡客戶端負責單方面儲存與伺服器互動情形的空間是cookie，而伺服器負責單方面儲存與客戶端互動情況的空間是session

## cookie

當伺服器想要客戶端儲存目前互動狀態時，伺服器會發送一個擁有Set-Cookie標頭(header)的封包給客戶端，其標頭內容會是以能夠代替這次的互動狀態，其內容形式會是如下，客戶端一收到就會根據標頭內容來設置自己的cookie內容為cookie-name:cookie-value，
```
Set-Cookie: <cookie-name>=<cookie-value>; 
```

當客戶端收到之後，就會紀錄伺服器所指定的cookie內容並建立一個cookie，並於每一次向伺服器發送請求封包時，就會在請求封包內附加其cookie內容，形式會是如下
```
Cookie: <cookie-name>=<cookie-value>
```
舉一個例子：假設伺服器設定以下標頭內容的封包並傳送至客戶端，
```
Set-Cookie: food=icecream; flavor=cheese;
```
那麼客戶端收到Set-Cookie標頭封包便會建立cookie這空間來紀錄food、flavor對應的資料及其屬性，並且在每次向伺服器發送封包時，就會附加起司冰淇淋這cookie內容至封包內的cookie標頭中
```
Cookie: food=icecream; flavor=cheese;
```

## 客戶端的cookie本身設定

另外由於考量到cookie本身的安全問題、cookie內容隸屬於哪個伺服器下的哪個位置、存留客戶端多久的問題，會允許伺服器端藉由Set-Cookie標頭來設置其Cookie本身的設定，在這伺服器會以option來設定
```
Set-Cookie: <cookie-name>=<cookie-value>; <option>
```

主要option會有
1. 

### 安全設定
  - Secure: 要求客戶端只能在https加密協議下才能附加對應cookie給伺服器端，若是沒有就不附加，若有就附加cookie內容
  - HttpOnly: 要求只能透過解析http封包本身的標頭才能讀取到cookie內容，除此之外的方法皆無法被存取，也不能透過JavaScript讀取，如透過Document.cookie來讀取和變更
  > Forbids JavaScript from accessing the cookie, for example, through the Document.cookie property
  SameSite cookies Experimental

SameSite 讓伺服器要求 cookie 不應以跨站請求的方式寄送，某種程度上避免了跨站請求偽造的攻擊（CSRF）。SameSite cookies 目前仍在實驗階段，尚未被所有的瀏覽器支援。

功能：可以限制 cookie 的跨域发送，此属性可有效防止大部分 CSRF 攻击，有三个值可以设置：

    None ：同站、跨站请求都发送 cookie，但需要 Secure 属性配合一起使用。

    Set-Cookie: flavor=choco; SameSite=None; Secure

    Strict ：当前页面与跳转页面是相同站点时，发送 cookie；

    Set-Cookie: key=value; SameSite=Strict

    Lax ：与 Strict 类似，但用户从外部站点导航至URL时（例如通过链接）除外。 在新版本浏览器中，为默认选项，Same-site cookies 将会为一些跨站子请求保留，如图片加载或者 frames 的调用，但只有当用户从外部站点导航到URL时才会发送。如 link 链接


### cookie內容隸屬於哪個伺服器下的哪個位置
具體會是以Domain來指定隸屬於哪個伺服器，而Path則是指定Domain下的哪個路徑。

1. Domain：實際標記cookie內容是隸屬於與哪個伺服器下進行互動交流，並且嚴格規範每一次客戶端要附加cookie內容會根據Domain所指定來附加。

比如說伺服器設定cookie內容是與mozilla.org進行交流，那麼每一次客戶端向mozilla.org就會附加起士冰淇淋，若不是mozilla.org就不會附加
  
```
Set-Cookie: food=icecream; flavor=cheese; Domain=mozilla.org 
```

Domain 剩餘細節：
  - 若設定Domain這設定值，會同樣指定cookie內容也同樣綁定於其Domain的subdomain，比如假設mozilla.org的subdomain為developer.mozilla.org，那麼伺服器設定Domain為mozilla.org，其cookie也就同時隸屬於mozilla.org和developer.mozilla.org
  ```
  Set-Cookie: food=icecream; flavor=cheese; Domain=mozilla.org 
  ```
  - 若伺服器未設定Domain這設定值，就預設以當前伺服器的主機域名為主，但不會像前者那樣也讓subdomain也隸屬。

2. Path：指定Domain下的哪個路徑，比如假設以下內容為Set-Cookie標頭內容，那麼
```
Set-Cookie: food=icecream; flavor=cheese; Domain=mozilla.org Path=/docs
```

比如說
```
Set-Cookie
```




    Cookie 中的域名 与 当前站点域名相同，称为 第一方cookie（ first-party cookie）；

    Cookie 中的域名 与 当前站点域名不同，称为 第三方cookie（ third-party cookie）；

当前站点会使用一些其他站点资源（譬如图片、广告等），在请求第三方服务器获取这些资源时，也会返回 Set-Cookie 属性，让浏览器保留第三方的 cookie，这些cookie 主要用于用户跟踪，流量分析等。


Cookies 會帶有他們所屬的網域名。若此網域和你所在的頁面網域相同，cookies 即為第一方（first-party）cookie，不同則為第三方（third-party）cookie。第一方 cookies 只被送到設定他們的伺服器，但一個網頁可能含有存在其他網域伺服器的圖片或組件（像橫幅廣告）。透過這些第三方組件傳送的 cookies 便是第三方 cookies，經常被用於廣告和網頁上的追蹤。參見 Google 常用的 cookies 種類。大部分的瀏覽器預設允許第三方 cookies，但也有些可以阻擋他們的 add-on（例如 EFF 的 Privacy Badger）。

第一方：
若cookie的隸屬網域是與使用者目前所在的網域相同，即cookie的隸屬網域就對於目前網域而言，且由於在這裡是以伺服器為第一方且會隸屬網域又是以伺服器為主，所以故此稱為第一方 cookie，若cookie的隸屬網域是與使用者目前所在的網域不同，即為cookie的隸屬網域就對於目前網域而言，就只是外來網域的cookie內容，是第三方cookie，另外沒有第二方cookie是因為第二方本身是指client


若沒有事先告訴消費者第三方 cookies 的存在，當消費者發現你使用 cookie 時，對你的信任將會受損。因此，公開表明 cookie 的使用（像在隱私權條款中）將減低發現 cookie 時的負面影響。有些國家有關於 cookies 的法律條文。範例可以參見維基百科的 cookie statement。

參考資料
[一篇解释清楚Cookie是什么？](https://learn-anything.cn/http-cookie)
[]()