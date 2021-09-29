---
sidebar_position: 5
---

# 提交版本準則

## 撰寫 commit 訊息的目的
1. 讓開發者、維護者能夠更快理解這次版本更新內容(含這次版本出現的bug、bug修正內容、增加的功能)
2. 讓負責寫release note的開發者能夠更快速掌握這次更新內容是什麼
## 什麼時候要commit
1. 本身如同coding style那樣沒有絕對性標準，但必須依照當時參與團隊能夠接受的形式
2. 協作者可以根據commit來得知這份專案的開發歷史、開發過程、結果。
3. 基於協作者可以看到的內容，我們可以考慮三種面向來思考什麼時候要commit：粒度(granularity)、相關性、獨立性，粒度指的是每一次提交的內容有多大，而相關性則是每一次提交的內容是否相關，獨立性則是指的是每一次提交的內容是否真是獨立的，藉此打造出粒度適當、相關性高、獨立性高的提交
4. 另外也有一派的人提出先寫commit訊息再來開發，讓開發方向專注於commit所寫的目標。

## 如何撰寫 commit 訊息
1. commit訊息的語言使用是依據開發團隊來決定，不過通常會是以英文為主
2. 使用祈使句(命令他人)：用動詞開頭，不需要時態或者複數。
3. 不加句點。
4. 控制在50字以內，不得超過72個字母。
5. 開頭大寫。
6. 其他資訊：commit訊息可以換行，第一行可以先寫主要內容，第二行再把細節寫下，或者叫人去看對應文件。

## 別人期待從commit訊息了解
1. 為甚麼要寫這些程式碼
2. 具體修改了什麼問題
3. 具體增加/移除了什麼功能
4. 具體發生了什麼樣的問題
5. 內容是在什麼樣環境下或者時間限制下完成的(特殊案例)


## 錯誤提交訊息例子
1. 毫無意義的文字內容，比如一堆亂數、無法看出內容的英文字母
2. 只有自己看得懂的訊息內容

## 參考資料
1. [conventional commits](https://www.conventionalcommits.org/en/v1.0.0-beta.3/)
2. [Writing good commit messages](https://github.com/erlang/otp/wiki/writing-good-commit-messages)
3. [Commit Message Guidelines](https://gist.github.com/robertpainsi/b632364184e70900af4ab688decf6f53)
4. [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
5. [louie.lu 中譯](https://blog.louie.lu/2017/03/21/%E5%A6%82%E4%BD%95%E5%AF%AB%E4%B8%80%E5%80%8B-git-commit-message/)