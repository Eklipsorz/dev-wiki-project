---
sidebar_position: 1
---

# 設定eslint


安裝eslint
```
npm install eslint
```


## 設定eslint內容


```
  parserOptions: {
    // 讓未被eslint支援的語法強制被eslint檢查並校正。
    parser: '@babel/eslint-parser',
    // 指定ecma 標準的版本
    ecmaVersion: '2021'
  }
}
```