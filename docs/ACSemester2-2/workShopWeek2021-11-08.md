---
sidebar_position: 4
---

# 學期2-2 第三週workshop


## HTML vs. DOM 
1. HTML：用文字和標籤描述的網頁草圖，標籤會使文字在呈現上具有特殊意義或擁有特殊功能
2. DOM： 將標籤、文字轉換成物件節點並依據草圖來構成的實體模型，而模型會是以樹狀結構
3. 當瀏覽器成功索取HTML時，會先將HTML轉換為DOM來方便渲染網頁的畫面，爾後所有對於網頁上的變更、存取、渲染皆是根據DOM進行，而非根據HTML

## HTML 和 DOM 兩者間關係
草稿和依據草稿來畫出的實體模型


## template 
1. 字義本身為用來量產的模具、模板，其模板會將固定形狀先做出來，其填料會由製造者來決定
2. 套用在程式語言上，固定形狀是以固定文字語法/內容，而開發者唯一能在模具上做變動的地方是填料，在這裡的填料是指特定語言形式的表達式或者變數
3. 例子：除了{}和其內容為填料以外，剩餘皆為模板的固定內容。
```
`
<li class="list-group-item">
  ${product.name} X ${cartItem.quantity}.  小計:${cartItemAmount} 元
</li>
`
```


## 資料流
1. 英文為data flow，別名為data binding
2. 在資料模組獨立於UI呈現的情況下，資料管理和UI呈現會是不同的程式碼模組，但問題是UI本身會依賴著資料管理模組的資料來做變動，而資料也有可能依賴UI本身變動而改變自己的資料，在這裏資料會有不同的變更/對應流向，一種是資料模組一變動就跟著渲染跟資料相關的UI，另一種則是資料相關的UI只要變動而渲染就跟著變動資料模組的資料，而資料在資料模組和UI呈現中以其中一個方向來做變更和對應的行為就叫做Data Flow/Data Flow

變更/對應方向稱之為Data Flow/Data Binding
3. 總結一下，在資料模組獨立於UI呈現的情況下，Data Flow/Data Binding是指著資料本身在資料模組和UI中以特定方向來做變更和對應的行為，而特定方向又分為兩種方向：
 - 從資料模組出發，每一次資料模組的資料只要一更新，就隨後更新UI，也就是資料模組 -> UI
 - 從UI出發，每一次UI只要一更新，就隨後更新資料模組，也就是UI -> 資料模組

4. 若 框架X 或 模組X 能輕易做出其中任一流向，代表該 框架X 或 模組X 是容易讓開發者做到one-way data flow / one-way data binding這件事情，而若框架X 或 模組X 能輕易做出兩種方向，代表該 框架X 或 模組X 是容易讓開發者做到two-way data flow / two-way data binding。

5. 通常Data Flow/Data Binding本身是行為，而不是指著具備某種功能或者性質，若要稱呼某框架或者某模組是一個one-way data bind模組/框架是不太嚴謹的句型，比較正確的描述是該框架或者某模組是很容易做到one-way data bind這行為

