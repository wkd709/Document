---
title: canvas
---

>< canvas >标签只有两个属性——width 和 height。

* Document.getElementById() 方法获取HTML < canvas > 元素的引用。
* HTMLCanvasElement.getContext() 方法获取这个元素的context——图像稍后将在此被渲染。
* fillStyle 属性设置style样式。
* fillRect() 方法（x,y,width,height),起点位置和宽高。

```js?linenums
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

ctx.fillStyle = 'green';
ctx.fillRect(10, 10, 150, 100);
```