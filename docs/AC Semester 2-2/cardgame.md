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



## 枚舉(Enumeration)
1. 從一些元素中替每一個元素命名特殊名字，在這裡會是以類似陣列的形式來產生多個元素，並替這些元素命名，命名的過程就叫語義化，其結果是讓這些元素在程式中具有特殊意義，其形式會類似於：建立一個存放7個數字的集合，數字是從0至6，在這裡，每一個數字都代表著一週的星期幾，比如6代表著星期六。
```
enum DAY            /* Defines an enumeration type    */
{
    saturday = 6,       /* Names day and declares a       */
    sunday = 0,     /* variable named workday with    */
    monday = 1,         /* that type                      */
    tuesday = 2,
    wednesday = 3,      /* wednesday is associated with 3 */
    thursday = 4,
    friday = 5
} workday;
```
2. 應用於實作以狀態圖為架構的設計，比如利用枚舉來實作每種狀態
3. 大部分程式語言會有枚舉語法，但JavaScript沒有，只能透過物件來建立，比如說紙牌遊戲中的五種狀態，每一種狀態都用具體屬性和屬性值來代表，FirstCardAwaits代表著等待第一張牌，SecondCardAwaits代表著第二張牌，CardsMatchFailed代表著兩張牌配對失敗，CardsMatched代表著兩張牌配對成功，GameFinished代表遊戲結束。

```
const GAME_STATE = {
  FirstCardAwaits: "FirstCardAwaits",
  SecondCardAwaits: "SecondCardAwaits",
  CardsMatchFailed: "CardsMatchFailed",
  CardsMatched: "CardsMatched",
  GameFinished: "GameFinished",
}
```

### 參考資料
1. https://segmentfault.com/a/1190000024456410



## setTimeout 的呼叫者和this
1. setTimeout的原型是window.setTimeout
2. 它是由瀏覽器額外提供給其他程式語言的API，主要是用作計時器並於指定時間內去做某件事情，這件事情會以函式來代替
3. 形式為以下，function是時間到了要執行的程式碼內容，milliseconds是指定時間，單位是毫秒(ms)，若要1秒的話，必須填入1000

```
setTimeout(function, milliseconds)
```

```
window.setTimeout(function, milliseconds)
```

4. 由於本身是由瀏覽器額外提供的，所以每當JS呼叫它時，便會麻煩瀏覽器內部的執行緒去執行setTimeout的工作，當然由於setTimeout呼叫者會是window(瀏覽器)，所以在setTimeout內部的函式會因為呼叫者間接呼叫而讓內部函式的this變數變成window物件。


## 模組(Module)
1. 將特定程式碼(含變數、函式)以達到一個特定功能的形式封裝成一個介面，這個介面可以讓開發者直接調用，而不用顧慮內部的實作方式
2. 實現方式會根據程式語言的不同而有所不同，有些是使用package語法、有些使用檔案來獨立、甚至也有些包裝成函式或者物件中


## 模組化(Modularization)
1. 是將整體程式碼依據功能的不同而分割成獨立的模組，然後再由這些模組相互運作來達到最終的目標
2. 模組化可以帶來幾種好處：
 - 程式開發維護上會更好地被管理
 - 測試時可以依據獨立性功能來測試模組，而模組也代表著不同的獨立功能
 - 關注點分離（開發者對於程式的考量點、關注的地方)，比如像MVC，將關注點分成介面、資料、控制器這三塊)
 - 可以產生低耦合(模組之間的依賴性很低)的模組，這讓模組可以容易被替換


### 參考資料
https://www.ithome.com.tw/voice/88488

