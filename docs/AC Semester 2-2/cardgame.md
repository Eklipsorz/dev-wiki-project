---
sidebar_position: 1
---

# U33-53 知識&技術


## UML 狀態圖
1. 是UML下的圖表之一
2. 主要將軟體中某項任務/物件的執行狀況來轉換成具體易讀可控管的圖形來表達，從圖形中可看出任務的起始狀態、狀態之間的轉換、結束狀態等等。
3. 圖示如下所示，第一個圖示代表著任務/物件是開始發生第一個狀態的時期，第二個圖示則表示狀態，第三個則是狀態要結束的時期，最後一個則是代表著狀態和狀態之間的轉換

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632408263/blog/temp/component_state_diagram_qu32de.png)


4. ，通常會搭配行為(action)或者條件(condition)來表達任務/物件做了哪些行為來從一個狀態(State 1)轉換另一個狀態(State 2)或者任務或者表達任務/物件滿足了哪些條件來從一個狀態(State 1)轉換另一個狀態(State 2)

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632409434/blog/temp/connectionBetweenStates_xmxgau.png)


5. 範例：在這裡是以該實作下的controller.dispatchCardAction方法來表示玩家點擊牌時，它會有的狀態，在這裡有五種狀態，分別為等待第一張牌、等待第二張牌、配對失敗、配對成功、遊戲結束，一開始會由全黑的黑點來開始，由它來開始轉變第一個狀態-等待第一張牌，接著當使用者翻開第一張牌時，就轉變為等待第二張牌這狀態，接著使用者翻開第二張牌時，系統就會判定兩張牌是否一樣，若不一樣就轉換成配對失敗，在這個情況下，會直接因為牌沒翻完而跑回第一個狀態，而若兩張牌都一樣就轉換成配對成功，接著若牌還沒完全翻完，就跑回第一個狀態，但若牌全都翻完的話，就轉換為遊戲結束，此時再由遊戲結束進入最後的節點。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632409782/blog/temp/exampleOfStateDiagram_zhne8h.png)