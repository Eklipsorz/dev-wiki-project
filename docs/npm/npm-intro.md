---
sidebar_position: 1
---

# npm 簡介


## npm scope 
定義npm package的命名空間(識別空間)並為package進一步分類，同一個package名稱可以因為scope名稱不同，而被npm辨別為不同的模組，格式會是如下，存放形式的話會是以名為scope的目錄來存放名為package目錄


```
@scope/package 
```

例子：
```
@eslint/eslintrc
```

定義一個名為eslint的scope，並存放著名為eslintrc的package，同時在實體儲存空間會是以eslint目錄來存放eslintrc目錄，該eslintrc目錄存放eslintrc程式碼相關模組

## npm init
1. 主要用來為新的npm模組或者已存在的npm模組設定(執行前所需的設定和內容)初始化，若沒添加任何參數，就是以目前目錄來初始化，其中initializer為初始化用的執行檔案，主要會是由npm-exec所建立，檔案名稱格式為create-initializer，其中initializer會是變數，當執行initializer時，就便會更新或者建立package.json和執行相關初始化內容的操作。
> npm init <initializer> can be used to set up a new or existing npm package. initializer in this case is an npm package named create-<initializer>, which will be installed by npm-exec, and then have its main bin executed -- presumably creating or updating package.json and running any other initialization-related operations.
```
npm init [--force|-f|--yes|-y|--scope]
npm init <@scope> (same as `npx <@scope>/create`)
npm init [<@scope>/]<name> (same as `npx [<@scope>/]create-<name>`)
```

e.g., 

```
npm init foo -> npm exec create-foo
npm init @usr/foo -> npm exec @usr/create-foo
npm init @usr -> npm exec @usr/create
```



### scope 用途
1. 進一步對package分類，同一個package名稱可以因為scope名稱不同，而被npm辨別為不同的模組
2. 一個scope可以存放多個package

參考資料：
[npm scope的作用](https://blog.csdn.net/hbiao68/article/details/104177047)

## npm executable
npm 模組的執行檔案(以指令cmd)，執行方式為如下，皆以npm來執行對應cmd
```
npm exec <cmd>
npx <cmd>
```

預設上的執行檔案位置會依據全域和區域預設目錄來找到對應執行檔案名稱：
  - 全域(global mode)，會是指prefix/bin，prefix為專門在全域存放npm模組的node_module所在
  - 區域(local mode)，會是指目前目錄所在的./node_modules/.bin，


## npx vs npm exec
1. npx 為 npm 5.2額外開發的指令，而npm exec 為 npm 7.0額外開發的指令，當執行某個cmd時會以npm的形式來執行cmd
2. npx 和 npm exec 兩者皆為執行npm模組的執行檔案，格式分別如下
```
npx <cmd>
npm exec <cmd>
```
3. 不同的地方為：
  - npx 執行 cmd 前，若要替npx這指令添加flag或者option的話，就在cmd語句之前添加flag或者option，比如若要替npx指定--package參數，那就會在foo這指令之前放
  ```
  npx --package=@npmcli/foo foo@latest bar 
  // 等同於執行
  foo bar
  ```
  若是改放foo之後的話，會直接被當作npx沒任何參數使用，並且指定--package這參數為foo指令的參數來執行
  ```
  npx foo@latest bar --package=@npmcli/foo
  // 等同於執行以下，foo指令以bar和--package作為參數來執行
  // foo bar --package=@npmcli/foo
  ```
  - npm exec 執行 cmd之前，會先解析參數，並直接當作是npm exec這指令所要使用的參數，而非cmd所要使用的
  ```
  npm exec foo@latest bar --package=@npmcli/foo
  // 等同於
  foo bar
  ```
  若是添加--的話，則是告知npm為停止解析後續參數為npx exec的參數

  ```
  npm exec -- foo@latest bar --package=@npmcli/foo
  // 等同於
  foo bar --package=@npmcli/foo
  ```


[npm-exec](https://docs.npmjs.com/cli/v8/commands/npm-exec)
[npm exec: A new way to run commands from npm packages](https://www.eckher.com/c/21gvqjs4vg)