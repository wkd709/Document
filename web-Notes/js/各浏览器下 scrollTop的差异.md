---
title: 各浏览器下 scrollTop的差异 
---
>一个元素的 scrollTop 值是这个元素的顶部到它的最顶部可见内容（的顶部）的距离的度量。当一个元素的内容没有产生垂直方向的滚动条，那么它的 scrollTop 值为0。

## 一、各浏览器下 scrollTop的差异 

### 1. IE6/7/8

   
   * 没有doctype声明
   
        document.body.scrollTop 
		
	* 有doctype声明
	
	    document.documentElement.scrollTop
### 2. IE9及以上