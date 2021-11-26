---
sidebar_position: 22
---


## 物件概念


## polymorphism
1. the fact that something such as an animal or organism can exist in different forms
2. 描述同種性質的東西可以以不同形式、外形、結構來呈現/存在，比如同屬於動物的東西可以以有翅膀、有魚鰭、有爪子的形式來存在


## object copy
1. 將一個物件複製成另一個物件的方法，在這裡由於每一個物件都具有屬性和方法，若物件A被複製成物件B，那麼這兩個物件所擁有屬性名稱、對應屬性值、方法名稱、對應方法皆會一樣。
2. 但若屬性值或者對應方法本身是一個參照或者一個記憶體位址的話，那麼物件A和物件B很有可能會一起共享著同一個參照或者同一個記憶體位址，只要任何一方針對該參照對應的內容做更動，另一方就即可透過同一個參照看到更動內容，而這就是淺拷貝(Shadow Copy)
3. 若物件A被複製成物件B時，也連同替物件A的所有參照對應的物件進行複製：將整個內容複製給物件B，進而讓雙方不再共享同一個參照或者同一個記憶體位址，那麼任何一方做任何更動時不會影響另一方的內容

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1637855634/blog/SE/deep_copy_whhuov.png)

### 參考資料
1. [object copy](https://en.wikipedia.org/wiki/Object_copying#Shallow_copy)

## object oriented language
1. 物件導向語言是一種以物件(object)來當做程式開發上的基本單位，而物件上主要有以下性質：
  - 在這裡一個物件都如同世界上的每一個物件那樣，具有能夠進一步描述自己的屬性(property)以及進一步描述自己能做些什麼事情的方法(method)
  - 物件和物件存在著某種關係使其中一個物件擁有(繼承)另一個物件所擁有的屬性和方法，進而使物件們在屬性和方法很相似，比如兩個物件因為親子關係，擔任兒子的物件會繼承父母的物件所擁有的屬性和方法
  - 物件和物件不存在某種關係來使其中一個物件擁有(繼承)另一個物件所擁有的屬性和方法，而是在屬性和方法上可以統一成一個物件來分類，換言之就是以同種性質的物件但卻額外以不同形式、外型、結構來衍生出多個同性質且不同外形的物件，比如鳥和魚可以統一成一個物件

2. 對於以上性質的實現，程式語言上主要有三種實現概念分別對應它們：
  - Encapsulation：將程式碼以物件為軟體開發的基本單位來進行封裝，而每一個物件下所擁有的屬性和方法都皆封裝了對應的實作程式碼
  - Inheritance：允許物件A可以從而繼承(擁有)另一個物件B所擁有的屬性和方法，而物件A本身也可以擁有自己本身的屬性和方法
  - Polymorphism：允許多個物件整合成一個同種性質(某些屬性和方法相同的)的物件A，而物件A藉由相同性質以及附加額外屬性和方法來衍生多個物件們
3. 由於軟體開發上是以物件為基本單元來開發，但電腦是無法直接辨識物件以及不知道如何實作物件背後的屬性和方法，因此必須事先告知電腦電腦物件是什麼樣子，而根據程式語言是否為直譯或者編譯來區分成兩種主要方法：
  - class-based：僅限於編譯語言，在編譯時期是先定義每一個物件是什麼樣子、具有哪些屬性和方法、與其他物件又具有什麼關係
  - prototype-based：僅限於直譯語言，prototype 在執行期間代表物件概念X的實體物件，而這個實體正是原型機(prototype，可藉由其本身來慢慢演進成對應物件的原型物件)，而該實體可藉由執行過程中添加屬性和方法來慢慢讓這個物件成形，並且允許每個物件都擁有prototype來指定物件所屬的物件概念，從而實現定義物件，而實現Inheritance和Polymorphism則是藉由對於物件的deep copy來做。

### 參考資料
1. [prototype based vs. class based inheritance](https://stackoverflow.com/questions/816071/prototype-based-vs-class-based-inheritance)



## JavaScript 物件導向
1. JavaScript 由於本身是直譯語言，只能以ProtoType-based 風格來實現物件導向這概念，但後來為了讓更多人適應JS而在ES2015之後提供class這關鍵字，但實際上只是被封裝後的語法糖(Syntactic Sugar)，其本質仍為ProtoType-based


## prototype 
prototype 的 原意是為了實現某些概念或想法而實做出來的第一個實體物，而往後的開發或者進展將會以該實體物為主，在這裡是以實現某些物件概念的第一個實體物件，本身會是定義這個物件概念擁有的屬性和方法，任意物件都擁有著prototype這屬性，而這屬性正是指定這物件是屬於哪一種物件概念，而屬性值會是代表該物件概念的實體物件，當任意物件透過prototype去指定任意一個物件概念，就能擁有(共享)該物件概念所擁有的屬性和方法，如物件1的prototype設定為物件2的prototype，那麼這兩個物件就能擁有(共享)同一個物件概念所擁有的屬性和方法，prototype屬性的設定方法：
	- 當透過new方法時，就會按照constructor綁定的prototype來設定
	- 直接透過prototype屬性值來設定


### prototype 定義
然而實際上prototype在概念上仍屬於實體物件，本質上仍需要額外內容來定義prototype具體所擁有的屬性和方法，當有了prototype具體的定義之後，就能依據其定義來建構出代表物件概念的prototype實體，而JS具體定義prototype所擁有的屬性和方法是透過函式和函式名稱來實現，函式名稱代表著prototype的名稱，而函式本身為該prototype的constructor - 負責建立對應prototype實體物件的函式，其中key為屬性或者方法，而value可以填入原型、物件、函式等，該函式若是以constructor形式來執行，會回傳一個prototype名為prototype1的物件
```
function prototype1 () {
  this.key1 = value1
  this.key2 = value2
        .
        .
        .
}
```

### prototype 例子
假設要定義一個名為person的prototype，在這個定義中會有名字和電子郵件，那麼內容會是如下，當person被當作以constructor形式來執行時，就回傳一個prototype名為person的物件
```
function person (name, email) {
  this.name = name
  this.email = email
}
```

### prototype 相關術語
1. prototype：本身是用以實現某些物件概念的第一個實體物件，本身會是定義這個物件概念擁有的屬性和方法，實際上，該物件是第一個透過能夠建立某種物件概念的Constructor之實體物件，但本質上並不能完全是一般實體物件，此外，它代表著每一個物件所屬於的物件概念是為何。
2. Constructor.prototype: 每一個構造函式(Constructor)能夠具有的屬性之一，會指向該構造函式自己本身所具有的原型物件(prototype)，然而在語法上是可以允許一般函式是擁有prototype屬性
- 若是以一般函式(未以new來構成構造函數)的話，由於會指向構建function的函式所具有的原型物件，而那個原型物件正好是JavaScript Object本身。
```
function example(parameter1) {

	this.parameter = parameter1
}
console.log(example.prototype instanceof Object)
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1637937478/blog/prototype/normalFunctionExample_xl5yre.png)


- 若是以構造函數Manager的話，會是對應該構造函數Manager本身的原型物件，在這裡由於是設定Employee的第一個實體物件來代表，所以它在判別是否為Employee的實體物件的情況下會是true，且這也代表著Manager是基於Employee這物件概念往外延伸的另一個物件概念。

```
function Employee() {
	this.name = "";
	this.dept = "general";
}

function Manager() {
	Employee.call()
	this.reports = [];
}

Manager.prototype = new Employee;

console.log(Manager.prototype instanceof Employee)
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1637937478/blog/prototype/constructorExample_ha30k1.png)

3. Instance.\__proto__: 是任何物件的屬性之一，指向物件所屬的構造函數所擁有的原型物件(prototype)
 - 若將Instance設定為構造函式 Manager時，由於構造函式本身是物件，所以系統會根據構造 "構造函式Manager" 的構造函式來找，但該構造函式Manager本身就是函式，系統會最終找上構造 "函式" 的構造函式，而它所擁有的原型正是JavaScript Object。
```
function Employee() {
	this.name = "";
	this.dept = "general";
}

function Manager() {
	Employee.call()
	this.reports = [];
}

Manager.prototype = new Employee;

console.log(Manager.__proto__ instanceof Employee)
console.log(Manager.__proto__ instanceof Object)
```
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1637938539/blog/prototype/__proto___example_mw95l5.png)

4. Object.\[\[prototype\]\]：是ECMAScript標準下的產物，等同於Object.\__proto__，並指派Object.getPrototypeOf() 和 Object.setPrototypeOf()來存取每一個物件下所擁有的構造函數所擁有的原型，但由於大部分瀏覽器在推廣之前就已經先用\__proto__來代替它實現。

### Object.prototype 屬性移除/增加

## prototype 實作繼承方式

### prototype 子類

### prototype 實作繼承例子


## __proto__ 作用

Object.prototype 代表 Object 的原型物件，其本身是實體物件。