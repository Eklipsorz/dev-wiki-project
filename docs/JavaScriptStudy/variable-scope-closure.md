---
sidebar_position: 1
---


# Variable Scope, closure



## Lexical Environment

在JavaScript中，定義某執行環境下所能夠使用的名字以及這些名字對應了哪些變數(值)、函式等等，Lexical Environment 會根據執行環境下的巢狀關係(如下)而區分成每一個函式下、由{...}所構成的空間下、整個script 檔案本身下的Lexical Environment

```
{           // A 
  {         // B (區塊A包含了B)
    {       // C (區塊B包含了C)

    }
  }
}
```

每一個Lexical environment 物件會包含兩個部分：
  - Environemnt Record：一種物件X，專門定義一個空間本身能夠使用的識別名稱以及這些名稱對應的區域變數、函式是什麼，最後以物件X的屬性來表示(有些空間會儲存this變數)
  - 一個參照於外層的Lexical Environment，而外層是相對於目前所在的Lexical Environment，拿上面的案例就是假使目前環境是以C為主，那麼外層的Lexical Environment就會是B或者A
  
###  Variables
> 若名稱對應著變數(值)，會由於變數的內容會本身會因爲複雜的邏輯和控制流程而不能夠在一開始的pre-populated 流程確定好其內容是為何，所以會在執行過程中很有可能不斷變動其對應的內容為何，進而使名稱對應的變數值無法確定。
1. 當一執行script時，會先將事先宣告好的識別字放入對應的Lexical environment再來做後續的直譯執行，在這裡假定只有variable1這識別字
```
// start                  variable1: <Uninitialized>
let variable1;
variable1 = value1;
variable1 = value2;
```
2. 在這裏由於事先將宣告好的變數variable1存放在Lexical environment，而此時的variable1會對應著Uninitialized，這代表著解析引擎已經知道這script會有variable1這識別字名稱，只是還未用const、let、var來宣告其識別字來使這識別字對應至適合的內容。

```
// start                  variable1: <Uninitialized>
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1646572672/backend/lexical%20environment/lexical2var1_nyf40x.png)
3. 使用let來宣告variable1，但沒有指派任何值給它，所以目前它對應的內容會是undefined來表示還沒指派任何內容給它。
```
let variable1;            variable1: undefined
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1646572672/backend/lexical%20environment/lexical2var2_ve6hgl.png)
4. 以value1內容來指派給variable1，接著再以value2內容指派給variable1
```
variable1 = value1;       variable1: value1
variable1 = value2;       variable1: value2
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1646572672/backend/lexical%20environment/lexical2var3_pevvl3.png)
### functions
> 名稱對應著函式，由於函式本身不像變數那樣，會因為邏輯和控制流程而變動，只會在一開始的pre-populated 流程確定(無論是否有重複名稱的函式，都依照最後一個定義或者所在區域來定義)，所以名稱對應的函式會是確定的，換言之，通過這流程，可以馬上透過名稱來找到對應函式

1. 當一執行script時，會先將事先宣告好的識別字放入對應的Lexical environment再來做後續的直譯執行，在這裡假定只有這variable1識別字和say，接著由於variable1在pre-populated流程中是確定為對應著變數，而say則是因為對應著函式而被確定是函式，當下可直接say這名稱來調用對應的函式功能
```
// start                      variable1: <uninitialized>, say: function
let variable1 = "Hello"       

function say(name) {          
  ......
}
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1646572672/backend/lexical%20environment/lexical2fun1_gjfsz8.png)
2. 正常來說，這項確定只適用函式的宣告/定義，並不會因為涉及到函式而使得名稱能夠對應確定的值，比如下列表達的賦予並不會使say這個名稱能夠確定對應值就是是函式物件本身或者該函式物件回傳內容，這是因為已經使用let來宣告say這個名稱對應到變數。
```
let say = function(name) 
/* */
let say = test(name)
```
> Naturally, this behavior only applies to Function Declarations, not Function Expressions where we assign a function to a variable, such as let say = function(name)..

### Lexical Environment 參考資料解析

> A Lexical Environment is a specification type used to define the association of Identifiers to specific variables and functions based upon the lexical nesting structure of ECMAScript code. A Lexical Environment consists of an Environment Record and a possibly null reference to an outer Lexical Environment.

1. lexical environemnt：規格種類
2. 定義一個名字與變數、名字與函式之間的對應關係，簡而言之，哪個名字是指某執行環境上的變數或者是指執行環境上的函式

> “Lexical Environment” is a specification object: it only exists “theoretically” in the language specification to describe how things work. We can’t get this object in our code and manipulate it directly. JavaScript engines also may optimize it, discard variables that are unused to save memory and perform other internal tricks, as long as the visible behavior remains as described.
1. Lexical Environemnt 是不能直接存取和操作的物件
2. 只要Lexical Environment仍保持著可見的狀態下，JavaScript引擎將會試著優化，如釋放記憶體或者做些處理。


> Lexical Environment : it's the internal js engine construct that holds identifier-variable mapping. (here identifier refers to the name of variables/functions, and variable is the reference to actual object [including function type object] or primitive value). A lexical environment also holds a reference to a parent lexical environment.

[Lexical environment and function scope](https://stackoverflow.com/questions/12599965/lexical-environment-and-function-scope)




> In computer programming, a declaration is a language construct that specifies properties of an identifier:
1. 在電腦科學中，宣告只是語言用來建立特定識別字是具有什麼樣性質，換言之，指示識別字會是什麼樣的型別，但宣告後的識別字並不會存在於硬體中


> Definition of a variable says where the variable gets stored. i.e., memory for the variable is allocated during the definition of the variable.
1. 在電腦科學中，定義是指識別字實際對應的記憶體空間為何，換言之，識別字所獲取的記憶體空間為何並且儲存什麼樣的內容至該記憶體空間。