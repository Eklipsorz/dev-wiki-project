---
sidebar_position: 1
---

# react 簡介和使用方式 

React 草稿

React apps: elements and components.


React is a JavaScript library,



> these three points should get you most of the way:
1. We define variables with let and const statements.
2. We use the class keyword to define JavaScript classes. There are two things worth remembering about them. Firstly, unlike with objects, you don't need to put commas between class method definitions. Secondly, unlike many other languages with classes, in JavaScript the value of this in a method depends on how it is called.
3. We sometimes use => to define "arrow functions".

JSX 語法的重點在於允許我們在 JS 的檔案中使用 HTML 的標籤，並且使用 JSX 語法建立的是「一個 React 的 element」，此外這樣的標籤語法比起 HTML，更貼近於 JavaScript。

## XML -> HTML
1. XML 衍生出 HTML

## JSX 
1. 全名為JavaScript and XML ，在這裡只會強調JavaScript XML
> JSX stands for JavaScript XML. With React, it's an extension for XML-like code for elements and components. Per the React docs and as you mentioned:
2. 在React中，它是一種方便支援XML/XML-Like語法能夠在JS被正常解析和解讀的延伸語法：
  - why so called XML? why not use JavaScript and HTML: 原本想法是想從JS層面來撰寫對應網頁的HTML以方便輸入資料來衍生出對應資料的網頁內容；所以實際實現方式為在JS語法上允許HTML語法與JS混著使用，而HTML原本是從XML演化而來的：以帶有自描述資料特性的標籤以及其標籤所建構的巢狀結構而建立出來的網頁畫面，只是在這裡HTML是以元件呈現為目標而發展，而XML則是以資料傳遞為目標發展，但也可以朝著HTML那樣方法，所以為了進一步讓更好讓資料傳遞能夠跟網頁呈現結合，而採用可以兼具這兩者特性的XML，因此選定XML語法來與JavaScript混著使用，而非使用HTML

  - 適合表示UI結構的表達方式為S-expression和XML，不過後者其實也算是S-expression。
  > XML，很多人一提XML就生理不适。选XML有两点，一个是HTML本来就是XML，因此你用模板也是一样的，除非搞Jade之类的其他模板。另外XML本身就是一个难写版的S-expression，非常适合用来表示这种UI结构。至于为啥不用S-exp，因为原语是HTML不是Lisp。

  [说说对react中JSX语法的理解?](https://www.zhihu.com/question/265784392)

3. JSX並不是正式官方語言，實際上JavaScript和XML兩者混著使用的語法，必須依賴 能夠將任意語言轉換成JavaScript語言或者高版本的JS語言轉換成低版本的JS語言之 **babel轉譯器**
> https://ithelp.ithome.com.tw/articles/10238640
 然而若單純允許HTML語法混著使用，其功能性和開發性受限於HTML語法
 
4. JSX 定位在畫面呈現上的模板，而React沒太強求一定要使用JSX



## React 團隊主要推薦以下的方案：
* 如果你正在學習 React 或建立全新的 single-page 應用程式，請使用 Create React App。
* 如果你正在建立一個使用 Node.js 的 server-rendered 網頁，請使用 Next.js。
* 如果你正在建立一個靜態內容的網頁，請使用 Gatsby。
* 如果你正在建立一個 component 函式庫或與現存程式碼倉庫進行接軌，請使用更靈活的 Toolchain。

Create-React-App
1. 並不會處理 backend 邏輯或資料庫；
2. 它只會建立一個 frontend build pipeline，以便你配合任何 backend 來使用。
3. 基本上，它是使用 Babel 和 webpack，但你不需要了解任何關於它們的細節。

如果你正在學習 React 或建立全新的 single-page 應用程式，請使用 Create-React App
create-react-app



npm install -g create-react-app


npx create-react-app my-app
cd my-app
npm start // 等同npm run start ，主要開啟react server來幫助開發


Vscode 套件
1. Eslint
2. JS JSX Snippets

## element 構建方式

react 應用程式的最小單位是element，而element相當於DOM結構上的node概念，透過多個element可以組成一個大元件(component)，在JSX上會以指派的形式來定義一個元素和其元素的內容，會有以下形式，而判定被指定內容是否element內容的邏輯為
  - 檢查當前指派內容是否與定義element的程式碼為同一行，若同一行，則後續的指派內容會繼續被系統解析為同一個element的內容；若沒有的話，就往下檢查
  - 檢查當前指派內容是否有圓括號()和中括號\[\]，若有的話，就以括號所囊括著的內容為element的內容
  - 若都沒有的話，則無法將指派內容構成其元素的內容
```
// 定義一個名為tag1的element 和一個名為tag2的element
// 方式1：將指派的內容放在const element的同一行
const element = <tag1> content <tag1>
  <tag2> content <tag2>;

// 方式2：用圓括號囊括著哪些內容為element的內容，並且允許指派內容以換行形式來呈現
const element = (
  <tag1> content <tag1>
  <tag2> content <tag2>;
)
// 方式2：用中括號囊括著哪些內容為element的內容，並且允許指派內容以換行形式來呈現
const element = [
  <tag1> content <tag1>
  <tag2> content <tag2>;
]
```
建議以圓括號來囊括著要被指派給元素的內容，第一、可以滿足開發者的通用習慣(中括號會被時常當作是陣列)，第二、同樣也能換行解析

參考資料為：
  [React之JSX里render中return方法添加括号()或者[]](https://www.cnblogs.com/fightjianxian/p/12350083.html)


Note:
1. 若沒添加換行用的括號，會直接以當前內容為該element的主要內容，比如說以下指派內容會直接以與const element保持同一行的換行或者空白來當作element的內容，並不會把tag1和tag2視為element的內容
```
const element = 
  <tag1> content <tag1>
  <tag2> content <tag2>
```