### 參考資料
1. [簡單聊一下 one-way data flow、two-way data binding 與前端框架](https://devs.tw/post/40)
2. [one way data flow vs. one way data binding](https://reactjs.org/docs/thinking-in-react.html)



## 由資料模型來同步畫面
1. 隸屬於one-way data flow，而資料模組是指負責管理/存取資料的程式模組
2. 這種模式下的Data flow會從Data Model -> template/template logic -> 畫面 來進行，換言之，由資料模組的資料來決定/變動畫面。
3. 其中template是指模板網頁內容，而template logic定義將資料模組下的動態資料與template合併時所需要處理的判斷或者邏輯/顯示內容時需要處理的判斷或程式邏輯，如if、for
4. Data Model主要負責請求API原始資料、並將原始資料轉換成整個專案架構很好存取的資料結構並存入專案中，畫面渲染則根據Data Model所存下的資料來渲染畫面，最後事件綁定與處理是指事件發生後所需要做的資料變更和畫面重新渲染。



## Model
1. 思考重點：
  - 原始資料長什麼樣子？
  - 最後要放進畫面 template 的資料有哪些、是什麼資料型別？ (可以從前端計算出來的？還是只能從後端獲取？)
  - 資料與另一種資料的對應關係是什麼(如商品資料以及購物車資料，從中列出共同會有屬性以及足以代表那商品的屬性名稱)
  - 因此，資料要如何整理和轉換，才能更容易的存取並放進template進行渲染
2. 多個資料表盡量去重複，並抓一個可以辨別且獨立的屬性值當作key值來分割出原始資料和延伸性的資料

## 循序存取
1. 英文為Sequential access。
2. 指的是要存取某項位置X下的內容時，必須要先經過從位置X前的連續位置，比如說位置有5個位置，分別為位置0、位置1、位置2、位置3、位置4，要存取位置3的內容，必須先經過位置0~2才能夠存取位置3的內容


## 隨機存取
1. 英文為Random Access，而Random在英文代表著隨機、任意
2. 指的是存取位置X下的內容時，不需要經過任何非位置X的位置，就能直接存取位置X


## 將陣列轉為物件
1. 形式為如下：其中轉換後的物件為productMap
```
// 將 products 陣列 轉換成 物件格式的資料，以方便存取
productMap = {}
products.forEach(product => {
  productMap[product.id] = product
})
```

2. productMap物件會擁有數個product.id的屬性，比如product-1、product-2、product-3、product-4
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1636390157/blog/week3workshop/objectResult_h2k6ob.png)

3. 好處為：
  - 可以利用物件對屬性值的索引來實現隨機存取，比如要存取第四個產品的資訊，只需要告訴系統要存取的產品編號是什麼就能直接存取，透過這樣子可以盡可能減少循序存取的效能浪費
  ```
  // first way
  productMap['product-4']
  
  // second way
  productId = 'product-4'
  productMap[productId]
  ```

## 有序集合
1. 英文為ordered set
2. 集合中的每一個元素都按照特定順序來存放，並且依照順序來分配位置/索引值給每一個元素，當要存取某種元素時，必須透過索引值才能找到
3. 典型代表為陣列
4. 可以容易實現循序存取

## 無序集合
1. 英文為unordered set
2. 集合中的每一個元素都不按照特定順序來存放，換言之，也不會有按照特定順序的索引值或者相關值來代表每一個元素。
3. 典型代表為單獨物件中的屬性和屬性值
4. 可以容易實現隨機存取

### 參考資料
1. [JavaScript 中 Object 相關知識點整理](https://www.gushiciku.cn/pl/pe6r/zh-tw)


## View 畫面渲染
1. 找出不一樣的地方和一樣的地方進行歸納，一樣的地方當作template來處理，不一樣的地方試著用template logic來處理

## innerHTML清空原因
1. 形式為如下，productListElement.innerHTML會在處理前會被清空
```
productListElement.innerHTML = ''
  
  products.forEach(product => {
    const isSoldOut = tempProductRemainingMap[product.id] === 0;
    productListElement.innerHTML += `
      <div class="col-3">
         <div class="card disabled">
            <img src=${product.imgUrl} class="card-img-top">
            <div class="card-body">
              <h5 class="card-title">${product.name} </h5>
              <p class="card-text">
                ${product.price} 元
              </p>
              <p class="card-text">
                可訂購數量 ${tempProductRemainingMap[product.id]} 份
              </p>
              <button
                id=${product.id}
                class="btn btn-primary add-to-cart-button ${isSoldOut ? 'disabled' : ''}"
               >
                ${isSoldOut ? '已售完' : '加入購物車'}
              </button>
            </div>
          </div>
        </div>
    `
  })
```

2. 這樣做的原因是為了怕過去的渲染內容會累加至這或者避免過去重複商品也同時和目前商品資料出現在這



## Controller 事件處理
1. 根據重畫方式，有兩種策略：
  - 僅修改的地方需要重畫，不變的地方就不重複重畫
  - 不分修不修改，一律重畫全部
2. 重畫方式得必須根據專案下所採用的data flow來決定，若採用於從Data Model來更新UI，那麼重畫方式勢必會先更新Data Model，然後再重畫
3. react 是採用一律重畫的策略，而vue則是採用於僅修改的地方需要重畫，不變的地方就不重複重畫，vue會監聽資料來更新，但兩者都偏易於實現單向資料流。

## 剩餘可訂購數功能在資料上如何定義
1. 資料定義一個可由product-id當索引值的物件，其物件存放剩餘庫存量，其中product-id會當這物件
```
    tempProductRemainingMap = {}

    products.forEach(product => {
      productMap[product.id] = product
      tempProductRemainingMap[product.id] = product.inventory
    })
    
```