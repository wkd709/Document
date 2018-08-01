---
title: HTML head 头标签
tags:
--- 
>HTML head 头部分的标签、元素有很多，涉及到浏览器对网页的渲染，SEO 等等，而各个浏览器内核以及各个国内浏览器厂商都有些自己的标签元素,这就造成了很多差异性。移动互联网时代，head 头部结构，移动端的 meta 元素，显得更为重要。了解每个标签的意义，写出满足自己需求的 head 头标签。

----------


## 一、 DOCTYPE
>DOCTYPE(Document Type)，该声明位于文档中最前面的位置，处于 html 标签之前，此标签告知浏览器文档使用哪种 HTML 或者 XHTML 规范。<br/>
>DTD(Document Type Definition) 声明以 <!DOCTYPE> 开始，不区分大小写，前面没有任何内容，如果有其他内容(空格除外)会使浏览器在 IE 下开启怪异模式(quirks mode)渲染网页。公共 DTD，名称格式为注册//组织//类型 标签//语言,注册指组织是否由国际标准化组织(ISO)注册，+表示是，-表示不是。组织即组织名称，如：W3C。类型一般是 DTD。标签是指定公开文本描述，即对所引用的公开文本的唯一描述性名称，后面可附带版本号。最后语言是 DTD 语言的 ISO 639 语言标识符，如：EN 表示英文，ZH 表示中文。XHTML 1.0 可声明三种 DTD 类型。分别表示严格版本，过渡版本，以及基于框架的 HTML 文档。
------
最新 HTML5 推出更加简洁的书写，它向前向后兼容，推荐使用。

==<!doctype html>==

在 HTML中 doctype 有两个主要目的。
  * 对文档进行有效性验证。
  
    它告诉用户代理和校验器这个文档是按照什么 DTD 写的。这个动作是被动的，每次页面加载时，浏览器并不会下载 DTD 并检查合法性，只有当手动校验页面时才启用。
	
  * 决定浏览器的呈现模式
    
	对于实际操作，通知浏览器读取文档时用哪种解析算法。如果没有写，则浏览器则根据自身的规则对代码进行解析，可能会严重影响 html 排版布局。浏览器有三种方式解析 HTML 文档。 * 非怪异（标准）模式 * 怪异模式 * 部分怪异（近乎标准）模式 关于IE浏览器的文档模式，浏览器模式，严格模式，怪异模式，DOCTYPE 标签，可详细阅读模式？标准！的内容。

**langth属性**

 简体中文
 ```html
 <html lang="zh-cmn-Hans">
 ```
 繁体中文
 ```html
 <html lang="zh-cmn-Hant">
 ```
 常用写法：
  ```html
 <html lang="zh">
 
 <html lang="zh-CN">
 ```
 **favicon icon**
 ```html
 <link rel="shortcut icon" type="image/ico" href="/favicon.ico" />
 <!-- 添加 favicon icon -->
 ```
## 二、 meta 元素 （常见的一些用法）

>标签提供关于html文档的元数据。元数据不会显示在页面上，但是对于机器是可读的。它可用于浏览器（如何显示内容或者重新加载页面），搜索引擎（关键词），或其他web服务。
---------------
meta标签共两个属性，分别是http-equiv属性和name属性。

### 1. name属性

name属性主要用于描述网页。比如网页的关键词，叙述等。与之对应的属性值为content，content中的内容是对name填入类型的具体描述，便于搜索引擎抓取。
meta标签中name属性语法格式为：

```html?linenums
<meta name ='参数' content='具体的描述'>
```
其中name属性共有以下几种参数（A-C为常用属性）

