---
title: http协议
--- 


----------

>- http构建与TCP/IP协议之上，默认端口号是80
>- htpp是无连接无状态的

http协议的主要特点：
*  支持客户/服务器模式
*  简单快速： 
    客户向服务器请求服务时，只需要传送请求方式和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
* 灵活：HTTP允许传输任意类型的数据对象。正在传输的类型有==Content-type==加以标记。
* 无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
* 无状态：HTTP协议是无状态协议。无状态是指协议对事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大 。另一方面，在服务器不需要先前信息时它的应答就比较快。
## 一、HTTP报文
-------------

### 1.请求报文

http协议是以 ASCII 码传输，建立在TCP/IP协议之上的应用规范。规范把http请求份为三个部分：状态行、请求头、消息主体。类似于下面这样：

```html?linenums
<method><request-URL><version>
<headers>

<entity-body>
```

http定义了与服务器交互的不同方法，基本的方法有4种，分别是==GET==，==POST==，==PUT==,==DELETE==。==URL==全称是资源描述符，我们可以这样认为：一个==URL==地址，它用于描述一个网络上的资源，而http中 的==GET==，==POST==，==PUT==,==DELETE==就对应着这个资源的查，增，改，删4个操作。

 1. GET用于信息获取，而且应该是安全的 和 幂等的。
 
      所谓安全的意味着该操作用于获取信息而非修改信息。换句话说，GET 请求一般不应产生副作用。就是说，它仅仅是获取资源信息，就像数据库查询一样，不会修改，增加数据，不会影响资源的状态。
	  
	  幂等的意味着对同一URL的多个请求应该返回同样的结果。
	  
	  GET请求报文示例：
	  
	  ```tex?linenums
		   GET  /books/?sex=man&name=Professional HTTP/1.1
		   Host: www.example.com
		   User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
		   Gecko/20050225 Firefox/1.0.1
		  Connection: Keep-Alive
	  ```

 * http条件get使用的时机？
    
	客户之前已经访问过某网站，并打算再次访问该网站。
	
  
 * http条件get使用的方法？
 
    客户端向服务器发送一个包询问是否在上一次访问网站的时间后是否更改了页面，如果服务器没有更新，显然不需要把整个网页传给客户端，客户端只要使用本地缓存即可，如果服务器对照客户端给出的时间已经更新了客户端请求的网页，则发送这个更新了的网页给用户。
	
	下面是一个具体的发送接受报文示例：
	
	客户端发送请求：
	
	```tex?linenums
	  	 GET / HTTP/1.1  
		 Host: www.sina.com.cn:80  
		 If-Modified-Since:Thu, 4 Feb 2010 20:39:13 GMT  
		 Connection: Close  
    ```
     第一次请求时，服务器端返回请求数据，之后的请求，服务器根据请求中的 If-Modified-Since 字段判断响应文件没有更新，如果没有更新，服务器返回一个 304 Not Modified响应，告诉浏览器请求的资源在浏览器上没有更新，可以使用已缓存的上次获取的文件。
    ```tex?linenums
	  	 HTTP/1.0 304 Not Modified  
		 Date: Thu, 04 Feb 2010 12:38:41 GMT  
		 Content-Type: text/html  
		 Expires: Thu, 04 Feb 2010 12:39:41 GMT  
		 Last-Modified: Thu, 04 Feb 2010 12:29:04 GMT  
		 Age: 28  
		 X-Cache: HIT from sy32-21.sina.com.cn  
		 Connection: close 
    ```
	
	如果服务器端资源已经更新的话，就返回正常的响应。

 2. POST表示可能修改变服务器上的资源的请求。
     ```tex?linenums
	     POST / HTTP/1.1
		 Host: www.example.com
		 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
		 Gecko/20050225 Firefox/1.0.1
		 Content-Type: application/x-www-form-urlencoded
		 Content-Length: 40
		 Connection: Keep-Alive

		 sex=man&name=Professional  
	  ```
  

 3. 注意：
 
	（1）、GET 可提交的数据量受到URL长度的限制，HTTP 协议规范没有对 URL 长度进行限制。这个限制是特定的浏览器及服务器对它的限制
	（2）、理论上讲，POST 是没有大小限制的，HTTP 协议规范也没有进行大小限制，出于安全考虑，服务器软件在实现时会做一定限制
	（3）、参考上面的报文示例，可以发现 GET 和 POST 数据内容是一模一样的，只是位置不同，一个在URL里，一个在 HTTP 包的包体里

