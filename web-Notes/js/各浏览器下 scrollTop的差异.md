---
title: 各浏览器下 scrollTop的差异 
---
>一个元素的 scrollTop 值是这个元素的顶部到它的最顶部可见内容（的顶部）的距离的度量。当一个元素的内容没有产生垂直方向的滚动条，那么它的 scrollTop 值为0。

## 一、各浏览器下 scrollTop的差异 

### 1. IE6/7/8

   
   * 没有doctype（DTD）声明
   
        document.body.scrollTop 
		
	* 有doctype（DTD）声明
	
	    document.documentElement.scrollTop
		
### 2. IE9及以上
     
  可以使用window.pageYOffset或者document.documentElement.scrollTop 

### 3. Safari

 window.pageYOffset 与document.body.scrollTop都可以； 
 
### 4. Firefox

火狐等等相对标准些的浏览器就省心多了，直接用window.pageYOffset 或者 document.documentElement.scrollTop 

### 5. Chrome

   * window.pageYOffset


   * 没有doctype（DTD）声明
   
        document.body.scrollTop 
		
	* 有doctype（DTD）声明
	
	    document.documentElement.scrollTop


## 二、获取scrollTop的值

document.body.scrollTop与document.documentElement.scrollTop两者有个特点，就是同时只会有一个值生效。

可以使用window.pageYoffset

所有主流浏览器都支持 pageXOffset 和 pageYOffset 属性。

注意： IE 8 及 更早 IE 版本不支持该属性,但可以使用 "document.documentElement.scrollLeft" 和 "document.documentElement.scrollTop" 属性 。

兼容性写法：
```javascript?linenums
var scroll = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0;
```