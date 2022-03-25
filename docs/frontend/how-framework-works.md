---
sidebar_position: 2
---


# 前端框架和後端之間如何運作

在這裡假定有Frontend Server會負責提供網頁給端點客戶端。
## 純CSR路線
1. 首先先從Frontend server獲取擁有框架對應的網頁和JS，網頁內容會是空白
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1648132349/frontend/csr-init-phase_kmkeh4.png)

2. 客戶端上的JS根據使用者互動請求來索要資料變動自己所擁有的DOM節點
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1648132349/frontend/csr-progress-phase_k1gnxi.png)


## 先SSR後CSR

1. 首先先從Frontend server獲取擁有框架：server收到請求後便為了將資料與模板網頁合併成對應網頁給client，首先會先發送索取資料的請求至backend server，backend server收到便會從資料庫獲取以及做一些處理，隨後回傳處理後的資料至Frontend server，由該server負責將資料和模板網頁合併轉換成對應的網頁至client端，該網頁還夾雜著對應框架的JS和CSS
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1648132349/frontend/ssr-init-phase_kpob9u.png)

2. 客戶端上的JS根據使用者互動請求來索要資料變動自己所擁有的DOM節點
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1648132349/frontend/csr-progress-phase_k1gnxi.png)