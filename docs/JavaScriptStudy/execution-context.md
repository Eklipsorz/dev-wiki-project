---
sidebar_position: 2
---


# Execution context
1. Execution 是指程式執行時的背景、過程，而context是指某種事物存在/發生的背景或者情況，兩者加起來就等同於描述執行時會是什麼樣的狀態或者背景，換言之，就是目前程式的執行環境，在JavaScript中，會是引擎執行JavaScript的執行環境。
2. 在JavaScript中，當JavaScript引擎開始解析執行某個檔案上的JS程式碼時，會替該檔案建立Global Execution Context(GEC)來設定執行會需要的環境變數以及儲存過程，當建立完之後，引擎就隨後執行JS程式碼，便以該context為基礎來執行以及更新context所需的資訊至context，每一個execution context都具有兩個階段：
  - creation phase：execution context建立的階段或者程式剛開始被執行的階段
  - exection phase：程式依據著初期設定的execution context執行的時候，在這時候會邊依據邊把需要更動的資訊紀錄至context。
> When the JavaScript engine executes the JavaScript code, it creates execution contexts. Each execution context has two phases: the creation phase and the execution phase.

## GEC - creation phase
1. 發生時間點：引擎要執行某個檔案上的JS程式碼時
2. GEC範圍：以檔案內容為一個區塊(block)，不包含函式、函式呼叫、由括號所構成的區塊
3. 製作流程：
  - 建立一個全域物件：在瀏覽器會是名為window的全域物件，在Node.js會是名為global的全域變數
  - 建立this物件並決定this參照於誰：在這裡建立完會去指向當前被建立的全域變數
  - 建立Lexical Environment：主要根據變數、函式的性質來分為LexicalEnvironment、VariablEenvironment，前者根據識別名稱是否對應函式宣告、用let/const宣告的變數來收集同一個區塊的識別名稱以及對應內容是為何，最後收集完並建立一個LexicalEnvironment；後者根據識別名稱是否為用var宣告的變數來收集同一個區塊的識別名稱以及對應內容是為何，最後收集完並建立一個VariablEenvironment
  - 定義scope chain：目前所擁有的lexical environment透過一個outer reference來指向至呼叫該區塊的區塊所擁有的lexical environment進行鏈結，以方便在找不到對應的識別字的時候，導向至鏈結的另一方來找到該字。

### creation phase - example

```
let x = 10;

function timesTen(a){
    return a * 10;
}

let y = timesTen(x);

console.log(y); // 100
```



## GEC - execution phase




### 補充知識： parameter vs. argument
1. parameter: 函式輸入內容的規範
> a set of facts or a fixed limit that establishes
2. argument：實際輸入至函式的內容