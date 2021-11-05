---
sidebar_position: 4
---

# Middleware


## 背景

過去想要將多個獨立且語言形式皆不一樣的程式碼模組組合成一個大型系統，往往都需要改寫這些模組(Module)的原始程式碼，並使他們合併成一個大型系統或者一個新的模組(New Module)，而這樣的改寫和合併總會花上大量時間才能完成。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636099819/blog/middleware/merge2NewModule_us6gfy.png)


## Middleware 的出現

因此就有人想著，在不改寫這些程式碼模組或者保留其模組的功用的情況下，來針對每個模組的輸入輸出規格和所使用的資源是什麼來額外建立一個程式碼模組在不同模組之間擔任介面，透過這個介面讓不同程式碼模組能夠相互合作組合成新的功能或者形成合併後會有的功能，將這些模組和多個介面組合在一起就能構成一個新的模組或者系統，而這個系統通常本身會是Middleware，而Middleware本身存在這多個功能給予使用者使用，而這些功能會對應著一個模組集合以及介面集合，當使用者去使用著透過Middleware去使用某個功能時，該Middleware就會去呼叫對應的模組們來透過介面進行互動來給予對應的功能，並將功能對應結果回應給Middleware，再由Middleware回應結果給使用者。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636100923/blog/middleware/middleware2NewModule_n8h1jk.png)





這些模負責模組溝通的模組組的原始程式碼，就


就針對每個模組的輸入輸出規格和所使用的資源是什麼來額外建立一個程式碼模組在不同模組之間擔任介面或者，




每一個套件都能透過介面去調用自己難以調用的另一個套件功能來使用，






接著利用這個構成介面的程式碼套件概念，來建立一個大型系統，而這個大型系統本身也是一個介面，而使用者只需要透過這個介面執行想要的功能，就會去呼叫多個未修改的對應套件，且這些對應套件又會透過額外的介面進行溝通。





在這裏介面主要會實現在兩個模組之間(如下圖)，介面會封裝其中一個模組(如下圖模組2)，
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636100309/blog/middleware/middleware_jzkfy1.png)

而封裝形式會以介面自身專屬概念、語法、功能來將模組2封裝成能讓模組1直接使用的形式，通常會是對應模組1的函式或者API，形式會是如同以下，但實際上會是比較複雜的封裝形式，而
```
function applicationLayerFunction(parameter) {
  function systemLayerFunction(parameter) {

  } 
}
```








## Middleware 簡介
具體來說，Middleware 是一種

來使用，通常會在兩個套件之間建立這樣子的介面，而其中介面會封裝其中一個套件(下圖的套件2)的功能，而介面使用會偏易於另一個套件(下圖的套件1)，使另一個套件(下圖的套件1)方便從介面去使用套件(下圖的套件2)的功能。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636100309/blog/middleware/middleware_jzkfy1.png)

接著透過

多個產品並讓他們合併成一個大型系統
保持產品的原樣，針對每個產品的輸入輸出標準在產品之間建立一個另一個軟體產品

1. 中間層，通常是指層級關係中的兩個層級之間的層級，在電腦科學裡頭，是指系統軟體和應用軟體之間的層級或者軟體。
2. 這個層級會以自身專屬概念、語法、功能來將系統軟體層級封裝成能讓應用軟體直接使用的形式，通常會是對應系統軟體功能的函式或者API，形式會是如同以下，但實際上會是比較複雜的封裝形式，
```
function applicationLayerFunction(parameter) {
  function systemLayerFunction(parameter) {

  } 
}
```

而應用軟體只需要使用中間層所提供的形式就能讓應用軟體使用對應形式的系統軟體功能，也就是說中間層是一個讓應用軟體層使用系統軟體功能的介面。


## 參考資料
1. [What is middleware exactly?](https://stackoverflow.com/questions/2904854/what-is-middleware-exactly)