### 2、POST提交数据的方式

HTTP 协议中规定 POST 提交的数据必须在 body 部分中，但是协议中没有规定数据使用哪种编码方式或者数据格式。实际上，开发者完全可以自己决定消息主体的格式，只要最后发送的 HTTP 请求满足上面的格式就可以。

但是，数据发送出去，还要服务端解析成功才有意义。一般服务端语言如 php、python 等，以及它们的 framework，都内置了自动解析常见数据格式的功能。服务端通常是根据请求头（headers）中的 Content-Type 字段来获知请求中的消息主体是用何种方式编码，再对主体进行解析。

所以说到 POST 提交数据方案，包含了 Content-Type 和消息主体编码方式两部分。

下面就正式开始介绍它们：

  ```tex?linenums
     application/x-www-form-urlencoded
  ```

这是最常见的 ==POST 数据提交==方式。浏览器的原生form表单，如果不设置 enctype 属性，那么最终就会以 ==application/x-www-form-urlencoded== 方式提交数据。上个小节当中的例子便是使用了这种提交方式。

可以看到 body 当中的内容和 GET 请求是完全相同的。

 ```tex?linenums
     multipart/form-data
  ```

这又是一个常见的 POST 数据提交的方式。我们使用表单==上传文件==时，必须让 form表单的 enctype 等于 ==multipart/form-data==。直接来看一个请求示例：


 ```tex?linenums
    POST http://www.example.com HTTP/1.1
	Content-Type:multipart/form-data; boundary=----    WebKitFormBoundaryrGKCBY7qhFd3TrwA

	------WebKitFormBoundaryrGKCBY7qhFd3TrwA
	Content-Disposition: form-data; name="text"

	title
	------WebKitFormBoundaryrGKCBY7qhFd3TrwA
	Content-Disposition: form-data; name="file"; filename="chrome.png"
	Content-Type: image/png

	PNG ... content of chrome.png ...
	------WebKitFormBoundaryrGKCBY7qhFd3TrwA--
  ```

上面提到的这两种 POST 数据的方式，都是浏览器原生支持的，而且现阶段标准中原生 form 表单也只支持这两种方式（通过 form元素的 enctype 属性指定，默认为 application/x-www-form-urlencoded。其实 enctype 还支持 text/plain，不过用得非常少）。

随着越来越多的 Web 站点，尤其是 WebApp，全部使用 Ajax 进行数据交互之后，我们完全可以定义新的数据提交方式，例如 ==application/json==，==text/xml==，乃至 ==application/x-protobuf== 这种二进制格式，只要服务器可以根据 Content-Type 和 Content-Encoding 正确地解析出请求，都是没有问题的。


### 3. 响应报文

http响应与http请求类似，http响应也由三个部分构成，分别是：
> - 状态行
> - 响应头（Response Header）
> - 响应正文

状态行由协议版本、数字形式的状态代码、以及响应的状态描述，各元素之间以空格分隔。

常见的状态码如下：

> - 200 OK 客户端请求成功
> - 301 Moved Permanently 请求永久重定向
> - 302 Moved Temporarily 请求临时重定向
> - 304 Not Modified 文件未修改，可以直接使用缓存的文件。
> - 400 Bad Request 由于客户端请求有语法错误，不能被服务器所理解。
> - 401 Unauthorized 请求未经授权。这个状态代码必须和WWW-Authenticate报头域一起使用
> - 403 Forbidden 服务器收到请求，但是拒绝提供服务。服务器通常会在响应正文中给出不提供服务的原因
> - 404 Not Found 请求的资源不存在，例如，输入了错误的URL
> - 500 Internal Server Error 服务器发生不可预期的错误，导致无法完成客户端的请求。
> - 503 Service Unavailable 服务器当前不能够处理客户端的请求，在一段时间之后，服务器可能会恢复正常。

## 二、消息报头
  HTTP消息由客户端到服务器的请求和服务器到客户端的响应组成。请求消息和响应消息都是由开始行（对于请求消息，开始行就是请求行，对于响应消息，开始行就是状态行），消息报头（可选），空行（只有CRLF的行），消息正文（可选）组成。
  http消息爆头包括：
   * 请求报头
   *  响应报头
   *  实体报头
   每一个报头域都是由名字+“： ”+ 空格+ 值 组成，消息报头域的名字是大小写无关的。
