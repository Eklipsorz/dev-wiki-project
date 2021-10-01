---
sidebar_position: 1
---


# DOM 是什麼

DOM (Document(註1) Object Model)是將一份HTML文件內的標籤、文字、註解等內容以及檔案本身皆以物件形式來表示，並且依照他們彼此間的巢狀結構來將這些物件構築成一個樹狀結構，而這就是DOM，也可以稱之為DOM Tree，樹狀結構會由代表檔案本身的html標籤來作為根節點(最一開始的節點)，而樹狀的每一個節點都是物件，依據原本對應的內容，我們可以將這些節點分類成：

- 若是代表HTML檔案本身的話，會被當作是文件節點(Document Node)或者根節點(Root Node)
- 若是HTML元素或者標籤的話，會被當作是元素節點(Element Node)
- 若是代表元素在HTML檔案所擁有的屬性(註2)，會被當作是屬性節點(Attribute Node)
- 若是代表一般的文字內容(本身並無特別意義)，會被當作是文字節點(Text Node)
- 若是代表HTML檔案上的註解內容，會被當作是註解節點(Comment Node) 

在這裡，屬性節點只是在物件導向的角度繼承了名為Node的物件所擁有的特性和方法，但實際上並不會被DOM當作節點或者子節點看待，而其他種類的節點則是可以被DOM當作正常節點或者子節點。而網頁通常會是由HTML標籤來夾雜其他內容來表達整體網頁的呈現，所以會由元素節點帶領著其他種類的節點，每一個元素節點(Tag 1)都會帶有代表(對應元件的)屬性(如id和class)的屬性節點、一些代表文字內容、註解的子節點、由元素節點構成的子節點(Tag 2)，在這裡只有屬性節點類似貼紙一樣標示著這元素節點有這什麼樣的屬性，並不會真的被DOM當作節點，而這個子節點(Tag 2)會如同每個元素節點夾帶著一些子節點組合以及屬性節點。

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1633109630/blog/dom/aDomNode_x6bv3g.png)







attribute vs. property
https://javascript.info/dom-attributes-and-properties

屬性節點只是在物件導向的角度繼承了名為Node的物件所擁有的特性和方法，但實際上並不會被DOM當作節點或者子節點看待：
https://stackoverflow.com/questions/18102854/in-dom-structure-are-attributes-child-nodes-or-siblings
