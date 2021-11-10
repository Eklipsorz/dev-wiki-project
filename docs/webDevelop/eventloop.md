---
sidebar_position: 1
---

# Event loop 與 在JavaScript中的它



## Event loop 簡介
說明event loop 是電腦科學名詞，它為了實現輪詢而會建立一個空間來存放....

它具有多種event loop和空間來進行

## Event loop 運作程序
說明event loop如何運作

## 在JavaScript中的它
為了進一步了解它，會先從JavaScript如何執行、如何面對任務、在Node.js又是如何


## JavaScript 簡介
主要說明JavaScript 是單執行緒和原因、瀏覽器執行它


## JavaScript 工作模式
JavaScript 單執行

### JavaScript 面對同步任務

### 那 JavaScript 如何面對異步任務
event loop 

## Event Loop 在 Node.js 上


## 補充資料

### 如何允許 JavaScript 產生多個執行緒


### Stack 

### Callback
1. 在名詞上是指回電行為，而要稱之為回電行為必須要滿足以下幾個條件：a. 限定於兩個對象之間，其中一方為對象A，另一方則為對象B.  b. 對象A呼叫對象B，而對象B呼叫對象A，若滿足條件，其中對象B呼叫對象A就即為回電。
2. 若名詞套用在電腦科學的話，會是套用在程式模組之間的呼叫，根據以上條件來描述，兩個對象都皆為程式模組，對象A擁有著用來 ”可以呼叫自己(對象A)” 來使用對象A程式模組的小程式模組X，而對象B就只是程式模組，當對象A呼叫對象B時，為了讓對象B使用自己的功能或者呼叫告訴他(對象A)我(對象B)已經完成工作，可以過來檢查而提供小程式模組X給對象B呼叫對象A，而對象B當需要用上對象A的功能或者呼叫他(對象A)並告知自己(對象B)的工作狀態時，會透過小程式模組X去呼叫了對象A，請他幫忙執行某些功能或者檢查對象B的工作狀態。而對象B用上小程式模組X來呼叫對象A這行為就是回電(callback)的行為，而在這裡的小程式模組會因為協助回電而被取名為callback function，也可以被簡稱為callback
3. 簡而言之，Callback function就是給予其他程式模組呼叫自己(程式模組)的function，透過呼叫自己來調用自己才有的功能或者其他程式模組用來告知自己並說明他們的工作狀態是如何。
4. Call = call then back ，即為對象B被對象A呼叫，而對象B又呼叫對象A來返回為對象A來執行


### Polling


### 參考資料
1. [Callback](https://en.wikipedia.org/wiki/Callback_(computer_programming))
2. [Callback functions](https://docs.microsoft.com/en-us/dotnet/framework/interop/callback-functions)