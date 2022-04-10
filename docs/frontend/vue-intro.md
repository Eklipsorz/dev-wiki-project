---
sidebar_position: 4
---

# vue basic intro

## vue：什麼是漸進式框架(Progressive Framework)
Vue 框架主張最小權責(Small-Scope)，將框架會有的功能主要切割成核心和其他獨立程式模組而構成的生態鏈/工具鏈，一開始開發者只需要藉由核心程式來啟用Vue所擁有的基本功能-宣告式渲染以及元件系統，並在只有基本功能的框架下進行開發，當目前核心程式並不能夠滿足開發者需求時，可以藉由其他工具鏈的獨立程式來與Vue核心程式結合成更大的框架，這也是為何叫做漸進式框架的原因：可以隨著開發者需求來壯大成更能夠滿足需求的框架。

> The Progressive Framework": it's a framework that can grow with you and adapt to your needs

## Imperative programming vs. Declarative programming
1. Imperative programming：告訴電腦程式要完成的指令步驟有哪些才能達成現開發者所要達到的目標
2. Declarative programming：告訴電腦程式所需要達成的目標是什麼，接著由電腦程式負責進行實現，而定義所需要達成的目標會是以語法來告知，如用SQL語法來告知系統開發者所想要找的資料是什麼、透過Vue語法來告知系統所要渲染畫面是什麼
3. 從這兩個名詞衍生過來的新名詞：
  - Imperative rendering：告訴電腦程式要完成的指令步驟有哪些才能完成渲染畫面的任務
  - Declarative rendering：透過程式語法來定義所需要達成的渲染畫面是什麼，接著由電腦程式負責進行渲染，定義方式為用語法告知要渲染的資料是為何以及對應資料的部分要放在哪

