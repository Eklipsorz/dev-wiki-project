---
sidebar_position: 3
---



# vue - 工具介紹


## vue-cli
1. vue-cli 是 vue 框架用來快速建立/管理vue專案環境的指令集
2. 存放在node_modules/vue-cli專案下
3. 執行語法為如下：
  - create：建立一個開發環境專案
```
vue <command> [options]
```

## vue-cli-service
1. vue-cli-service 是 vue 框架用來測試原始碼/產生部署用的指令集
2. 存放在node_modules/vue-cli-service專案下
3. 執行語法為如下：
  - serve: 啟動開發用的網頁伺服器來將目前程式碼轉換成對應的網頁檔案和JS，當開發者一變動程式碼，就會跟著重新轉換。
  > start development server
  - build: 將目前程式碼轉換成對應的網頁檔案和JS
  > build for production
```
vue-cli-service <command> [options]
```