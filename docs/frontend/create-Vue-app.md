---
sidebar_position: 5
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



## 補充知識
### CDN所回傳的vue函式庫
1. 語法為如下

```
<script src="https://unpkg.com/vue@3.0.2"></script>
```
2. 會回傳一個Vue 物件，裡頭擁有該物件所擁有屬性和方法
3. 由於是使用Var 來回傳該Vue，所以會是在這個檔案中會視為全域變數來看待
4. 該物件主要會控制View (full website or part of a webpage)


### Vue vs. Vue.createApp()
1. 前者為用來調用Vue框架方法和屬性的物件，後者為實際透過前者方法而建立的root component或者application instance

### 建立完Application實例會有的問題
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






### 為啥component 和 application 的實例要用data()
1. application 使用data()
```
new Vue({
  ...,
  data() {
    return {
      key1: value1,
      key2: value2,
      .
      .
    }
  }
})
```

2. component 使用data()
```
export default {
  data() {
    return {
      count: 0
    }
  }
}
```

3. 原因在於每一次application (prototype)和 component (prototype)一旦建立出實例時，會執行於下面的程式碼來達成，過程中會檢查事先定義好建構式下的options參數之data是否為函式，若不是函式，則會參照於同一份data，若是函式就會執行該函式並從中以函式回傳結果來當作實例所擁有的data屬性，在這會因為回傳的內容會是一份新的物件，所以每個實例會是獲取獨立的data，在這是**為了讓每一個實例都有不同的data而採取data()的做法**

```
function initData (vm: Component) {
  let data = vm.$options.data
  data = vm._data = typeof data === 'function'
    ? getData(data, vm)
    : data || {}
  if (!isPlainObject(data)) {
    data = {}
    process.env.NODE_ENV !== 'production' && warn(
      'data functions should return an object:\n' +
      'https://vuejs.org/v2/guide/components.html#data-Must-Be-a-Function',
      vm
    )
  }
```

4. 參考文獻：
- [Vue 组件 data 为什么必须是个函数，而 Vue 的根实例却没有此限制？](https://www.zhihu.com/question/384454093)
- [Why must vue component data be a function?](https://stackoverflow.com/questions/46826709/why-must-vue-component-data-be-a-function)


### 建構vue專案(環境)
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