[声明式渲染与命令式渲染](https://zhuanlan.zhihu.com/p/342747079)



## Vue 和 MVVM 模式
1. MVVM 為 Model-View-ViewModel的縮寫，源自於2005年微軟所提出來的前端網頁開發方法或者App圖形介面開發方法之一，其中Model模組是定義App會有的狀態/資料，View模組是定義App會有的畫面/介面，而ViewModel則是負責定義Model和View這兩者之間如何彼此同步：
  - 狀態/資料一變動就跟著改動對應的畫面上元件
  - 畫面上元件一變動就跟著改動對應狀態/資料

2. ViewModel實現概念為 **單方面將特定元件與特定資料進行綁定** 和 **單方面將特定資料與特定元件進行綁定**
  - 單方面將特定元件與特定資料進行綁定：當使用者與特定元件X發生互動或者事件時，會透過DOM Listeners來根據情況來同步與特定元件X綁定的資料
  - 單方面將特定資料與特定元件進行綁定：當資料一發生變動時，會透過綁定(Data Bindings)來找到對應的元件並隨之根據資料內容來更動元件

3. Vue框架會使用MVVM作為框架來實現畫面和資料之間的同步渲染及同步更新
  ![該圖源自於 Kuro Hsu大大，之後會內化成自己的圖](https://book.vue.tw/assets/img/1-1-mvvm.22bc0dc7.png)

4. 衍生問題：
  - **MVVM的元件會是什麼或者說資料要與什麼View內容進行彼此間的綁定**，可以選擇的候選有DOM節點、HTML的標籤、前兩者任一個所構成的集合(節點集合、標籤集合)

##  Vue.js 與元件系統
1. 延續前者所衍生出來的問題，Vue為了讓這些被綁定的View內容能夠重複被使用，而將完整的網頁畫面依據是否可重複使用以及是否可獨立使用的內容來轉換成特定元件，每個元件都由模板、樣式、邏輯(HTML、CSS、JS程式碼)所構成，這些已經被轉換的元件會如同積木一般，與其他積木組合可以合併成不同的網頁畫面
  ![該圖源自於 Kuro Hsu大大，之後會內化成自己的圖](https://book.vue.tw/assets/img/1-1-mvvm.22bc0dc7.png)

2. 一般來說，網頁會由DOM作為基本單元來組合成DOM Tree，進而構成Rendering Tree來渲染瀏覽器上的畫面，而經由轉換過的元件，為了更好轉換成對應的DOM Tree，會將元件依照巢狀關係來組合成元件樹(Component tree)，而樹狀的每一個節點都皆為元件，元件可以是：
  - 由好幾個DOM節點所構成的元件
  - 包含元件的元件

4. 案例：左邊是完整網頁畫面，右邊是畫面對應的元件樹，其中最頂層是代表root component的application 實例，而接下來的子節點分別為畫面上的上面部分和下左部分、下右部分，接著由於代表上面部分並未包含其他元件，所以元件樹的第一個子節點會沒有其他子節點，而第二個子節點會因為下左部分包含著兩個元件，所以會有兩個子節點接著，最後第三個子節點會因為下右部分包含著三個元件，所以接著第三個子節點會接著那三個元件。
  ![該圖源自於 Kuro Hsu大大，之後會內化成自己的圖](https://book.vue.tw/assets/img/1-1-component-tree.b27ab51e.png)
5. 衍生問題：
  - 元件之間的狀態傳遞與管理
  - 單頁式的路由
  - 如何與其他前端建構工具的結合


### 參考資料：元件系統

1. [Differences between vue instance and vue component?](https://stackoverflow.com/questions/38318536/differences-between-vue-instance-and-vue-component)

> A root Vue instance is a Vue application launcher, Vue component is an extension of the Vue instance

> An application instance won't render anything until its .mount() method is called. It expects a "container" argument, which can either be an actual DOM element or a selector string:

2. [Components Basics](https://vuejs.org/guide/essentials/component-basics.html)



## 額外知識

### 比較：從vue2.x至vue3.x
主要更新有：
1. 新增composition API: 每個元件和應用程式皆用著setup function來建立對應實例
```
// Composition API
export default {
  setup() {
    const name = ref('John');
    
    const doIt = () => console.log(`Hello ${name.value}`);
    
    onMounted(() => {
      doIt();
    });
    
    return { name };
  },
};
```
2. Fragments 功能：vue3.x 正式支援template元素內存在多個root元素，如下所示，2.x版本只支援一個root元素，這使得得額外建立一個元素X來包含其他原本可以當root元素(如header、main、footer元素)，3.x版本則直接支援。
```
<!-- vue2.x -->
<template>
  <div>
    <header>...</header>
    <main>...</main>
    <footer>...</footer>
  </div>
</template>

<!-- vue3.x -->
<template>
  <header>...</header>
  <main v-bind="$attrs">...</main>
  <footer>...</footer>
</template>
```

3. Teleport component： 允許開發者渲染原本不在DOM 結構的元件內容，應用於modal
> Teleport is a built-in component that allows us to "teleport" a part of a component's template into a DOM node that exists outside the DOM hierarchy of that component.



### 資料流
1. 英文為data flow，別名為data binding，是指**資料模組對於UI的變更方向**
2. 在資料模組(Data Model)獨立於UI呈現的情況下，資料管理和UI呈現會是不同的程式碼模組，而原則上，
UI呈現本身會依賴著資料管理模組的資料來做變動，而資料也有可能依賴UI本身變動而改變自己的資料，在這裏資料會有不同的變更/對應流向：
  - 一種是資料模組一變動就跟著渲染跟資料相關的UI
  - 另一種則是資料相關的UI只要變動而渲染就跟著變動資料模組的資料
而資料在資料模組和UI呈現中以其中一個方向來做變更和對應的行為就叫做Data Flow/Data Flow

3. 若 框架X 或 模組X 能輕易做出其中任一流向，代表該 框架X 或 模組X 是容易讓開發者做到one-way data flow / one-way data binding這件事情，而若框架X 或 模組X 能輕易做出兩種方向，代表該 框架X 或 模組X 是容易讓開發者做到two-way data flow / two-way data binding。

### Data Model
1. 提出背景為 **由於UI或元件上的資料很容易因為該UI或元件的變動而更動資料，如刪除元件X，那麼元件X上的資料將會遺失**
2. 為了避免元件上的資料因元件本身的變動而遺失，所以會以獨立的程式模組來將元件上的資料儲存起來或者定義一個名為Data Model的程式模組來儲存所有資料，接著提供可以存取Data Model的API來給予其他想用的模組。

3. 實現方式為：
  - 定義MVC架構，其中Model會儲存所有專案下的資料以及提供資料上的業務邏輯、存取方式

4. 案例：該程式碼出自於尤川豪大大的文章，在這裏是簡單定義了一個陣列來儲存所有todo事項的Data Model，並以陣列形式來存取/使用者每個事項的內容來進行相關渲染，如renderTodoList
```
<ul id='todo-list'></ul> 
<script> 
    var todos = [ 
        {text: 'Exercise.'}, 
        {text: 'Learn JavaScript.'}, 
        {text: 'Write a blog.'}, 
    ]; 

    function renderTodoList() { 
        $('#todo-list').empty(); 

        todos.map(function(el){ 
            $('#todo-list').append(new $('<li>' + el.text + '</li>')) 
        }) 
    } 

    function clearAllTodo() { 
        todos = []; 

        renderTodoList(); 
    } 
</script>
```

### 資料流的用詞澄清
1. Data Flow 和 Data Binding 是一樣的：
  - Data Flow 是指著資料和UI之間的變更方向，如資料一變動就更改UI畫面的方向、UI畫面一變動就更改資料的方向
  - Data Binding 是指資料與什麼元件/內容進行綁定來進行變更，如綁定於某元件Y的資料X只要一變動就會變動某元件Y，而元件Y本身一變動就更改綁定於Y的資料X
  - 由於Data Flow 本身是個概念，而概念上會以 Data Binding 來實現。
2. Data Flow/Data Binding本質上是一種變動內容的行為，而不是來描述某東西是具有Data Flow/Data Binding功能或者性質：
  - (錯誤說法)：某某框架或某某程式是one-way data binding 性質的程式/框架
  - (正確說法)：某某框架或某某程式很容易做到one-way data binding


### 參考資料：資料流
1. [簡單聊一下 one-way data flow、two-way data binding 與前端框架](https://devs.tw/post/40)
2. [one way data flow vs. one way data binding](https://reactjs.org/docs/thinking-in-react.html)

### Vue API Style
1. vue 主要有兩個API 風格：
  - Composition API: 又名為Function-based API，每一個application 或者component 一旦需要建立成實例就會以setup函式來建立它所擁有的屬性、方法
  
  [初探 Vue 3.0 Function-based API](https://kuro.tw/posts/2019/08/06/初探-Vue-3-0-Function-based-API/)

  ```
  Vue.createApp({
    setup() {
      //....
      return data
    }
  })
  ```
  - Options-base API: 每一個application 或者component 一旦需要建立成實例就會以一個option object所指示的設定來建立
  ```
  // 方法1:
  const option = {...}
  Vue.createApp(option)

  // 方法2:
  Vue.createApp({
    ...
  })
  ```

2. Composition API Style vs. Options-base

```
// Composition API
export default {
  setup() {
    const name = ref('John');
    
    const doIt = () => console.log(`Hello ${name.value}`);
    
    onMounted(() => {
      doIt();
    });
    
    return { name };
  },
};
```

```
// Options API
export default {
  data() {
    return {
      name: 'John',
    };
  },
  methods: {
    doIt() {
      console.log(`Hello ${this.name}`);
    },
  },
  mounted() {
    this.doIt();
  },
};
```



參考資料：
[What is the Vue Options API?](https://stackoverflow.com/questions/68986744/what-is-the-vue-options-api)
[Vue 3 Composition API vs. Options API](https://markus.oberlehner.net/blog/vue-3-composition-api-vs-options-api/)
