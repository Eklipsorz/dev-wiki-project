---
sidebar_position: 6
---

# CSRF concept
1. CSRF(Cross Site Request Forgery)



## CORS (Cross-Origin Resource Sharing)
1. 它是什麼：一種在維持Same Origin Policy下的跨站資料存取技術
2. 目的：在維持Same Origin Policy的情況下，進一步讓客戶端來存取不同網域上的資源，並且設定該種網域上是合法的。
3. 具體實作概念：當瀏覽器使用XMLHttpRequest/Fetch API向某個伺服器索要資源時發生跨網域連線的話，

客戶端和伺服器之間會彼此按照伺服器CORS設定來確定跨網域連線是合法的，若不合法的話，客戶端就無法向伺服器發送跨網域連線請求，若合法就

[什麼是CORS？](https://sibevin.github.io/posts/2017-06-05-101518-note-cors)
[跨來源資源共用（CORS）](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS)

## 跨網域連線
1. 當瀏覽器於一個網域A下使用XMLHttpRequest/Fetch API向另一個伺服器(所在的網域B)索要資源時，若網域A(網域、協定、埠號)與網域B(網域、協定、埠號)是一樣的，那麼其請求會是於Same Origin Policy的情況下完成

2. 若網域A(網域、協定、埠號)與網域B(網域、協定、埠號)是不一樣的，那麼瀏覽器與伺服器之間會是以跨領域連線來存取資源，如下所示：內容為瀏覽器於http://foo.example對應網頁下向http://bar.other發送GET /resources/public-data/請求，由於http://foo.example和http://bar.other兩者是不同的網域，因此會是跨領域連線
```
GET /resources/public-data/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Connection: keep-alive
Referer: http://foo.example/examples/access-control/simpleXSInvocation.html
Origin: http://foo.example
```
第 1 - 10 行是送出的標頭。第 10 行之主要 HTTP 請求標頭中的 Origin (en-US) 標頭，它標示出請求是來自 http://foo.example 網域上的內容。

3. Referer 補充：
  - HTTP 標頭欄位會填入的內容，內容是可選的，所以具體來說會看瀏覽器支不支援以及願不願意提供
  - 內容指定為從哪裡連結至目前網域，對接收方的伺服器而言，會是指請求的來源處

> Referer is the name of an optional HTTP header field that identifies the address of the web page (i.e., the URI or IRI), from which the resource has been requested

4. Origin & Host 補充：
Origin: 標示請求源自於哪個網域
Host：指定請求發送至哪個網域


## Same Origin Policy：
1. 它是什麼：一種提升存取安全的安全政策
2. 目的：為了確保客戶端只能夠在(對於伺服器來說的)合法網域上進行同個網域上的資源存取，以盡可能避免一些資安問題，如CSRF攻擊
3. 具體實作概念：限制客戶端目前所在的網域是否與獲取/存取資源的網域為一致，若一致的話就給予存取；若不一致就不給予存取
4. 程式碼實作上：部分專門獲取資源的API、函式、程式模組會針對這政策而有所限制，比如：
  - XMLHttpRequest 
  - Fetch
5. 參考資料：
  - [同源政策 (Same-origin policy)](https://developer.mozilla.org/zh-TW/docs/Web/Security/Same-origin_policy)