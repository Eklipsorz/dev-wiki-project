---
sidebar_position: 1
---


# DOM 是什麼


## DOM介紹
DOM (Document(註1) Object Model)是將一份HTML文件內的標籤、文字、註解等內容以及檔案本身皆以物件形式來表示，並且依照他們彼此間的巢狀結構來將這些物件構築成一個樹狀結構，而這就是DOM，也可以稱之為DOM Tree，一開始由代表檔案本身的html標籤來作為根節點(最一開始的節點)，從根節點開始與其他節點進行連接，這些節點又與其他節點連接，進而形成樹狀結構，但實際情形的樹狀結構會比下圖中表示的樹狀結構還要複雜

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633244696/blog/dom/simpleTree_htkvmv.png)


## DOM上的節點

而樹狀的每一個節點都是物件，依據原本對應的內容，我們可以將這些節點分類成：

- 若是代表HTML檔案本身的話，會被當作是文件節點(Document Node)或者根節點(Root Node)
- 若是HTML元素或者標籤的話，會被當作是元素節點(Element Node)
- 若是代表元素在HTML檔案所擁有的屬性(註2)，會被當作是屬性節點(Attribute Node)
- 若是代表一般的文字內容(本身並無特別意義)，會被當作是文字節點(Text Node)
- 若是代表HTML檔案上的註解內容，會被當作是註解節點(Comment Node) 

在這裡，屬性節點只是在物件導向的角度繼承了名為Node的物件所擁有的特性和方法，但實際上並不會被DOM當作節點或者子節點看待，而其他種類的節點則是可以被DOM當作正常節點或者子節點。而網頁通常會是由HTML標籤來夾雜其他內容來表達整體網頁的呈現，所以會由元素節點帶領著其他種類的節點，每一個元素節點(Tag 1)都會帶有代表(對應元件的)屬性(如id和class)的屬性節點、一些代表文字內容、註解的子節點、由元素節點構成的子節點(Tag 2)，在這裡只有屬性節點類似貼紙一樣標示著這元素節點有這什麼樣的屬性，並不會真的被DOM當作節點，而這個子節點(Tag 2)會如同每個元素節點夾帶著一些子節點組合以及屬性節點。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633109630/blog/dom/aDomNode_x6bv3g.png)


### 例子
以一個例子來說明一個元素(標籤)在DOM中會是什麼樣子，在這裡我們以p標籤為例子，並給予p標籤一些屬性值，比如class和id，另外再讓p標籤去包含一般文字、em元素、註解。
```
<p class="class1 class2" id="id1">
	<!-- TEST COMMENT -->
  This is a <em>simple</em> website.
</p>   
```

經過瀏覽器解析而轉換成DOM後，其p標籤會如同下圖那樣，標籤以及被包含的標籤會被當作元素節點(以橘紅色來標示)，原本標籤上的屬性值會是屬性節點(以藍綠色來標示)，而子節點出現順序將會以HTML檔案的讀取順序來決定，越先讀取的就放的越前面，首先我們會看到由"TEST COMMENT"所構成的註解節點(以綠色來標示)，接著就是以"This is a "所構成的文字節點，緊接著是&lt;em&gt;元素節點，最後是" website."構成的文字節點(以淺藍色來標示)，而&lt;em&gt;元素節點還會包含著"simple"所構成的文字節點(以淺藍色來標示):

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1630055616/blog/dom/aDomNodeExample_ncvwwt.png)


## 縮排會構成文字節點
當瀏覽器去解析HTML檔案時會根據讀取到的內容所具有的特徵來判定該內容是屬於什麼種類的節點，而剛好縮排(換行和空白)會被瀏覽器當作一般文字並轉化成文字節點，這情況時常發在標籤之間的縮排，比如&lt;ul&gt;和&lt;li&gt;之間存在著"\n      "，而&lt;li&gt;之間也存在同樣的縮排
```
<ul>
   <li>sugar</li>
   <li>cocoa</li>
</ul>
```

這會使DOM Tree多產生幾個存放縮排內容的文字節點(如下圖)，而這樣的節點有可能會影響著DOM節點的遍歷，且會增加渲染過程的成本。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633275050/blog/dom/additionalTextNode_vvbtiu.png)

### 解法
由於是因爲出現縮排內容才額外產生存放縮排內容的文字節點，因此只要把縮排刪除就能解決，但這樣會對易讀性有所影響，因此實務上會在正式發佈網站前，會採用一種自動化工具能將文件裡的縮排全部取消，而這個程序稱為 Minify，取消後的縮排也會使檔案縮小，另外這程序也時常在一般專案出現，比如Bootstrap的函式庫裡都會含有min檔名，那是經過 Minify 程序的檔案。

## 註解
1. Document Object Model中的Document是指HTML檔案本身
2. 在HTML語法中，除了可以用&lt;tag&gt;&lt;/tag&gt;來定義其tag對應的元素以外，還可以在括號內部增加屬性值，來進一步描述其元素在HTML會呈現的樣子，而該屬性值包含了class、id、href、src等等，這些屬性值將會在DOM架構中被當作屬性節點。

## 參考資料
1. [屬性節點只是在物件導向的角度繼承了名為Node的物件所擁有的特性和方法](https://stackoverflow.com/questions/18102854/in-dom-structure-are-attributes-child-nodes-or-siblings)
2. [HTML 屬性](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)






