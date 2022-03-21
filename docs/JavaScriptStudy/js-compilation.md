---
sidebar_position: 7
---

# JavaScript 編譯？直譯？
1. 根據MDN所定義的JS，可以是輕量型直譯語言或者JIT編譯語言
> JavaScript (JS) is a lightweight, interpreted, or just-in-time compiled programming language
2. 實際上來說JavaScript會依據ECMA標準和所使用的引擎/執行環境是什麼來決定：
  - ECMA標準並未限定JS得是哪種語言，只是規定JS標準上概念會是什麼樣子
  - 若執行環境/引擎是只支援JIT的話，那麼該環境上的JS就會是JIT編譯語言，如Chrome V8、Node.js
  - 若執行環境/引擎是只支援直譯的話，那麼該環境上的JS就會是直譯語言，如Rhino、KJS
  - 實際上來說目前普遍的執行環境/引擎都採用於JIT，因此主流上會以JIT為主

## JIT 編譯語言
1. Just-In-Time(JIT) Compiled Language：相對於編譯語言，該語言的編譯器會於程式執行期進行編譯，換言之，程式碼會在即將執行時進行編譯（因此稱為「Just-In-Time」），並且根據程式執行時的狀態和資訊來進一步優化產生出更好的原生機械語言，比如說會將實際經常使用(呼叫)的函式對應的原生機械語言儲存至快取，並於該函式呼叫的地方去直接從快取內部取得該函式對應的原生機械語言來執行。
> A JIT compiler runs after the program has started and compiles the code (usually bytecode or some kind of VM instructions) on the fly (or just-in-time, as it's called) into a form that's usually faster, typically the host CPU's native instruction set. A JIT has access to dynamic runtime information whereas a standard compiler doesn't and can make better optimizations like inlining functions that are used frequently.

>  the code can be compiled when it is about to be executed (hence the name "just-in-time"), and then cached and reused later without needing to be recompiled.

2. JIT由於是根據程式執行來編譯以及優化，所以又被稱之為動態編譯，傳統上的編譯會是執行之前就編譯，且不會獲取執行狀態/資訊來優化，所以會被稱之為靜態編譯。

3. 參考資料：
[what-does-a-just-in-time-jit-compiler-do](https://stackoverflow.com/questions/95635/what-does-a-just-in-time-jit-compiler-do)


### 補充知識：inline function
1. 一種編譯器優化手段，將被呼叫的函式內部以一行來展開來轉換成對應程式碼並取代原本呼叫函式的地方，比如建立一個test函式，其函式內部會隨機產生兩個數字指派給a、b，接著透過呼叫test函式來獲取兩個隨機數的相加，透過inline function手段，就會試著將test函式以一行來展開來轉換成對應程式碼，展開後的程式碼會是Math.random() * 10 + Math.random() * 10，接著再用程式碼去取代原本呼叫test函式的地方
```
// before
function test() {
  let a = Math.random() * 10
  let b = Math.random() * 10
  return a + b
}

let result = test()

// after

let result = Math.random() * 10 + Math.random() * 10
```


2. 案例：
```
// before
function funct1() {
  // do something
}

let result1 = funct1()
....
let result2 = funct1()

// after

let result1 = // result of do something
....
let result2 = // result of do something

```


>  inline expansion, or inlining, is a manual or compiler optimization that replaces a function call site with the body of the called function