### 1. 普通报头
  在普通报头中，有少数报头域用于所有的请求和响应消息，但并不用于被传输的实体，只用于传输的消息。
  *  ==Cache-Control==
  eg：
    <div style='text-indent: 30px;'>Cache-Control 用于指定缓存指令，缓存指令是单向的（响应中出现的缓存指令在请求中未必会出现），且是独立的（一个消息的缓存指令不会影响另一个消息处理的缓存机制），http1.0使用的类似的报头域为Pragma。</div>
    <div style='text-indent: 30px;'>请求的缓存指令包括：no-cache(用于指示请求或响应消息不能缓存)、no-store、max-age、max-stale、min-fresh、only-if-cached;</div>
   
  <div style='text-indent: 30px;'>
	     响应时的缓存指令包括：public、private、no-cache、no-store、no-trnsform、must-revalidate、proxy-revalidate、max-age、s-maxage。
</div>
<br/>
eg:
  <div style='text-indent: 30px;'>
  为了指示IE浏览器（客户端）不要缓存页面，服务端的jsp程序可以编写如下：response.setHeader（'Cache-Control','no-cache'）;
  </div><div style='text-indent: 30px;'>
  //response.setHeader（pragma','no-cache'）;作用相当于上述代码，通常两者 //合用 这句代码将在发送的响应消息中设置普通报头域：Cache-Control:no-cache   <br/> <br/> <br/></div>
 
  *  ==Date== 普通报头域表示消息产生的日期和时间
  
  *  ==Connection==  普通报头域允许发送指定连接的选项。例如指定连接是连续，或者指定“close”选项，通知服务器，在响应完成后，关闭连接。
  
      <div style='text-indent: 30px;'>我们知道 HTTP 协议采用“请求-应答”模式，当使用普通模式，即非 Keep-Alive 模式时，每个请求/应答客户和服务器都要新建一个连接，完成之后立即断开连接（HTTP协议为无连接的协议）；当使用 Keep-Alive 模式（又称持久连接、连接重用）时，Keep-Alive 功能使客户端到服务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive 功能避免了建立或者重新建立连接。<div>

     <div style='text-indent: 30px;'>在 HTTP 1.0 版本中，并没有官方的标准来规定 Keep-Alive 如何工作，因此实际上它是被附加到 HTTP 1.0协议上，如果客户端浏览器支持 Keep-Alive ，那么就在HTTP请求头中添加一个字段 Connection: Keep-Alive，当服务器收到附带有 Connection: Keep-Alive 的请求时，它也会在响应头中添加一个同样的字段来使用 Keep-Alive 。这样一来，客户端和服务器之间的HTTP连接就会被保持，不会断开（超过 Keep-Alive 规定的时间，意外断电等情况除外），当客户端发送另外一个请求时，就使用这条已经建立的连接。</div>

      <div style='text-indent: 30px;'>在 HTTP 1.1 版本中，默认情况下所有连接都被保持，如果加入 "Connection: close" 才关闭。目前大部分浏览器都使用 HTTP 1.1 协议，也就是说默认都会发起 Keep-Alive 的连接请求了，所以是否能完成一个完整的 Keep-Alive 连接就看服务器设置情况。</div>

      <div style='text-indent: 30px;'>由于 HTTP 1.0 没有官方的 Keep-Alive 规范，并且也已经基本被淘汰，以下讨论均是针对 HTTP 1.1 标准中的 Keep-Alive 展开的。</div>

<div style='text-indent: 30px;'>

注意：
> - HTTP Keep-Alive 简单说就是保持当前的TCP连接，避免了重新建立连接。
> - HTTP 长连接不可能一直保持，例如 Keep-Alive: timeout=5, max=100，表示这个TCP通道可以保持5秒，max=100，表示这个长连接最多接收100次请求就断开。
> - HTTP是一个无状态协议，这意味着每个请求都是独立的，Keep-Alive没能改变这个结果。另外，Keep-Alive也不能保证客户端和服务器之间的连接一定是活跃的，在HTTP1.1版本中也如此。唯一能保证的就是当连接被关闭时你能得到一个通知，所以不应该让程序依赖于Keep-Alive的保持连接特性，否则会有意想不到的后果。
> - 使用长连接之后，客户端、服务端怎么知道本次传输结束呢？两部分：1. 判断传输数据是否达到了Content-Length 指示的大小；2. 动态生成的文件没有 Content-Length ，它是分块传输（chunked），这时候就要根据 chunked 编码来判断，chunked 编码的数据在最后有一个空 chunked 块，表明本次传输数据结束。


