# React_Study
        
## 三、webpack
        

### 作者：冰红茶  
### 参考源：[webpack官网文档https://webpack.js.org/concepts/](https://webpack.js.org/concepts/)  
        
------    
        

        
   没想到就一个打包工具webpack就那么多的东西需要掌握，跟browserify不同的是，webpack真的是大而全，除了支持js，还支持图片，css还有其他的格式。这一次我直接看官网的文档，感觉讲的比其他的书更加具体和详细，但是书本有一个特点比文档要好的是有很多作者的体会在里面^_ ^  
        

## 目录

## [三、webpack](#3)
### [3.1 简介，安装与卸载](#3.1)
### [3.2 一个简单的例子](#3.2) 
### [3.3 资源管理](#3.3) 
### [3.4 多文件输入和输出](#3.4) 
### [3.5 开发者模式](#3.5) 
### [3.6 模块热替换](#3.6) 
### [3.7 摇树优化和代码压缩](#3.7) 
### [3.8 开发模式与产品模式](#3.8) 
### [3.9 代码拆分](#3.9) 
### [3.10 懒加载lazy loading](#3.10) 
------      
        
        
<h2 id='3'>三、webpack</h2>
<h3 id='3.1'>3.1 简介，安装与卸载</h3>  
        
#### 1) 简介
> -  现代的静态模块包装器webpack is a static module bundler for modern JavaScript applications
> -  建立一个依赖图对应你每一个需要打一个或者多个包的依赖模块it internally builds a dependency graph which maps every module your project needs and generates one or more bundles 
> - 打包工具的意义在于减少组件的入口文件书，尽可能将所有的依赖进行内部声明，可以提高组件的内聚度，便于开发和维护
> - webpack简单流程
        
