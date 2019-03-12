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

## 二、区别各种IE浏览器CSS写法

### 2.1 先介绍css hack

#### 2.1.1 css hack概念与书写顺序
**什么是CSS hack？**

>由于不同厂商的流览器或某浏览器的不同版本（如IE,Firefox/Safari/Opera/Chrome等），对CSS的支持、解析不一样，导致在不同浏览器的环境中呈现出不一致的页面展现效果。
>这时，我们为了获得统一的页面效果，就需要针对不同的浏览器或不同版本写特定的CSS样式，我们把这个针对不同的浏览器/不同版本写相应的CSS code的过程，叫做CSS hack!


**CSS hack书写顺序**

>一般是将适用范围广、被识别能力强的CSS定义在前面。

#### 2.1.2 CSS hack分类

**CSS Hack大致有3种表现形式：**

* CSS属性前缀法
* 选择器前缀法
* IE条件注释法（即HTML头部引用if IE）Hack
   实际项目中CSS Hack大部分是针对IE浏览器不同版本之间的表现差异而引入的。
   
   属性前缀法(即类内部Hack)：例如 IE6能识别下划线" _ "和星号"  *  "，IE7能识别星号"   *   "，但不能识别下划线" _ " ，IE6~IE10都认识"\9"，但firefox前述三个都不能认识。
   
   选择器前缀法(即选择器Hack)：例如 IE6能识别   * html .class{}， IE7能识别*+html .class{}或者*:first-child+html .class{}。
   
   IE条件注释法(即HTML条件注释Hack)：针对所有IE(注：IE10+已经不再支持条件注释)： IE浏览器显示的内容 ，针对IE6及以下版本： 只在IE6-显示的内容 。这类Hack不仅对CSS生效，对写在判断语句里面的所有代码都会生效。
   
**CSS hack方式一：条件注释法**

这种方式是IE浏览器专有的Hack方式，微软官方推荐使用的hack方式。

```js?linenums
//只在IE下生效
<!--[if IE]>
//这段文字只在IE浏览器显示
<![endif]-->


//只在IE6下生效
<!--[if IE 6]>
//这段文字只在IE6浏览器显示
<![endif]-->


//只在IE6以上版本生效
<!--[if gte IE 6]>
//这段文字只在IE6以上(包括)版本IE浏览器显示
<![endif]-->


//只在IE8上不生效
<!--[if ! IE 8]>
//这段文字在非IE8浏览器显示
<![endif]-->


//非IE浏览器生效
<!--[if ! IE]>
//这段文字只在非IE浏览器显示
<![endif]-->
```

**CSS hack方式二：类内属性前缀法**

属性前缀法是在CSS样式属性名前加上一些只有特定浏览器才能识别的hack前缀，以达到预期的页面展现效果

* IE hack 技术

	```css?linenums
	_width: 400px;            /* _是针对IE6*/
	+width: 300px;           /* +是针对IE6、IE7*/
	*width: 400px;            /*  *是针对IE6、IE7*/
	width: 200px\9;          /* \9是针对IE6 IE7 IE8 IE9 IE10*/
	width: 100px\0;          /* \0是针对IE8 IE9 IE10 IE11 */
	```

* 浏览器内核与前缀
	![](./images/1552361726009.png)
	
**CSS hack方式三：选择器前缀法**

选择器前缀法是针对一些页面表现不一致或者需要特殊对待的浏览器，在CSS选择器前加上一些只有某些特定浏览器才能识别的前缀进行hack。

目前最常见的是：

*html	*前缀只对IE6生效
*+html	*+前缀只对IE7生效
@media screen\9 {body { background: gray; }}	只对IE6/7生效
@media \0screen {body { background: red; }}	只对IE8有效
@media \0screen\,screen\9 {body { background: blue; }}	只对IE6/7/8有效
@media screen\0 {body { background: green; }}	只对IE8/9/10有效
@media screen and (min-width:0\0) {body { background: gray; }}	只对IE9/10有效
@media screen and (-ms-high-contrast: active), (-ms-high-contrast: none)  {body { background: orange; }}	只对IE10有效

**CSS3选择器结合JavaScript的Hack**

    我们用IE10进行举例：

        由于IE10用户代理字符串（UserAgent）为：Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Trident/6.0)，所以我们可以使用Javascript将此属性添加到文档标签中，再运用CSS3基本选择器匹配。
		

```js?linenums
//JavaScript代码:
var htmlObj = document.documentElement;
htmlObj.setAttribute('data-useragent',navigator.userAgent);
htmlObj.setAttribute('data-platform', navigator.platform );
```

```css?linenums
//CSS3匹配代码：
html[data-useragent*='MSIE 10.0'] #id {
        color: #F00;
}
```

#### 2.1.3 CSS hack利弊

一般情况下，我们尽量避免使用CSS hack，但是有些情况为了顾及用户体验实现向下兼容，不得已才使用hack。

