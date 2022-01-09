---
sidebar_position: 22
---

# 作業3獲得的新知和技術

## ``效果
1. ``是製作一個字串模板的特殊符號，內部肯定會有模板上的固定字元、可變動字元(模板上的凹槽形狀)，而可變動字元會是由表達式所構成，而這表達式會是JS的表達式
```
`${expression}`
```

2. 當系統遇到表達式時，會先執行表達式來得到對應內容，最後呼叫類似toString的形式來轉換成字串，當形成字串之後，就會串連其他固定字串或者其他可變動字串來合併成一個大字串輸出
```
`${(expression).toString()}
```

3. 添加``和沒添加之間的差別：  
```
console.log(expression)           // 系統會執行expression並取出回應值來印出
console.log(`${expression}`)      // 系統會執行expression並取出回應值，接著轉換成字串做最後的印出
```


## === 對於物件的處理
1. 對於非物件的資料型別，會檢查雙方(a和b)的型別是否一致，若一致就繼續檢查兩者的值是否一樣
```
a === b
```
2. 對於物件的資料型別，會檢查雙方的型別是否都為物件，若都是物件的話就透過參照位址檢查雙方是否為同一個物件


```
const obj1 = {
  property1: 123
}

let obj2 = {
  property1: 123
}

console.log(obj1 === obj2) // return false
```

```
const obj1 = {
  property1: 123
}

let obj2 = {
  property1: 123
}

obj1 = obj2

console.log(obj1 === obj2) // return true
```


## mongoose.Types.ObjectId
1. 實際透過mongoose和其schema所建立的objectId類別下之實例，跟專門定義mongoose schema下的objectId不同(如Mongoose.ObjectId或者Shema.Types.ObjectId)
2. 換言之，它是每一筆資料下的_id所擁有的原型prototype，每當透過schema建立資料時，其資料下的_id會被賦予了該原型。

3. 該原型的new 建構式會是如下，其id為實際的Object ID數值，只是用著mongoose.Types.ObjectId來包覆著

```
new ObjectId(id)
```

4. new ObjectId() and new ObjectId and ObjectId() 皆為代表著ObjectId原型下的實例
[What is the difference between new ObjectId() and new ObjectId and ObjectId()?](https://stackoverflow.com/questions/44265981/what-is-the-difference-between-new-objectid-and-new-objectid-and-objectid)

```
  // Duck-typing to support ObjectId from different npm packages
  if (id instanceof ObjectID) return id;
  if (!(this instanceof ObjectID)) return new ObjectID(id);
```
查明誰是this

## handlebars parent context
1. handlebars 會依據block helper的切分而將渲染檔案切割成parent context和child context，其中child context會是每個block helper能夠處理的內容，當這些helper執行時，往往只能夠辨識它們能理解的關鍵字(如this、root、../等)，若遇到變數名稱，會直接視為空值或者不存在。

2. parent context則是這些child context的上一層內容或者說包含block helper 的內容，其結構可以如下，首先最外層的context由於沒被任何block helper包含著，所以可以直接存取response物件所賦予的locals變數，第一個被block helper包住context A則只能夠使用該helper能夠辨識或者存取的變數，不能夠存取locals變數，context A也有部分內容被block helper包含著，該context也同樣地被限制在helper 所能夠辨識的

```
context               // parent of parent context A
{{#each test}}        
  context             // parent context A
  {{#each comments}}
    context          // child context of parent context A
  {{/each}}
{{/each}}
```

3. 由這些來區分內容的話，最外層的context會被稱之為root context或者initial context，而剩下就是某某context的child context來區分。

4. 若要在child context存取parent context所擁有的locals變數，可以使用以下方法：
  - 使用@root.指明要存取最上層的initial context所擁有的變數
  - ../ 使用parent context所能夠辨識的變數內容，../可以重複疊加，比如../../就指名要parent context的parent context。


5. 舉例