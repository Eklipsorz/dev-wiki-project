---
sidebar_position: 12
---

# U47-49 表格驗證(簡易版)

## 驗證
1. 目的是在伺服器正式處理資料前，先在前端提前確定使用者的輸入內容是符合能處理的格式，並減少後端進行判斷的負擔。
2. 


## novalidate 
1. 表單元件 form 擁有的屬性之一
2. 用途是關閉瀏覽器的預設自動驗證功能(包含調整樣式)，並讓瀏覽器不再阻擋沒通過驗證的表單。
```
<form novalidate>

</form>
```
3. 若沒設定novalidate的話，瀏覽器預設上會在第一時間攔截到沒通過驗證的情況並做處理。

## checkValidity()
1. DOM表單物件會有的JS方法。
2. 主要是確定表單(含表單子元件)所截取的資料是否符合預設標準，若符合就回傳true，若不符合就為false
3. 形式為：

```
const form = document.querySelector('#form')
form.checkValidity()
```

4. 預設標準為：
 - [HTML標準對於checkValidity的標準](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#the-constraint-validation-api)
 - [Validity的標準](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Constraint_validation)

## 負責驗證的偽類
1. 會根據以下標準所定下的內容來判別對應元件所截取的內容是否正確：
 - [HTML標準對於checkValidity的標準](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#the-constraint-validation-api)
 - [Validity的標準](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Constraint_validation)
2. 偽類(Pseudo-Class)主要有以下類別：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1635324990/blog/dom/validationPseudoClass_ens4le.png)

3. 舉例：會先找到selector對應的元件是什麼，當如果它是能擷取使用者資料的元件時，便會由系統根據標準來自動判定擷取到的內容是否合法，若不合法的話，就以selector:invalid指定的樣式來渲染selector對應的元件
```
selector:invalid {
  property1: value1,
        .
        .
        .
}
```


## 手刻驗證功能
1. 關閉瀏覽器對於前端的預設自動驗證功能(包含調整樣式)，並讓瀏覽器不再第一時間攔截沒通過驗證的表單，藉由novalidate來實現
```
<form novalidate>

</form>
```
2. 關閉瀏覽器對於提交事件的預設行為
```
const form = document.querySelector('#form')
form.addEventListener('submit', function onFormSubmitted(event) {
  if (!form.checkValidity()) {
    event.stopPropagation()
    event.preventDefault()
  }
})

```
