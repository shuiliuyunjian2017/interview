# webpack入门 #

### 1.什么是webpack? ###

   WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。


### 2.WebPack和Grunt以及Gulp相比有什么特性 ###

   Gulp/Grunt是一种能够优化前端的开发流程的工具，而WebPack是一种模块化的解决方案，不过Webpack的优点使得Webpack可以替代Gulp/Grunt类的工具。

   Grunt和Gulp的工作方式是：在一个配置文件中，指明对某些文件进行类似编译，组合，压缩等任务的具体步骤，这个工具之后可以自动替你完成这些任务。

   Webpack的工作方式是：把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个浏览器可识别的JavaScript文件。

### 3.开始使用Webpack ###

   用npm安装，新建一个空的文件夹，转到该文件夹后执行下述指令完成安装。

    //全局安装
	npm install -g webpack


### 4.正式使用Webpack前的准备 ###

   4.1 用npm init命令可以自动创建package.json文件，这是一个标准的npm说明文件，里面蕴含了丰富的信息，包括当前项目的依赖模块，自定义的脚本任务等等。

	npm init

   输入这个命令后，终端会问你一系列诸如项目名称，项目描述，作者等信息，不过不用担心，如果你不准备在npm中发布你的模块，这些问题的答案都不重要，回车默认即可。

   4.2 安装Webpack作为依赖包

	// 安装Webpack
	npm install --save-dev webpack

   在当前文件夹下再创建两个文件夹,app文件夹和public文件夹，app文件夹用来存放原始数据和JavaScript模块，public文件夹用来存放准备给浏览器读取的数据（包括使用webpack生成的打包后的js文件以及一个index.html文件）。在这里还需要创建三个文件，index.html 文件放在public文件夹中，两个js文件（Greeter.js和main.js）放在app文件夹中，此时项目结构如下图所示

	testwebpack
		-app
			-Greeter.js
			-main.js
		-node_modules
		-public
			-index.html
		-package.json

   **index.html**文件只有最基础的html代码，它唯一的目的就是加载打包后的js文件（bundle.js）

	<!DOCTYPE html>
	<html lang="en">
	  <head>
	    <meta charset="utf-8">
	    <title>Test Webpack</title>
	  </head>
	  <body>
	    <div id='root'>
	    </div>
	    <script src="index.js"></script>
	  </body>
	</html>

   **Greeter.js**只包括一个用来返回包含问候信息的html元素的函数。

	module.exports = function() {
	  var greet = document.createElement('div');
	  greet.textContent = "Hi there and greetings!";
	  return greet;
	};
	
   **main.js**用来把Greeter模块返回的节点插入页面。

	var greeter = require('./Greeter.js');
	document.getElementById('root').appendChild(greeter());

### 5.正式使用Webpack ###
	
   **5.1在终端通过命令打包**

   全局安装的webpack的，在当前目录下输入

	webpack app/main.js public/index.js
	//webpack 源文件地址 目标文件地址

   
   非全局安装的，需要额外指定其在node_modules中的地址，如下：

	node_modules/.bin/webpack app/main.js public/bundle.js
   	//未验证，不知真假（我用的全局安装）

   然后在浏览器上打开index.html页面，输出‘Hi there and greetings!’。可见webpack已经把Greeting.js和main.js打包成index.js这一个文件了。

   **5.2通过配置文件来打包**

   Webpack拥有很多其它的比较高级的功能（比如说本文后面会介绍的loaders和plugins），这些功能其实都可以通过命令行模式实现，但是正如已经提到的，这样不太方便且容易出错的，一个更好的办法是定义一个配置文件，这个配置文件其实也是一个简单的JavaScript模块，可以把所有的与构建相关的信息放在里面。

   还是继续上面的例子来说明如何写这个配置文件，在当前练习文件夹的根目录下新建一个名为webpack.config.js的文件，并在其中进行最最简单的配置，如下所示，它包含入口文件路径和存放打包后文件的地方的路径。

	module.exports = {
	  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
	  output: {
	    path: __dirname + "/public",//打包后的文件存放的地方
	    filename: "index.js"//打包后输出文件的文件名
	  }
	};

   **注：**“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。

   现在如果你需要打包文件只需要在终端里你运行webpack**(非全局安装需使用node_modules/.bin/webpack)**命令就可以了，这条命令会自动参考webpack.config.js文件中的配置选项打包你的项目。

   **5.3快捷的执行打包任务**

   执行类似于`node_modules/.bin/webpack`这样的命令其实是比较烦人且容易出错的，不过值得庆幸的是npm可以引导任务执行，对其进行配置后可以使用简单的`npm start`命令来代替这些繁琐的命令。在package.json中对npm的脚本部分进行相关设置即可，设置方法如下。

	{
	  "name": "test4webpack",
	  "version": "0.0.1",
	  "description": "test for webpack",
	  "main": "index.js",
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1",
	    "start": "webpack" //配置的地方就是这里啦，相当于把npm的start命令指向webpack命令
	  },
	  "author": "liu",
	  "license": "ISC",
	  "devDependencies": {
	    "webpack": "^3.3.0"
	  }
	}

   **注：**package.json中的脚本部分已经默认在命令前添加了`node_modules/.bin`路径，所以无论是全局还是局部安装的Webpack，你都不需要写前面那指明详细的路径了。

   npm的`start`是一个特殊的脚本名称，它的特殊性表现在，在命令行中使用`npm start`就可以执行相关命令，如果对应的此脚本名称不是`start`，想要在命令行中运行时，需要这样用`npm run {script name}`如`npm run build`

