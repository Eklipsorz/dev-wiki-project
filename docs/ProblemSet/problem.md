---
sidebar_position: 1
---

# 日期不可考的問題集


https://m.youtube.com/watch?v=jfYWwQrtzzY

1. Anemic domain model是什麼？
但為了不把詞完全偏向商業，所以我才用業務邏輯來中翻譯
另外也有人特意把model 的業務邏輯轉移至其他層級，就是只把model當作資料管理，而這樣的model 叫做Anemic domain model


module.exports

https://miahsuwork.medium.com/第三週-node-js-基礎-module-exports-和-require-2f9f6915d9f0
https://blog.hellojcc.tw/module-exports-vs-exports-in-node-js/
https://dwatow.github.io/2018/02-13-js-module-require-exports/

2. data-intensive real-time - 直接翻譯是「數據密集以及實時」的應用 是什麼
3. Command -v nvm 指令是什麼？
4. Yarn 是什麼

Yarn 是一個新的套件管理工具
https://ithelp.ithome.com.tw/articles/10206917


6. 函式可以允許同名且參數不同，而呼叫時會依照宣告的順序而改變，比如test()放後面，test(1)就回傳nothing，而換test(a)時就是回傳1
function test(a) {

  console.log(a)
}

function test() {

  console.log('nothing')
}
test(1)

Marjor、minor、patch版本是什麼？

Given a version number MAJOR.MINOR.PATCH, increment the:
1. MAJOR version when you make incompatible API changes,
2. MINOR version when you add functionality in a backwards compatible manner, and
3. PATCH version when you make backwards compatible bug fixes.
Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

了解package.json和package-lock.json之間的差異和功能 (不過裡頭還有好多種類的樹，什麼Physical Tree、什麼logical tree，有點想爬，只能放到後面爬了)


npm uninstall &lt;package&gt;：移除指定package以及該package所依賴的package也會跟著刪掉，但有些套件會不會因為其他套件正在用而不刪掉

1. Npm下載代表這些檔案的套件，然後重新以webpack來打包來方便給客戶端下載(待學習)
2. middleware function  是什麼

