#Webpack


##简介


WHY：越来越多的网站已经从网页模式进化到了 Webapp 模式。它们运行在现代的高级浏览器里，使用 HTML5、 CSS3、 ES6 等更新的技术来开发丰富的功能，网页已经不仅仅是完成浏览的基本需求，并且webapp通常是一个单页面应用，每一个视图通过异步的方式加载，这导致页面初始化和使用过程中会加载越来越多的 JavaScript 代码，这给前端开发的流程和资源组织带来了巨大的挑战。如何在开发环境组织好这些碎片化的代码和资源，并且保证他们在浏览器端快速、优雅的加载和更新，就需要一个模块化系统。

 WHAT：Webpack 是当下最热门的前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。Webpack 是由德国开发者 Tobias Koppers 开发。
 
![图片](webpack.svg)

HOW：Webpack把你的项目当做一个整体，通过一个给定的主文件找到项目的所有依赖文件，并使用loaders处理它们，最后打包为一个浏览器可识别的JavaScript文件。在编译的时候，要对整个代码进行静态分析，分析出各个模块的类型和它们依赖关系，然后将不同类型的模块提交给适配的加载器来处理。

Webpack 本身只能处理原生的 JavaScript 模块，但是 loader 转换器可以将各种类型的资源转换成 JavaScript 模块。这样，任何资源都可以成为 Webpack 可以处理的模块。

### 优点
模块来源广泛，支持包括npm/bower等等的各种主流模块安装／依赖解决方案

模块规范支持全面，amd/cmd以及shimming等一应具全

浏览器端足迹小，移动端友好，却对热加载乃至热替换有很好的支持

插件机制完善，实现本身实现同样模块化，容易扩展
### 与 Gulp/Grunt，require/seajs 的区别
Wepack 与 Gulp/Grunt 并非同类。Gulp / Grunt 是一种工具，能够优化前端工作流程。比如自动刷新页面、combo、压缩css、js、编译less等等。Webpack 则是模块化的方案，可以对比 require 和 seajs。那么它和 require/seajs 又有什么区别呢？

require/seajs 是一种在线"编译" 模块的方案，相当于在页面上加载一个 CMD/AMD 解释器。这样浏览器就认识了 define、exports、module 这些东西。也就实现了模块化。
webpack则是一个预编译模块的方案，相比于上面 ，这个方案更加智能。首先，它是预编译的，不需要在浏览器中加载解释器。另外，你在本地直接写JS，不管是 AMD / CMD / ES6 风格的模块化，它都能认识，并且编译成浏览器认识的JS。


##用法



###主要配置项（四大核心概念）
entry：js入口源文件

output：生成文件

loader：模块和资源的转换器

plugins：插件，比loader更强大，能使用更多webpack的api
###用法

安装 webpack 模块之后, 可是使用 webpack 这个命令行工具
搭配使用参数, 也可以配置 webpack.config.js 文件直接运行 webpack 调用，推荐使用配置文件。

常见目录结构

    app
    |__dist
    |   |__styles
    |   |__js
    |   |    |__bundle.js
    |   |__index.html
    |__src
    |   |__styles
    |   |__js
    |       |__main.js
    |__node_modules
    |__package.json
    |__webpack.config.js
    
下面是一个简单的配置文件示例：

```js
// webpack.config.js
module.exports = {
    entry: './main.js',
    output: {
      	 path: __dirname + '/dist/js',
        filename: 'bundle.js'       
      }
    };
```
此处 main.js 就是入口文件，webpack 将从这个文件入手开始查找项目依赖的文件；output 是打包生成的文件

通过使用不同的loader，webpack通过调用外部的脚本或工具可以对各种各样的格式的文件进行处理，比如说分析JSON文件并把它转换为JavaScript文件，或者说把下一代的JS文件（ES6，ES7)转换为现代浏览器可以识别的JS文件。或者说对React的开发而言，合适的Loaders可以把React的JSX文件转换为JS文件。

test：一个匹配loaders所处理的文件的拓展名的正则表达式（必须）

loader：loader的名称（必须）

include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；

query：为loaders提供额外的设置选项（可选）

插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务。
Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，SCSS，LESS等），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

### 常用Loaders介绍

处理样式，转成css，如：less-loader, sass-loader

图片处理，如: url-loader, file-loader。两个都必须用上。否则超过大小限制的图片无法生成到目标文件夹中

处理js，将es6或更高级的代码转成es5的代码。如：
babel-loader，babel-preset-es2015，babel-preset-react

将js模块暴露到全局，如果expose-loader

### 常用Plugins介绍

代码热替换, HotModuleReplacementPlugin

生成html文件，HtmlWebpackPlugin

将css成生文件，而非内联，ExtractTextPlugin

报错但不退出webpack进程，NoErrorsPlugin

代码丑化，UglifyJsPlugin，开发过程中不建议打开

多个 html共用一个js文件(chunk)，可用CommonsChunkPlugin

清理文件夹，Clean

调用模块的别名ProvidePlugin，例如想在js中用$，如果通过webpack加载，需要将$与jQuery对应起来
### webpack-dev-server
inline：热加载----资源改变时自动重载整个页面；

hot：热替换----重新加载组件改变的部分

当两个参数同时使用时会先尝试热替换，如果失败则使用热加载




