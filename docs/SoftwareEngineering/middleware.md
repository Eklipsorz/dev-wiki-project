---
sidebar_position: 4
---

# Middleware


## 背景
當想要將多個獨立且語言形式皆不一樣的程式碼套件組合成一個大型系統時，主要會有兩個方法：
1. 改寫這些套件(Module)的原始程式碼，並使他們合併成一個大型系統或者一個新的套件(New Module)
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636099819/blog/middleware/merge2NewModule_us6gfy.png)
2. 不改寫這些套件的原始程式碼，針對每個套件的輸入輸出規格、所使用的資源是什麼來額外建立一個程式碼套件在不同套件之間擔任介面，每一個套件都能透過介面去調用自己難以調用的另一個套件功能來使用，接著利用這個構成介面的程式碼套件概念，來建立一個大型系統，而這個大型系統本身也是一個介面，而使用者只需要透過這個介面執行想要的功能，就能去呼叫多個未修改的對應套件，且這些對應套件又會透過額外的介面進行溝通。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636100923/blog/middleware/middleware2NewModule_n8h1jk.png)




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