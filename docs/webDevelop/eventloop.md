---
sidebar_position: 1
---

# Event loop



## Callback
1. 在名詞上是指回電行為，而要稱之為回電行為必須要滿足以下幾個條件：a. 限定於兩個對象之間，其中一方為對象A，另一方則為對象B.  b. 對象A呼叫對象B，而對象B呼叫對象A，若滿足條件，其中對象B呼叫對象A就即為回電。
2. 若名詞套用在電腦科學的話，會是套用在程式模組之間的呼叫，根據以上條件來描述，兩個對象都皆為程式模組，對象A擁有著用來 ”可以呼叫自己(對象A)” 來使用對象A程式模組的小程式模組X，而對象B就只是程式模組，當對象A呼叫對象B時，為了讓對象B使用自己的功能或者呼叫告訴他(對象A)我(對象B)已經完成工作，可以過來檢查而提供小程式模組X給對象B呼叫對象A，而對象B當需要用上對象A的功能或者呼叫他(對象A)並告知自己(對象B)的工作狀態時，會透過小程式模組X去呼叫了對象A，請他幫忙執行某些功能或者檢查對象B的工作狀態。而對象B用上小程式模組X來呼叫對象A這行為就是回電(callback)的行為，而在這裡的小程式模組會因為協助回電而被取名為callback function，也可以被簡稱為callback
3. 簡而言之，Callback function就是給予其他程式模組呼叫自己(程式模組)的function，透過呼叫自己來調用自己才有的功能或者其他程式模組用來告知自己並說明他們的工作狀態是如何。
4. Call = call then back ，即為對象B被對象A呼叫，而對象B又呼叫對象A來返回為對象A來執行



### 參考資料
1. [Callback](https://en.wikipedia.org/wiki/Callback_(computer_programming))
2. [Callback functions](https://docs.microsoft.com/en-us/dotnet/framework/interop/callback-functions)