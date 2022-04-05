---
sidebar_position: 1
---

# 資料庫表格-key種類



## 子集

### subset & superset
假設有兩個集合(A、B)，若集合A的任一元素都是集合B的元素，那麼就集合A為集合B的子集(subset)，而B就是A的超集(superkey)，記為A⊆B，讀作A包含於B。舉例來說集合B為\{B1, B2, B3, B4\}，那麼若集合A為集合B的子集，那麼集合A可以是
```
{B1}
{B1, B2}
{B1, B2, B3}
{B1, B2, B3, B4}
```

Note：
1. sub-和super-開頭的字都分別泛指某項事物在某種程度來說是次要、淺下、低下、從屬，而super則是指重要、超越、之上、包含。
2. sub和super 描述set的話就是指在被包含和包含，前者是指被包含的set，後者是指包含其他set的set

### proper subset
假設有兩個集合(A、B)，若兩者間的關係為A⊆B且A≠B的話，集合A就是集合B的真集合(proper subset)，以B為\{B1, B2, B3, B4\}的例子，那麼若集合A為集合B的真子集，那麼集合A可以是前面三個，最後一個由於會是A=B，所以不能夠算上它
```
{B1}
{B1, B2}
{B1, B2, B3}
==================
{B1, B2, B3, B4}
```


Note：
1. proper 在這指最主要、最重要、嚴格(正確)意義上的，在這描述集合是強調該集合是確實是它所描述。
> belonging to the main, most important, or typical part



參考資料：
- [集合基礎知識](https://www.itread01.com/content/1547202186.html)



If two or more statements, ideas, sets of numbers, etc. agree, they are the same or very similar.
一致，相符，相同；極相似
We have five accounts of what happened and none of them agree.
對於所發生的事情我們手頭有五種各不相同的說法。


https://stackoverflow.com/questions/22064977/what-is-the-difference-between-primary-key-and-prime-attribute
https://www.mysql.tw/2015/04/super-keycandidate-keyprimary.html

https://www.itread01.com/content/1545892442.html


## key && superkey

若一個表格或者relation上的欄位組合\{A1, A2,..., An\}能構成該表格的key，得滿足以下條件
1. 這些欄位/屬性必須能夠功能上確定剩下欄位/屬性，也就是說同樣的欄位組合\{A1, A2,..., An\}只會對應同個紀錄，不會對應到兩個不同的紀錄。
> Those attributes functionally determine all other attributes of the relation. That is, it is impossible for two distinct tuples of R to agree on all of A1, A2,..., An

2. \{A1, A2,..., An\}中的所有真子集合中的組合(如只有A1和A2、或者是只有A1、A2、A3)不能夠功能上確定剩下屬性，換言之，只有完整的 \{A1, A2,..., An\}才能確定且在決定紀錄的key中是擁有最小欄位量的key
> No proper subet of {A1, A2,...., An} functionally determines all other attributes of R; i.e., a key must be minimal

若一組欄位集合能夠包含key的話，該欄位集合會稱作為superkey，或者稱key的超集，該集合中的鑰匙都會滿足於key的第一條件，不一定能夠滿足第二個條件，比如說一個表格擁有以下這幾個欄位，其中A1和A2是key，

```
A1, A2, A3, A4,..., An
```

那麼superkey就會是

```
{A1, A2}
{A1, A2, A3}
{A1, A2, A3, A4}
{A1, A2, A3, A4, A5}
{A1, A2, A3, A4, A5, A6}
.
.
{A1, A2, A3, A4, A5, A6,..., An}
```