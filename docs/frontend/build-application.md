---
sidebar_position: 5
---

# Vue 建立 application




## 補充資訊：為啥component 和 application 的實例要用data()
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

3. 原因在於每一次application (prototype)和 component (prototype)一旦建立出實例時，會執行於下面的程式碼來達成，過程中會檢查事先定義好建構式下的options參數之data是否為函式，若不是函式，則會參照於同一份data，若是函式就會執行該函式並獲取獨立的data，在這是**為了讓每一個實例都有不同的data而採取data()的做法**

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