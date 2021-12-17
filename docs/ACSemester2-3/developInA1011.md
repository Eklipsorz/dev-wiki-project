---
sidebar_position: 35
---

# Development in A10 and A11


## Mongoose 沒反應的原因
1. 沒載入mongoose模組
2. 程式碼沒啟動連線
3. 沒開資料庫

## Express error middleware
1. 每一個在Express Server下的middleware必須滿足以下格式才會被系統辨識為負責處理錯誤的middleware：
其中middleware function必須是四個參數的函式物件，第一個參數會是代表著錯誤訊息的物件，第二、三則是分別代表為請求和回應的物件，第四個物件則是代表著下一個被呼叫的middleware。
```
function funct(err, req, res, next) {
  // do something
}
```

2. 