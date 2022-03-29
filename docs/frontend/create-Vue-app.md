---
sidebar_position: 9
---

# 建立application 實例 - 新知和技術

## 建立application 步驟
1. 透過Vue 物件來建立Vue application實例

```
const app = Vue.createApp({
  //..
})
```

2. 由於createApp本身並不會和特定元件進行綁定，得主動將Vue application實例與#app對應元件進行綁定，才能透過app或者component來控制對應元件的(動態/靜態)資料、畫面之間的渲染
```
app.mount('#app')
```

## 設定App instance上的template
1. App instance 中的template會是以HTML string來表示被綁定的元件上的模板DOM內容會是什麼，該內容通常能和資料合併成一個全新dom內容，語法為如下，其中string會是以HTML語法和Vue語法為主為主

```
Vue.createApp({
	template: string
})
```

> Template Another thing that we can specify inside this is the component template. That is the template that will be rendered inside the app element.


> Vue uses an HTML-based template syntax that allows you to declaratively bind the rendered DOM to the underlying component instance's data.  All Vue templates are syntactically valid HTML that can be parsed by spec-compliant browsers and HTML parsers.

## 補充知識：CDN所回傳的vue函式庫
1. 語法為如下

```
<script src="https://unpkg.com/vue@3.0.2"></script>
```
2. 會回傳一個Vue 物件，裡頭擁有該物件所擁有屬性和方法
3. 由於是使用Var 來回傳該Vue，所以會是在這個檔案中會視為全域變數來看待
4. 該物件主要會控制View (full website or part of a webpage)


## 補充知識：Vue vs. Vue.createApp()
1. 前者為用來調用Vue框架方法和屬性的物件，後者為實際透過前者方法而建立的root component或者application instance

## 補充知識：建立完Application實例會有的問題
1. createApp本身只是建立instance，所以存在一個問題：
 - Instance 是綁定於哪個網站或者網頁來當作該網頁或者網站的畫面渲染
> But how do we tell this up right here that we want it to control a certain part of the Web page?

2. 解法為使用application下的mount方法來將instance綁定在特定元件上，語法為如下：argu1是指網頁上的元件識別參數(可填入id或者class selector name) 由createApp所建構出來的app instance，並呼叫它的mount來將argu1對應的元件與app instance 進行綁定
```
app.mount(argu1)
```

> that method tells the app at what point or where in the dom to mount this application.

3. 細節：當下達mount之後，被綁定的元件將會由component 實例來控制，主要會做的事情會有：
  - 對於該元件的事件管理，由component所負責，比如當點擊該元件時，就會由component所綁定的點擊事件處理進行
  - 對於該元件的資料和畫面渲染，由component所負責



