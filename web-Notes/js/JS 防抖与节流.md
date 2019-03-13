---
title: JS 防抖与节流
---

>本文涉及知识点：
>防抖与节流
>重绘与回流
>浏览器解析URL
>DNS域名解析
>TCP三次握手与四次挥手
>浏览器渲染页面

## 一、原因

在工作中，我们可能碰到这样的问题：
* 用户在搜索的时候，在不停敲字，如果每敲一个字我们就要调一次接口，接口调用太频繁，给卡住了。
* 用户在阅读文章的时候，我们需要监听用户滚动到了哪个标题，但是每滚动一下就监听，那样会太过频繁从而占内存，如果再加上其他的业务代码，就卡住了

所以，这时候，我们就要用到 防抖与节流 了。

那么，讲到 防抖与节流，我们可以顺带探秘下 重绘与回流。

## 二、防抖与节流

### 2.1 防抖

```html?linenums
<button id="debounce">点我防抖！</button>
<script>
	window.onload = function () {
		// 1、获取这个按钮，并绑定事件
		var myDebounce = document.getElementById("debounce");
		myDebounce.addEventListener("click", debounce(sayDebounce));
	}

	// 2、防抖功能函数，接受传参
	function debounce(fn) {
		// 4、创建一个标记用来存放定时器的返回值
		let timeout = null;
		return function () {
			// 5、每次当用户点击/输入的时候，把前一个定时器清除
			clearTimeout(timeout);
			// 6、然后创建一个新的 setTimeout，
			// 这样就能保证点击按钮后的 interval 间隔内
			// 如果用户还点击了的话，就不会执行 fn 函数
			timeout = setTimeout(() => {
				// console.log(this, arguments);
				fn.call(this, arguments);
			}, 1000);
		};
	}

	// 3、需要进行防抖的事件处理
	function sayDebounce() {
		console.log(this);
		// ... 有些需要防抖的工作，在这里执行
		console.log("防抖成功！");
	}

</script>
```