>>>>>> ![图1-1 webpack简单流程](https://github.com/hblvsjtu/React_Study/blob/master/picture/图1-1%20webpack简单流程.png?raw=true)
        
> - 一般来讲，管理JavaScript projects会有以下三个问题：
>> - JavaScript的外部依赖不明显
>> - 如果依赖丢失或者加载的顺序出错，那么这个project的功能会出问题
>> - 如果包含进来的依赖没有用的话，浏览器依然会下载这些没有用的代码
> - 四大核心概念
>> - Entry
>> - Output
>> - Loaders
>> - Plugins
#### 2) 安装
> -  全局安装
        
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install webpack-cli -g
                /Users/lvhongbin/software/node-v10.3.0-darwin-x64/bin/webpack-cli -> /Users/lvhongbin/software/node-v10.3.0-darwin-x64/lib/node_modules/webpack-cli/bin/cli.js
                npm WARN webpack-cli@3.0.2 requires a peer of webpack@^4.x.x but none is installed. You must install peer dependencies yourself.

                + webpack-cli@3.0.2
                added 105 packages from 44 contributors in 20.705s
                
                LvHongbins-Mac-2:webpacktest lvhongbin$ ln -s /Users/lvhongbin/software/node-v10.3.0-darwin-x64/bin/webpack-cli /usr/local/bin/webpack
                LvHongbins-Mac-2:webpacktest lvhongbin$ webpack -v
                3.0.2
> -  本地安装
        
                # 初始化package.json文件
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm init -y
                Wrote to /Users/lvhongbin/Desktop/React_Study/webpacktest/package.json:

                {
                  "name": "webpacktest",
                  "version": "1.0.0",
                  "description": "",
                  "main": "index.js",
                  "scripts": {
                    "test": "echo \"Error: no test specified\" && exit 1"
                  },
                  "keywords": [],
                  "author": "",
                  "license": "ISC"
                }

                # 本地安装webpack webpack-cli两个都要装
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install webpack webpack-cli  --save-dev

                > fsevents@1.2.4 install /Users/lvhongbin/Desktop/React_Study/webpacktest/node_modules/fsevents
                > node install

                [fsevents] Success: "/Users/lvhongbin/Desktop/React_Study/webpacktest/node_modules/fsevents/lib/binding/Release/node-v64-darwin-x64/fse.node" already installed
                Pass --update-binary to reinstall or --build-from-source to recompile
                npm notice created a lockfile as package-lock.json. You should commit this file.
                npm WARN webpacktest@1.0.0 No description
                npm WARN webpacktest@1.0.0 No repository field.

                + webpack@4.10.2
                added 386 packages from 296 contributors and audited 3358 packages in 37.567s
                found 0 vulnerabilities

<h3 id='3.2'>3.2 一个简单的例子</h3>  
        
#### 1) 文件夹
> - src 放源码的地方 The "source" code is the code that we'll write and edit.   
> - dist 经过压缩和优化的，需要最终加载进入浏览器的代码  The "distribution" code is the minimized and optimized output of our build process that will eventually be loaded in the browser: 
#### 2) 本地安装lodash
        
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save lodash
                + lodash@4.17.10
                added 1 package from 2 contributors and audited 3359 packages in 5.532s
                found 0 vulnerabilities
#### 3) 往src/index.js文件中引入lodash 为什么要显式引入呢？主要是为了避免污染全局变量 in this setup, index.js explicitly requires lodash to be present, and binds it as _ (no global scope pollution)
        
                /* ***************************************************************
                 *
                 * * Filename: index.js
                 *
                 * * Description:test for webpack
                 *
                 * * Version: 1.0.0
                 *
                 * * Created: 2018/06/05
                 *
                 * * Revision: none
                 *
                 * * Compiler: node
                 *
                 * * Author: Lv Hongbin
                 *
                 * * Company: Shanghai JiaoTong Univerity
                 *
                /* **************************************************************/

                import _ from 'lodash';

                 function component() {
                   var element = document.createElement('div');

                   /*
                    * Lodash, currently included via a script, is required for this line to work
                    * Our index.js file depends on lodash being included in the page before it runs.
                    * This is because index.js never explicitly declared a need for lodash;
                    * it just assumes that the global variable _ exists.
                    */
                    // Lodash, now imported by this script
                   element.innerHTML = _.join(['Hello', 'webpack'], ' ');

                   return element;
                 }

                 document.body.appendChild(component());
#### 4) dist/index.html
        
                LvHongbins-Mac-2:webpacktest lvhongbin$ cat dist/index.html
                <!--* ***************************************************************
                 *
                 * * Filename: index.html
                 *
                 * * Description:test for webpack
                 *
                 * * Version: 1.0.0
                 *
                 * * Created: 2018/06/05
                 *
                 * * Revision: none
                 *
                 * * Compiler: Browser
                 *
                 * * Author: Lv Hongbin
                 *
                 * * Company: Shanghai JiaoTong Univerity
                 *
                /* **************************************************************-->

                <!DOCTYPE html>
                <html>
                    <head>
                        <meta charset="utf-8">
                        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
                        <title>Getting Started</title>
                        <meta name="description" content="test for webpack">
                        <meta name="keywords" content="test for webpack">
                        <!-- 由于在src/index.js中引入了lodash模块，所以不需要在这里引入 -->
                        <!-- <script src="https://unpkg.com/lodash@4.16.6"></script> -->
                    </head>
                    
                    <body>
                        <!-- 经过打包bundle之后，输出的文件变成了bundle.js -->
                        <!-- <script src="./src/index.js"></script> -->
                        <script src="bundle.js"></script>
                    </body>
                </html>
#### 5) 修改package.json文件，Private可选字段，布尔值。如果private为true，npm会拒绝发布。这可以防止私有repositories不小心被发布出去。
                
                # package.json
                LvHongbins-Mac-2:webpacktest lvhongbin$ cat package.json
                {
                  "name": "webpacktest",
                  "version": "1.0.0",
                  "description": "",
                  "private": true,
                  "scripts": {
                    "test": "echo \"Error: no test specified\" && exit 1"
                  },
                  "keywords": [],
                  "author": "",
                  "license": "ISC",
                  "devDependencies": {
                    "webpack": "^4.10.2",
                    "webpack-cli": "^3.0.2"
                  },
                  "dependencies": {
                    "lodash": "^4.17.10"
                  }
                }
#### 6) 运行 ./node_modules/.bin/webpack命令 或者是npx webpack进行打包
        
                LvHongbins-Mac-2:webpacktest lvhongbin$ ./node_modules/.bin/webpack
                Hash: cbc93658b30e05c9a93b
                Version: webpack 4.10.2
                Time: 685ms
                Built at: 06/05/2018 4:06:35 PM
                  Asset      Size  Chunks             Chunk Names
                main.js  70.4 KiB       0  [emitted]  main
                [1] (webpack)/buildin/module.js 497 bytes {0} [built]
                [2] (webpack)/buildin/global.js 489 bytes {0} [built]
                [3] ./src/index.js 950 bytes {0} [built]
                    + 1 hidden module

                WARNING in configuration
                The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
                You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
#### 7) 配置webpack.config.js
                
                // webpack.config.js
                const path = require('path');
                  
                module.exports = {
                  entry: './src/index.js',
                  output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                  }
                }; 
#### 8) 配合webpack.config.js进行打包
> - npx webpack --config                
                
                # 配合webpack.config.js进行打包
                LvHongbins-Mac-2:webpacktest lvhongbin$ npx webpack --config webpack.config.js
                Hash: f5c5c1cdb63051604e41
                Version: webpack 4.10.2
                Time: 365ms
                Built at: 06/06/2018 12:08:22 AM
                    Asset      Size  Chunks             Chunk Names
                bundle.js  70.4 KiB       0  [emitted]  main
                [1] (webpack)/buildin/module.js 497 bytes {0} [built]
                [2] (webpack)/buildin/global.js 489 bytes {0} [built]
                [3] ./src/index.js 950 bytes {0} [built]
                    + 1 hidden module

                WARNING in configuration
                The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
                You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
> - 假设我们不想用命令行的话，还可以用npm script
                
                npm run build

                # package.json
                  {
                    "name": "webpack-demo",
                    "version": "1.0.0",
                    "description": "",
                    "main": "index.js",
                    "scripts": {
                      "test": "echo \"Error: no test specified\" && exit 1",
                +     "build": "webpack"
                    },
                    "keywords": [],
                    "author": "",
                    "license": "ISC",
                    "devDependencies": {
                      "webpack": "^4.0.1",
                      "webpack-cli": "^2.0.9",
                      "lodash": "^4.17.5"
                    }
                  }

<h3 id='3.3'>3.3 资源管理</h3>  
        
#### 1) 安装插件
> - css文件： style-loader和css-loader
> - 图片资源和字体资源 file-loader
> - Data资源 csv-loader xml-loader
                
                #style-loader css-loader
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev style-loader css-loader
                + style-loader@0.21.0
                + css-loader@0.28.11
                added 130 packages from 222 contributors and audited 4120 packages in 86.743s
                found 0 vulnerabilities

                # postcss-loader
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev postcss-loader
                + postcss-loader@2.1.5
                added 11 packages from 49 contributors and audited 7742 packages in 13.852s
                found 0 vulnerabilities

                # sass-loader node-sass
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev node-sass

                > node-sass@4.9.0 install /Users/lvhongbin/Desktop/React_Study/webpacktest/node_modules/node-sass
                > node scripts/install.js
                
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev sass-loader
                + sass-loader@7.0.3
                added 8 packages from 16 contributors and audited 7762 packages in 11.981s
                found 0 vulnerabilities

                # file-loader
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev file-loader
                + file-loader@1.1.11
                added 1 package from 1 contributor and audited 4133 packages in 7.704s
                found 0 vulnerabilities

                #Data资源 csv-loader xml-loader
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev csv-loader xml-loader
                + csv-loader@2.1.1
                + xml-loader@1.2.1
                added 5 packages from 50 contributors and audited 4147 packages in 17.517s
                found 0 vulnerabilities
> - webpack.config.js
        
                LvHongbins-Mac-2:webpacktest lvhongbin$ cat webpack.config.js
                const path = require('path');
                  
                module.exports = {
                  entry: './src/index.js',
                  output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                  },
                  module: {
                    rules: [
                      {
                        test: /\.css$/,
                        use: [
                          'style-loader',
                          'css-loader'
                        ]
                      },
                      {
                        test: /\.(png|svg|jpg|gif)$/,
                        use: [
                          'file-loader'
                        ]
                      },
                      {
                        test: /\.(woff|woff2|eot|ttf|otf)$/,
                        use: [
                          'file-loader'
                        ]
                      },
                      {
                        test: /\.(csv|tsv)$/,
                        use: [ 
                          'csv-loader'
                        ] 
                      },
                      { 
                        test: /\.xml$/,
                        use: [ 
                          'xml-loader'
                        ] 
                      } 
                    ]
                  }
                };
> - src/style.css
        
                LvHongbins-Mac-2:webpacktest lvhongbin$ cat src/style.css

                LvHongbins-Mac-2:webpacktest lvhongbin$ cat src/style.css
                @font-face {
                  font-family: 'MyFont';
                  src:  url('./YouRock-Regular.woff') format('woff'),
                        url('./YouRock-Extras.woff') format('woff');
                  font-weight: 600;
                  font-style: normal;
                }

                .hello {
                  display:inline-block;
                  width: 400px;
                  height: 200px;
                  font-family: 'MyFont';
                  color: red;
                  background: url('./background.png');
                }
> - data.xml
        
                <?xml version="1.0" encoding="UTF-8"?>
                <note>
                  <to>Mary</to>
                  <from>John</from>
                  <heading>Reminder</heading>
                  <body>Call Cindy on Tuesday</body>
                </note>
> - src/index.js
        
                /* ***************************************************************
                 *
                 * * Filename: index.js
                 *
                 * * Description:test for webpack
                 *
                 * * Version: 1.0.0
                 *
                 * * Created: 2018/06/05
                 *
                 * * Revision: none
                 *
                 * * Compiler: node
                 *
                 * * Author: Lv Hongbin
                 *
                 * * Company: Shanghai JiaoTong Univerity
                 *
                /* **************************************************************/

                import _ from 'lodash';
                import './style.css';
                import Picture from './picture.png';
                import Data from './data.xml';

                 function component() {
                   var element = document.createElement('div');

                   /*
                    * Lodash, currently included via a script, is required for this line to work
                    * Our index.js file depends on lodash being included in the page before it runs.
                    * This is because index.js never explicitly declared a need for lodash;
                    * it just assumes that the global variable _ exists.
                    */
                    // Lodash, now imported by this script
                   element.innerHTML = _.join(['Hello', 'webpack'], ' ');
                   element.classList.add('hello');

                   // Add the image to our existing div.
                   var myPicture = new Image();
                   myPicture.src = Picture;

                   var imageDiv = document.createElement("div");
                   imageDiv.appendChild(myPicture);
                   element.appendChild(imageDiv);

                   console.log(Data);
                   return element;
                 }
> - package.json
        
                LvHongbins-Mac-2:webpacktest lvhongbin$ cat package.json
                {
                  "name": "webpacktest",
                  "version": "1.0.0",
                  "description": "",
                  "private": true,
                  "scripts": {
                    "test": "echo \"Error: no test specified\" && exit 1",
                    "build": "webpack" 
                  },
                  "keywords": [],
                  "author": "",
                  "license": "ISC",
                  "devDependencies": {
                    "css-loader": "^0.28.11",
                    "style-loader": "^0.21.0",
                    "webpack": "^4.10.2",
                    "webpack-cli": "^3.0.2"
                  },
                  "dependencies": {
                    "lodash": "^4.17.10"
                  }
                }
> - 打包
        
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm run build

                > webpacktest@1.0.0 build /Users/lvhongbin/Desktop/React_Study/webpacktest
                > webpack

                Hash: e7782d12a81c6931ee2e
                Version: webpack 4.10.2
                Time: 3333ms
                Built at: 06/06/2018 12:07:17 PM
                                                Asset      Size  Chunks             Chunk Names
                 af01dccc1a3bc9b4fc6e8032010da11a.png  28.9 KiB          [emitted]  
                 1dbdd9ff2c27da280c13e7bd31188cb7.png  20.2 KiB          [emitted]  
                c5c3c84b266a0e76cfb78b01f35843f2.woff  79.2 KiB          [emitted]  
                6f05ec37b4e216c8e298ce784b60f14a.woff   140 KiB          [emitted]  
                                            bundle.js  77.1 KiB       0  [emitted]  main
                 [0] ./src/data.xml 113 bytes {0} [built]
                 [1] ./src/picture.png 82 bytes {0} [built]
                 [5] ./src/background.png 82 bytes {0} [built]
                 [6] ./src/YouRock-Extras.woff 83 bytes {0} [built]
                 [7] ./src/YouRock-Regular.woff 83 bytes {0} [built]
                [10] ./node_modules/css-loader!./src/style.css 656 bytes {0} [built]
                [11] ./src/style.css 1.05 KiB {0} [built]
                [12] (webpack)/buildin/module.js 497 bytes {0} [built]
                [13] (webpack)/buildin/global.js 489 bytes {0} [built]
                [14] ./src/index.js 1.29 KiB {0} [built]
                    + 5 hidden modules

                WARNING in configuration
                The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
                You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
> - 效果图
>>>>>> ![图1-2 资源加载效果图](https://github.com/hblvsjtu/React_Study/blob/master/picture/图2-1%20资源加载效果图.png?raw=true)
        

<h3 id='3.4'>3.4 多文件输入和输出</h3>  
        
#### 1) 多文件输入
> - 需要在webpack.config.js的entry属性赋予一个文件对象，文件对象的形式如下：
        
                entry: {
                   app:'./src/index.js',
                   print: './src/print.js',
                   findGF: "./src/findGF.js"
                }

#### 2) 多文件输出
> - 则在filename中添加一个变量\[name\]
        
                output: {
                  filename: '[name]/bundle.js',
                  path: path.resolve(__dirname, 'dist')
                },
> - 奇怪的是，在webpack.config.js中引用的是commonJS的规范，而对于打包的commonJS的规范的JS文件虽然可以通过，但是浏览器却找不到相应的引用模块变量，所以等同于不支持ommonJS的规范的JS文件，对于支持es6的文件是可以成功运行的
#### 3) 插件html-webpack-plugin    
> - 有时候假如更改入口文件的名字，那么相应的html也需要跟着改变，带来了不便
> - html-webpack-plugin可以自动生成index.html文件，并把相应的依赖加进去body标签里面
        
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev html-webpack-plugin
                + html-webpack-plugin@3.2.0
                added 49 packages from 66 contributors and audited 4221 packages in 22.032s
                found 0 vulnerabilities
> - 设置webpack.config.js文件
        
                const HtmlWebpackPlugin = require('html-webpack-plugin');
                plugins: [
                    new HtmlWebpackPlugin({
                        title: 'Output Management'
                      })
                    ],
> - 不过这里有一个问题：会把原来的index.html文件自动覆盖掉 
#### 3) 插件clean-webpack-plugin    
> - 有时候我们在build的时候，由于旧的文件的存在会干扰我们对于每次生成的新文件的判断
> - 所以最好的方式每次在build之前都清空dist文件，再进行build，使得每次的dist文件都是最新的
                
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install clean-webpack-plugin --save-dev
                + clean-webpack-plugin@0.1.19
                added 1 package from 1 contributor and audited 4237 packages in 7.353s
                found 0 vulnerabilities
> - 设置webpack.config.js文件
        
                const CleanWebpackPlugin = require('clean-webpack-plugin');
                const HtmlWebpackPlugin = require('html-webpack-plugin');
                plugins: [
                    new CleanWebpackPlugin(['dist
                    new HtmlWebpackPlugin({
                        title: 'Output Management'
                      })                    
                    ],
> - 结果
                
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm run build

                > webpacktest@1.0.0 build /Users/lvhongbin/Desktop/React_Study/webpacktest
                > webpack

                clean-webpack-plugin: /Users/lvhongbin/Desktop/React_Study/webpacktest/dist has been removed.
                Hash: 43296dc863ac84457e39
                Version: webpack 4.10.2
                Time: 808ms
                Built at: 06/06/2018 4:36:00 PM
                                                Asset       Size  Chunks             Chunk Names
                 af01dccc1a3bc9b4fc6e8032010da11a.png   28.9 KiB          [emitted]  
                 1dbdd9ff2c27da280c13e7bd31188cb7.png   20.2 KiB          [emitted]  
                c5c3c84b266a0e76cfb78b01f35843f2.woff   79.2 KiB          [emitted]  
                6f05ec37b4e216c8e298ce784b60f14a.woff    140 KiB          [emitted]  
                                     findGF/bundle.js   1.52 KiB       0  [emitted]  findGF
                                      print/bundle.js   1.02 KiB       1  [emitted]  print
                                        app/bundle.js   77.4 KiB    2, 1  [emitted]  app
                                           index.html  317 bytes          [emitted]  
                 [0] ./src/print.js 451 bytes {1} {2} [built]
                 [1] ./src/data.xml 113 bytes {2} [built]
                 [2] ./src/picture.png 82 bytes {2} [built]
                 [4] ./src/findGF.js + 1 modules 2.61 KiB {0} [built]
                     | ./src/findGF.js 692 bytes [built]
                     | ./src/condition.js 1.93 KiB [built]
                 [7] ./src/background.png 82 bytes {2} [built]
                 [8] ./src/YouRock-Extras.woff 83 bytes {2} [built]
                 [9] ./src/YouRock-Regular.woff 83 bytes {2} [built]
                [12] ./node_modules/css-loader!./src/style.css 656 bytes {2} [built]
                [13] ./src/style.css 1.05 KiB {2} [built]
                [14] (webpack)/buildin/module.js 497 bytes {2} [built]
                [15] (webpack)/buildin/global.js 489 bytes {2} [built]
                [16] ./src/index.js 1.58 KiB {2} [built]
                    + 5 hidden modules

                WARNING in configuration
                The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
                You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
                Child html-webpack-plugin for "index.html":
                     1 asset
                    [0] (webpack)/buildin/module.js 497 bytes {0} [built]
                    [1] (webpack)/buildin/global.js 489 bytes {0} [built]
                        + 2 hidden modules

<h3 id='3.5'>3.5 开发者模式</h3>  
        
#### 1) Using source maps
> - 用来检查出错的源代码的位置
> - 在webpack.config.js文件中添加
                
                devtool: 'inline-source-map'
> - source maps好像没啥用浏览器本来就可以显示错误的地方
>>>>>> ![图1-3 source-maps好像没啥用浏览器本来就可以显示错误的地方](https://github.com/hblvsjtu/React_Study/blob/master/picture/图1-3%20source-maps好像没啥用浏览器本来就可以显示错误的地方.png?raw=true)
        
#### 2) Using Watch Mode
> - 实质上是运行lic命令 webpack --watch
> - 当然了你也可以在package.json中添加脚本
        
                "watch": "webpack --watch",
> - 作用是监察文件的变动，一旦文件发生了更新就会自动编译
> - You can instruct webpack to "watch" all files within your dependency graph for changes. If one of these files is updated, the code will be recompiled so you don't have to run the full build manually.
#### 3) Using webpack-dev-server
> - 相当于livereload，一旦文件发生了更新，就会自动编译，并在网页中自动更新
> - 安装
        
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev webpack-dev-server
                + webpack-dev-server@3.1.4
                added 183 packages from 125 contributors and audited 6914 packages in 75.843s
                found 0 vulnerabilities
> - 在webpack.config.js文件中添加
        
                devServer: {
                    contentBase: './dist'
                  },
> - This tells webpack-dev-server to serve the files from the dist directory on localhost:8080.
> - 初次运行的时候需要执行命令
        
                webpack-dev-server --open
> - 当然了你也可以当然了你也可以在package.json中添加脚本
        
                "start": "webpack-dev-server --open"
> - webpack-dev-server有很多警告，并且结束之后会删掉dist文件夹
>>>>>> ![图1-4 webpack-dev-server有很多警告](https://github.com/hblvsjtu/React_Study/blob/master/picture/图1-4%20webpack-dev-server有很多警告.png?raw=true)
#### 4) Using webpack-dev-middleware
> -  这个我不是很理解，好像需要配合express一起使用，检测任意指定的端口
        
<h3 id='3.6'>3.6 模块热替换</h3>  
        
#### 1) 实时的局部更新
> - It allows all kinds of modules to be updated at runtime without the need for a full refresh.
#### 2) 更新webpack-dev-server的设置
> - 在webpack.config.js文件中添加
        
                # "NamedModulesPlugin" to make it easier to see which dependencies are being 
                # 
                const webpack = require('webpack');
                devServer: {
                  contentBase: './dist',
            +     hot: true
                },
                plugins: [
            +     new webpack.NamedModulesPlugin(),
            +     new webpack.HotModuleReplacementPlugin()
                ],
> - 在index.js中添加
                
                + import {printMe} from './print.js';
                + if (module.hot) {
                +   module.hot.accept('./print.js', function() {
                +     console.log('Accepting the updated printMe module!');
                +     printMe();
                +   })
                + }
> - 调试命令行
        
                webpack-dev-server --hotOnly
> - 在print.js中增加胰腺癌语句然后保存
                
                export var printMe = function() {
                    console.log('Updating print.js...');
                }
> - 调试结果
>>>>>> ![图1-5 模块热替换](https://github.com/hblvsjtu/React_Study/blob/master/picture/图1-5%20模块热替换.png?raw=true)
> - If you took the route of using webpack-dev-middleware instead of webpack-dev-server, please use the webpack-hot-middleware package to enable HMR on your custom server or application.
#### 3）Via the Node.js API
> - To enable HMR, you also need to modify your webpack configuration object to include the HMR entry points. The webpack-dev-server package includes a method called addDevServerEntrypoints which you can use to do this. 
                    
                    const webpackDevServer = require('webpack-dev-server');
                    const webpack = require('webpack');

                    const config = require('./webpack.config.js');
                    const options = {
                      contentBase: './dist',
                      hot: true,
                      host: 'localhost'
                    };

                    webpackDevServer.addDevServerEntrypoints(config, options);
                    const compiler = webpack(config);
                    const server = new webpackDevServer(compiler, options);

                    server.listen(5000, 'localhost', () => {
                      console.log('dev server listening on port 5000');
                    });        

        
<h3 id='3.7'>3.7 摇树优化和代码压缩</h3>  
        
#### 1) 简介
> - Tree shaking is a term commonly used in the JavaScript context for dead-code elimination
> - ["在webpack中，tree-shaking指的就是按需加载，即没有被引用的模块不会被打包进来，减少我们的包大小，缩小应用的加载时间，呈现给用户更佳的体验。"](https://blog.csdn.net/haodawang/article/details/77199980)
> - 那什么叫dead-code呢？dead-code meaning an unused export that should be dropped
#### 2) 那应该怎么做呢？
> - 针对package.json
                
                {
                  "name": "your-project",
                  "sideEffects": false
                }
> - All the code noted above does not contain side effects, so we can simply mark the property as false to inform webpack that it can safely prune unused exports.
> - 如果确实需要dead-code的副作用，那么可以这样写
                
                {
                  "name": "your-project",
                  "sideEffects": [
                    "./src/some-side-effectful-file.js"
                  ]
                }
> - 摇树优化后
>>>>>> ![图1-6 摇树优化前](https://github.com/hblvsjtu/React_Study/blob/master/picture/图1-6%20摇树优化前.png?raw=true)
> - 摇树优化前
>>>>>> ![图1-7 摇树优化后](https://github.com/hblvsjtu/React_Study/blob/master/picture/图1-7%20摇树优化后.png?raw=true)
#### 3) 代码压缩 
> -  we'll use the -p (production) webpack compilation flag to enable the uglifyjs minification plugin.
> - 第一种方法 使用产品模式
                
                # package.json文件
                mode: "production"
> - 第二种方法 使用命令
                
                webpack --optimize-minimize
> - Note that the --optimize-minimize flag can be used to insert the UglifyJsPlugin as well.
> - tree shaking can yield a significant decrease in bundle size when working on larger applications with complex dependency trees.
> - 第三种方法 安装uglifyjs-webpack-plugin，只针对js
>> - 安装命令 
                
                # 安装命令
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev uglifyjs-webpack-plugin
                + uglifyjs-webpack-plugin@1.2.5
                updated 1 package and audited 7172 packages in 14.704s
                found 0 vulnerabilities
>> - 使用方法 
                # 使用方法 更改webpack.config.js文件
                const UglifyJsPlugin = require('uglifyjs-webpack-plugin')

                module.exports = {
                  plugins: [
                    new UglifyJsPlugin()
                  ]
                }
  
 **Name** | **Type** | **Default** | **Description**
 - | - | - | -
 test | \{RegExp或者Array<RegExp>\} | /\.js$/i | Test to match files against
 include | \{RegExp或者Array<RegExp>\} | undefined | Files to include
 exclude | \{RegExp或者Array<RegExp>\} | undefined | Files to exclude
 cache | \{Boolean或者String\} | false | Enable file caching
 parallel | \{Boolean或者Number\} | false | Use multi-process parallel running to improve the build speed
 sourceMap | \{Boolean\} | false | Use source maps to map error message locations to modules (This slows down the compilation) ⚠️ cheap-source-map options don't work with this plugin
 uglifyOptions | \{Object} | \{...defaults\} | uglify Options
 extractComments | \{Boolean或者RegExp或者Function<(node, comment) -> \{Boolean或者Object\}>} | false | Whether comments shall be extracted to a separate file, (see details (webpack >= 2.3.0)
 warningsFilter | \{Function(source) -> \{Boolean\}\} | () => true | Allow to filter uglify warnings 

> - 第四种方法 [安装Babel Minify Webpack Plugin](https://github.com/webpack-contrib/babel-minify-webpack-plugin) 只针对js，一边Babel一边压缩不要太爽
> - 第五种方法 [安装webpack-closure-compiler](https://github.com/roman01la/webpack-closure-compiler) 只针对js，流程化的Babel和压缩过程 不过需要提前安装Java SDK.
#### 4) css压缩 
> - webpack 5 是自带 CSS minimizer，但是webpack 4 及以下需要你自己去安装
> - While webpack 5 is likely to come with a CSS minimizer built-in, with webpack 4 you need to bring your own.
> - 安装 [mini-css-extract-plugin](https://webpack.js.org/plugins/mini-css-extract-plugin/#minimizing-for-production)
                
                # mini-css-extract-plugin
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev mini-css-extract-plugin
                + mini-css-extract-plugin@0.4.0
                added 1 package from 1 contributor and audited 7180 packages in 8.707s
                found 0 vulnerabilities
> - 使用方法
                
                # webpack.common.js
                const MiniCssExtractPlugin = require("mini-css-extract-plugin");
                const devMode = process.env.NODE_ENV !== 'production'

                module.exports = {
                  plugins: [
                    new MiniCssExtractPlugin({
                      // Options similar to the same options in webpackOptions.output
                      // both options are optional
                      filename: devMode ? '[name].css' : '[name].[hash].css',
                      chunkFilename: devMode ? '[id].css' : '[id].[hash].css',
                    })
                  ],
                  module: {
                    rules: [
                      {
                        test: /\.s?[ac]ss$/,
                        use: [
                          devMode ? 'style-loader' : MiniCssExtractPlugin.loader,
                          'css-loader',
                          'postcss-loader',
                          'sass-loader',
                        ],
                      }
                    ]
                  }
                }
#### 5) 整体优化 [Optimize CSS Assets Webpack Plugin](https://github.com/NMFR/optimize-css-assets-webpack-plugin) 
> - It will search for CSS assets during the Webpack build and will optimize \ minimize the CSS (by default it uses cssnano but a custom CSS processor can be specified).Solves extract-text-webpack-plugin CSS duplication problem:
> - 安装  
                
                # optimize-css-assets-webpack-plugin
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev optimize-css-assets-webpack-plugin
                + optimize-css-assets-webpack-plugin@4.0.2
                added 2 packages from 1 contributor in 25.703s
> - 使用方法
                
                var OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');
                optimization: {
                    minimizer: [
                      new UglifyJsPlugin({
                        cache: true,
                        parallel: true,
                        sourceMap: true // set to true if you want JS source maps
                      }),
                      new OptimizeCSSAssetsPlugin({})
                    ]
                  },

                       
<h3 id='3.8'>3.8 开发模式与产品模式</h3>  
        
#### 1) 开发模式和产品模式
> - 开发模式针对的是强烈的源码映射和主机服务器的实时刷的特性或者是一些热替换模块
> - 产品模式则针对的是最小化打包，轻量化源码映射和最优化资源加载时间
> - The goals of development and production builds differ greatly. In development, we want strong source mapping and a localhost server with live reloading or hot module replacement. In production, our goals shift to a focus on minified bundles, lighter weight source maps, and optimized assets to improve load time. With this logical separation at hand, we typically recommend writing separate webpack configurations for each environment
#### 2) webpack-merge
> - 虽然我们总是希望把开发模式和产品模式分的很开，但是他们有些配置的相同的，而把这些相同的配置合并在一起，这时候我们就需要用到webpack-merge
> - 安装webpack-merge
                
                LvHongbins-Mac-2:webpacktest lvhongbin$ npm install --save-dev webpack-merge
                + webpack-merge@4.1.2
                added 1 package from 1 contributor and audited 6916 packages in 16.389s
                found 0 vulnerabilities
#### 3) 实例
> - **webpack.common.js**  设置入口和输出，还有一些共同的插件 
> - In webpack.common.js, we now have our entry and output setup configured and we've included any plugins that are required for both environments
                
                /* ***************************************************************
                 *
                 * * Filename: webpack.common.js
                 *
                 * * Description:common configuration for webpack
                 *
                 * * Version: 1.0.0
                 *
                 * * Created: 2018/06/07
                 *
                 * * Revision: none
                 *
                 * * Compiler: node
                 *
                 * * Author: Lv Hongbin
                 *
                 * * Company: Shanghai JiaoTong Univerity
                 *
                /* **************************************************************/

                const path = require('path');
                const CleanWebpackPlugin = require('clean-webpack-plugin');
                const HtmlWebpackPlugin = require('html-webpack-plugin');

                module.exports = {
                  entry: {
                    app:'./src/index.js',
                    findGF: "./src/findGF.js"
                  },
                  plugins: [
                    new CleanWebpackPlugin(['dist']),
                    new HtmlWebpackPlugin({
                      title: 'Production'
                    })
                  ],
                  output: {
                    filename: '[name].bundle.js',
                    path: path.resolve(__dirname, 'dist')
                  },
                  module: {
                    rules: [
                      {
                        test: /\.css$/,
                        use: [
                          'style-loader',
                          'css-loader'
                        ]
                      },
                      {
                        test: /\.(png|svg|jpg|gif)$/,
                        use: [
                          'file-loader'
                        ]
                      },
                      {
                        test: /\.(woff|woff2|eot|ttf|otf)$/,
                        use: [
                          'file-loader'
                        ]
                      },
                      {
                        test: /\.(csv|tsv)$/,
                        use: [
                          'csv-loader'
                        ]
                      },
                      {
                        test: /\.xml$/,
                        use: [
                          'xml-loader'
                        ]
                      }
                    ]
                  }
                }
> - webpack.dev.js
> - 在开发模式者模式下，我们添加了开发者工具，包括源码映射和一些简单的开发者服务器设置
> - In webpack.dev.js, we've set mode to development .Also, we've added the recommended devtool for that environment (strong source mapping), as well as our simple devServer configuration. 
                
                /* ***************************************************************
                 *
                 * * Filename: webpack.dev.js
                 *
                 * * Description: Development configuration for webpack
                 *
                 * * Version: 1.0.0
                 *
                 * * Created: 2018/06/08
                 *
                 * * Revision: none
                 *
                 * * Compiler: node
                 *
                 * * Author: Lv Hongbin
                 *
                 * * Company: Shanghai JiaoTong Univerity
                 *
                /* **************************************************************/


                const merge = require('webpack-merge');
                const common = require('./webpack.common.js');
                const webpack = require('webpack');

                module.exports = merge(common, {
                  mode: 'development',
                  devtool: 'inline-source-map',
                  devServer: {
                    contentBase: './dist',
                    hot: true,
                  },
                  plugins: [
                    new webpack.NamedModulesPlugin(),
                    new webpack.HotModuleReplacementPlugin()
                  ],
                });
> - webpack.prod.js
> - 产品模式下自动加载JS文件压缩模块UglifyJSPlugin
> - Finally, in webpack.prod.js,mode is set to production which loads UglifyJSPlugin which was first introduced by the tree shaking guide.
                
                /* ***************************************************************
                 *
                 * * Filename: webpack.prod.js
                 *
                 * * Description: Production configuration for webpack
                 *
                 * * Version: 1.0.0
                 *
                 * * Created: 2018/06/07
                 *
                 * * Revision: none
                 *
                 * * Compiler: node
                 *
                 * * Author: Lv Hongbin
                 *
                 * * Company: Shanghai JiaoTong Univerity
                 *
                /* **************************************************************/


                const merge = require('webpack-merge');
                const common = require('./webpack.common.js');
                const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
                const webpack = require('webpack');

                module.exports = merge(common, {
                    mode: 'production',
                    devtool: 'source-map',
                    optimization: {
                        minimizer: [
                          new OptimizeCSSAssetsPlugin({})
                        ]
                    },
                    plugins: [
                        new UglifyJSPlugin({
                          cache: true,
                          parallel: true,
                          sourceMap: true // set to true if you want JS source maps
                        }),
                        new MiniCssExtractPlugin({
                          filename: "[name].css",
                          chunkFilename: "[id].css"
                        }),
                        new webpack.DefinePlugin({
                          'process.env.NODE_ENV': JSON.stringify('production')
                        })
                    ],
                });
> - 添加脚本NPM Scripts
                
                "scripts": {
                    "start": "webpack-dev-server --open --config webpack.dev.js",
                    "build": "webpack --config webpack.prod.js"
                },
> - 产品模式下的源码映射
> - 在产品模式追求的是性能和加载时间，但是有时候确实需要源码映射的需求，为了满足这两者之间的平衡，最好采用source-map option，相反在开发模式下使用inline-source-map
> - We encourage you to have source maps enabled in production, as they are useful for debugging as well as running benchmark tests. That said, you should choose one with a fairly quick build speed that's recommended for production use (see devtool). For this guide, we'll use the source-map option in production as opposed to the inline-source-map we used in development:
                
                 const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
                 devtool: 'source-map',
                   plugins: [
                    new UglifyJSPlugin({
                      cache: true,
                      parallel: true,
                      sourceMap: true // set to true if you want JS source maps
                    })
                    ]
> - 指定环境Specify the Environment
> - 把一些不需要用到的测试或者编译的日记logging去掉
> - when not in production some libraries may add additional logging and testing to make debugging easier. However, with process.env.NODE_ENV === 'production' they might drop or add significant portions of code to optimize how things run for your actual users
> - 
                
                # webpack.prod.js
                const webpack = require('webpack');
                plugins: [
                    new webpack.DefinePlugin({
                      'process.env.NODE_ENV': JSON.stringify('production')
                    })
                ]

                // index.js 用来检验产品模式还是开发模式 
                if (process.env.NODE_ENV !== 'production') {
                  console.log('Looks like we are in development mode!');
                }                       

<h3 id='3.9'>3.9 代码拆分</h3>  
        
#### 1) 简介
> - 把bundle拆成几份，然后并行或者根据命令进行加载，从而提升性能，缩短加载时间
> - This feature allows you to split your code into various bundles which can then be loaded on demand or in parallel. It can be used to achieve smaller bundles and control resource load prioritization which, if used correctly, can have a major impact on load time.
#### 2) 几种常用的方法
> - 入口配置 Entry Points: Manually split code using entry configuration.
> - 避免复制 Prevent Duplication: Use the SplitChunks to dedupe and split chunks.
> - 动态导入 Dynamic Imports: Split code via inline function calls within modules.
#### 3) 拆分：入口配置
> - 这个比较简单，即使在入口那里设两个文件名，出口采用\[name\]变量
> - 但是有两个比较明显的缺点
>> - 如果导进的模块中有重复的部分，会被一起导入 If there are any duplicated modules between entry chunks they will be included in both bundles.
>> - 无法动态加载 It isn't as flexible and can't be used to dynamically split code with the core application logic.
                
                const path = require('path');

                module.exports = {
                  mode: 'development',
                  entry: {
                    index: './src/index.js',
                +   another: './src/another-module.js'
                  },
                  output: {
                    filename: '[name].bundle.js',
                    path: path.resolve(__dirname, 'dist')
                  }
                };
#### 4) 拆分：避免复制
> - 用到一个比较重要的属性[optimization.splitChunks](https://webpack.js.org/plugins/split-chunks-plugin/)
> - 他的主要属性包括：
                
                splitChunks: {
                    chunks: "async",
                    minSize: 30000,
                    minChunks: 1,
                    maxAsyncRequests: 5,
                    maxInitialRequests: 3,
                    automaticNameDelimiter: '~',
                    name: true,
                    cacheGroups: {
                        vendors: {
                            test: /[\\/]node_modules[\\/]/,
                            priority: -10
                        },
                        default: {
                            minChunks: 2,
                            priority: -20,
                            reuseExistingChunk: true
                        }
                    }
                } 
> - 实例
                
                +   optimization: {
                +     splitChunks: {
                +       chunks: 'all'
                +     }
                +   }
> - 拆分前
>> - findGF.js
>>>>>> ![图1-8 拆分前findGFbunldjs](https://github.com/hblvsjtu/React_Study/blob/master/picture/图1-8%20拆分前findGFbunldjs.png?raw=true)
>> - index.js
>>>>>> ![图1-9 拆分前indexbundlejs](https://github.com/hblvsjtu/React_Study/blob/master/picture/图1-9%20拆分前indexbundlejs.png?raw=true)
> - 拆分后 vendors~app~findGF.bundle.js 很明显拆分后就把相同的部分放在单独的一个文件里面
>>>>>> ![图1-10 拆分后vendors~app~findGFbundlejs](https://github.com/hblvsjtu/React_Study/blob/master/picture/图1-10%20拆分后vendors~app~findGFbundlejs.png?raw=true)
#### 5) 拆分：动态导入
> - 这个比较复杂，有兴趣可以看[这里](https://webpack.js.org/guides/code-splitting/)

<h3 id='3.10'>3.10 懒加载lazy loading</h3>  
        
#### 1) 简介
> - 就是需要用的时候才加载，不用就不加载，“按命令加载”
> - Lazy, or "on demand", loading is a great way to optimize your site or application. This practice essentially involves splitting your code at logical breakpoints, and then loading it once the user has done something that requires, or will require, a new block of code. This speeds up the initial load of the application and lightens its overall weight as some blocks may never even be loaded.
#### 2) 例子
                
                button.onclick = e => import(/* webpackChunkName: "print" */ './print').then(module => {
                     var print = module.default;
                
                     print();
                   });
> - 使用了ES6的import()，这里的import()扮演了一个返回promise对象的函数角色，参数是模块路径，然后得到文件模块的对象module，而模块输出的对象挂载在这个module上；
> - Note that when using import() on ES6 modules you must reference the .default property as it's the actual module object that will be returned when the promise is resolved.
 
> - 未完待续。。。
