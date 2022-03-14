---
sidebar_position: 6
---

# CSRF concept
1. CSRF(Cross Site Request Forgery)



## CORS
1. CORS(Cross-Origin Resource Sharing)
它是什麼：一種在維持Same Origin Policy下的跨站資料存取技術
目的：為了在維持Same Origin Policy的情況下，給予其他合法

[什麼是CORS？](https://sibevin.github.io/posts/2017-06-05-101518-note-cors)
[跨來源資源共用（CORS）](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS)
## Same Origin Policy：
1. 他是什麼：一種提升存取安全的安全政策
2. 目的：為了確保客戶端只能夠在伺服器合法網域上進行資源存取，盡可能避免一些資安問題，如CSRF攻擊，但該攻擊仍有空間可以繼續攻擊
3. 概念：限制客戶端目前所在的網域是否與獲取/存取資源的網域為一致，若一致的話就給予存取；若不一致就不給予存取
4. 程式碼實作上：部分專門獲取資源的API、函式、程式模組會針對這政策而有所限制，比如：
  - XMLHttpRequest 
  - Fetch
5. 參考資料：
  - [同源政策 (Same-origin policy)](https://developer.mozilla.org/zh-TW/docs/Web/Security/Same-origin_policy)