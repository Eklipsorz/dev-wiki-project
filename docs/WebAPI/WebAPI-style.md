---
sidebar_position: 1
---

# Web API 風格
客戶端和伺服器端在網路上的溝通過程往往要考量幾個衡量點：
  - 客戶端和伺服器端平台如何傳遞/成功接收以及所需的資料形式是為何
  - 客戶端上的應用程式和伺服器上的應用程式如何傳遞/成功接收以及所需資料形式是為何
  - 客戶端和伺服器端之間的中介點如何幫忙成功轉遞以及所需的資料形式是為何
需要一個共同介面或者協議來讓彼此傳遞的內容都能轉換成彼此都能看得懂並處理得當的形式


## RPC Style API

面向於函式的API

1. 以函式呼叫的形式來調用伺服器的功能，通常API介面會是以函式名稱來命名，並不特別限制
2. API命名方式通常會是一個domain名稱(業務邏輯處在哪個領域進行)+動詞，比如GetUserInfo
3. 由於本身是以函式呼叫為主，並未對HTTP方法進行很嚴格的限制，因此隨意使用，通常會以GET、POST

stub
> stub code是占坑的代码，桩代码给出的实现是临时性的/待编辑的。它使得程序在结构上能够符合标准，又能够使程序员可以暂时不编辑这段代码。我的一位同事需要编写一个程序，要求在某一地点存储每个文件的文件名和相关信息。数据存储于一个结构表中，他决定使用散列表。这里就需要用到可调试性编码。他并不想一步登天，一次完成所有的任务。他首先让最简单的情况能够运行，就是散列函数总是返回一个0,，这个散列函数如下：int hash_filename(char *s){    return 0;}这个函数的效果就是一个散列表还未被使用。所有的元素都存储在第零个位置后面的链表中，这使得程序很容易调试，因为无需计算散列函数的具体值。



https://www.zhihu.com/question/24844900
https://zhuanlan.zhihu.com/p/56955812
https://www.itread01.com/content/1542392106.html
https://www.zhihu.com/question/28570307

## SOAP

## REST

## GraphQL



http://dockone.io/article/2434575