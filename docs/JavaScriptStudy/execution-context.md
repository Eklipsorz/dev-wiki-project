---
sidebar_position: 2
---


# Execution context
1. Execution 是指程式執行時的背景、過程，而context是指某種事物存在/發生的背景或者情況，兩者加起來就等同於描述執行時會是什麼樣的狀態或者背景，換言之，就是目前程式的執行環境，在JavaScript中，會是引擎執行JavaScript的執行環境。
2. 在JavaScript中，當JavaScript引擎開始解析執行某個檔案上的JS程式碼時，會替該檔案建立Global Execution Context(GEC)來設定執行會需要的環境變數以及儲存過程，當建立完之後，引擎就隨後執行JS程式碼，便以該context為基礎來執行以及更新context所需的資訊至context，每一個execution context都具有兩個階段：
  - creation phase：execution context建立的階段或者程式剛開始被執行的階段
  - exection phase：程式依據著初期設定的execution context執行的時候，在這時候會邊依據邊把需要更動的資訊紀錄至context。
> When the JavaScript engine executes the JavaScript code, it creates execution contexts. Each execution context has two phases: the creation phase and the execution phase.
3. Execution Context主要共分為兩種：
  - Global Execution Context(GEC): 以程式碼全域所在的程式碼為主來構成，不包含function的內部執行、block的內部執行。
  - Function Execution Context(FEC): 當GEC呼叫function並執行function時或者執行對應block的內部執行時就會以當時所要執行的內部程式碼為範疇來建構，比如説：當GEC呼叫了test(a)時，系統就便會執行test內部的第一行，此時就會進入該函式的FEC並開始進入creation phase和execution phase
  ```
  let a = 0
  
  function test(num) {
    console.log(num)
  }

  test(a)
  ```

## GEC - creation phase
0. 這裡的概念以ECMA2015-ECMA2019為主。
1. 發生時間點：當引擎要開始執行某個檔案上的JS程式碼時，就會先建立GEC
2. GEC範圍：以檔案內的全域區塊為一個區塊(block)，不包含函式內部的執行、區塊內的另一個區塊的(Block)的內部執行
3. 製作流程：
  - 建立一個全域物件：在瀏覽器會是名為window的全域物件，在Node.js會是名為global的全域變數
  - 建立this物件並決定this參照於誰：在這裡建立完會去指向當前被建立的全域變數
  - 建立Lexical Environment：主要分為LexicalEnvironment、VariablEenvironment，這兩者會包含Environment Records、Outer reference、Thisbinding這三種主要屬性(如下)，而EnvironmentRecord的Type可以定義為Declarative或者Object來決定EnvironmentRecord的類型是什麼，最後Lexical Environment主要定義該Execution Context能夠使用的識別名字是為何以及對應何種變數、函式，
```
GlobalExectionContext = {  
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative" | "Object"
      // Identifier bindings go here
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },  
  VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative" | "Object"
      // Identifier bindings go here
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```
  - LexicalEnvironment 從GEC收集函式宣告、let/const形式的變數宣告並以下面形式來存放在EnvironmentRecord屬性中，
  ```
  identifier: <function>
  identifier: <const variable>
  identifier: <let variable>
  ```
  在這裡由於只有函式宣告不受scope的影響，所以函式宣告部分能夠確定其對應內容，而const/let的變數宣告會受限於scope而無法馬上確定，因此對應let/const的變數宣告會是uninitialized來表示該變數還未宣告，接著Outer reference和ThisBinding會因為目前EC為GEC而分別設定為null和Global Object。

  ```
  GlobalExectionContext = {  
    LexicalEnvironment: {
      EnvironmentRecord: {
        Type: "Declarative",
        // Identifier bindings go here
        identifier1: <uninitialized>,
        identifier2: <uninitialized>,
        identifier3: <func>
      }
      outer: <null>,
      ThisBinding: <Global Object>
    }
  }  
  ```
  - VariablEenvironment：從GEC收集函式宣告、var形式的變數宣告並以下面形式來存放在EnvironmentRecord屬性中，
  ```
  identifier: <var variable>
  ```
  在這裡由於只有var變數本身沒有scope的概念，所以不受到scope的影響，因此對應var的變數值會是undefined代表已經宣告但只是還未指派任何初始值給予，接著Outer reference和ThisBinding會因為目前EC為GEC而分別設定為null和Global Object。
  ```
  GlobalExectionContext = {  
    VariableEnvironment: {
      EnvironmentRecord: {
        // Identifier bindings go here
        identifier1: undefined
      }
      outer: <null>,
      ThisBinding: <Global Object>
    }
  }
  ```
  - 補充資訊：Outer reference是用來實現scope chain，當目前EC找不到對應名稱時就會往outer所指向的EC來尋找，而ThisBinding則是指定This變數要指定哪個對象。

