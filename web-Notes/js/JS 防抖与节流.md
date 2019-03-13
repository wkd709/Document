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
				clearTimeout(timeout);
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

以上代码，我们在不端输入的时候，ajax会按照我们设定的时间，每1s执行一次，当与上一次间隔时间比较长时会立即执行，这个同一个时间段只执行一次。

**总结**

* 函数防抖和函数节流都是防止某一时间频繁触发，但是这两兄弟之间的原理却不一样。
* 函数防抖是某一段时间内只执行一次，而函数节流是间隔时间执行。

**结合应用场景**

* debounce
    * search搜索联想，用户在不断输入值时，用防抖来节约请求资源。
    * window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次
* throttle
    *  鼠标不断点击触发，mousedown(单位时间内只触发一次)
    *  监听滚动事件，比如是否滑到底部自动加载更多，用throttle来判断

### 2.2 节流

```html?linenums
<button id="throttle">点我节流！</button>
<script>
	window.onload = function () {
		// 1、获取按钮，绑定点击事件
		var myThrottle = document.getElementById("throttle");
		myThrottle.addEventListener("click", throttle(sayThrottle));
	}

	// 2、节流函数体
	function throttle(fn) {
		// 4、通过闭包保存一个标记
		let canRun = true;
		return function () {
			// 5、在函数开头判断标志是否为 true，不为 true 则中断函数
			if (!canRun) {
				return;
			}
			// 6、将 canRun 设置为 false，防止执行之前再被执行
			canRun = false;
			// 7、定时器
			setTimeout(() => {
				fn.call(this, arguments);
				// 8、执行完事件（比如调用完接口）之后，重新将这个标志设置为 true
				canRun = true;
			}, 1000);
		};
	}

	// 3、需要节流的事件
	function sayThrottle() {
		console.log("节流成功！");
	}
</script>
```
![](https://user-gold-cdn.xitu.io/2019/3/12/169721de93ace1d0?imageslim)

**节流：**  指定时间间隔内只会执行一次

**那么，节流在工作中的应用？**

* 懒加载要监听计算滚动条的位置，使用节流按一定时间的频率获取。

* 用户点击提交按钮，假设我们知道接口大致的返回时间的情况下，我们使用节流，只允许一定时间内点击一次。

这样，在某些特定的工作场景，我们就可以使用防抖与节流来减少不必要的损耗。