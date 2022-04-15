---
sidebar_position: 4
---

# 作業19獲得的新知和技術

## 知識緣由
1. 由於在SQL嚴格模式違反only_full_group_by規範，而跑出以下訊息
> Error Code: 1055. Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'icecream.flavors.id' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
2. 解法為：
  - 遵從規範來修改
  - 使用aggregate function來規避規範處理
  - 使用ANY_VALUE()
  - 調整SQL mode為寬鬆模式

### 補充：GROUP BY 
1. 在關聯式資料庫裡會使用謂詞邏輯來定義著資料庫和存放的資料，其中會將每筆擁有屬性值的資料定義為零階集合或者紀錄(record)，而存放每筆紀錄的空間會是一階集合或者稱之為表格(table)，最後存放每個表格的空間會是二階集合或者資料庫(Database)，**每一階集合都存在著各自所能夠使用的SQL語法、指令、存取的屬性，每一階只能用同一階能夠使用的語法、指令、屬性，且每一階的元素都有著識別碼來幫助系統辨識和處理**
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1644324438/blog/database/zero-one-two-order_i9vto9.png)

2. GROUP BY 本身會將多筆擁有相同欄位值的紀錄構成一個1階(紀錄的集合、相當於表格)集合或者表格，在這裡集合的每一筆紀錄都只能用某些屬性作為他們在那集合的識別內容，而這個識別內容會依照GROUP BY所指定的內容而定，拿下面來說的案例來說的話， GROUP BY那段會從flavors內部構成一個一階集合來存放紀錄，在集合中的每筆紀錄都會有id 和 name，每筆紀錄都用id來當作該集合當中的主鍵。
```
CREATE TABLE `flavors` (
	`id` INT AUTO_INCREMENT,
    `name` VARCHAR(255) NOT NULL,
    `amount` INT UNSIGNED,
    `price` INT UNSIGNED,
    PRIMARY KEY (`id`)
);

INSERT INTO `flavors` (`name`, `amount`, `price`)
VALUES ('cherry', 10, 1);

INSERT INTO `flavors` (`name`, `amount`, `price`)
VALUES ('strawberry', 20, 2);

SELECT `id`, `name` FROM `flavors` GROUP BY `id`;
```
3. 若SELECT + GROUP BY搭配著將一階集合轉換為零階紀錄的Aggregate functions，那麼就會有多個一階集合會被轉換成多個擁有單一零階紀錄(只擁有單一屬性值)的一階集合，由於集合中的每筆紀錄都只有一個的屬性值，所以可以直接拿它來當做集合中的每筆紀錄所擁有之主鍵
```
SELECT AVG(column1)
FROM `table`
GROUP BY `column1`
```
但若 SELECT 又多從table那裡取得一些屬性值，那麼就會有多個一階集合會被轉換成多個擁有單一零接紀錄(只擁有著column2至columnN的屬性和代表AVG運算結果屬性)的一階集合，在這裡必須從column1-N挑出屬性來代表存在集合的每筆欄位，在這裡可以是：
  - 由column1 決定 AVG(column1) - columnN
  - 由column1, column2 決定 AVG(column1) - columnN
  - 由column1, column2, column3 決定 AVG(column1) - columnN
  - ...以此類推
  - 由column1,....,columnN 決定 AVG(column1) - columnN
```
SELECT column2, column3,... column N, AVG(column1)
FROM `table`
GROUP BY ????
```

### SQL Mode
1. 定義關聯式資料庫系統對於SQL語法上和資料上的合法性是否嚴格看待以及檢驗合法性的程度
>  Modes affect the SQL syntax MySQL supports and the data validation checks it performs 
2. 在MySQL 中的SQL Model會有：
  - ANSI模式：寬鬆模式
  - TRADITIONAL模式：嚴格模式
  - STRICT_TRANS_TABLES：比TRADITIONAL模式還嚴格的模式
3. 參考資料：
  - [MySQL only_full_group_by 1055报错的三种解决方案，临时关闭有影响吗？](https://kalacloud.com/blog/solve-query-failures-regarding-only-full-group-by-sql-mode/)
## ONLY_FULL_GROUP_BY 
1. MySQL 嚴格模式下的其中一個規範
2. 主要會嚴格看待GROUP BY的合法性來確保每一個集合的紀錄都有辦法被識別或者擁有其屬性來代表他們




### ONLY_FULL_GROUP_BY 規範
1. ONLY_FULL_GROUP_BY 規範會特別確保每一個集合的紀錄都有辦法被識別或者擁有其屬性來代表他們，以避免後續拿去做一階集合運算或者其他運算的時候，會有資料錯亂問題或者讓資料庫系統無法在GROUP所構成的空間來辨識各個低階元素，比如說若都允許的話，代表集合中的元素可以用空格和undefined辨識，但這些資訊本身無法代表元素本身，或者只能代表錯誤，所以會有同個空格值或者同個undefined值，但卻擁有不同欄位值的紀錄或者元素，為了避免這樣而規範了以下規則：
  - GROUP BY 必須是字串的欄位名稱，它必須是能夠從結果紀錄的欄位名稱(包含alias)能夠對應，它本身不帶有計算，只會從結果獲取對應欄位
  > 对于 GROUP BY 聚合操作，如果 SELECT中 的列，没有在 GROUP BY 中出现，那么将认为这个 SQL 是不合法的
  - GROUP BY 結果紀錄的剩餘欄位(除了GROUP BY 引用的欄位X以外)必須被欄位X功能確定或者功能依賴
  - SELECT + GROUP BY 搭配aggregate function的話，由於aggregate function本身會轉換成只有單一卻有不同欄位值的紀錄之一階集合，而在這裡為了遵從前面兩者，每個集合中必須要從欄位值來代表集合中的每一個元素，在這裡只能是以下這幾個選項
     * 由column1,....,columnN-1 決定 AVG(column1) - columnN 
     * 由column1,....,columnN 決定 AVG(column1) - columnN
     * 所有未被aggregate function處理的欄位值來決定剩餘被其function處理的欄位。
  
      ```
      SELECT column2, column3,... column N, AVG(column1)
      FROM `table`
      GROUP BY ????
      ```

2. 參考資料：
[MySQL only_full_group_by 1055报错的三种解决方案，临时关闭有影响吗？](https://kalacloud.com/blog/solve-query-failures-regarding-only-full-group-by-sql-mode/)
[](https://www.itread01.com/content/1519471374.html)