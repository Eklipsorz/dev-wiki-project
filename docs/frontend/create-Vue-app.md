---
sidebar_position: 9
---

<script src="https://unpkg.com/vue@3.0.2"></script>
1. 會回傳一個Vue 物件，裡頭擁有該物件所擁有屬性和方法
2. 由於是使用Var 來回傳該Vue，所以會是在這個檔案中會視為全域變數來看待
3. 該物件主要會控制View (full website or part of a webpage)
4. 


Vue vs. Vue.createApp()
前者為用來調用Vue框架方法和屬性的物件，後者為實際透過前者方法而建立的root component或者application instance

createApp本身只是建立instance，所以存在一個問題：
 - Instance 是綁定於哪個網站或者網頁來當作該網頁或者網站的畫面渲染
But how do we tell this up right here that we want it to control a certain part of the Web page?

解法為： argu1是指網頁上的元件識別參數(可填入id或者class selector name) 由createApp所建構出來的app instance，並呼叫它的mount來將argu1對應的元件與app instance 進行綁定
 - app.mount(argu1)
that method tells the app at what point or where in the dom to mount this application.


當下達mount之後，被綁定的元件將會由component 實例來控制，主要會做的事情會有：
1. 對於該元件的事件管理，由component所負責，比如當點擊該元件時，就會由component所綁定的點擊事件處理進行
2. 對於該元件的資料和畫面渲染，由component所負責

But what does this mean.

事件處理
Well it means that any kind of dynamic data that we want to output inside this app element or any events or interactivity which occur inside it like click events or mouse move events, all of that will now be controlled by our View app.


For example, if the user clicked on something inside this element inside here, like a button or something else, then we could react to that inside our View app.
We could set up a function to handle that click event. Or if we have some data, like a list of book titles, we could use our View app to make that data accessible inside this element so we could output it to the browser.



資料和畫面渲染
Or if we have some data, like a list of book titles, we could use our View app to make that data accessible inside this element so we could output it to the browser.



App instance 中的template會是以HTML string來表示被綁定的元件上的模板DOM內容會是什麼，該內容通常能和資料合併成一個全新dom內容

Vue.createApp({
	template: string
})

Template 
Another thing that we can specify inside this is the component template.
That is the template that will be rendered inside the app element.


Vue uses an HTML-based template syntax that allows you to declaratively bind the rendered DOM to the underlying component instance's data.  All Vue templates are syntactically valid HTML that can be parsed by spec-compliant browsers and HTML parsers.