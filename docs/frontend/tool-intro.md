---
sidebar_position: 3
---



# vue - 工具介紹


## vue-cli
1. vue-cli 是 vue 框架用來快速建立/管理vue專案環境的指令集
2. 存放在node_modules/vue-cli專案下
3. 執行語法為如下：
  - create：建立一個開發環境專案(自動安裝所需套件、設定基本lint配置、git設定)
```
vue <command> [options]
```

### 安裝vue指令集
1. vue-cli 是 vue 框架用來快速建立/管理vue專案環境的指令集
2. 安裝方式：
  - 方式1：區域安裝
  - 方式2：全域安裝
  ```
  // way 1
  npm install @vue/cli

  // way 2
  npm install -g @vue/cli
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


## vscode: vetur 套件
1. vetur 針對 vue 框架提供 syntax highlighting、snippet、Emmet功能:
  - syntax highlighting：以不同顏色和不同字體來凸顯vue框架語法
  - snippet：在程式開發用語中，snippet是指原始碼中較為重複的部分程式碼，通常IDE會針對這些snippet進行儲存以做管理，等到下次使用特定關鍵字或者該程式碼的一部分時，系統會根據現有儲存的snippet來補全後續程式碼  
  > Snippet is a programming term for a small region of re-usable source code
  - Emmet：以較為簡短的指令來自動轉換成完整程式碼，通常適用於HTML、CSS
  - template lint
