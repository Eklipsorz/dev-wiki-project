---
sidebar_position: 3
---


# Web: Request/Response Cycle
由客戶端和伺服器之間的請求回應所構成的循環

## Client & Server
當發生資料傳遞時，肯定會出現兩台實體主機，其中發送要求的主機被稱之為客戶端(client)，而回應請求並給予服務的主機被稱之為伺服器端(server)

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633596645/blog/network/ClientAndServer/simpleModel_hnh2nh.png)

## Case Study


### Case1: 請求遠端網頁
1. 當使用者在瀏覽器輸入以下www.youtube.com來向對應URL的伺服器發出請求，
2. 伺服器這邊會開始藉由解析URL來解析請求，
3. 根據請求來將客戶端所需要的資源、檔案(HTML、CSS、JS)傳遞至客戶端
4. 客戶端收到資源和檔案後便開始解析、渲染
5. 渲染成網頁的樣子

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633621069/blog/network/request_response_cycle/cycleExample_m8rrl3.png)



### Case2: 在同一個網站內切換頁面




