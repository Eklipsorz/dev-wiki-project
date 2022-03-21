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
