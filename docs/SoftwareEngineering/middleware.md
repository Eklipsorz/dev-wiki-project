---
sidebar_position: 4
---

# Middleware

1. 中間層，通常是指層級關係中的兩個層級之間的層級，在電腦科學裡頭，是指系統軟體和應用軟體之間的層級或者軟體。
2. 這個層級會以自身專屬概念、語法、功能來將系統軟體層級封裝成能讓應用軟體直接使用的形式，通常會是對應系統軟體功能的函式或者API，形式會是如同以下，但實際上會是比較複雜的封裝形式，
```
function applicationLayerFunction(parameter) {
  function systemLayerFunction(parameter) {

  } 
}
```

而應用軟體只需要使用中間層所提供的形式就能讓應用軟體使用對應形式的系統軟體功能，也就是說中間層是一個讓應用軟體層使用系統軟體功能的介面。