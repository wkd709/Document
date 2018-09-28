---
title: 常见的web问题
---

## 一、说说 cookies，sessionStorage 、 localStorage 你对它们的理解？

* cookie是网站为了表示用户身份而存储在用户本地终端上的数据（通常经过加密），cookie数据始终在同源的http请求中携带，也会在浏览器与服务器间来回传递。
* sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。
* 大小：cookie数据大小不能超过4k。sessionStorage和localStorage虽然也有存储大小的限制，但是比cookie大得多，可以达到5M或者更大。
* 时效：localStorage存储持久数据，浏览器关闭后数据不丢失除非用户主动删除数据或者清除浏览器/应用缓存；sessinStorage数据在当前浏览器窗口关闭后自动删除。cookie设置的cookie过期时间之前一直有效，即使窗口或者浏览器关闭
* 部分面试官可能还会再深入一些：

> 1）、如何让cookie浏览器关闭就失效？   不对cookie设置任何正、负或者0时间的即可。
> 2）、sessionStroage在浏览器多窗口之间（同域）数据是否互通共享？   不会，都是独立的localStorage会共享；
> 3）、能让localStorage也跟cookie一样设置过期时间？   可以的，在存储数据时，也存储一个时间戳，get数据之前，先拿当前时间跟你之前存储的时间戳做比较 。

## 二、简述一下你对HTML语义化的理解 ？
* 语义化是指根据内容的类型，选择合适的标签(代码语义化)，即用正确的标签做正确的事情；
* html语义化让页面的内容结构化，结构更清晰，有助于浏览器、搜索引擎解析对内容的抓取；
* 语义化的HTML在没有css的情况下也能呈现较好的内容结构与代码结构；
* 搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO；

## 三、:before 和 :after 两伪元素，平时都是使用双冒号还是单冒号？有什么区别？以及它们的作用？

* 单冒号（ : ）用于CSS3伪类，双冒号（::）用于CSS3伪元素。（伪元素由双冒号和伪元素名称组成）；
* 双冒号是在当前规范中引入的，用于区分伪类和伪元素。不过浏览器需要同时支持旧的已经存在的伪元素写法， 比如:first-line、:first-letter、:before、:after等， 而新的在CSS3中引入的伪元素则不允许再支持旧的单冒号的写法;
* 想让插入的内容出现在其它内容前，使用::before，之后则使用::after； 
* 在代码顺序上，::after生成的内容也比::before生成的内容靠后。 如果按堆栈视角，::after生成的内容会在::before生成的内容之上;

**伪类**

>是添加到选择器的关键字，指定要选择的元素的特殊状态。

例如，:hover 可被用于在用户将鼠标悬停在按钮上时改变按钮的颜色。

伪类连同伪元素一起，他们允许你不仅仅是根据个文档DOM树中的内容对元素应用样式，而且还允许你根据诸如像导航历史这样的外部因素来应用样式（例如 ==:visited==），同样的，可以鞥见内容的状态（例如在一些表单元素上的 ==:checked==），或者鼠标的位置（例如: ==:hover== 让你知道鼠标是否在一个元素上悬浮）来应用样式。

<div class='note'>
注意： 与伪类相反，pseudo-elements 可被用于为一个元素的 特定部分 应用样式。
</div>

* {
  font-family: -apple-system,PingFang SC,Hiragino Sans GB,Microsoft YaHei,Helvetica Neue,Arial,sans-serif;
    color: #333;
    text-rendering: optimizeLegibility;
    letter-spacing: 0.5px;
}
.note {
   padding: 10px 0 ;
    padding-left: 30px;
    background: #fff3d4;
    border-color: #f6b73c;
    border-left-width: 5px;
    border-left-style: solid;
}
blockquote {
    background: #eee;
    border-left: 5px solid #3d7e9a;
    padding: 10px;
    
}