**6.生成Source Maps（使调试更容易）**

   通过配置Source Maps，对应编译文件和源文件，使得编译后的代码可读性更高，更易调试。在webpack的配置文件中配置source maps，需要配置devtool，它有以下四种不同的配置选项，各具优缺点，描述如下：

| devtool选项       | 配置结果  |

| source-map | 在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包文件的构建速度； |

| cheap-module-source-map | 在一个单独的文件中生成一个不带列映射的map，不带列映射提高项目构建速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便； |

| eval-source-map | 使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。不过在开发阶段这是一个非常好的选项，但是在生产阶段一定不要用这个选项； |

| cheap-module-eval-source-map | 这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点； |

   在学习阶段以及在小到中性的项目上，eval-source-map是一个很好的选项，不过记得只在开发阶段使用它。

	module.exports = {
	  devtool: 'eval-source-map',//配置生成Source Maps，选择合适的选项
	  entry:  __dirname + "/app/main.js",
	  output: {
	    path: __dirname + "/public",
	    filename: "bundle.js"
	  }
	}

>`cheap-module-eval-source-map`方法构建速度更快，但是不利于调试，推荐在大型项目考虑打包时间成本时使用。

**7.使用webpack构建本地服务器**[实践未成功]

 Webpack提供一个可选的本地开发服务器，这个本地服务器基于node.js构建，实现浏览器监测你代码的修改，并自动刷新修改后的结果。不过它是一个单独的组件，在webpack中进行配置之前需要单独安装它作为项目依赖。

	npm install --save-dev webpack-dev-server


| devserver配置选项  | 功能描述  |

| contentBase | 默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到“public"目录） |

| port | 设置默认监听端口，如果省略，默认为”8080“ |

| inline | 设置为true，当源文件改变时会自动刷新页面 |

| colors | 设置为true，使终端输出的文件为彩色的 |

| historyApiFallback | 在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html |

   继续把这些命令加到webpack的配置文件中，现在的配置文件如下所示

	module.exports = {
		devtool: 'eval-source-map',//配置生成Source Maps，选择合适的选项
	  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
	  output: {
	    path: __dirname + "/public",//打包后的文件存放的地方
	    filename: "index.js"//打包后输出文件的文件名
	  },
	  devServer: {
	    contentBase: "./public",//本地服务器所加载的页面所在的目录
	    colors: true,//终端中输出结果为彩色
	    historyApiFallback: true,//不跳转
	    inline: true//实时刷新
	  } 
	};

**8.Loaders**

   通过使用不同的loader，webpack通过调用外部的脚本或工具可以对各种各样的格式的文件进行处理，比如说分析JSON文件并把它转换为JavaScript文件，或者说把下一代的JS文件（ES6，ES7)转换为现代浏览器可以识别的JS文件。或者说对React的开发而言，合适的Loaders可以把React的JSX文件转换为JS文件。

   Loaders需要单独安装并且需要在webpack.config.js下的modules关键字下进行配置，Loaders的配置选项包括以下几方面：

- test：一个匹配loaders所处理的文件的拓展名的正则表达式（必须）
- loader：loader的名称（必须）
- include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
- query：为loaders提供额外的设置选项（可选）

   我们把Greeter.js里的问候消息放在一个单独的JSON文件里,并通过合适的配置使Greeter.js可以读取该JSON文件的值，配置方法如下

	//安装可以装换JSON的loader
	npm install --save-dev json-loader
	
   修改配置文件:

	module.exports = {
		devtool: 'eval-source-map',//配置生成Source Maps，选择合适的选项
		  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
		  output: {
		    path: __dirname + "/public",//打包后的文件存放的地方
		    filename: "index.js"//打包后输出文件的文件名
		  },
		  devServer: {
		    contentBase: "./public",//本地服务器所加载的页面所在的目录
		    colors: true,//终端中输出结果为彩色
		    port: 8080,
		    historyApiFallback: true,//不跳转
		    inline: true//实时刷新
		  },
		  module: {//在配置文件里添加JSON loader
		    loaders: [
		      {
		        test: /\.json$/,
		        loader: "json"
		      }
		    ]
		  }
		};

   创建带有问候信息的JSON文件(命名为config.json):

	//config.json
	{
	  "greetText": "Hi there and greetings from JSON!"
	}

   更新后的Greeter.js

	var config = require('./config.json');

	module.exports = function() {
	  var greet = document.createElement('div');
	  greet.textContent = config.greetText;
	  return greet;
	};

   Loaders很好，不过有的Loaders使用起来比较复杂，比如说Babel。
