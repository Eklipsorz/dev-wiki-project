---
sidebar_position: 3
---

# 第三週收穫新知


## 表格若沒有添加tbody的話
若像以下語法沒添加tbody的話，

```
  <table>
    <tr>
      <td>The table body</td>
      <td>with two columns</td>
    </tr>
  </table>
```

渲染後的結果，瀏覽器會自動添加tbody標籤

![](https://res.cloudinary.com/dqfxgtyoi/image/upload/v1632929746/blog/week3workshop/autoAddTbody_ls1p5x.png)

## nth-child 偽類



## 事件處理器內含的函式

其函式被另一個夾雜event參數的函式包覆著：

```

elemenet.addEventListener('click', functName1)

function functName (evet) {
  function functName1 (){

  }
}

```