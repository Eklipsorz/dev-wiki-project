---
sidebar_position: 4
---

# 學期2-2 第三週workshop


## HTML vs. DOM 
1. HTML：用文字和標籤描述的網頁草圖
2. DOM： 將標籤、文字轉換成物件節點並依據草稿來構成的實體模型，而模型會是以樹狀結構


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
2. data flow 和 data binding 在基於將資料存取和管理獨立於UI的程式碼模組(被稱之為Data Model)之下， 是指著(負責管理資料存取，所有模組要存取都要經由Data Model才能存取) Data Model 和 UI 保持著一致性的行為，所有跟Data Model的UI會按照資料來變動，而所有UI會按照資料來變動。
3. Data Flow/Data binding 主要具有兩種方向：
 -  從Data Model出發，每一次Data Model只要一更新，就隨後更新UI，也就是Data Model -> UI
 -  從UI出發，每一次UI只要一更新，就隨後更新Data Model，也就是UI -> Data Model
 
4. 若 框架X 或 模組X 能輕易做出其中任一方向，代表該 框架X 或 模組X 是容易讓開發者做到one-way data flow / one-way data binding這件事情，而若框架X 或 模組X 能輕易做出兩種方向，代表該 框架X 或 模組X 是容易讓開發者做到two-way data flow / two-way data binding。

5. 通常Data Flow/Data Binding本身是指著保持一致性的行為，而不是指著具備某種功能或者性質，若要稱呼某框架或者某模組是一個one-way data bind模組/框架是不太嚴謹的句型，比較正確的描述是該框架或者某模組是很容易做到one-way data bind這行為

### 參考資料
1. [簡單聊一下 one-way data flow、two-way data binding 與前端框架](https://devs.tw/post/40)
2. [one way data flow vs. one way data binding](https://reactjs.org/docs/thinking-in-react.html)





