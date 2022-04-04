---
sidebar_position: 1
---

# 資料庫表格-key種類



## 子集

### subset & superset
假設有兩個集合(A、B)，若集合A的任一元素都是集合B的元素，那麼就集合A為集合B的子集(subset)，而B就是A的超集(superkey)，舉例來說集合B為\{B1, B2, B3, B4\}，那麼若集合A為集合B的子集，那麼集合A可以是
```
{B1}
{B1, B2}
{B1, B2, B3}
{B1, B2, B3, B4}
```

Note：
1. sub-和super-開頭的字都分別泛指某項事物在某種程度來說是次要、淺下、低下、從屬，而super則是指重要、超越、之上、包含。
2. sub和super 描述set的話就是指在被包含和包含，前者是指被包含的set，後者是指包含其他set的set

參考資料：
https://www.itread01.com/content/1547202186.html
