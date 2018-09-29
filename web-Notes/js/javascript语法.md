---
title: javascript语法
---

## 一、readyState

一个==docment==的Document.readyState属性描述了文档的加载状态。

可以判断 docment 、js 、css等是否加载完成

**redayState**有以下几个值：
* **loading** / 加载中
* **interactive**  / 互动
     文档已经完成加载，文档已被解析，但是诸如图像，样式和框架之类的子资源仍在加载。
* **complete**  /  完成
     T文档和所有子资源都已完成加载。状态表示load事件即将被触发。
	 
	 
当这个属性的值发生变化时，某个对象上的==readstatechange==事件将被触发。
例如：
```js?linenums
// 模拟 load/onload 事件
document.onreadystatechange = function () {
  if (document.readyState === "complete") {
    initApplication();
  }
}
```

## 二、 document.currentScript

返回其所包含的脚本中正在被执行的 ==< script >== 元素.

**浏览器兼容性**

![](./images/1538190508333.png)