---
sidebar_position: 9
---

# Database introduction


## 資料
1. 原文是：facts or information used usually to calculate, analyze, or plan something
2. 軟體提供主要功能時所需要處理的資訊，而這資訊會是由多個文字、圖片所構成並代表，可以是由軟體自行提供，也可以由外部提供
3. 從上述來看，資料具有以下特質：
 - 提供主要功能時所需的資訊
 - 資訊會由文字、圖片所構成
4. 舉例來說：
- Instagram：一名使用者、一張使用者分享的照片
- Amazon：一名使用者、一個產品、一筆訂單
- Gmail：一名使用者、一封電子郵件


## 資料庫管理系統
1. 英文為Database Management System, DBMS)
2. 主要功能為定義Data Model和實現Data Model，具體來說會是
 - 負責定義資料如何存進資料庫，每個資料的結構會是如何、每個資料本身的限制是如何
 - 提供Schema定義功能來確保資料都如預期
 - 提供一系列查詢語言API來幫助應用程式或者開發者透過API來產生對應指令讓系統解析並執行
 - 確保每一次操作和存取都能保證資料完整性(不破壞原資料內容)、資料一致性

3. 資料庫大致上分為兩種：基於關聯式資料庫架構(Relational Database)、基於非關聯式資料庫(NO SQL)
4. 當你要在你的應用程式裡整合資料庫管理系統時，需要瞭解以下三件事：
  - 應用程式如何與資料庫溝通
  - 選擇用什麼資料庫
  - 如何在應用程式裡定義資料結構

