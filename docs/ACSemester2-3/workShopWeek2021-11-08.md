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

## 資料若不集中存放一個模組
1. 主要有幾種情況：這些情況會使得資料會變得難以管理，而且很容易因其他模組X、畫面在單方面(就其他模組X和畫面對於資料)的變更而改變，特別是放到DOM上的資料
  - 資料分散在其他模組、畫面
  - 資料會分散在其他模組
  - 資料會集中在畫面



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
2. 這種模式下的Data flow會從Model -> template/template logic -> 畫面 來進行，換言之，由資料模組的資料來決定/變動畫面。
3. 其中template是指模板網頁內容，而template logic定義將資料模組下的動態資料與template合併時所需要處理的判斷或者邏輯/顯示內容時需要處理的判斷或程式邏輯，如if、for
4. Model主要負責請求API原始資料、並將原始資料轉換成整個專案架構很好存取的資料結構並存入專案中，畫面渲染則根據Model所存下的資料來渲染畫面，最後事件綁定與處理是指事件發生後所需要做的資料變更和畫面重新渲染。



## Model
1. 思考重點：
  - 原始資料長什麼樣子？
  - 最後要放進畫面 template 的資料有哪些、是什麼資料型別？ (可以從前端計算產生出來的？還是只能從後端獲取？)
  - 資料與另一種資料的對應關係是什麼(如商品資料以及購物車資料，從中列出共同會有屬性以及足以代表那商品的屬性名稱)
  - 因此，資料要如何整理和轉換，才能更容易的存取並放進template進行渲染
  - 另外從後端獲取的原始資料本身，不應該從前端那邊進行更改，應該由後端以及PM進行修正，所以要保持原始資料的只讀不更改特性且前端有可能沒權限能夠更改原始資料，在這情況下，前端進行修改會變得毫無意義。
2. 多個資料表盡量去掉重複，並抓一個可以辨別且獨立的屬性值當作key值來分割出原始資料和延伸性的資料，以避免多個資料表擁有多個不能辨識且重複的屬性以及同步問題

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



## innerHTML 本身
1. innerHTML 是以指派HTML語法形式的htmlString來操作DOM，一開始它會將htmlString解析成DOM，然後安插至對應的元素節點
```
element.innerHTML = htmlString;
```
2. innerHTML比起document來建立元素，還更容易維護更好讓人讀取。

## Controller 事件處理
0. 基於Model -> View 這個one-way data flow
1. 根據重畫方式，有兩種策略：
  - 僅資料修改的地方需要重畫，沒資料修改的地方就不用重畫，但需要有個機制能夠偵測資料變動以及要能夠找到跟該資料有關的UI進行渲染：優點能將渲染功能專注在需要修改的地方、節省效能，缺點是由於只根據變更的資料來變更相對應UI，很容易因為少更新跟資料有關的UI而導致資料和畫面的同步問題，比如資料某一塊是從4變到5，但畫面就只顯示4。
  - 不分修不修改，一律重畫全部，優點是渲染和資料之間不容易不同步的問題，缺點就是容易造成不必要的渲染，比如某塊UI上只有20%的UI需要更改，但重畫就只能按照100%的比例來重畫
2. 重畫方式得必須根據專案下所採用的data flow來決定，若採用於從Model來更新UI，那麼重畫方式勢必會先更新Model，然後再重畫
3. react 是採用一律重畫的策略，而vue則是採用於僅資料修改的地方需要重畫，沒修改的地方就不重複重畫，vue會監聽資料和能找到資料對應的UI來更新，但兩者都偏易於實現單向資料流。



## 剩餘可訂購數功能在資料上如何定義
1. 資料定義一個可由product-id當索引值的物件tempProductRemainingMap，然後屬性名稱當索引值，索引值是產品編號，而對應屬性值是對應產品的可訂購數量。
```
    tempProductRemainingMap = {}

    products.forEach(product => {
      productMap[product.id] = product
      tempProductRemainingMap[product.id] = product.inventory
    })
    
```
2. 額外利用庫存量來定義可訂購數量，而不是用庫存量來計算可訂購數量：
  - 後端獲取的原始資料在前端會是維持只讀不寫的特性，且前端不允許修改後端的資料庫，在這情況下，若前端去直接利用更動庫存量來渲染，但重新渲染後又會以後端原始資料來渲染，所以會違背資料同步渲染的特性
  - 若單方面修改後端獲取的原始資料，會容易使前端與後端出現資料不一致的問題
  - 可訂購數量本身與庫存量是不同功能的資料，前者是顯示目前還能訂購多少個數量，後者就只是訂購數量的上限，兩者關係為可訂購數量的預設值會是庫存量

