---
sidebar_position: 1
---

# U33-53 知識&技術


## UML
1. 是一種用作於軟體設計的語言(圖形文字表達)
2. 能將如何開發軟體(含預期結果)統一成只用具體符號、具體概念、具體行為等等事物來重新架構軟體開發這件事情，而那些具體符號、具體概念、具體行為皆是語言的一部分，換言之就是用這些語言來將軟體開發轉化成很容易看懂的架構圖
3. 目的就是讓開發者們更快了解如何開發軟體
4. 這裡的具體通常是指讓開發者更快理解的



## UML 狀態圖
1. 是UML下的圖表之一
2. 主要將軟體中某項任務/物件的執行狀況來轉換成具體易讀可控管的圖形來表達，從圖形中可看出任務的起始狀態、狀態之間的轉換、結束狀態等等。
3. 圖示如下所示，第一個圖示代表著任務/物件是開始發生第一個狀態的時期，第二個圖示則表示狀態，第三個則是狀態要結束的時期，最後一個則是代表著狀態和狀態之間的轉換

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632408263/blog/temp/component_state_diagram_qu32de.png)


4. ，通常會搭配行為(action)或者條件(condition)來表達任務/物件做了哪些行為來從一個狀態(State 1)轉換另一個狀態(State 2)或者任務或者表達任務/物件滿足了哪些條件來從一個狀態(State 1)轉換另一個狀態(State 2)

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632409434/blog/temp/connectionBetweenStates_xmxgau.png)


5. 範例：在這裡是以該實作下的controller.dispatchCardAction方法來表示玩家點擊牌時，它會有的狀態，在這裡有五種狀態，分別為等待第一張牌、等待第二張牌、配對失敗、配對成功、遊戲結束，一開始會由全黑的黑點來開始，由它來開始轉變第一個狀態-等待第一張牌，接著當使用者翻開第一張牌時，就轉變為等待第二張牌這狀態，接著使用者翻開第二張牌時，系統就會判定兩張牌是否一樣，若不一樣就轉換成配對失敗，在這個情況下，會直接因為牌沒翻完而跑回第一個狀態，而若兩張牌都一樣就轉換成配對成功，接著若牌還沒完全翻完，就跑回第一個狀態，但若牌全都翻完的話，就轉換為遊戲結束，此時再由遊戲結束進入最後的節點。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632409782/blog/temp/exampleOfStateDiagram_zhne8h.png)

6. 藉由狀態圖的設計可讓程式：
 - 容易追蹤狀態
 - 不發生預期外的狀態
 - 執行時必屬於某種狀態，可以更具體設定狀態之間的過渡(transition，從一個狀態到另一個狀態的轉變)

 7. 細節：不是每一個專案都適合狀態機或者UML下的狀態圖


## namespace
1. 是一塊某些識別字(變數名稱、函式名稱等)的可見(存取)範圍
2. 具體會以類似於區域作用域(local scope)的製作方式來將特定程式碼封裝成一塊，並以特殊名稱來稱呼被封裝的區塊，這項功能通常會賦予開發者來進行封裝。
3. 可以盡可能讓功能相近、目的性相關的變數、函式封裝在一塊來維護，並且可以避免在同一個作用域上的同名衝突問題，比如說，兩個不同專案的全域作用域都有一個類別 Foo，這樣在編譯或運行時期會造成衝突。如果每個專案將程式碼置於不同命名空間中，project1::Foo 和 project2::Foo 在專案中就可以被視為不同的 symbols 而不會發生衝突

4. 範例：在紙牌遊戲設計專案中，會盡可能按照與model、view、controller相關性最大的程式碼以物件的屬性來封裝指定物件，跟資料處理有關就放入model，跟渲染畫面有關就放入view，跟與使用者互動或者委派model和view有關就放入controller。

```
const model = {

}

const view = {

}

const controller = {

}

```

### 參考資料
1. https://tw-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/scoping.html
2. https://softwareengineering.stackexchange.com/questions/273302/what-is-the-relationship-between-scope-and-namespaces-in-python



## 枚舉