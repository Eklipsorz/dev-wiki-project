---
sidebar_position: 8
---

# View在MVC架構的詳細分工

1. view 在 MVC 架構中負責呈現使用者介面以及透過介面來收集資料
2. 由於view本身的實現會藉由controller來獲取model所得到的動態資料來和自己內部所存下的模板網頁(template)一起合併，至於如何合併就有兩個小課題：
 - template：定義靜態網頁和靜態網頁佈局
 - logic：定義將動態資料與template合併成指定畫面時所需要處理的判斷或者邏輯/顯示內容時需要處理的判斷或程式邏輯