---
sidebar_position: 9
---

# package.json、package-lock.json 兩者用途


## package.json
1. 以JSON形式來定義目前專案的性質、功能描述、作者、使用憑證、script、所需要的NPM套件是什麼，而這些NPM套件會是從NPM平台下載回來並設定為區域性質的套件。
2. 允許使用者使用語義化版本規則(semantic versioning)來描述每個所需套件的版本是什麼，換言之，就是用特定語法來定義每個所需套件的版本是什麼，通常這些規則只是定義專案所需的套件版本範圍是什麼
3. 可以方便與其他開發者分享以及重複使用
4. 每一次透過npm指令來對套件進行更新、下載、變更都會影響(獲取)著package.json的內容



