---
sidebar_position: 9
---

# package.json、package-lock.json 兩者用途


## package.json
1. 以JSON形式來定義目前專案的性質、功能描述、作者、使用憑證、script、所需要的NPM套件是什麼，而這些NPM套件會是從NPM平台下載回來並設定為區域性質的套件。
2. 允許使用者使用語義化版本規則(semantic versioning)來描述每個所需套件的版本是什麼，換言之，就是用特定語法來定義每個所需套件的版本是什麼，通常這些規則只是定義專案所需的套件版本範圍是什麼
3. 可以方便與其他開發者分享以及重複使用
4. 每一次透過npm指令來對套件進行更新、下載、變更都會影響(獲取)著package.json的內容

## package-lock.json
1. 由於package.json通常會搭配semantic versioning 來定義專案所支援的版本範圍，然而在這些範圍內有些版本存在與專案相衝突的版本，所以若單純採用package.json來下載安裝，勢必會遇到相衝突或者花更多時間去調試的問題，而為了解決這樣的問題，npm就提供package-lock.json這份檔案來鎖定每一次套件安裝時的具體版本、具體所依賴的套件是什麼，好讓使用者多了一份選擇可以安裝合適的具體版本來安裝，而非只是單純靠package.json。
2. Package-lock.json 主要紀錄安裝套件的具體版本以及套件所需的套件是什麼(依賴鏈)
3. 用途和package.json一樣，開發者可以觀看package.json和package-lock.json來得知目前專案依賴著什麼樣的模組以及透過他們來安裝
4. 每一次透過npm指令來對套件進行更新、下載、變更都會影響(獲取)著package.json的內容
5. 注意細節：
  - 當以區域形式來下達npm install時，npm會根據package-lock.json和 package.json來安裝(覆蓋)套件至目前專案下的node_modules
  - 不允許透過修改package.json來回溯至舊版本： 以往安裝錯誤的套件，可以在package.json指定正確的版本並下達npm install來安裝正確版本，但在npm5之後，就不允許npm install來回溯舊版本，只能透過npm install package@version 來安裝才能成功安裝正確版本
  - 允許透過修改package.json來升級至新版本：版本更新的話，可允許直接修改package.json並npm install，即可成功安裝以及更新package-lock.json的內容



## Semantic versioning
1. 使用特定語法來指定套件的版本範圍
2. 通常會使用^(caret)、~(tilde)、>=、<=、*、-等特定語法來定義。
3. ^ 使用方式為如下，會先匹配版本號(最左邊的數字)是A的版本並從中找到大於等於指定版本(version)，比如^1.2.3，就會先從有1.x.x版本的相同套件，並在這情況下，挑大於等於1.2.3的版本號，如1.2.3、1.4.5等版本。
```
^<version>    // version 格式會是 A.B.C，而A、B、C皆為正整數
// A為Major Version，B為Minor Version，C為Patch Version
```

4. 若^ 採用至Major Version是0時，就會改成 ~ 的形式來匹配，比如^0.2.3這樣，那麼^會如同~那樣，會只挑選0.2.x版本且大於等於0.2.3這版本。

5. ~ 使用方式為如下，會先匹配版本號(最左邊的數字和次高數字)是A.B並從中找到大於等於指定版(version)，比如~1.2.3，就會先從有1.2.x版本來找，不會是2.x.x版本，更不會是1.3.x版本，並在這情況下，挑選大於等於1.2.3的版本號，如1.2.3、1.2.5等版本。

```
~<version>   // version 格式會是 A.B.C，而A、B、C皆為正整數。
```

6. &gt;= 使用方式為如下，會尋找大於等於version的版本，比如&gt;=1.2.3，就會找到1.2.3、2.3.4等版本。

```
>=<version>  // version 格式會是 A.B.C，而A、B、C皆為正整數。
```

7. &lt;= 使用方式為如下，會尋找小於等於version的版本，比如&lt;=1.2.3， 就會找到1.2.3、1.1.1等版本

```
<=<version>  // version 格式會是 A.B.C，而A、B、C皆為正整數。
```

8. \* 使用方式為如下，會尋找任意版本，比如 &gt;= 0.0.0

```
*
```

9. \- 使用方式為如下，會尋找小於等於version1且大於等於version2的版本。

```
<version1> - <version2> 
```


### 參考資料
[package.json詳解](https://iter01.com/467432.html)



## 版本週期、階段
1. Alpha 第一階段，一般只供內部測試使用
2. Beta 第二階段，已經完成第一階段的測試並消除軟體大部分的不完善之處，但仍有一些缺陷和漏洞，一般會進一步增加測試人員而開放外面的人來測試，來找到漏洞和缺陷
3. γ - RC(Release Candidate) 第三階段，產品已經完成第一二階段，大致上已經是準備要上市的版本，但仍有一些優化處理需要做