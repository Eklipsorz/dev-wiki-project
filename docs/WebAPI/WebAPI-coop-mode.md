---
sidebar_position: 4
---

# Web API 前後端開發協作形式
在開發網頁前後分離專案時，前後兩端總是需要一份API文件來確定資料該如何調用、參數要什麼、回傳什麼、具有什麼功能，但對於整個開發流程而言，傳統上是採用**Code first概念**，API文件會在後端完成整個API邏輯層面的程式碼完工後才會公開，但這樣所花費的時間會使前端無法直接使用API來完成後續前後端的對接，進而讓整個專案因API文件拖到後頭才生成而被延宕，而且完工後的API文件也不見得能讓前端方便對接，總是需要時間去協調該文件對應的程式碼以及文件該如何修改才能解決。

為此就衍生出**API-first 概念**，來讓前後端人員以API文件來協調、確定，接著確定之後，在按照API文件打造一個假的後端伺服器以供前端使用，在這樣子的過程中，前端直接連接該假的伺服器來完成對接，從而讓前端不會再被後端的實作給延宕，而後端可以各自以自己的權責來同時完成手邊的任務，過程中很有可能因為前後端對於功能、規格上具有一定程度的衝突而修改API文件、假的伺服器，但這些因素換作是傳統概念也會遇到，所以是可以容忍的。最後當後端完成真正的伺服器時，前端只需要改連接至真正的伺服器就能直接用。

## 發生過程
當商業團隊發現可行的產品想法時，會在進一步將想法轉換成具體如何實現的產品構思和企劃書，接著傳遞至實際開發部門來探討如何實現，這時開發部分可以有兩種選擇：
  - API first：基於產品構思、企劃書來定義API文件以及Mock，隨後再來根據文件和Mock來實作，並將真正的API替換掉Mock
  - Code first：基於產品構思、企劃書來實作API實際對應的業務邏輯，接著完成後在編寫API文件
![](https://static1.smartbear.co/swagger/media/blog/design-first-vs-code-first-swaggerhub-graphic.png)
## API first
API first 又名為Contract First，主張前後端協作**以先建立API(包含其API規格/合約)為主，而隨後按照API來實作**，在這裡API文件會被視為一份有關於前後端能夠達成一致的API合約，合約形式通常會是：
  - OPEN API
  - API Blueprint
合約上的語言將會是以人類和機器皆能看懂和能夠解析的語言，為的就是一方面讓人類很好理解並開發對應合約，二方面就是能夠讓電腦解析合約語言並產生對應更為看得懂的合約形式。

API適用時機點：
  - 考量到擁有較好的Developer Experience
  - 考量到需要交付較重要的API給客戶，尤其是需要一份契約來給予雙方信任的情況
  - 考量到團隊對於API的一致了解、溝通問題：人們可以透過易懂的文件中找出程式碼潛在的問題並給予修改建議，同時也讓使用者更快速暸解如何使用。

Note:
1. Contract: 一份代表雙方在某件事上正式達成一致的正式文件
> A legal document states and explains a formal agreement bewteen two different people or groups


## Code first
Code first是以

建立在**犧牲開發者體驗**的先決條件下，

適用時機點：
  - 若以快速投入市場和敏捷性作為成功的API形式，在這種情況下搭配自動化會有助於加強Code-first帶來的敏捷和快速。
  > This is important if your go-to market strategy emphasizes speed and agility as important factors for the success of the API program. The fact that automation is much easier in the code-first approach helps strengthen this case, with a lot of libraries supporting scaffolding server code, functional testing, and deployment automation. 
  - 考量以團隊內部、架構較小的API，使用Code-first會較為理想，畢竟受到影響的層面已經縮小至內部，而非到外部。

