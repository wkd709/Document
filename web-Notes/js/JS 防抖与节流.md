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

![](https://user-gold-cdn.xitu.io/2019/3/12/169721dc213d832b?imageslim)


* 防抖： 任务频繁触发的情况下，只有任务触发的时间间隔超过指定间隔的时候任务才会执行。

结合上面的代码，我们可以了解到，在触发点击事件后，如果用户再次点击了，我们会清空之前的定时器，重新生成一个定时器。意思就是：这件事儿需要等待，如果你反复催促，我就重新计时！

空讲无益，show you 场景：

有个输入框，输入之后会调用接口，获取联想词。但是，因为频繁调用接口不太好，所以我们在代码中使用防抖功能，只有在用户输入完毕的一段时间后，才会调用接口，出现联想词。

代码：

**函数节流(throttle)**
>规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。

```html?linenums
<input type="text" id='debounce'>
<script>
	window.onload = function () {
		// 1、获取这个按钮/input，并绑定事件
		var myDebounce = document.getElementById("debounce");
		myDebounce.addEventListener("keyup", debounce(sayDebounce, 1000));
	}

	// 2、防抖功能函数，接受传参
	function debounce(fn,delay) {
		// 4、创建一个标记用来存放定时器的返回值
		let timeout = null;
		let last;
		return function () {
			// 5、每次当用户点击/输入的时候，把前一个定时器清除
			let now = new Date().getTime();
			if (last && now < last + delay) {

				clearTimeout(timeout);

				// 6、然后创建一个新的 setTimeout，
				// 这样就能保证点击按钮/输入后的 interval 间隔内
				// 如果用户还点击/输入了的话，就不会执行 fn 函数
				timeout = setTimeout(() => {
					fn.call(this, arguments);
					last = now;
				}, delay);
			} else {
				last = now;
				fn.call(this, arguments);
			}
		};
	}

	// 3、需要进行防抖的事件处理
	function sayDebounce() {
		// ... 有些需要防抖的工作，在这里执行
		console.log("防抖成功！值为："+this.value);
	}

</script>
```