4. 最終結果會是如下：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1646750816/backend/lexical%20environment/GlobalExecContext_ylw8a9.png)

## GEC - execution phase
1. 建立完GEC後，JavaScript 引擎隨後就會在GEC的環境下一行又一行執行程式碼，並根據執行結果來更新Lexical Environment內某個特定名稱的對應值或者調用其他區塊或者其他函式，使其產生該區塊或者該函式的execution context
> JavaScript Engine executes the code line by line
2. 在這裏主要會做：
  - 更新Lexical Environment的對應值

## FEC - creation phase
0. 這裡的概念以ECMA2015-ECMA2019為主。
1. 發生時間點：當從GEC的執行過程呼叫了某個區塊或者某個函式時，就會先建立該FEC
2. FEC範圍：以區塊內或者函式內的所有區域變數、函式為主(不含該函式的內部執行)
3. 製作流程：
  - 建立Arguments物件來儲存賦予對應函數的參數：主要會包含參數、參數長度，以下面函式呼叫為例，當呼叫並執行test函式的第一段前會先建立對應的FEC，其中會先建立Arguments物件儲存1、3、4以及參數長度，其中0-2為代表著test呼叫中的argument1至argument3，而length則是代表著參數長度(如result那樣呈現)，而Arguments所儲存的變數皆為let類型的變數宣告，所以會將Arguments放在FEC中的LexicalEnvironment區塊。
  ```
  function test(a, b, c) {
      ....
  }
  test(1, 3, 4)

  // result
  Arguments: {0: 1, 1: 3, 2: 4,length: 3},
  ```
  - 建立this物件並決定this參照於誰：在這裡建立完會依據哪個物件呼叫該函式，而決定FEC的this為那個物件，否則就依照outer reference所指向的EC來決定this為誰
  - 建立屬於FEC的Lexical Environment：主要分為LexicalEnvironment、VariablEenvironment，如同GEC那樣，唯二不同的事情就是：第一、outer會是指向呼叫該EC的EC，也就是代表從全域呼叫的GlobalExectionContext或者代表從其他函式呼叫的EC，第二、ThisBinding會是根據呼叫目前FEC的對象是否為物件，若將ThisBinding設定該物件，否則就依照outer 來找到對應的this變數是誰。
```
FunctionExectionContext = {  
  LexicalEnvironment: {
    EnvironmentRecord: {
      type: "Declarative",
      // Identifier bindings go here
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },  
  VariableEnvironment: {
    EnvironmentRecord: {
      // Identifier bindings go here
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```
  - LexicalEnvironment 從FEC收集函式宣告、let/const形式的變數宣告並以下面形式來存放在EnvironmentRecord屬性中，
  ```
  identifier: <function>
  identifier: <const variable>
  identifier: <let variable>
  ```
  剩餘就如同GEC中的LexicalEnvironment那樣
  ```
  FunctionExectionContext = {  
    LexicalEnvironment: {
      EnvironmentRecord: {
        Type: "Object",
        // Identifier bindings go here
        identifier1: <uninitialized>,
        identifier2: <uninitialized>,
        identifier3: <func>
      }
      outer: <null>,
      ThisBinding: <Global Object>
    }
  }  
  ```
  - VariablEenvironment：從GEC收集函式宣告、var形式的變數宣告並以下面形式來存放在EnvironmentRecord屬性中，
  ```
  identifier: <var variable>
  ```
  剩餘就如同GEC中的VariablEenvironment那樣
  ```
  FunctionExectionContext = {  
    VariableEnvironment: {
      EnvironmentRecord: {
        Type: "Object",
        // Identifier bindings go here
        identifier1: undefined
      }
      outer: <null>,
      ThisBinding: <Global Object>
    }
  }
  ```

### example
以下面程式碼為例來建立GEC和timesTen的FEC
```
let x = 10;
var z = 100;
function timesTen(a){
    var c = 10
    return a * 10 * c;
}

let y = timesTen(x);

console.log(y); // 100
```
流程為：

1. JS引擎讀取全域區間的程式碼並建立GEC：在這裡會搜索的變數宣告會是let類型的x和y、var類型的z、函式宣告類型的timesTen，將他們分別寫入至LexicalEnvironment、VariableEnvironment，並更動他們對應的outer為null，而ThisBinding為Global Object

