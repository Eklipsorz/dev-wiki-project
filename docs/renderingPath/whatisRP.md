---
sidebar_position: 1
tags:
  - HTML
  - CSS
---


# 渲染路徑 

Rendering Path 是瀏覽器如何將網頁檔案轉化成網頁的處理路徑，其路徑包含了網路(Network)、HTML、CSS檔案轉化成兩顆獨立樹狀結構、兩顆樹狀結構合併成渲染樹(Render Tree)、版面配置(Layout)、繪製(Paint)，每個路徑之間關係會如同下圖所示那樣，首先會先從網路找到提供網頁的伺服器獲取對應網頁(由HTML、CSS)、當客戶端的瀏覽器一拿到這些檔案，便會將他們轉化為名為 DOM Tree 和 CSSOM Tree，接著再將兩顆樹合併成渲染樹，接著根據渲染樹和DOM Tree來配置網頁的每個元件的擺放位置，最後再用瀏覽器的繪製功能來對螢幕上的pixel進行處理，比如上色之類的。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1629987931/blog/RenderingPath/Critical_Rendering_Path_ntcjvi.png)


## 網路

當使用者開始透過URL來瀏覽網頁時，瀏覽器會先試著解析URL對應的IP是誰，唯有知道IP是哪個伺服器負責提供對應的網頁服務才能進行網頁的相關處理以及向誰發送"要求網頁檔案回傳過來"的請求。

而解析URL的流程主要流程為：
- 檢查瀏覽器本身的快取(Cache)是否有URL的對應IP，若沒有則繼續朝下一個目標找
- 檢查(執行目前的瀏覽器)作業系統的快取(Cache)是否有URL的對應IP，若沒有則繼續朝下一個目標
- 檢查離本地端較近的路由器(Router)是否有URL的對應IP，沒有朝下一個目標找
- 對ISP發送請求詢問它那邊的快取是否有對應IP，沒有朝下一個目標找
- 就開始針對ISP的DNS Server進行遞迴式搜查，直到找到對應IP

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1629970812/blog/RenderingPath/howtofindIP_pku1yw.png)

不論流程會如何處理，最後結果只會有對應的IP和不存在對應IP的訊息，在這裡只探討前者，當瀏覽器已經得知對應IP是什麼，那麼使用者(瀏覽器)會再重新對該IP來要求伺服器回傳網頁的對應檔(包含了HTML檔案、CSS檔案、JavaScript檔案)給使用者的瀏覽器，而回傳檔案的形式並不會一口氣以一個完整檔案傳過去，而是以固定大小的封包(Packet)形式將原檔案切分成好幾等分傳給使用者的瀏覽器來處理。

## 


##


##




