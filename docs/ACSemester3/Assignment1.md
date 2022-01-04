---
sidebar_position: 20
---

# 作業獲得的新知和技術


## Options.fn(this) 
1. handlebars註冊helper所內建的callback會有的參數之一
2. 回傳該helper所包含的內容
3. 若添加{{else}}的話，只會回傳true的內容
4. This 會是指 handlebars實體本身，該函式會直接調用包含的內容
5. 參考資料：
[What is the options parameter in a Handlebars helper function?](https://stackoverflow.com/questions/8853396/logical-operator-in-a-handlebars-js-if-conditional)
> The options object contains a fn method that works just like a compiled template... var html = options.fn(context); gives you the rendered template from inside the block

## Options.inverse(this) 
1. handlebars註冊helper所內建的callback會有的參數之一
2. 若添加{{else}}的話，只會回傳false或者else包含的內容
3. 若沒添加{{else}}的話，會是空字串
4. 參考資料：
[What is the options parameter in a Handlebars helper function?](https://handlebarsjs.com/guide/block-helpers.html#basic-blocks)
> Handlebars provides the block for the else fragment as options.inverse. You do not need to check for the existence of the else fragment: Handlebars will detect it automatically and register a "noop" function.
```
{{#if isActive}}
  <img src="star.gif" alt="Active">
{{else}}
  <img src="cry.gif" alt="Inactive">
{{/if}}


Handlebars.registerHelper("if", function(conditional, options) {
  if (conditional) {
    return options.fn(this); // 回傳<img src="star.gif" alt="Active">
  } else {
    return options.inverse(this); // 回傳<img src="cry.gif" alt="Inactive">
  }
});
```



## Moment.js
1. 第三方套件，用來轉換時間和處理時間的模組


## 邊緣情況 (Edge case)
1. 譯自英文-極端情況是僅在極端工作參數下才會發生的問題或情況