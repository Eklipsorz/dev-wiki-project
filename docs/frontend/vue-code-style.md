---
sidebar_position: 4
---

# vue API style
1. vue 主要有兩個API 風格：
  - Composition API: 又名為Function-based API，每一個application 或者component 一旦需要建立成實例就會以function setup來建立它所擁有的屬性、方法
  
  [初探 Vue 3.0 Function-based API](https://kuro.tw/posts/2019/08/06/初探-Vue-3-0-Function-based-API/)
  - Options-base API: 每一個application 或者component 一旦需要建立成實例就會以一個option object所指示的設定來建立

Composition API Style

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

Options-base

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