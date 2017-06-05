# 学习流程
> 参考文档: 
>   1. [入门Webpack，看这篇就够了](http://www.jianshu.com/p/42e11515c10f)
>    2. [Webpack for React](http://www.pro-react.com/materials/appendixA/)

## 一. 简单使用webpack打包node模块并且调用步骤
1. 安装好node.js和全局的npm
2. 新建文件夹webpack, 打开cmd转到此处
3. 转到E盘: `E:`
4. 转到路径: `cd WorkSpace\webpack`
5. 初始化npm说明文件package.json: `npm init`
    1. 注意: 此处需要输入一些包的信息, 在name的时候不能用默认,不能用字符 否则后面安装webpack的时候会报错, 可以随便给个名字, 如: 1
6. 在本地安装Webpack作为依赖包: `npm install --save-dev webpack`
    1. 会生成一个node_modules的文件夹, 里面放了各种node的模块包,webpack也是其中一个, 然后它本身依赖的包也会一并加入进来
7. 创建需要的文件夹: `md app` 和 `md public`
8. 进入app文件夹`cd app`, 创建需要的文件 `type nul>Greeter.js` 和 `type nul>main.js`
9. 进入Public文件夹 `cd ..` , `cd public`, 创建页面文件 `type nul>Index.html`
10. 创建好的文件结构如下图:
    ![](http://i4.buimg.com/588926/50ff1684a4a29f4e.png)
11. 在Index.html写入下面文本，加载打包的bundle.js文件
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Webpack Sample Project</title>
    </head>
    <body>
        <div id="root"></div>
        <script src="bundle.js"></script>
    </body>
    </html>
    ```
12. 在Greeter.js文件中写一个node.js的模块来打个招呼
    ```
    // Greeter.js
    module.exports = function () {
        var greet = document.createElement('div');
        greet.textContent = '你好呢';
        return greet;
    };
    ```
    > 注意: module.exports是声明了一个node的模块，为了解决直接用<script>来引用没有命名空间的尴尬, 详细参考： [node.js module初步理解](http://www.cnblogs.com/dolphinX/p/3485260.html)
    
13. 在main.js里获取Greeter模块插入页面,实际上就是调用了greeter的函数
    ```
    // main.js
    var greeter = require('./Greeter.js');
    document.getElementById('root').appendChild(greeter());
    ```
    
14. 开始用webpack指定入口文件， 并打包所有依赖的js文件到指定文件
    ```
    E:/workspace/webpack/node_modules/.bin/webpack app/main.js public/bundle.js
    ```
    ![](http://i4.buimg.com/588926/1108d6c8ca77762e.png)
    > 注意, 如果不是全局安装的webpack, 这里需要用绝对路径来使用webpack命令， 第一个参数(app/main.js)就是用来指定入口文件， 而第二个参数(public/bundle.js)则是用来把依赖的所有js文件打包到这个bundle.js文件
    
15. 我们来调试下压缩的bundle.js, 看下执行顺序以及怎么执行依赖模块的

    ```js
    /******/ (function(modules) { // webpackBootstrap ----------------------------------    第一步   -----------
    /******/ 	// The module cache
    /******/ 	var installedModules = {};
    /******/
    /******/ 	// The require function
    /******/ 	function __webpack_require__(moduleId) {
    /******/
    /******/ 		// Check if module is in cache  -------------------------------  第四步  --------------
    /******/ 		if(installedModules[moduleId]) {
    /******/ 			return installedModules[moduleId].exports;
    /******/ 		}
    /******/ 		// Create a new module (and put it into the cache)
    /******/ 		var module = installedModules[moduleId] = {
    /******/ 			i: moduleId,
    /******/ 			l: false,
    /******/ 			exports: {}
    /******/ 		};
    /******/
    /******/ 		// Execute the module function
    /******/ 		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
    /******/
    /******/ 		// Flag the module as loaded
    /******/ 		module.l = true;
    /******/
    /******/ 		// Return the exports of the module
    /******/ 		return module.exports;
    /******/ 	}
    /******/
    /******/
    /******/ 	// expose the modules object (__webpack_modules__) --------------------    第二步  -------------
    /******/ 	__webpack_require__.m = modules;
    /******/
    /******/ 	// expose the module cache
    /******/ 	__webpack_require__.c = installedModules;
    /******/
    /******/ 	// identity function for calling harmony imports with the correct context
    /******/ 	__webpack_require__.i = function(value) { return value; };
    /******/
    /******/ 	// define getter function for harmony exports
    /******/ 	__webpack_require__.d = function(exports, name, getter) {
    /******/ 		if(!__webpack_require__.o(exports, name)) {
    /******/ 			Object.defineProperty(exports, name, {
    /******/ 				configurable: false,
    /******/ 				enumerable: true,
    /******/ 				get: getter
    /******/ 			});
    /******/ 		}
    /******/ 	};
    /******/
    /******/ 	// getDefaultExport function for compatibility with non-harmony modules
    /******/ 	__webpack_require__.n = function(module) {
    /******/ 		var getter = module && module.__esModule ?
    /******/ 			function getDefault() { return module['default']; } :
    /******/ 			function getModuleExports() { return module; };
    /******/ 		__webpack_require__.d(getter, 'a', getter);
    /******/ 		return getter;
    /******/ 	};
    /******/
    /******/ 	// Object.prototype.hasOwnProperty.call
    /******/ 	__webpack_require__.o = function(object, property) { return Object.prototype.hasOwnProperty.call(object, property); };
    /******/
    /******/ 	// __webpack_public_path__
    /******/ 	__webpack_require__.p = "";
    /******/
    /******/ 	// Load entry module and return exports
    /******/ 	return __webpack_require__(__webpack_require__.s = 1); //--------------  第三步  ---------------
    /******/ })
    /************************************************************************/
    /******/ ([
    /* 0 */
    /***/ (function(module, exports) {
    
    // Greeter.js  ---------------------------------------------------------------   第六步  ------------------
    module.exports = function () {
        var greet = document.createElement('div');
        greet.textContent = '你好呢';
        return greet;
    };
    
    /***/ }),
    /* 1 */
    /***/ (function(module, exports, __webpack_require__) {
    
    // main.js  ------------------------------------------------------------------   第五步  ------------------
    var greeter = __webpack_require__(0);
    document.getElementById('root').appendChild(greeter());
    
    /***/ })
    /******/ ]);
    ```
    1. ==第一步==：进来可以看到modules就是一个数组，里面是放的就是两个模块函数的引用
    ![](http://i1.piimg.com/588926/16d45a88f878a12f.png)
    2. ==第二步==:  声明了一个`__webpack_require__`的函数对象， 用于按照id来调用并且返回模块，在里面存放modules这个全局模块容器，以后方便调用， 还放了些功能函数方便调用
    3. ==第三步==： 重要的一步，开始调用`__webpack_require__`函数了， 注意这里的1， 上面我们看到modules[1]就是main.js里面函数的引用， 所以这里是  从主要模块开始调用
    4. ==第四步==： 先看是否有缓存，没有的话根据ID用call直接开始调用真正的main.js里面的模块函数了
    5. ==第五步==： 调用到main模块， 可以看到把`__webpack_require__`函数的引用也传进来了， 方便进一步调用依赖的模块
    6. ==第六步==:  果然它开始调用0模块了， 就是Greeter.js, 这个模块没有依赖项了， 所有就没有把函数的引用传进来，然后返回了一个div, 里面写着你好呢， 这样一层一层返回就执行完所有的模块了，该做的事做完了， 自然页面就渲染出来了

## 二. 通过配置文件来使用Webpack
1. 通过配置文件来配置webpack包, 不容易出错而且易于部署, 用来代替上面指定入口文件, 压缩文件路径等, 当然还有跟多新功能, 新建配置文件 `type nul>webpack.config.js`, 这也是一个JS的模块
2. 简单定义下入口文件, 和输出路径
    ```
    module.exports = {
      entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
      output: {
        path: __dirname + "/public",//打包后的文件存放的地方
        filename: "bundle.js"//打包后输出文件的文件名
      }
    }
    ```
    > 注：“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。
    
3. 如果现在你要打包就直接输入`E:/workspace/webpack/node_modules/.bin/webpack`就可以了, 这条命令会自动参考配置文件来打包项目
4. 有没有发现上面还要输入路径很烦的, 不过npm可以通过配置来引导任务执行, 相当于规定好npm的配置文件来指定执行任务, 用统一的`npm start`就可以去执行了, 不用知道具体细节, 这些都在配置文件中写清楚了
5. 更改package.json文件, 让`npm start`来执行webpack工作
    ```
    {
      "name": "1",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "start": "webpack" //配置的地方就是这里啦，相当于把npm的start命令指向webpack命令
      },
      "author": "",
      "license": "ISC",
      "devDependencies": {
        "webpack": "^2.6.0"
      }
    }
    ```
6. **Source Map**, 生成方便调试的代码，在webpack.config.js中加入这一句配置：`devtool: "eval-source-map"`
7. **热更新服务器(webpack-dev-server)**：Webpack有一个很实用的功能叫做热替换, 开发过程中都不需要刷新浏览器，任何前端代码的更改都会实时的在浏览器中表现出来，首先需要安装`Webpack-dev-server`到本地工作目录,一个轻量的node.js express服务器: 
    1. 安装命令：`npm install webpack-dev-server --save-dev`
    2. 看下版本： `E:\workspace\webpack\node_modules\.bin\webpack-dev-server -v`, 显示出来的是2.4.5, ==注意了==：网上一堆教你各种写配置文件的, 但是他们没有说是基于什么版本，这是十分坑爹的，看到版本后，一定要去[官方手册](https://webpack.js.org/configuration/)去查怎么配置，因为1.0和2.0版本相差很大，一不小心就错了，比如本文参考的文章中可以用`colors`这个属性，但是在自己的电脑上会报错，一定切记！！！
        ```        
        webpack-dev-server 2.4.5
        webpack 2.6.0
        ```
    3. 配置webpack.config.js如下
        ```
        module.exports = {
          devtool: "eval-source-map",
          entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
          output: {
            path: __dirname + "/public",//打包后的文件存放的地方
            filename: "bundle.js"//打包后输出文件的文件名
          },
        
          devServer: {
            contentBase: "./public",//本地服务器所加载的页面所在的目录
            //colors: true,//终端中输出结果为彩色, 这句不能要，应该是兼容性问题
            historyApiFallback: true,//不跳转
            inline: true//实时刷新
          } 
        }
        ```
    4. 启动服务器： `E:\workspace\webpack\node_modules\.bin\webpack-dev-server`, 如下服务器就算是被启动了, 他提示你可以通过`http://localhost:8080/`访问，并且你动态更改js文件代码，就可以实时更新，注意：更改html是不能更新的，感觉没什么鸟用啊， 刷新下有多大不了的事呀.
    ![](http://i4.buimg.com/588926/60ad230666edca53.png)
        > 小技巧: 服务器启动后要停止用`Ctrl+C`, 再按提示输入`Y`就可以了
        
8. **Loader**：Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。Loader 可以理解为是模块和资源的转换器，它本身是一个函数，接受源文件作为参数，返回转换的结果。这样，我们就可以通过 require 来加载任何类型的模块或文件，比如 CoffeeScript、 JSX、 LESS 或图片。下面就用loader加载一个json文件。
    1. 安装可以转换JSON的loader： 
        ```
        npm install --save-dev json-loader
        ```
    2. 在webpack.config.js中配置如下
        ```
        module.exports = {
          devtool: "eval-source-map",
          entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
          output: {
            path: __dirname + "/public",//打包后的文件存放的地方
            filename: "bundle.js"//打包后输出文件的文件名
          },
        
          devServer: {
            contentBase: "./public",//本地服务器所加载的页面所在的目录
            historyApiFallback: true,//不跳转
            inline: true//实时刷新
          }，
          
          module: {
            loaders: [
                {
                    test: /\.json$/, //一个匹配loaders所处理的文件的拓展名的正则表达式（匹配以.json结尾的的字符串，$表示结尾位置）
                    loader: "json-loader" //loader的名称（必须）
                }
            ]
          }
        }
        ```
    3. 在app目录下:  `cd app` 创建config.json文件：`type nul>config.json`, 并写入招呼语
        ```
        {
            "greetText":"你好呀，我来自JSON!"
        }
        ```
    4. 在Greeter.js中引用JSON
        ```
        var config = require('./config.json');
        module.exports = function () {
            var greet = document.createElement('div');
            greet.textContent = config.greetText;
            return greet;
        };
        ```
    5. 用`npm start`重新压缩下, 打开Index.html即可看见
9. **Babel**: babel其实是一个编译javascript的符合node的模块包，可以通过编译ES6， ES7这些下一代没有被当前浏览器支持的标准编译为当前支持的js代码， 或者将react的JSX扩展语言转为标准的js, 下面使用babel的6版本来示例
    1. 下载babel核心包，loader编译包, es6的转码规则包，react的JSX转码规则包， 中间用空格分开
        ```
        npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
        ```
    2. 配置babel-loader和babel自己的配置文件, 这样webpack会自动读.babelrc配置结合loader允许写ES6的语法和JSX了，webpack会自动转换为当前js标准
        > webpack.config.js
        
        ```
        module.exports = {
          devtool: "eval-source-map",
          entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
          output: {
            path: __dirname + "/public",//打包后的文件存放的地方
            filename: "bundle.js"//打包后输出文件的文件名
          },
        
          devServer: {
            contentBase: "./public",//本地服务器所加载的页面所在的目录
            historyApiFallback: true,//不跳转
            inline: true//实时刷新
          }，
          
          module: {
            loaders: [
                {
                    test: /\.json$/,
                    loader: "json-loader" 
                },
                {
                    test: /\.js$/,
                    exclude: /node_modules/, // 这个模块下的js文件不通过babel转换
                    loader: "babel-loader"
                }
            ]
          }
        }
        ```
        > 创建.babelrc文件`type nul>.babelrc`，并且引入规则如下 (这个奇怪的名字来源于linux系统习惯，用rc结尾的文件代表运行时自动加载的配置等文件)
        
        ```
        {
            "presets": [
                "react",
                "es2015"
            ]
        }
        ```
    3. 我们来用React来试一下， 先装react和react-dom
        ```
        npm install --save-dev react react-dom 
        ```
    4. 使用ES6语法和react的JSX语法来改写下Greeter.js和main.js如下, 
        ```
        import React, {Component} from 'react'
        import config from './config.json';
        class Greeter extends Component{
          render() {
            return (
              <div>
                {config.greetText}
              </div>
            );
          }
        }
        export default Greeter
        ```
        
        ```
        import React from 'react';
        import {render} from 'react-dom';
        import Greeter from './Greeter';
        render(<Greeter/>, document.getElementById('root'));
        ```
    5. 先`npm start`重新编译， 再打开服务器`E:\workspace\webpack\node_modules\.bin\webpack-dev-server`就可以热更新！
10. **CSS处理**:  webpack提供两个工具处理样式表，`css-loader` 和 `style-loader`，二者处理的任务不同，`css-loader`使你能够使用类似@import 和 url(...)的方法实现 require()的功能,`style-loader`将所有的计算后的样式加入页面中，二者组合在一起使你能够把样式表嵌入webpack打包后的JS文件中。
    1. 安装样式的包
        ```
        npm install --save-dev style-loader css-loader
        ```
    2. 在webpack.config.js配置文件中增加loader配置
        ```
        module.exports = {
          devtool: "eval-source-map",
          entry: __dirname + "/app/main.js",//已多次提及的唯一入口文件
          output: {
            path: __dirname + "/public",//打包后的文件存放的地方
            filename: "bundle.js"//打包后输出文件的文件名
          },
        
          devServer: {
            contentBase: "./public",//本地服务器所加载的页面所在的目录
            historyApiFallback: true,//不跳转
            inline: true//实时刷新
          },
        
          module: {
            loaders: [
              {
                test: /\.json$/,
                loader: "json-loader"
              },
              {
                test: /\.js$/,
                exclude: /node_modules/, // 这个模块下的js文件不通过babel转换
                loader: "babel-loader"
              },
              {
                test: /\.css$/,
                loader: 'style-loader!css-loader?modules'//添加对样式表的处理, 并且分开为css模块
              }
            ]
          }
        }
        ```
        > 注：`(style!css?modules)`:感叹号(!)的作用在于使同一文件能够使用不同类型的loader,而`?modules`则是把css打包为单独的模块, 单独打包为一个css文件, 不要这句则会和js打包到一起(05/26注: 调试发现并没有打包为单独的css文件, 而且不用`?modules`css都不起作用, 应该是版本问题, 加了这句才能正常显示样式, 而且后来了解到module里面用loaders是1.*版本的问题, 2.*后都用rules了)
        
    3. 在app文件夹里新建main.css文件: `cd app` >> `type nul>main.cs`, 并且设置写样式如下
        ```
        html {
          box-sizing: border-box;
          -ms-text-size-adjust: 100%;
          -webkit-text-size-adjust: 100%;
        }
        
        *, *:before, *:after {
          box-sizing: inherit;
        }
        
        body {
          margin: 0;
          font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
        }
        
        h1, h2, h3, h4, h5, h6, p, ul {
          margin: 0;
          padding: 0;
        }
        ```
    4. webpack只有单一的入口，其它的模块需要通过 import, require, url等导入相关位置，为了让webpack能找到”main.css“文件，我们把它导入单一入口”main.js “中, main.js配置如下
        ```
        import React from 'react';
        import { render } from 'react-dom';
        import Greeter from './Greeter';
        import './main.css'; //使用require导入css文件
        render(<Greeter />, document.getElementById('root'));
        ```
    5. 再单独为Greeter.js模块增加Greeter.css文件增加样式并且引入Greeter.js文件
        ```
        type nul>Greeter.css
        ```
        
        ```
        #root {
            color: red;
            background-color: #eee;
            padding: 10px;
            border: 3px solid #ccc;
        }
        ```
        ```
        import React, { Component } from 'react'
        import config from './config.json';
        import styles from './Greeter.css'; //导入
        class Greeter extends Component {
            render() {
                return (
                    <div className={styles.root}> // 增加css类名
                        {config.greetText}
                    </div>
                );
            }
        }
        export default Greeter
        ```
11. **插件(Plugins)**: 用来扩展webpack功能的组件, 会在整个构建过程中生效, 执行相关的任务, loaders和Plugins是完全不同的东西, loaders是在打包构建过程中处理各种源文件(JSX, Sass, Less)等转化为当前浏览器支持的格式的一种模块, 而插件是整个构建过程都起作用的
    1. HtmlWebpackPlugin插件:这个插件的作用是依据一个自定义的简单的模板，帮你生成一个最终的Html5文件，这个文件中自动引用了你用wbpack打包后的JS文件, 相当于一个基本页面,  每次编译都在文件名中插入一个不同的哈希值, 防止引用缓存的外部文件问题. 用法如下:
        1. 安装这个插件
            ```
            npm install --save-dev html-webpack-plugin
            ```
        2. 删除目前文件结构中的public文件夹, 里面的bundle.js由webpack打包自动生成, 而Index.html页面则可以通过这个插件自动生成(本例子中不管这个文件夹也是可以的)
        3. 在app目录下创建一个用于插件的Html的模版, 这是个基本的html5默认页面. 注意此刻不用引用例如bundle.js等js或者css文件, 等到插件自动生成的时候会自动引用进来, 本例中命名模版名称为: `index.templ.html`, 代码如下
            ```
            <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="UTF-8">
                <meta name="viewport" content="width=device-width, initial-scale=1.0">
                <meta http-equiv="X-UA-Compatible" content="ie=edge">
                <title>Document</title>
            </head>
            <body>
                <div id='root'>
                </div>
            </body>
            </html>
            ```
        4. 更改webpack配置文件, 新建一个build文件夹来存放最终输出的文件(为什么叫build, 这是个工程习惯)
            ```
            var webpack = require('webpack');
            var HtmlWebpackPlugin = require('html-webpack-plugin');
            module.exports = {
              devtool: "eval-source-map",
              entry: __dirname + "/app/main.js",//已多次提及的唯一入口文件
              output: {
                path: __dirname + "/build",//打包后的文件存放的地方
                filename: "bundle.js"//打包后输出文件的文件名
              },
            
              devServer: {
                contentBase: "./build",//本地服务器所加载的页面所在的目录
                historyApiFallback: true,//不跳转
                inline: true//实时刷新
              },
            
              module: {
                loaders: [
                  { test: /\.json$/,loader: "json-loader"},
                  { test: /\.js$/, exclude: /node_modules/,  loader: "babel-loader"},
                  { test: /\.css$/, loader: "style-loader!css-loader?modules"}
                ]
              },
              plugins: [
                new HtmlWebpackPlugin({
                  template: __dirname + "/app/index.tmpl.html"//new 一个这个插件的实例，并传入相关的参数
                })
              ],
            }
            ```
        5. 运行`npm start`后看到自动生成Public文件夹和bundle.js, Index.html文件

12. 使用`extract-text-webpack-plugin`插件分离css文件:
    1. 安装extract-text-webpack-plugin
        ```
        npm install --save-dev extract-text-webpack-plugin
        ```
    2. 配置webpack.config.js
        ```
        var webpack = require('webpack');
        var HtmlWebpackPlugin = require('html-webpack-plugin');
        const ExtractTextPlugin = require("extract-text-webpack-plugin"); // 引入插件
        
        module.exports = {
          devtool: "eval-source-map",
          entry: __dirname + "/app/main.js",//已多次提及的唯一入口文件
          output: {
            path: __dirname + "/build",//打包后的文件存放的地方
            filename: "bundle.js"//打包后输出文件的文件名
          },
        
          devServer: {
            //contentBase: "./build",//本地服务器所加载的页面所在的目录
            historyApiFallback: true,//不跳转
            inline: true//实时刷新
          },
        
          module: {
            loaders: [
              { test: /\.json$/, loader: "json-loader" },
              { test: /\.js$/, exclude: /node_modules/, loader: "babel-loader" },
              // { test: /\.css$/, loader: "style-loader!css-loader?modules" },
              {
                test: /\.css$/,
                loader: ExtractTextPlugin.extract({ // 分离配置
                  fallback: "style-loader",
                  use: "css-loader"
                })
              }
            ]
          },
          plugins: [
            new HtmlWebpackPlugin({
              template: __dirname + "/app/index.tmpl.html"//new 一个这个插件的实例，并传入相关的参数
            }),
            new ExtractTextPlugin("styles.css"),  // 新建style.css文件打包所有css
          ],
        }
        ```
        
    3. 重新压缩文件`npm start`, 这样就会在build文件夹下面自动生成style.css文件存放所有的css样式, 同样Index.html文件也会自动引入此文件, 实现css文件从bundle.js中分开
    
    
        
## 三. 产品阶段Webpack构建

        