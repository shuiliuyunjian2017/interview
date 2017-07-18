#js与flash交互#


Flash提供了**ExternalInterface**接口与JavaScript通信，ExternalInterface有两个方法，**call**和**addCallback**：

	ExternalInterface.addCallback("在js里可调用的flash方法名",flash内方法)
	//在flash中通过这个方法公开 在js中可调用的flash内的方法;
	
	ExternalInterface.call("js方法",传给js的参数) //在flash里调用js里的方法


1.javascript调用flash中的函数


   在flash的脚本中增加代码: `import flash.external.ExternalInterface;`

   假定要调用的函数是hello，as代码如下:

	function hello(){
	   return "hello";
	}
	ExternalInterface.addCallback("hello", this, hello);
	//第一个参数为导出函数名，第三个参数为as的函数名

   这样就可以在js中调用as的hello函数了。

2.flash调用js的函数

	ExternalInterface.call("hello2", "jacky");
	//第一个参数是js的函数名，后面的是js函数的参数

3.如何互相调用  

	<object type="application/x-shockwave-flash" data="test.swf" width="525" height="390" name="test">
        <param name="allowScriptAccess" value="sameDomain" />
        <param name="movie" value="test.swf" />
        <param name="quality" value="high" />
        <param name="scale" value="noScale" />
        <param name="wmode" value="transparent" />                        
	</object>
	function callFromFlash() {
	    var a = thisMovie("test").hello();
	    alert(a);
	}
	
	function thisMovie(movieName) {
	    if (navigator.appName.indexOf("Microsoft") != -1){
	        return window[movieName];
	    } else {
	        return document[movieName];
	    }
	}

   BTW：要记得设置allowScriptAccess属性
