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
1. 發生時間點：當引擎要執行某個檔案上的JS程式碼時
2. GEC範圍：以檔案內的全域區塊為一個區塊(block)，不包含函式內部的執行、區塊內的另一個區塊的(Block)的內部執行
3. 製作流程：
  - 建立一個全域物件：在瀏覽器會是名為window的全域物件，在Node.js會是名為global的全域變數
  - 建立this物件並決定this參照於誰：在這裡建立完會去指向當前被建立的全域變數
  - 建立Lexical Environment：主要分為LexicalEnvironment、VariablEenvironment，這兩者會包含Environment Records、Outer reference、Thisbinding這三種主要屬性(如下)、Lexical Environment主要定義該Execution Context能夠使用的識別名字是為何以及對應何種變數、函式，
```
GlobalExectionContext = {  
  LexicalEnvironment: {
    EnvironmentRecord: {
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
  
  LexicalEnvironment會從EC收集函式宣告、let/const形式的變數宣告，同時由於函式可以在EC確定為何以及let/const宣告則因為const/let變數的內容本身受限於scope概念而不能夠馬上確定，因此對應let/const的變數值會是uninitialized
```
GlobalExectionContext = {  
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
  },  
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
  
  
  
  
  
  前者會從範圍內搜尋對應為函式宣告、或用let/const變數的識別名稱並以下面格式收集至其EnvironmentRecord屬性，同時設定 
  ```
  identifier: <function>
  identifier: <const variable>
  identifier: <let variable>
  ```
  
  收集同一個區塊的識別名稱以及對應內容是為何，最後收集完並建立一個LexicalEnvironment；後者根據識別名稱是否為用var宣告的變數來收集同一個區塊的識別名稱以及對應內容是為何，最後收集完並建立一個VariablEenvironment
  - 定義scope chain：目前所擁有的lexical environment會透過一個outer reference來指向至呼叫該區塊的區塊所擁有的lexical environment進行鏈結，以方便在找不到對應的識別字的時候，導向至鏈結的另一方來找到該字。
4. 最終結果會是如下：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1646750816/backend/lexical%20environment/GlobalExecContext_ylw8a9.png)
5. 補充資訊：
  - LexicalEnvironment、VariablEenvironment的程式碼表現會是如下所示：LexicalEnvironment的EnvironmentRecord屬性會是GEC上的識別字和函式宣告、let/const變數之間的對應關係，而其outer由於這層是Global，所以會指向null，而VariableEnvironment的EnvironmentRecord屬性會是GEC上的識別字和var變數之間的對應關係，最後的outer也由於Global而指向null。


## GEC - execution phase
1. 建立完GEC後，JavaScript 引擎隨後就會在GEC的環境下一行又一行執行程式碼，並根據執行結果來更新Lexical Environment內某個特定名稱的對應值或者調用其他區塊或者其他函式，使其產生該區塊或者該函式的execution context
> JavaScript Engine executes the code line by line
2. 在這裏主要會做：
  - 更新Lexical Environment的對應值

### example

```
let x = 10;

function timesTen(a){
    return a * 10;
}

let y = timesTen(x);

console.log(y); // 100
```








### 補充知識： parameter vs. argument
1. parameter: 函式輸入內容的規範
> a set of facts or a fixed limit that establishes
2. argument：實際輸入至函式的內容