---
sidebar_position: 75
---

# 作業18獲得的新知和技術

## dashboard
1. 原指車子內部提供駕駛者一些效能指標來評估的元件，效能指標為車子速度、車子距離
> the part of a car that contains some of the controls used for driving and the devices for measuring speed and distance
2. 在電腦科學裡，會是指一種以圖形化介面來呈現某些軟體或者系統上在某些部分的實際效能為何

## 補充：HTML元件上的href屬性可執行JS
1. href 屬性原指某件事件發生所會導向的目的地，通常會是指點擊事件發生生所會導向
2. href形式上可填入javascript來指示某件事件發生時所會執行的JS程式碼，形式會是如下，其中javascript:是告知系統後面內容要以javascript來執行jscode
```
href="javascript:jscode"
```
3. 舉例：a 元件與href進行綁定，在這裏每當a元件被點擊就會將頁面導向href所指示的目的地，但由於href是只是以javascript來執行指定程式碼，所以點擊a元件後都會執行history.back();來回到上一頁
```
<a class="btn btn-primary" href="javascript:history.back();"> 回到上一頁</a>
```

## 補充：model.increment(...)用法
1. 替model實例所對應的紀錄之特定欄位進行遞增或者加法運算，換言之就是欄位值會與特定值X相加並將結果修改至原欄位值所在。
> Increment the value of one or more columns. This is done in the database, which means it does not use the values currently stored on the Instance. The increment is done using a

2. 具體語法形式為如下，fields會是特定紀錄上的欄位，而options是定義增加欄位值的設定，其中options擁有
by屬性，能夠指定特定值X，預設X是1，所以新欄位值會是原欄位值加上特定值X或者原欄位值加上1
```
 increment(fields: string | Array | object, options: object): Promise<Model>
```
