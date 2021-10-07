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
1. 使用者在瀏覽器輸入以下www.youtube.com來向對應URL的伺服器發出請求
2. 伺服器這邊會開始藉由解析URL來解析請求，
3. 根據請求來將客戶端所需要的資源、檔案(HTML、CSS、JS)傳遞至客戶端
4. 客戶端收到資源和檔案後便開始解析、渲染
5. 渲染成網頁的樣子

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633621069/blog/network/request_response_cycle/cycleExample_m8rrl3.png)



### Case2: 在同一個網站內切換頁面
若假設你想要向www.example.com對應的伺服器索取/fruit/1網頁，其完整的請求或者URL會是如下：
```
http://wwww.example.com/fruit/1
```

你輸入完整的URL並送出請求，接著伺服器(wwww.example.com)收到後便會解析整個URL的樣子，過程中它會發現你的請求是存取它底下的/fruit/1的請求，這時他會去做一系列檢查，比如：
  - 檢查有沒有對應這請求的controller或者對應的動作，若沒有就立即返回404或者其他錯誤訊息給客戶端(瀏覽器)，若有的話，變回傳對應的資源和資料給瀏覽器做渲染
  - 檢查你有沒有權限來麻煩伺服器處理這請求，若沒有的話就回傳錯誤訊息給客戶端(瀏覽器)，若有的話，變回傳對應的資源和資料給瀏覽器做渲染

## 使用chrome來看cycle

直接打開chrome開發者工具下的Network面板，並在維持打開那面板的情況下重新刷新當前頁面，就能看到cycle的實際狀況

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633622612/blog/network/request_response_cycle/devNetwork_vtpuvw.png)