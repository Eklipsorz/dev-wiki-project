---
sidebar_position: 3
---

# 如何啟動 vue 專案

vue-cli 是 vue 框架用來快速建立/管理專案的指令集



## 安裝vue指令集
1. vue-cli 是 vue 框架用來快速建立/管理專案的指令集
2. 安裝方式：
  - 方式1：區域安裝
  - 方式2：全域安裝
  ```
  // way 1
  npm install @vue/cli

  // way 2
  npm install -g @vue/cli
  ```

## 建構vue專案(環境)
0. vue create 為建構指令，會建立一個事先設定好的開發環境讓開發者進行開發，裡頭會自動安裝以下程式模組：
  - vue
  - vue-service
1. 若為vue-cli是全域安裝的話，其建構專案指令為如下，執行後便建構名為project-name的vue專案名稱
```
vue create <project-name>
```
2. 若為vue-cli是區域安裝的話，其建構專案指令為如下，當中npx會從node_modules尋找對應的執行檔來執行
```
npx vue create <project-name>
```



