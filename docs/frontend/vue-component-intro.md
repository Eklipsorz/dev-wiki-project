---
sidebar_position: 8
---


# component 結構

component 是Vue 構成網頁畫面的基本元件(component)，這些元件會依據著是否包含其他元件來定義巢狀關係或者樹狀結構，在這裏會選擇構成樹狀結構，樹狀的每一個節點都皆為元件，其中最頂層的節點(root component)為 負責管理/啟動整份Vue專案以及管理剩下的節點，在開發上會稱之為application instance，而接著該節點之下的節點為視覺呈現上的元件，也就是畫面上所能夠切割的區塊和元件。 
![](https://vuejs.org/assets/components.7fbb3771.png)


### 參考資料

1. [Differences between vue instance and vue component?](https://stackoverflow.com/questions/38318536/differences-between-vue-instance-and-vue-component)

> A root Vue instance is a Vue application launcher, Vue component is an extension of the Vue instance

> An application instance won't render anything until its .mount() method is called. It expects a "container" argument, which can either be an actual DOM element or a selector string:

