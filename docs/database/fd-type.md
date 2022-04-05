---
sidebar_position: 3
---

# Functional dependency type


## Functional dependency

在同個表格X或者relation X上，該表格X擁有的屬性集合分別為屬性集合A和屬性集合B，若任兩筆紀錄在屬性集合A上是擁有相同屬性值集合X'，那麼這兩筆紀錄在屬性集合B也呈現出另一種相同的屬性值集合Y'的話，那麼就能以Y' = f(X')來表示同個屬性值集合X'能夠透過某函式對應(確定)另一個相同屬性值集合Y'是什麼，不會出現同個屬性集合值X'對應不同的屬性值集合，藉由這點也可以延伸成屬性集合A可以在某函式對應(確定)屬性集合B。
> If two tuples of R agree on all of the attributes A1, A2,..., An (i.e., the tuples have the same values in their respective components for each of these attributes), then they must also agree on all of another list of attributes B1, B2, ..., Bm. We write this FD formally as 
```
A1, A2,..., An -> B1, B2,..., Bm
// say that
A1, A2,..., An functionally determine B1, B2,..., Bm
```

這時會寫作成以下形式，其描述為屬性集合A在函式上確定屬性集合B、屬性集合A在功能方面上確定屬性集合B、屬性集合B藉由函式而依賴於屬性集合A
```
A -> B
A functionally determine B
```

## Full functional dependency

## Partial functional dependency

## Transitive functional dependency
