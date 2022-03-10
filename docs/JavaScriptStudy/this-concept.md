---
sidebar_position: 3
---


# this concept

this 變數表明function call 是由哪個物件產生的，並不是函式本身或者所有函式呼叫皆由同一個物件所擁有，而是同一個函式可以由多個物件來呼叫。

> this, self, and Me are keywords used in some computer programming languages to refer to the object, class, or other entity of which the currently running code is a part. The entity referred to by these keywords thus depends on the execution context (such as which object is having its method called).


> In layman’s terms, this points to the owner of the function call, I repeat, the function call, and NOT the function itself. The same function can have different owners in different scenarios.

[Javascript ‘This’. Who Is This?](https://medium.com/swlh/javascript-this-ac28f8e0f65d)

術語：綁定 (binding)

這裡講的綁定 (binding) 是 JavaScript 裡的常見術語，意思是「指向某個物件」，在操作 DOM 事件時，我們常常會用 console.log(this) 來印出 this 的內容，查看是哪個元件觸發了事件，當你這樣做的時候，你就是在檢查 this 的綁定對象。


不同的 this 綁定方式

this 綁定方式有以下四種：
隱式綁定和顯式綁定皆為 "開發者主動設定每一個function call隸屬於哪個物件"，只是差別就在於隱式比起顯式沒那麼明顯，隱式只是透過A.B()來代表物件A呼叫了自己的方法-函式B，顯式就是明顯地指定function call是隸屬於哪個物件，除此之外，預設綁定則是由系統自己決定function call 隸屬於哪個物件，而new關鍵字綁定則是只要透過建立實例，來將相關的方法綁定在該實例上。

### 隱式綁定
1. 隱式綁定 (implicit binding)：為開發者主動設定每一個function call隸屬於哪個物件，透過以下方式來設定函式B的function call是隸屬於物件A
```
A.B()
```
2. 案例1：定義一個alphaPhoneX物件，其中有個方法叫做showPhoneInfo這函式，

```
let alphaPhoneX = {
  name: 'AlphaPhoneX',   
  price: 14999,          
  features: ['long battery life', 'AI camera'], 
  showPhoneInfo: function() {                   
     console.log(`The price of ${this.name} is $${this.price}, which has the newest features such as ${this.features.join(', ')}.`)
  }
}

```
當用該物件alphaPhoneX呼叫showPhoneInfo函式時，就會將該函式呼叫的this設定為alphaPhoneX，換言之會於該showPhoneInfo的FEC中設定this
```
alphaPhoneX.showPhoneInfo()
```

3. 案例2：定義一個alphaPhoneX物件以及在物件以外額外定義showPhoneInfo這函式，其中alphaPhoneX物件還包含著名為similarPhone的屬性，該屬性對應著另一個物件，

```
const showPhoneInfo = function () {
  console.log(`The price of ${this.name} is $${this.price}, which has the newest features such as ${this.features.join(', ')}.`)
}

let alphaPhoneX = {
  name: 'AlphaPhoneX',   
  price: 14999,          
  features: ['long battery life', 'AI camera'], 
  showPhoneInfo: showPhoneInfo,
  similarPhone: {
    name: 'AlphaPhoneY',   
    price: 18900,          
    features: ['water proof', 'high screen resolution'], 
    showPhoneInfo: showPhoneInfo,
  }
}
```
當分別用該物件alphaPhoneX呼叫showPhoneInfo函式以及alphaPhoneX下的similarPhone來呼叫showPhoneInfo時，就會依據實際呼叫showPhoneInfo的物件來決定this，當alphaPhoneX.showPhoneInfo()時，系統會額外建立showPhoneInfo()的FEC並於內部設定this為alphaPhoneX

```
alphaPhoneX.showPhoneInfo()
```
而當alphaPhoneX.similarPhone.showPhoneInfo()時，系統會額外建立showPhoneInfo()的FEC並於內部設定this為alphaPhoneX.similarPhone所指向的物件。
```
alphaPhoneX.similarPhone.showPhoneInfo()
```

### 顯式綁定
顯式綁定 (explicit binding)：與隱式綁定相比，顯示綁定是以較為明顯的方式來決定每一個function call是隸屬於哪個物件，具體有三種方法(call、apply、bind)

## binding - call method
1. 
> The call() method calls a function with a given this value and arguments provided individually.

```
function.prototype.call()
function.prototype.call(thisArg)
function.prototype.call(thisArg, arg1)
function.prototype.call(thisArg, arg1, arg2)
function.prototype.call(thisArg, arg1, ... , argN)
```

Return value

The result of calling the function with the specified this value and arguments.

## binding - apply method

> The apply() method calls a function with a given this value, and arguments provided as an array (or an array-like object).

```
function.prototype.apply(thisArg)
function.prototype.apply(thisArg, argsArray)
```
Return value

The result of calling the function with the specified this value and arguments.

## binding - bind method

> The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

```
function.prototype.bind(thisArg)
function.prototype.bind(thisArg, arg1)
function.prototype.bind(thisArg, arg1, arg2)
function.prototype.bind(thisArg, arg1, ... , argN)
```
Return value

A copy of the given function with the specified this value, and initial arguments (if provided).



預設綁定 (default binding)
new 關鍵字綁定 (new binding)