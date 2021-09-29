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
1. 是一種偽類(pseudo class)
2. 在兄弟節點中挑選指定位置的元素，具有兩種形式：第一種的x是空格，代表著會在指定範圍內或者整份網頁中從兄弟節點中找到符合n的位置的兄弟節點來搭配其樣式，而第二種除了會看其兄弟節點是否滿足n以外，還會看是否符合selector所指定的選擇器，都符合才會以該樣式來渲染
```
x:nth-child(n) {

}

selector:nth-child(n) {

}

```

例子1：在這裡會先看整份網頁有哪些兄弟節點，並且尋找第七個兄弟節點，接著再看看這個兄弟節點是否為ul的子元素，若是的話則顏色會以lime來渲染，在這裡會是清單項目中的第七項。

CSS內容如下：
ul :nth-child(7) {
  color: lime;
}

HTML內容如下：
```
<ul>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
	<li>7</li>
	<li>8</li>
	<li>9</li>
	<li>10</li>
</ul>
```

例子1：在這裡會先看整份網頁有哪些兄弟節，並且尋找第二個兄弟節點，接著看看這兄弟節點是否為p元素，若都是的話，會擁有黃色背景，在這裡會是第二段p元素被染成黃色。

CSS內容如下：
```
p:nth-child(2) {
  background: yellow;
}
```

HTML內容如下：
```
<p>The first paragraph.</p>
<p>The second paragraph.</p>
<p>The third paragraph.</p>
<p>The fourth paragraph.</p>
```

3. nth-child(n)括號中的n是an+b的形式，ab可以是正負值，甚至不填入ab值，若只有b值的話，會從兄弟節點挑選第b個，而若有an+b的話，那麼就會挑選第b、a+b、2a+b、.....、2n+b個兄弟節點，另外若結果數值<=0就等同挑選不出來任何元件。
```
a x 0 + b = b
a x 1 + b = a + b
a x 2 + b = 2a + b
.
.
.
a x n + b = 2n + b
```

4. :nth-child(n):nth-child(n)可以設定上下限，比如說n+8可以挑選第8個兄弟節點(含)之後的兄弟節點，而-n+15則是挑選第1至15個兄弟節點，這兩者會取共同的地方也就是挑選第8~15個兄弟節點來渲染，但還要滿足是否為p元素。

```
p:nth-child(n+8):nth-child(-n+15)
```

## 事件處理器內含的函式

其函式被另一個夾雜event參數的函式包覆著：

```

elemenet.addEventListener('click', functName1)

function functName (evet) {
  function functName1 (){

  }
}

```