```
GlobalExectionContext = {  
  LexicalEnvironment: {
    EnvironmentRecord: {
      // Identifier bindings go here
      x: <uninitialized>,
      y: <uninitialized>,
      timesTen: <function>
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },  
  VariableEnvironment: {
    EnvironmentRecord: {
      // Identifier bindings go here
      z: undefined
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

2. JS引擎開始執行程式碼，並執行到呼叫timesTen(x)來指派回傳值給y前的GEC會是如下：其中x、z因為執行過程而分別更新成10和100。
```
GlobalExectionContext = {  
  LexicalEnvironment: {
    EnvironmentRecord: {
      // Identifier bindings go here
      x: 10,
      y: <uninitialized>,
      timesTen: <function>
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },  
  VariableEnvironment: {
    EnvironmentRecord: {
      // Identifier bindings go here
      z: 100
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

3. 當引擎執行到呼叫timesTen(x)並執行函式的第一行，此時就會讓系統去建立該函式的FEC，在這裡會先利用參數來建立Arguments物件，而其內容為\{0: 10, length: 1\}並放到LexicalEnvironment，接著搜索該函式是否存在其他宣告，結果找到了var類型的c
```
FunctionExectionContext = {  
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      Arguments: {0: 10, length: 1}
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },  
  VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      c: undefined
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```
4. JS引擎開始執行函式程式碼，並執行到結束前，其FEC的狀態會是如下，更動var類型的c至10
```
FunctionExectionContext = {  
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      Arguments: {0: 10, length: 1}
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },  
  VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      c: 10
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```
5. 結束執行timesTen，並回到GEC將回傳結果-1000給c，最後執行到console.log(c)就結束:
```
GlobalExectionContext = {  
  LexicalEnvironment: {
    EnvironmentRecord: {
      // Identifier bindings go here
      x: 10,
      y: 1000,
      timesTen: <function>
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },  
  VariableEnvironment: {
    EnvironmentRecord: {
      // Identifier bindings go here
      z: 100
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

### 補充知識： parameter vs. argument
1. parameter: 函式輸入內容的規範
> a set of facts or a fixed limit that establishes
2. argument：實際輸入至函式的內容


### 補充知識：Environment Records的種類
1. Environment Records主要目的為目前environment有哪些識別字，會去搜尋全域、區域、函式所擁有的識別字有哪些以及對應哪些內容(變數、函式)
2. 至於紀錄方式，Environment Records的種類主要有兩種：
  - Object Environment records：以類似於with的形式將identifier綁定在特定物件或者一些特定的物件來當屬性，以此來表示，亦即為用特定物件的屬性來紀錄
  > same interface as a normal environment record, but keeps its bindings in a JavaScript Object. In this case, the object is the global object
  ```
  global.key1 = value1
  global.key2 = value2
  .
  .
  ```
  - Declarative Environment records：以宣告變數、函式宣告的形式來紀錄

  ### 補充知識：LHS vs. RHS
  1. 兩者主要描述內容與對象之間的賦予指派關係或者獲取關係，通常會使用下面方式來表示b內容指派a或者從a對象獲取值
  ```
  a = b
  ```
  2. LHS代表著左手邊(Left-Hand Side)，在這裡會是以=為主的左手邊，表示被贈與內容的那一方，而 RHS則代表著右手邊(Right-Hand Side)，在這裡會是以=為主的右手邊，表示負責贈與內容的那一方，在這裡分別引申為賦值查詢和獲取查詢
  3. 賦值查詢或LHS查詢：在JS引擎運行賦值前，會先檢查當前作用域是否存在識別字a，若沒有的話，就會透過scope chain來向其他有連結的作用域來找(通常會往全域的方向找)，若真的找不到會根據設定來自動於全域宣告該a或者報錯，等真的能夠找到a時，才會將content賦予至a
  ```
  a = content
  ```
  4. 獲取查詢或RHS查詢：在獲取某項識別字對應的內容時，會去查詢作用域是否存在該識別字，若沒有的話，就會透過scope chain來向其他有連結的作用域來找(通常會往全域的方向找)，若真的找不到就報錯，等真的能找到內容，才會回傳該對應內容。

  5. 案例：
    - LHS案例
    ```
    a = 2
    a = b
    ```
    - RHS案例
    ```
    console.log(a)
    ```

  ### 補充知識：JavaScript 編譯特性