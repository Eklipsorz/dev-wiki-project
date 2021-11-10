---
sidebar_position: 5
---

# 學期2-2 第四週workshop

## SRP
1. 英文名稱為Single Responsiblity Principle
2. 指的是每一個程式模組、類別、函式都只需要負責整個程式上的單一部分或者單一功能。



## drawCicle和drawCross的改良
1. 原形為如下，但為了盡可能讓畫圖功能緊縮在一個函式，而非n個圖形就以n種不同函式來渲染，變更後會是以最下面為主

```
// origin: 
function drawCircle(cell) {
  cell.innerHTML = "<div class='circle' />"
}

function drawCross(cell) {
  cell.innerHTML = "<div class='cross' />"
}

// modification

function draw(cell, shape) {
  cell.innerHTML = `<div class=${shape} />`
}


```




