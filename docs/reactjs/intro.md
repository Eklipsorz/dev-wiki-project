---
sidebar_position: 1
---

# react 簡介和使用方式 

React 草稿

React apps: elements and components.


React is a JavaScript library,



these three points should get you most of the way:

1. We define variables with let and const statements.
2. We use the class keyword to define JavaScript classes. There are two things worth remembering about them. Firstly, unlike with objects, you don't need to put commas between class method definitions. Secondly, unlike many other languages with classes, in JavaScript the value of this in a method depends on how it is called.
3. We sometimes use => to define "arrow functions".



JSX （JavaScript XML)

JSX allows us to write HTML in React.
JSX makes it easier to write and add HTML in React.

JSX, and it is a syntax extension to JavaScript

JSX 
1. 使用JavaScript 來代替HTML、CSS來描述網頁外觀
2. 是一種JavaScript為主的延伸語法，能夠以JS來寫出對應的HTML
3. 

React使用稱為JSX（JavaScript和XML）的HTML-in-JavaScript語法


React 團隊主要推薦以下的方案：
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
