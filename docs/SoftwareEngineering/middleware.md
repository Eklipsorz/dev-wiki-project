---
sidebar_position: 4
---

# Middleware


## 背景

過去為了要將多個獨立且語言形式皆不一樣的程式碼模組組合成擁有多個特定功能的單獨大型系統，往往都需要改寫這些模組(Module)的原始程式碼，並使他們合併成一個大型系統或者一個新的模組(New Module)，而這樣的改寫和合併總會花上大量時間才能完成。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636099819/blog/middleware/merge2NewModule_us6gfy.png)


## Middleware 的出現

因此就有人想著，在不改寫這些程式碼模組或者保留其模組的功用的情況下，來額外開發多個程式模組來擔任其他模組間的仲介，而這些仲介正是Middleware，通常仲介們會在兩個模組之間，並且為了讓其中一個模組X能夠更好使用另一個模組Y而提供偏易於模組X使用的函式，而這些函式會封裝模組Y的功能，這時模組X只需要呼叫仲介所提供的函式來呼叫，就能獲得模組X所能提供的對應功能。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636113062/blog/middleware/simpleMiddleware_saw0df.png)


同樣地，甚至可以將仲介擺放於使用者和模組們之間，為了讓使用者(User)能夠更好地使用模組們所組合出來的功能而提供偏易於使用者的函式或者使用方法，而這些函式和使用方法會封裝這些模組所要能夠組合出來的功能，如下圖的Function1至Function N"，每個Function會對應一個模組集合和仲介集合，這些集合當中會有幾個模組來從分配到仲介集合中挑出仲介來相互合作，進而提供對應的Function所要有的功能，所以當使用者對這個仲介呼叫或者使用其中一個Function時，便會調用對應的模組集合和仲介集合，由他們組合出對應的Function功能並回傳給仲介，然後再由仲介回傳功能或者結果給使用者，透過仲介軟體的幫助，可以不必改寫每個獨立的程式模組就獲得合併後的系統會有的功能。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636114216/blog/middleware/user_set_middleware_jv0iut.png)




## Middleware 簡介
從前面兩小節可以得知以下小節：
1. Middleware 是額外建立的程式模組
2. Middleware 本身會為了易於對象A方便使用而封裝對象B的功能的軟體或者程式碼模組
3. 當對象A想使用對象B的功能，只需透過Middleware即可間接調用對象B的對應功能
4. 對象A並不限於人類和電腦程式，對象B肯定限於電腦程式，只是這兩者若要直接進行互動，肯定要花更多時間去讓彼此改變以此更能辨識彼此以及相互使用。

同時在這裡會額外封裝所要做到的事情

### 封裝所要做到的事情
Middleware 的建立會基於封裝來呼叫對應功能，而封裝在這裡是指以對象A能夠辨識的概念、語法、功能來將對象B的概念、語法、功能包起來用，包裝形式會如同下列函式來包裝並放在Middleware給予對象A來呼叫，前面的objectAFunction是只有對象A能夠辨識的語法，而parameter1就是對象A傳過來的參數，而objectBFunction則是只有對象B能夠辨識的語法或者說對象B原本就提供的函式，只是對象B的函式對於parameter的形式可能會是對象A無法執行提供的形式，這時會透過Middleware本身的功能將對象A傳過來的parameter1轉變為對象所能辨識的形式。

```
function objectAFunction(parameter1) {
  parameter2 = convert(parameter1)
  call objectBFunction(parameter2) 
}
```

透過這樣子的包裝，對象A只需要呼叫objectAFunction(parameter1)就能讓Middleware去呼叫對象B去執行objectBFunction函式，然後再由那個函式回傳結果給Middleware，但objectBFunction回傳的something_A不一定會是對象A能夠辨識的形式，因此又得像參數那樣進行轉換，來讓形式轉換成對象A能夠辨識的。

```
function objectBFunction(parameter2) {
  // do something
  return something_A
}

```

然而實際封裝情況會比以上說明的還要更複雜，在這裡只是大概論述一些重點，從這些重點當中，Middleware 要能完整實現封裝得必須做以下事情：
1. Middleware 必須很了解對象A和B這兩者的輸入輸出格式和所要的資源種類為何
2. Middleware 必須要將對象A傳過來的參數轉變為對象B能夠辨識的形式
3. Middleware 必須要將對象B回傳過來的的結果轉變為對象A能夠辨識的形式


## 名詞解釋
1. Middleware： middle代表著中間的，ware是從 software 轉變過來的，組合在一起就是中間的軟體或者承擔仲介的軟體，而中間和仲介則是對於兩個對象之間的，也就是分別為兩個對象之間的軟體或者兩個對象之間的仲介，而這個仲介會提供易於某一個對象使用的方法，而這些方法會封裝另一個對象所能做的事情。

2. Middleware 也有中(仲)介層、介面的稱呼，而這個稱呼相對於被封裝的對象，比如對象A是被Middleware封裝的對象，那麼就可以稱對象A的介面、仲介層就是Middleware，想要使用對象A的功能，只需要使用介面、仲介曾即可間接調用。

## 參考資料
1. [What is middleware exactly?](https://stackoverflow.com/questions/2904854/what-is-middleware-exactly)