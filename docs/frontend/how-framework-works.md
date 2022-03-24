---
sidebar_position: 2
---


# 前端框架和後端之間如何運作

## 純CSR路線

首先先從Frontend server獲取擁有框架對應的網頁和JS，網頁內容會是空白
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1648132349/frontend/csr-init-phase_kmkeh4.png)

客戶端上的JS根據使用者互動請求來索要資料變動自己所擁有的DOM節點
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1648132349/frontend/csr-progress-phase_k1gnxi.png)


## 先SSR後CSR

首先先從Frontend server獲取擁有框架，網頁內容會是從模板網頁和索取到的資料做合併並轉換成對應的網頁，並夾雜著框架的JS
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1648132349/frontend/ssr-init-phase_kpob9u.png)

客戶端上的JS根據使用者互動請求來索要資料變動自己所擁有的DOM節點
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1648132349/frontend/csr-progress-phase_k1gnxi.png)