* A. keywords(关键字）

   说明：用于告诉搜索引擎，你网页的关键字。
   举例：
   ```html?linenums
   <meta name="keywords" content="Lxxyx,博客，文科生，前端">
   ```
 * B. description(网站内容的描述)
 
   说明：用于告诉搜索引擎，你网站的主要内容。
    ```html?linenums
    <meta name="description" content="文科生，热爱前端与编程。目前大二，这是我的前端博客">
    ```
* C. robots(定义搜索引擎爬虫的索引方式)

    说明：robots用来告诉爬虫哪些页面需要索引，哪些页面不需要索引。
	content的参数有all,none,index,noindex,follow,nofollow。默认是all。
    举例：
	 ```html?linenums
    <meta name="robots" content="none">
    ```
	具体参数如下：
	* 1.none : 搜索引擎将忽略此网页，等价于noindex，nofollow。
	* 2.noindex : 搜索引擎不索引此网页。
	* 3.nofollow: 搜索引擎不继续通过此网页的链接索引搜索其它的网页。
	* 4.all : 搜索引擎将索引此网页与继续通过此网页的链接索引，等价于index，follow。
	* 5.index : 搜索引擎索引此网页。
	* 6.follow : 搜索引擎继续通过此网页的链接索引搜索其它的网页。
 * D. author(作者)
 
     说明：用于标注网页作者
	举例：
	```html
	<meta name="author" content="Lxxyx,841380530@qq.com">
	```
 * E. generator(网页制作软件)
 * F. copyright(版权)
 
	说明：用于标注版权信息
	举例：
    ```html
		<meta name="copyright" content="Lxxyx"> //代表该网站为Lxxyx个人版权所有。
	```
 * G. revisit-after (搜索引擎爬虫重访时间)
 
    说明：如果页面不是经常更新，为了减轻搜索引擎爬虫对服务器带来的压力，可以设置一个爬虫的重访时间。如果重访时间过短，爬虫将按它们定义的默认时间来访问。
   举例：
	```html
	<meta name="revisit-after" content="7 days" >
	```
 * H renderer(双核浏览器渲染方式)
 
   说明：renderer是为双核浏览器准备的，用于指定双核浏览器默认以何种方式渲染页面。比如说360浏览器。
	举例：
	```html 
	<meta name="renderer" content="webkit"> //默认webkit内核
	<meta name="renderer" content="ie-comp"> //默认IE兼容模式
	<meta name="renderer" content="ie-stand"> //默认IE标准模式
	```
	国内双核浏览器默认内核模式如下：
	1. 搜狗高速浏览器、QQ浏览器：IE内核（兼容模式）
	2. 360极速浏览器、遨游浏览器：Webkit内核（极速模式）
	
	```html
	 <meta name="renderer" content="webkit | ie-comp | ie-stand">
	```
 * I . **viewport**(移动端的窗口) 
 
   能优化移动浏览器的显示。如果不是响应式网站，不要使用initial-scale或者禁用缩放。
   大部分4.7-5寸设备的viewport宽设为360px；5.5寸设备设为400px；iphone6设为375px；ipone6 plus设为414px。
   ```html
   <meta name="viewport" content="width=device-width, initial-scale=1.0,maximum-scale=1.0, user-scalable=no"/>
   ```
    `width=device-width` 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边 。
	* width：宽度（数值 / device-width）（范围从200 到10,000，默认为980 像素）
    * height：高度（数值 / device-height）（范围从223 到10,000）
    * initial-scale：初始的缩放比例 （范围从>0 到10）
    * minimum-scale：允许用户缩放到的最小比例
    * maximum-scale：允许用户缩放到的最大比例
    * user-scalable：用户是否可以手动缩 (no,yes)
    * minimal-ui：可以在页面加载时最小化上下状态栏。（已弃用）
    <br/>
  注意，很多人使用initial-scale=1到非响应式网站上，这会让网站以100%宽度渲染，用户需要手动移动页面或者缩放。如果和initial-scale=1同时使用user-scalable=no或maximum-scale=1，则用户将不能放大/缩小网页来看到全部的内容。使用（`<meta name=“viewport” content=“width=device-width,user-scalable=yes”>`）
   * 其他的用法
   
      ```html
			  <!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->
			<meta name="HandheldFriendly" content="true">
			<!-- 微软的老式浏览器 -->
			<meta name="MobileOptimized" content="320">
			<!-- uc强制竖屏 -->
			<meta name="screen-orientation" content="portrait">
			<!-- QQ强制竖屏 -->
			<meta name="x5-orientation" content="portrait">
			<!-- UC强制全屏 -->
			<meta name="full-screen" content="yes">
			<!-- QQ强制全屏 -->
			<meta name="x5-fullscreen" content="true">
			<!-- UC应用模式 -->
			<meta name="browsermode" content="application">
			<!-- QQ应用模式 -->
			<meta name="x5-page-mode" content="app">
			<!-- windows phone 点击无高光 -->
			<meta name="msapplication-tap-highlight" content="no">
       ```
	
	 
	   ```html
	   <meta name="apple-mobile-web-app-capable" content="yes" />
	   <!-- 启用 WebApp 伪装app，离线应用。)  全屏模式 -->
	   
	   <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
	   <!-- 隐藏状态栏/设置状态栏颜色：只有在开启WebApp全屏模式时才生效。content的值为default | black | black-translucent -->
	   
	   <meta name="apple-mobile-web-app-title" content="标题">
	   <!-- 添加到主屏后的标题 -->
	   
	   <meta content="telephone=no" name="format-detection" /> 
	   <!-- 忽略数字自动识别为电话号码 -->
	   
	   <meta content="email=no" name="format-detection" />
	   <!-- 忽略识别邮箱 -->
	   
	   <meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL"> 
	   <!-- 添加智能 App 广告条 Smart App Banner：告诉浏览器这个网站对应的app，并在页面上显示下载banner -->
	   ```
### 2. http-equiv属性
--------
http-equiv顾名思义，相当于http的文件头作用。
`相当于HTTP的作用，比如说定义些HTTP参数啥的。`

``` html?linenums
<meta http-equiv="参数" content="具体的描述">
```

其中http-equiv属性主要有以下几种参数：

  * A. content-Type(设定网页字符集)(推荐使用HTML5的方式)

    说明：用于设定网页字符集，便于浏览器解析与渲染页面
    举例：
	```html
	<meta http-equiv="content-Type" content="text/html;charset=utf-8"> 
	//旧的HTML，不推荐

    <meta charset="utf-8"> 
	//HTML5设定网页字符集的方式，推荐使用UTF-8
	```
  * B. X-UA-Compatible(浏览器采取何种版本渲染当前页面)
     
     说明：用于告知浏览器以何种版本来渲染页面。（一般都设置为最新模式，在各大框架中这个设置也很常见。
     举例：
	 ```html 
	 <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/> //指定IE和Chrome使用最新版本渲染当前页面
	 ```
	 优先使用 IE 最新版本和 Chrome
	 ```html
	 <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
	<!-- 关于X-UA-Compatible -->
	<meta http-equiv="X-UA-Compatible" content="IE=6" ><!-- 使用IE6 -->
	<meta http-equiv="X-UA-Compatible" content="IE=7" ><!-- 使用IE7 -->
	<meta http-equiv="X-UA-Compatible" content="IE=8" ><!-- 使用IE8 -->
	 ```
 * C. cache-control(指定请求和响应遵循的缓存机制)
 
   <img style='margin-left: 40px;' src=' https://segmentfault.com/image?src=http://7xoxxe.com1.z0.glb.clouddn.com/cache.png&objectId=1190000004279791&token=60cc5b81792e199feb8a6b032aff4b83'/>
  
   举例:
   ```html
   <meta http-equiv="cache-control" content="no-cache">
   ```
   共有以下几种用法：
     * no-cache: 先发送请求，与服务器确认该资源是否被更改，如果未被更改，则使用缓存。
     * no-store: 不允许缓存，每次都要去服务器上，下载完整的响应。（安全措施）
     * public : 缓存所有响应，但并非必须。因为max-age也可以做到相同效果
     * private : 只为单个用户缓存，因此不允许任何中继进行缓存。（比如说CDN就不允许缓存private的响应）
     * maxage : 表示当前请求开始，该响应在多久内能被缓存和重用，而不去服务器重新请求。例如：max-age=60表示响应可以再缓存和重用 60 秒。
     
	 <br/>
*  D. expires(网页到期时间)

   说明:用于设定网页的到期时间，过期后网页必须到服务器上重新传输。
   举例：
   ```html
   <meta http-equiv="expires" content="Sunday 26 October 2016 01:00 GMT" />
   ```
 * E. refresh(自动刷新并指向某页面)
 
	 说明：网页将在设定的时间内，自动刷新并调向设定的网址。
	举例:
	```html
   <meta http-equiv="refresh" content="2；URL=http://www.lxxyx.win/">
   //意思是2秒后跳转向我的博客
   ```

 * F. Set-Cookie(cookie设定)
 
    说明：如果网页过期。那么这个网页存在本地的cookies也会被自动删除。
	```html
	<meta http-equiv="Set-Cookie" content="name, date"> //格式

	<meta http-equiv="Set-Cookie" content="User=Lxxyx; path=/; expires=Sunday, 10-Jan-16 10:00:00 GMT"> //具体范例
	```
  * G. Content-Language （设置网页语言）
     ```html
	 <meta  http-equiv="content-Language" contect="zh-CN">
	 ```
### 3. 移动端的meta

 
* **format-detection**

  对电话号码的识别
  ```html
  <meta name="format-detection" content="telephone=no" />
  ```
 * **IOS私有属性**
     * **apple-mobile-web-app-capable**

		是否启动webapp功能，会删除默认的苹果工具栏和菜单栏。
		```html
		<meta name="apple-mobile-web-app-capable" content="yes" />
		```
	 * **apple-mobile-web-app-status-bar-style**

		当启动webapp功能时，显示手机信号、时间、电池的顶部导航栏的颜色。默认值为default（白色），可以定为black（黑色）和black-translucent（灰色半透明）。这个主要是根据实际的页面设计的主体色为搭配来进行设置。
		```html
		<meta name="apple-mobile-web-app-status-bar-style" content="black" />
		```
 示例：
``` html
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no" />
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<meta name="format-detection"content="telephone=no, email=no" />
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no" />
<meta name="apple-mobile-web-app-capable" content="yes" /><!-- 删除苹果默认的工具栏和菜单栏 -->
<meta name="apple-mobile-web-app-status-bar-style" content="black" /><!-- 设置苹果工具栏颜色 -->
<meta name="format-detection" content="telphone=no, email=no" /><!-- 忽略页面中的数字识别为电话，忽略email识别 -->
<!-- 启用360浏览器的极速模式(webkit) -->
<meta name="renderer" content="webkit">
<!-- 避免IE使用兼容模式 -->
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->
<meta name="HandheldFriendly" content="true">
<!-- 微软的老式浏览器 -->
<meta name="MobileOptimized" content="320">
<!-- uc强制竖屏 -->
<meta name="screen-orientation" content="portrait">
<!-- QQ强制竖屏 -->
<meta name="x5-orientation" content="portrait">
<!-- UC强制全屏 -->
<meta name="full-screen" content="yes">
<!-- QQ强制全屏 -->
<meta name="x5-fullscreen" content="true">
<!-- UC应用模式 -->
<meta name="browsermode" content="application">
<!-- QQ应用模式 -->
<meta name="x5-page-mode" content="app">
<!-- windows phone 点击无高光 -->
<meta name="msapplication-tap-highlight" content="no">
<!-- 适应移动端end -->
```