</div>

### 2. 请求报头

   请求报头允许客户端向服务器端传递请求的附加信息以及客户端自身的信息。
   
   常用的请求报头
   * ==Accept==
   
      Accept 请求报头域用于指定客户端接受哪些类型的信息。eg：Accept：image/gif，表明客户端希望接受 GIF 图象格式的资源；Accept：text/html，表明客户端希望接受 html 文本。
  * ==Accept-Charset==
  
      Accept-Charset 请 求 报 头 域 用 于 指 定 客 户 端 接 受 的 字 符 集 。 eg ：Accept-Charset:iso-8859-1,gb2312.如果在请求消息中没有设置这个域，缺省是任何字符集都可以接受。
   * ==Accept-Encoding==

     Accept-Encoding 请求报头域类似于 Accept，但是它是用于指定可接受的内容编码。eg：Accept-Encoding:gzip.deflate.如果请求消息中没有设置这个域服务器假定客户端对各种内容编码都可以接受。
  * ==Accept-Language==
    
	Accept-Language 请 求 报 头 域 类 似 于 Accept ， 但 是 它 是 用 于 指 定 一 种 自 然 语 言 。 eg ：Accept-Language:zh-cn.如果请求消息中没有设置这个报头域，服务器假定客户端对各种语言都可以接受。
  * ==Authorization==
    
	Authorization 请求报头域主要用于证明客户端有权查看某个资源。当浏览器访问一个页面时，如果收到服务器的响应代码为 401（未授权），可以发送一个包含 Authorization 请求报头域的请求，要求服务器对其进行验证。
 * ==Host==（发送请求时，该报头域是必需的）
   
   Host请求报头域主要用于指定被请求资源的Internet主机和端口号，它通常从HTTP URL中提取出来的。eg:
   我们在浏览器中输入： https://www.baidu.com/index.html
   浏览器发送的请求消息中，就会包含 Host 请求报头域，如下：
  Host：www.baidu.com
此处使用缺省端口号 443，若指定了端口号，则变成：Host：www.baidu.com:指定端口号
  * ==user-Agent==
     User-Agent 请求报头域允许客户端将它的操作系统、浏览器和其它属性告诉服务器。
	 
 ### 3. 响应报头
  响应报头允许服务器传递不能放在状态行中的附加响应信息，以及关于服务器的信息和对Request-URI 所标识的资源进行下一步访问的信息。
  
常用的响应报头：
 * ==Location==

    Location响应报头域用于重定向接受者到一个新的位置。Location响应报头域常用于更换域名的时候。
* ==Server==
   Server响应报头域包含了服务器用来处理请求的软件信息。与user-Agent请求报头域是相对应的。
   eg:
   <div style='text-indent: 30px;'>Server:nginx;使用的服务器为nginx</div>
### 4、实体报头
<br/>
请求和响应消息都可以传送一个实体。一个实体由实体报头域和实体正文组成，但并不是说实体报头域和实体正文要在一起发送，可以只发送实体报头域。实体报头定义了关于实体正文（eg：有无实体正文）和请求所标识的资源的元信息

常用的实体报头:
* ==Content-Encoding==
 
  Content-Encoding 实体报头域被用作媒体类型的修饰符，它的值指示了已经被应用到实体正文的附加内容的编码，因而要获得 Content-Type 报头域中所引用的媒体类型，必须采用相应的解码机制。Content-Encoding 这样用于记录文档的压缩方法，eg：Content-Encoding：gzip
 * ==Content-Language==
    
	描述了资源所用的自然语言。
 * ==Content-Length==
   
   Content-Length 实体报头域用于指明实体正文的长度，以字节方式存储的十进制数字来表示。
 * ==Content-Type==
    
	Content-Type实体报头域用语指明发送给接收者的实体正文的媒体类型。eg：
    Content-Type:text/html;charset=ISO-8859-1
* ==Last-Modified==
    
	
    Last-Modified 实体报头域用于指示资源的最后修改日期和时间。
* ==Expires==
   
   Expires 实体报头域给出响应过期的日期和时间。为了让代理服务器或浏览器在一段时间以后更新缓存中(再次访问曾访问过的页面时，直接从缓存中加载，缩短响应时间和降低服务器负载)的页面，我们可以使用 Expires 实体报头域指定页面过期的时间。eg：Expires：Thu，15 Sep 2006 16:23:12GMT
