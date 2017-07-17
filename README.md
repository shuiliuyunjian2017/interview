# 2017.7.16 IT专场 #

## 笔试题： ##

### 1.清除浮动的方法，分别用在哪些场景； ###

  　大概分为三种：

  　第一种：添加新的元素，应用**clear：both**。

  　HTML

	  　<div class="clearfix">
		    <div class="div1">1</div>
		    <div class="div2">2</div>
		    <div class="clear"></div>
		</div>

  　css

   		.clearfix {
			clear:both; 
			height: 0; 
			line-height: 0; 
			font-size: 0
		}

  　第二种：**给父级元素定义overflow**

   　HTML

	  　<div class="clearfix">
		    <div class="div1">1</div>
		    <div class="div2">2</div>
		    <!--<div class="clear"></div>-->
		</div>

  　css

	   	.clearfix{
			overflow: auto; //也可以设置hidden，hidden会省略超出元素，auto在子内容超出去父容器上可能会出现滚动条
			zoom: 1; //zoom: 1; 处理兼容性问题
		}

  　第三种：**:after 方法**（注意：作用于浮动元素的父亲）

　  其实现原理类似于clear:both方法，只是区别在于:clear在html插入一个div.clear标签，而outer利用其伪类clear:after在元素内部增加一个类似于div.clear的效果。

　  *当一个内层元素是浮动的时候，如果没有关闭浮动时，其父元素也就不会再包含这个浮动的内层元素，因为此时浮动元素已经脱离了文档流。也就是为什么外层不能被撑开了！

   　HTML

	  　<div class="clearfix">
		    <div class="div1">1</div>
		    <div class="div2">2</div>
		</div>

  　css

	   	.clearfix {zoom:1;}    /*==for IE6/7 Maxthon2==*/
		.clearfix :after {
		    clear:both;
		    content:'.';
		    display:block;
		    width: 0;
		    height: 0;
		    visibility:hidden;
		}   /*==for FF/chrome/opera/IE8==*/

   或使用双伪元素清除浮动。（大体一样）

		.clearfix:before,.clearfix:after {
		  content: "";
		  display: block;
		  clear: both;
		}
		
		.clearfix {
		  zoom: 1;
		}



### 2.call,apply的作用及区别； ###

   **定义：**

   apply：应用某一对象的一个方法，用另一个对象替换当前对象。例如：B.apply(A, arguments);即A对象应用B对象的方法。

   call：调用一个对象的一个方法，以另一个对象替换当前对象。例如：B.call(A, args1,args2);即A对象调用B对象的方法。

   **共同：**

   可以用来代替另一个对象调用一个方法，将一个函数的对象上下文从初始的上下文改变为由thisObj指定的新对象

   **区别：**

   在第二个参数中，call传单个的参数，apply传单个参数组成的数组。


### 3.cookie,sessionStorage,localStorage的差异； ###

   **cookie：**
   
   存储在用户本地终端上的数据。有时也用cookies，指某些网站为了辨别用户身份，进行session跟踪而存储在本地终端上的数据，通常经过加密。一般应用最典型的案列就是判断注册用户是否已经登过该网站。

   **sessionStorage：** 

   针对一个 session 的数据存储,当用户关闭浏览器窗口后，数据会被删除。

   **localStorage：**

   没有时间限制的数据存储,第二天、第二周或下一年之后，数据依然可用。

   **异同**

   共同点：都是保存在浏览器端，且同源的。

   区别：

   1. **存储方式不同**，cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递；cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。
   而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

   2. **数据有效期不同**，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。

   3. **作用域不同**，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。Web Storage 的 api 接口使用更方便。


### 4.一大型电商网站有很多图片，怎样做图片的优化； ###

### 5.事件代理的好处； ###

### 6.什么叫外边距重叠； ###

### 7.什么叫闭包？闭包的好处？请举例。 ###

　 **闭包的概念：**
   闭包就是能够读取其他函数内部变量的函数。
由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”。
所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

　 **闭包的用途:**
   一个是可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。

　 **使用闭包的注意点:**

   1. 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
   2. 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

		
		<ul id="aaa">
			<li>1</li>
			<li>2</li>
			<li>3</li>
			<li>4</li>
		</ul>
		var list = document.getElementById('aaa').getElementsByTagName('li');
		for (var i = 0, len = list.length; i < len; i++) {
			list[i].onclick = (function (n) {
				return function () {
					console.log(n);
				};
			})(i + 1);
		}
	   

### 8.有一个URL地址为：http://tao.com?a=1&c=2&d=3 ,请写一个方法，返回key-value键值对。 ###
	str.split('&');

### 9.什么叫JSONP？为什么说它不是真正的AJAX？ ###

   利用`<script>`标签没有跨域限制的“漏洞”来达到与第三方通讯的目的。当需要通讯时，本站脚本创建一个`<script>`元素，地址指向第三方的API网址，形如：

     <script src="http://www.example.net/api?param1=1&param2=2"></script>     
  并提供一个回调函数来接收数据（函数名可约定，或通过地址参数传递）。     

  第三方产生的响应为json数据的包装（故称之为jsonp，即json padding），形如：     `callback({"name":"hax","gender":"Male"})`   

  这样浏览器会调用callback函数，并传递解析后json对象作为参数。本站脚本可在callback函数里处理所传入的数据。

  AJAX即“Asynchronous Javascript And XML”（异步JavaScript和XML），是指一种创建交互式网页应用的网页开发技术。

  JSONP不是真正的AJAX，理由：传输过程并不是异步的，执行状态也无法判断（成功or失败），只相当于一个单纯的GET请求。

### 10.生成10个10－100之间的随机数，并排序。 ###
   
   	Math.floor(Math.random()*(end-start+1) + count);

	function compare(a, b){
	   return a - b;
	}
