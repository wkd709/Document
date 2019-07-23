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

**检查支持性:**

```js?linenums
var canvas = document.getElementById('tutorial');

if (canvas.getContext){
  var ctx = canvas.getContext('2d');
  // drawing code here
} else {
  // canvas-unsupported code here
}
```

## 1、绘制形状

### 1.1 绘制矩形

>不同于SVG，HTML中的元素canvas只支持一种原生的图形绘制：**矩形**。

* 绘制一个填充的矩形。fillRect(x, y, width, height)
* 绘制一个矩形的边框。strokeRect(x, y, width, height)
* 清除指定矩形区域，让清除部分完全透明。clearRect(x, y, width, height)

```js?linenums
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
    var ctx = canvas.getContext('2d');

    ctx.fillRect(25, 25, 100, 100);
    ctx.clearRect(45, 45, 60, 60);
    ctx.strokeRect(50, 50, 50, 50);
  }
}
```

![](./images/1563850785602.png)

**注解：**fillRect()函数绘制了一个边长为100px的黑色正方形。clearRect()函数从正方形的中心开始擦除了一个60 * 60px的正方形，接着strokeRect()在清除区域内生成一个50 * 50的正方形边框。


### 1.2 绘制路径

>图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。使用路径绘制图形需要一些额外的步骤。
>* 首先，你需要创建路径起始点。
>* 然后你使用画图命令去画出路径。
>* 之后你把路径封闭。
>* 一旦路径生成，你就能通过描边或填充路径区域来渲染图形。
