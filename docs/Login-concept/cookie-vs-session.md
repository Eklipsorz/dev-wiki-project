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




參考資料
[一篇解释清楚Cookie是什么？](https://learn-anything.cn/http-cookie)
[]()