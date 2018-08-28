---
title: 各浏览器下 scrollTop的差异 
---
>个元素的 scrollTop 值是这个元素的顶部到它的最顶部可见内容（的顶部）的距离的度量。

## 一、各浏览器下 scrollTop的差异 

### 1. **IE6/7/8**： 
   
   * 没有doctype声明
   
        document.body.scrollTop 
		
	* 有doctype声明
	
	    document.documentElement.scrollTop