---
title: 常见的css问题
---

## 一、css实现图片自适应宽高

### 1.1 transform 以及常见的一些问题

#### 1.1.1 transform限制position:fixed的跟随效果

position:fixed 可以让元素不跟随浏览器的滚动条滚动，而且这种跟随连它的兄弟们 position:relative/absolute 都限制不了。

position:fixed遇到 transform就被限制了，会相当于降级变成为position:absolute。

#### 1.1.2 transform改变overflow对absolute元素的限制

##### 1.1.2.1 父级overflow;hidden,子元素 position:absolute，则overflow失效
absolute 绝对定位元素，如果含有overflow不为visible的父级元素，同时，该父级元素以及该元素绝对定位元素之间任何嵌套元素都没position为非static属性的声明，则overflow对该absolute元素不起作用。

```html?linenums
<p style="width:96px; height:96px; border:2px solid #beceeb; overflow:hidden;">
    <img src="mm1.jpg"style="position:absolute;" />
</p>
```
.![](./images/1548142191106.png)


#### 1.1.3 transform限制absolute的100%宽度大小

##### 1.1.3.1 position:absolute 并且width:100%，此元素相对于 static 定位以外的第一个父元素进行定位和width百分比
```html?linenums
<style>
.container { width: 191px; height: 191px; border: 2px solid #beceeb; }
.container img {  position: absolute; height: 191px; width: 100%; }
</style>
<div class="container">
    <img src="mm1.jpg"/>
</div>
```
.![](./images/1548142176063.png)

##### 1.1.3.2 position:absolute 并且width:100%，父级 transform: scale(1);，不管设不设置position: relative;子元素的宽度会以父级的width为百分比

```html?linenums
<style>
.container { width: 191px; height: 191px; border: 2px solid #beceeb; }
.container img {  position: absolute; height: 191px; width: 100%; }
.transform { -webkit-transform: scale(1); -ms-transform: scale(1); transform: scale(1); }
</style>
<div class="container transform">
    <img src="mm1.jpg" />
</div>
```

### 1.2