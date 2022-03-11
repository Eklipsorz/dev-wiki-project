---
sidebar_position: 3
---


# this concept

this 變數表明function call 是由哪個物件產生的，並不是函式本身或者所有函式呼叫皆由同一個物件所擁有，而是同一個函式可以由多個物件來呼叫。

> this, self, and Me are keywords used in some computer programming languages to refer to the object, class, or other entity of which the currently running code is a part. The entity referred to by these keywords thus depends on the execution context (such as which object is having its method called).


> In layman’s terms, this points to the owner of the function call, I repeat, the function call, and NOT the function itself. The same function can have different owners in different scenarios.

[Javascript ‘This’. Who Is This?](https://medium.com/swlh/javascript-this-ac28f8e0f65d)

## 術語：綁定 (binding)
bind原文是指某事物A和某事物B綑綁在一塊，使它們融為一體或者單方面使他們之間產生連結，比如事物A連結著事物B，來代表著事物B，而binding則是描述著綑綁、連結的行爲、過程、結果。
> to tie something tightly or to fasten something

在JavaScript中，binding是描述著連結的結果，程式在執行過程中總是會有許多的變化或者代表狀態的數值、字串、特殊資料型態，比如一個老舊的值因爲某些處理而由新的值去替代著，而永遠取得不了老舊的值而做後續的處理，為了讓程式能夠儲存每一個執行時期的狀態(數值)，JavaScript提供一個名為binding的概念：會替每個執行時期的狀態(數值)綁定一個名稱，程式可以藉由該名稱來繼續延伸使用對應時期的狀態來處理，在這裡的名稱會是binding本身，而狀態和名稱之間的綁定則是bind，名稱則是binding所會有名稱。
> How does a program keep an internal state? How does it remember things? We have seen how to produce new values from old values, but this does not change the old values, and the new value has to be immediately used or it will dissipate again. To catch and hold values, JavaScript provides a thing called a binding, or variable:


> The concept of binding in computer science has many forms. For example, when you type foo in your code, binding is the act of working out which variable/function/type/... should be used. In JavaScript this is pretty straightforward, but in some languages it can get pretty hairy (due to things like overload resolution and so forth).

### 如何實現Binding
1. 程式語言中實現綁定(binding)可能會因特性而有所不同，所以Binding本身不一定和變數宣告本身一樣。
2. 在JavaScript中，實現方法較為直接，具體來說會使用內容指派操作來實現，來將代表不同執行時期的狀態(數值)指派給變數，而該變數可以使用不同的變數名稱來識別，藉此達到名稱與數值之間的綁定
```
// 變數
let A = B
const A = B
// 常數
let A = constant
const A = constant
```
3. 參考資料：
 - [What does binding mean in Javascript?](https://stackoverflow.com/questions/49662203/what-does-binding-mean-in-javascript)
 - [Program Structure](https://eloquentjavascript.net/02_program_structure.html)
 - [Is a variable declaration the same as a variable's binding?](https://stackoverflow.com/questions/50898687/is-a-variable-declaration-the-same-as-a-variables-binding)


## this 綁定方式
1. 主要有以下四種：隱式綁定、顯式綁定、預設綁定、new關鍵字綁定
2. 隱式綁定和顯式綁定皆為 "開發者主動設定每一個function call隸屬於哪個物件"，只是差別就在於隱式比起顯式沒那麼明顯，隱式只是透過A.B()來代表物件A呼叫了自己的方法-函式B，顯式就是明顯地指定function call是隸屬於哪個物件，除此之外，預設綁定則是由系統自己決定function call 隸屬於哪個物件，而new關鍵字綁定則是只要透過建立實例，來將相關的方法綁定在該實例上。

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
1. 顯式綁定 (explicit binding)：與隱式綁定相比，顯示綁定是以較為明顯的方式來決定每一個function call是隸屬於哪個物件，具體有三種方法(call、apply、bind)

2. call()為function原型提供的方法之一，設定要函式呼叫的this為指定物件
> The call() method calls a function with a given this value and arguments provided individually.

主要語法會是如下，其中function為要呼叫的函式名稱，該函式原型擁有call方法，方法內的thisArg參數為指定為哪個物件為該函式呼叫的this，而arg1至argN，則是原本function需要用到的參數：call內的arg1會對應函式的第一個參數，arg2會對應函式的第二個參數，arg3會對應函式的第三個參數，後面以此類推。回傳值會是呼叫該function的回傳結果，其function的參數和this會藉由call所指定的thisArg和arg1~argN
```
function.prototype.call()
function.prototype.call(thisArg)
function.prototype.call(thisArg, arg1)
function.prototype.call(thisArg, arg1, arg2)
function.prototype.call(thisArg, arg1, ... , argN)
```
舉例：以下面程式為例，在這會有showPhoneInfo函式來顯示該function的呼叫者是誰以及其資訊，此外還有alphaPhoneX和alphaPhoneY這兩個物件

```
const showPhoneInfo = function () {
  console.log("'this' now refers to", this)
  console.log(`The price of ${this.name} is $${this.price}, which has the newest features such as ${this.features.join(', ')}.`)
}

let alphaPhoneX = {
  name: 'AlphaPhoneX',
  price: 14999,
  features: ['long battery life', 'AI camera'],
}

let alphaPhoneY = {
  name: 'AlphaPhoneY',
  price: 18900,
  features: ['water proof', 'high screen resolution'],
}
```
在這裡會以showPhoneInfo這函式物件來呼叫call，並以三種形式來呼叫，第一種會是空參數，在這裡會是沒指定任何物件來呼叫showPhoneInfo，相等於showPhoneInfo()，在這裡的this會是由系統來指定global 物件來代表，第二種會是alphaPhoneX物件，在這裡會是alphaPhoneX物件來呼叫showPhoneInfo，相等於alphaPhoneX.showPhoneInfo()，第三種則會是alphaPhoneY物件，在這裡會是alphaPhoneY物件來呼叫showPhoneInfo，相等於alphaPhoneY.showPhoneInfo()。

```
showPhoneInfo.call()
showPhoneInfo.call(alphaPhoneX)
showPhoneInfo.call(alphaPhoneY)
```

若將showPhoneInfo修改成擁有參數的話，
```
const showPhoneInfo = function (name) {
  console.log("'this' now refers to", this)
  console.log('name is ', name)
}
```

下方呼叫會分別轉換為showPhoneInfo()、showPhoneInfo('hiii')、alphaPhoneX.showPhoneInfo()、alphaPhoneX.showPhoneInfo('hiii')
```
showPhoneInfo.call()
showPhoneInfo.call('hiii')
showPhoneInfo.call(alphaPhoneX)
showPhoneInfo.call(alphaPhoneX, 'hiii')
```

3. apply method除了給定原本函式呼叫的參數是以陣列來給予以外，其餘(回傳值)則與call method相同
> The apply() method calls a function with a given this value, and arguments provided as an array (or an array-like object).

主要語法會是如下，其中function為要呼叫的函式名稱，該函式原型擁有apply方法，方法內的thisArg參數為指定為哪個物件為該函式呼叫的this，而argsArray，則是原本function需要用到的參數：陣列第0個元素對應第一個函式參數、第1個元素對應第二個參數、第2個元素則對應第三個參數，後面以次類推。回傳值會是呼叫該function的回傳結果，其function的參數和this會藉由call所指定的thisArg和arg1~argN
```
function.prototype.apply(thisArg)
function.prototype.apply(thisArg, argsArray)
```

舉例：以下面程式為例，在這會有showPhoneInfo函式來顯示該function的呼叫者是誰以及其資訊，該函式會以ram和storage變數來儲存參數，此外還有alphaPhoneX和alphaPhoneY這兩個物件
```
const showPhoneInfo = function (ram, storage) {
  //   console.log("'this' now refers to", this)
  console.log(`The price of ${this.name} with ${ram}GB and ${storage}GB is $${this.price}, which has the newest features such as ${this.features.join(', ')}.`)
}

let alphaPhoneX = {
  name: 'AlphaPhoneX',
  price: 14999,
  features: ['long battery life', 'AI camera'],
}

let alphaPhoneY = {
  name: 'AlphaPhoneY',
  price: 18900,
  features: ['water proof', 'high screen resolution'],
}
```

在這裡會以showPhoneInfo這函式物件來呼叫apply，並以三種形式來呼叫，第一種會是空參數，在這裡會是沒指定任何物件來呼叫showPhoneInfo，相等於showPhoneInfo()，在這裡的this會是由系統來指定global 物件來代表，第二種會是alphaPhoneX物件以及指定\[3,64\]這陣列，在這裡會是alphaPhoneX物件來呼叫showPhoneInfo，相等於alphaPhoneX.showPhoneInfo(3, 64)，第三種則會是alphaPhoneY物件，在這裡會是alphaPhoneY物件來呼叫showPhoneInfo，相等於alphaPhoneY.showPhoneInfo(6, 128)。

```
showPhoneInfo.apply()
showPhoneInfo.apply(alphaPhoneX, [3, 64])
showPhoneInfo.apply(alphaPhoneY, [6, 128])
```



4. bind method 是透過指定參數來重新建立新的相同函式物件並回傳，它並不像前面的call、apply那樣，直接呼叫call/apply就執行對應函式，而是會回傳擁有新參數值得相同函式，並且給予開發者自行呼叫的機會。

> The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

主要語法會是如下，其中function為要呼叫的函式名稱，該函式原型擁有bind方法，方法內的thisArg參數為指定為哪個物件為該函式呼叫的this，而arg1-argN，則是原本function需要用到的參數：bind內的arg1會對應函式的第一個參數，arg2會對應函式的第二個參數，arg3會對應函式的第三個參數，後面以此類推。回傳值會是添加這些參數的相同函式。

```
function.prototype.bind(thisArg)
function.prototype.bind(thisArg, arg1)
function.prototype.bind(thisArg, arg1, arg2)
function.prototype.bind(thisArg, arg1, ... , argN)
```
舉例：以下面程式為例，在這會有showPhoneInfo函式來顯示該function的呼叫者是誰以及其資訊，該函式會以ram和storage變數來儲存參數，此外還有alphaPhoneX和alphaPhoneY這兩個物件

```
const showPhoneInfo = function (ram, storage) {
  //   console.log("'this' now refers to", this)
  console.log(`The price of ${this.name} with ${ram}GB and ${storage}GB is $${this.price}, which has the newest features such as ${this.features.join(', ')}.`)
}

let alphaPhoneX = {
  name: 'AlphaPhoneX',
  price: 14999,
  features: ['long battery life', 'AI camera'],
}

let alphaPhoneY = {
  name: 'AlphaPhoneY',
  price: 18900,
  features: ['water proof', 'high screen resolution'],
}
```
在這裡會以showPhoneInfo這函式物件來呼叫bind，並以三種形式來呼叫，第一種會是以alphaPhoneX，相等於showPhoneInfo()，在這裡的this會是由系統來指定global 物件來代表，第二種會是alphaPhoneX物件以及指定\[3,64\]這陣列，在這裡會是alphaPhoneX物件來呼叫showPhoneInfo，相等於alphaPhoneX.showPhoneInfo(3, 64)，第三種則會是alphaPhoneY物件，在這裡會是alphaPhoneY物件來呼叫showPhoneInfo，相等於alphaPhoneY.showPhoneInfo(6, 128)。


```
const showPhoneInfoOfAlphaPhoneX = showPhoneInfo.bind(alphaPhoneX)
const showPhoneInfoOfAlphaPhoneY = showPhoneInfo.bind(alphaPhoneY)

// invoke the function
showPhoneInfoOfAlphaPhoneX()
showPhoneInfoOfAlphaPhoneX(3, 64)
showPhoneInfoOfAlphaPhoneY(6, 128)
```


```
const showPhoneInfoOfAlphaPhoneX = showPhoneInfo.bind(alphaPhoneX, 3, 64)
showPhoneInfoOfAlphaPhoneX()
```

預設綁定 (default binding)
new 關鍵字綁定 (new binding)