### 參考資料
1. [資料分析面試被刷後，後悔沒早點會學會這些資料庫知識！ - 知乎](https://www.gushiciku.cn/pl/p9vL/zh-tw)

## Catalog vs Schema 
0. Schema 原意為表現某個想法的規格書；Catalog則是定義一本以一個清單來包含多個商品的書
1. 在SQL標準中，為了很好地分辨表格是隸屬於哪個資料庫，而哪個資料庫是隸屬於哪個資料庫範疇，會把Catalog和Schema定位一個資料儲存裝置/容器的抽象概念，換言之，會是視作為資料庫管理系統中的命名空間，Catalog這命名空間包含著哪些可用的Schema命名空間，而Schema則是包含哪些可用表格的命名空間
> a book with a list of all the goods that you can buy from a shop
> A database catalog of a database instance consists of metadata in which definitions of database objects such as base tables, views (virtual tables), synonyms, value ranges, indexes, users, and user groups are stored

2. 但實際上的實現並不會跟標準一致，必須得看每個資料庫實作對於名詞的實際解釋才能理解。
Note：
1. 命名空間，是指某些事物的識別空間或存活空間，用途可以將同一個事物依照識別空間的不同而進一步分類，比如說假設有二個事物C並分別放在識別空間A和B，那麼當要存取事物C的是識別空間A擁有的，那麼只需要
```
識別空間A.事物C
```
[資料庫schema與catalog簡介](https://www.796t.com/content/1547512594.html)
## Schema
1. Schema 原意為易讓人理解某種概念或者某種想法的表現形式(通常會是文字或者圖片)，換言之，表現某個想法的規格書，在資料庫管理系統的實現當中會是：
  - 定義一個表格上的資料組成是什麼樣的規格書
	- 定義一個使用者能夠使用什麼樣的資料庫(結構)之規格書
	- 定義一個命名空間是什麼樣子內容的空間之規格書，其命名空間可指定自己可用的表格是哪些
> So I think answer to your questions is:
> 1. It depends on implementation, whether catalog name is needed to identify objects. The meaning of catalog, schema and database vary from one implementation to another.
> 2. Yes, a catalog is an abstraction of data storage. I think it should be also defined as a self-contained isolated namespace, but not all SQL engines do it.
> 3. Database and schema are pretty well defined by all vendors. Catalog is sometimes synonymous to "database" (at least in Oracle and Postgres), sometimes synonymous to "schema", and sometimes synonymous to both. The term catalog also often means metadata collection (aka system tables).
> 4. Schema is what programmers should use to organize artifacts in SQL database as it represents a logical namespace with access control layer.
2. 若描述著一個表格上的Schema，那麼其Schema是定義每一組資料會有的屬性、型別、關係、限制等性質，並且要求儲存進資料庫的原生資料都必須按照Schema所指示的規格來存放或存取。
3. 規格書可以進一步確保每一筆資料的正確性。
4. Schema vs. Data Model：前者是描述著某種資料A會有的屬性名稱、屬性型別、屬性值本身的限制、與其他種類資料的關聯性，後者則是定義某種資料是什麼以及如何儲存、對這些資料會有怎麼樣的操作、資料本身的限制
[Difference between Data Model and Database Schema in DBMS?](https://stackoverflow.com/questions/25093452/difference-between-data-model-and-database-schema-in-dbms)

## Self-Describing
0. 一般而言會由外部程式來定義資料本身是具有什麼樣的結構，而若資料內容本身就能描述資料結構的話，那麼就會是Self-Describing資料
1. Self-Describing是指自己來描述自己是什麼樣的東西，在資料上，是指資料內容本身來解釋/描述自己是儲存什麼樣的內容或者描述自己是什麼樣的資料，進而透過其特性來定義資料的Data Model是為何，不用透過額外框架和套件來定義每一筆資料都是什麼樣的內容和型別，只需要解析其資料內容就能知道資料的架構。

2. 案例：XML、HTML、JSON，利用標籤和標籤值來說明每筆資料所具有的屬性和型別，其中標籤和標籤值就是資料的一部分。

3. XML: 建立四個標籤分別為to、from、heading、body，這些標籤包覆著內容就會是其標籤涵蓋的內容，而由四個標籤可以描述XML內容是主要Jani要傳遞給Tove的提醒訊息，並且內容會由標籤來組合，而外部程式只需要解析標籤和標籤內容就能構築成對應的key-value pair來解析，不需要重新定義
```
<to>Tove</to>
<from>Jani</from>
<heading>Reminder</heading>
<body>Don't forget me this weekend!</body>
```
### 參考資料
1. [自我描述性 是什么意思？](https://www.zhihu.com/question/46127181)

## Schemaless Database
1. Schema 是指資料庫的Schema，而less則是形容某個東西的數量較少或者程度較少，Schemaless則是形容資料庫本身對於Schema的需求程度不會到很必要，換言之，Schemaless Database本身不需要Schema就能夠正常儲存且不會透過Schema去驗證資料是否正確 或者 資料庫本身不支援Schema和相關檢驗，並允許資料可以不按照規則進行存取
2. 由於傳統資料庫管理系統都會用Schema都會要求資料按照其指定規格來存放，即使資料還有其他部分內容是具有價值的，只要該部分不在規格中就不會被存放進資料庫，為了進一步儲存更多資料而允許開發者、管理者、程式不用先定義Schema就即可先存放在資料庫，而這樣也省去預先定義Schema的成本
3. 由於資料庫本身對於Schema的規定很鬆散，可能資料庫本身不會提供Schema設定功能，但由於資料本身還是有可能因為資料型別的關係而產生預期外的錯誤結果，所以必須要在對資料庫進行直接操作的那一端建立類似Schema的機制才能做相關驗證或者寫一大堆if來判斷型別正確性，在這裡的對資料庫進行直接操作的那一端會是應用程式層級或者伺服器層級，所以會於該層級下建立ORM或者ODM上來建立一個Schema機制檢驗。


### 參考資料
1. [MongoDB 的 ODM：mongoose 簡單介紹](https://blog.chh.tw/posts/mongodb-odm-mongoose/)




## 關聯式資料庫
1. 是一種基於關聯模型(Relational model)的資料儲存和管理的資料庫架構
2. 關聯模型本身是基於數學(集合論、謂詞邏輯)形式來描述每個集合之間的關係本身，在這裡，關係本身就是以數學形式所描述的條件式，該關係會針對於多個集合中，而每個集合都存放著無窮無盡的資料，而關係最後的產物會是從每個集合中取出一個滿足於該條件式的元素，接著從而組成多組序列對(x1,x2,x3,...,xn)，x1從X1集合取出，x2從X2集合取出，後面以此類推。
    - 在關聯模型中，每筆資料會是每個元素所構成的序列對，而構成序列對的條件式會是關聯
    - 此外該產物會是n個集合的笛卡爾積之子集
    - 在這裡可以將集合們分別想像成名字集合、地址集合、性別集合、電話集合，而關係則是從這些集合中挑出適合的元素來組合成(x1,x2,x3,x4)這序列對。
3. 在這裡，條件式會是代表著關係或者關聯R，每組序列對就是資料本身，每一個集合都代表著資料會有的屬性，取出來組成序列對的元素就是對應著屬性的屬性值，為了很好地描述由關係而構築出來的所有序列對，會採用表格形式來儲存每一個序列對並分別定義序列對的每一個元素皆源自於哪個集合，而由於是對於特殊關係而計算出來的合併結果，所以會將表格稱之為relation

> 在任意兩個集合A, B中，若集合A中的元素a是與集合B的元素b具有某種數學關係/關聯R(比如a < b，或者說先前拿到的元素比現在拿的元素還大)，那麼所有滿足該數學關係/關聯R的元素，會構成另一個子集合，而子集合會使用(a,b)這序列對來表示

4. 表格中的每一個直行(Column)都代表著涉及關係/關聯R(只要有一個元素涉及關係/關聯都算)的集合，用來根據序列對中的每一個元素之原屬集合來存放：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1637311082/blog/database/pairs2table_qzvyg1.png)


5. 在關聯式資料庫中，會利用關聯式模型-表格來按照Schema來儲存對應的資料，而每一筆資料會夾雜著屬性、屬性值、屬性會有的資料型別，拿電影來說明，首先會先定義屬於電影的Schema，在這裡會有電影標題、年份、長度、種類，除了長度和年份是用整數型別來儲存以外，其餘皆用字串來儲存，在這裏假設只產生三部電影：Gone with the wind、Star Wars、Wayne's World，而為了構成這三部電影(關係/關聯)而從電影標題集合、年份集合、長度集合、種類集合挑出相關的元素來構成一部電影，另外每一個集合的元素可不只有指定那三部電影的相關資料，每一部電影所獲得的元素彼此間都有關係/關聯，比如Star Wars，會跟1977、124、sciFi產生關係/關聯而構成一個序列對(Star Wars, 1977, 124, sciFi)並放在表格中。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1637311889/blog/database/pairs2tableExample_nvyndh.png)

6. 同樣地，在關聯式資料庫中所有操作下的結果都以表格來儲存或者表示。

7. 補充：
  - Predicate logic：謂詞邏輯，Predicate是指由主詞和動詞所構成的句子，而Logic在這裡是指驗證和思考某項事實和想法的正式科學方法，組合起來就會以謂詞為主來當做事實和想法並進而驗證和思考的正式科學方法
  - 由於關聯式資料庫是使用表格來儲存每筆資料，所以本身會是Structured Data的經典案例之一

## SQL
1. 原文是Structured Query Language，本身是為了讓開發者或者程式本身能夠透過指令或者程式語言來向資料庫系統發送出對應資料庫操作的請求。

2. 由於該請求語言得基於Structured Data結構的關聯式資料庫來發送可辨識請求，Structured Data本身具有固定且可預知的特性，這可以進一步針對可能會有的操作結果來使請求拆分成幾個子句/子指令、目標為何、操作種類等固定格式，而這讓請求具有結構性(structured query)，另外由於該請求是依據英語語系來建立，所以又稱之為structured english query
> organized so that the parts relate well to each other

3. Query：原文是如下，指著一個關於某個資訊的正式請求，可以是針對某個資訊的進行CRUD的正式請求，而在電腦科學裡，會將此詞彙運用在資料庫或者資料上，是指某個存在資料庫上的某資訊之正式請求，一般資料庫系統會將Query視作為只索取資訊的正式請求，部分資料庫系統會進一步將Query區分為兩種：select query 和 action query，select query 會是指索要資料的正式請求，而action query則是指對資料進行增加、刪除、編輯、其餘非索要等操作的正式請求。
> A query is a question or a request for information expressed in a formal manner. A select query is one that retrieves data from a database. An action query asks for additional operations on data, such as insertion, updating, deleting or other forms of data manipulation.

[what is query](https://searchsqlserver.techtarget.com/definition/query)

4. 由於本身是基於關聯式資料庫管理系統而建立出來的特定查詢語言，因此可以把關聯式資料庫稱之為SQL Database或者簡稱為SQL，然而實際上這樣稱呼是不夠正式。


## NoSQL
1. 原本原文是Non-SQL，後面轉解為Not Only SQL，SQL代稱為基於關聯式資料庫架構的資料庫管理系統，指的是不同於傳統關聯式資料管理系統之統稱
2. 通常這類型的資料庫會是以不同於表格的儲存形式來儲存每一筆資料
3. NOSQL 種類：Key value (鍵值)、Document (文檔)、Graph (圖)、IN-MEMORY (內存)、SEARCH (搜索)


## Document Database
1. 屬於NoSQL的資料庫架構
2. 將資料以具有Self-Describing的文件形式來儲存，通常文件格式為是以近似JSON的格式，而這些文件又會組合成一個集合(Collection)，並統一儲存在資料庫
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1637423850/blog/database/documentDB_ykfvn2.png)
3. 每一筆資料會以key-value pairs的形式來儲存：
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1637328199/blog/database/dataInDocDatabase_yurf93.png)

## MongoDB 
1. 屬於Document Database
2. 其資料庫具有以下特色：
  - 使用具有Self-Describing的類JSON格式來儲存資料
  - 由於定義資料本身可由JSON來決定，所以系統本身不用先準備Schema，而且MongoDB是schemaless database
3. MongoDB可以管理多個資料庫(Database)，每一個資料庫中會存放多個集合(Collection)，而每一個集合當中會存放著代表每筆資料本身的類JSON格式文件(Document)來存放。
![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1637423850/blog/database/documentDB_ykfvn2.png)

## Data Model
1. 是一種定義資料或資訊的表示模型，通常會以一種形式來表達能夠存取的資料、這種資料形式如何進行管理/操作、其資料形式本身的限制，而定義該模型的角色通常會是管理資料的角色(資料庫管理系統)、使用資料的角色(應用程式本身)、資料本身。
2. 其模型主要含有：
  - Structure of the data：資料本身上的結構以及多個資料如何儲存
  - Operations on the data：對於資料上的操作
  - Contrains on the data：對於資料本身的限制，如星期幾，就只能填寫1-7
3. 資料能否預先定義出Data Model而區分成三種資料：

  - Structured Data：資料本身的結構、如何存取/操作、資料本身限制皆能夠被預想出在資料處理過程中會有的變動，可以固定成一個完整的Data Model來表示其資料，通常適用於可預先了解資料欄位名稱和型別的場景。
  > 结构化数据 固定的数据模型Schema，一组特定数据类型的数据组合，比如数据库表
  > • 存储在RDBMS或者Spreadsheet中• 优点:关联查询和修改简便
  > • 缺点:表结构固定，增加一个属性变更表结构困难。引入表关联会带来更多的应用的麻烦。
  > • 应用:CRM，ERP，航班火车预定系统
  - Unstructured Data：資料本身的結構、如何存取/操作、資料本身限制不太能夠被預想出資料處理過程會有的變動有哪些，因此無法構成較為固定的Data Model來表示其資料，通常適用於無法預先定義的資料之場景，在這裡由於沒有固定的Data Model，所以每一筆資料的存取、操作會隨意地進行。
  > 非结构化数据
  > • 没有固定的数据结构和类型，没有固定的数据模型schema 
  > • 存储在文件存储系统或者对象存储系统里
  > • 优点:随心所欲，各种类型
  > • 缺点:无标准，难以规范化管理，检索，查询
  > • 应用:BLOB，音视频文件，办公文档，报表，日志
  - Semi-Structured Data：資料本身既可以以預先定義好Data Model來定義資料，但又可以像Unstructured Data那樣不會完全遵從現有的Data Model來存放，可以於資料處理過程中隨意修改事先定義的Data Model來進行操作，由於這些特點使它介於Structured Data和Unstructured Data之間，所以才稱之為不完全(Semi)，換言之，Semi-Structured Data 是一種會隨著處理情況/需求而變更自身的Data Model之資料，但又可以在特定時間點表現成Structured Data或者Unstructured Data。較為有名的Semi-Structured Data案例是XML、HTML、JSON，這些資料可以以預先定義好的Data Model來存放每筆資料，同時允許資料不必按照現有Data Model來存放，透過資料本身的標籤和標籤值來去定義自己的Data Model  
  > • 有格式但没有固定的数据模型Schema，具备自描述的属性信息表达数据内容。
  > • 以键值对儿存储，JSON/XML类似的文档描述，存储在文件或者文档形数据库中，或者以图的方式存储在图数据库中，数据仓库中。
  > • 优点:扩展性好，对任意数据可以增、删、改描述信息，通过特定的算法和方法可以进行检索和分析。
  > • 缺点:特定应用场景的特定用法，不善于存储BLOB。

## 參考資料
1. [什么是结构化数据？非结构化数据？半结构化数据？](https://www.zhihu.com/question/50986354)






## 應用程式如何與資料庫溝通
1. 在關聯式資料庫中，會以資料庫所提供的SQL語言來給應用程式去調用
2. 使用與應用程式相同的語言X來調用資料庫的資料，其實現方式為object mapping，在應用程式和資料庫之間建立一個middleware，middleware 會為了易於應用程式使用而使用相同的語言，並且用那些語言X和語言中的物件概念去分別封裝資料庫所提供的操作API和資料庫所儲存的資料。
3. 對於middleware來說，關聯式資料庫：ORM (Object Relational Mapping)、文件資料庫：ODM (Object Document Mapper)，這兩者皆是用物件導向語法來操作著資料庫本身。
4. 關聯式資料庫：ORM (Object Relational Mapping)是針對應用程式和關聯式資料庫之間的middleware
5. 文件資料庫：ODM (Object Document Mapper)是針對應用程式和文件資料庫之間的middleware
6. 優點：易於使用，缺點：可能對應的語法不是那麼地精準和不那麼地有效率，甚至直接使用資料庫原生語言會比較精準和效率


## 資料庫系統儲存資料的地方
1. 早期使用磁帶(magnetic tape)
2. 現今使用SSD和記憶體這兩者間的組合，來快速處理大量資料

