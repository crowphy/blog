# Webpack

## 1. 思想概述
模块化，将所有资源视为模块来加载。Webpack 本身只能处理原生的 JavaScript 模块，但是 loader 转换器可以将各种类型的资源转换成 JavaScript 模块。这样，任何资源都可以成为 Webpack 可以处理的模块。
### 四个核心概念：
* 入口（entry）
* 输出（output）
* loader：模块和资源的转换器，让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。
* plugins：比loader更强大，能使用更多webpack的api。想要使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建它的一个实例。

## 2. 配置简介
安装 webpack 模块之后, 可以使用 webpack 这个命令行工具
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


    // webpack.config.js
    module.exports = {
    	entry: './main.js',
    	output: {
    	  	path: __dirname + '/dist/js',
    	filename: 'bundle.js'   
    	}
    };

## 3. 运行原理
### 大致流程 ###
1. 解析webpack配置参数，合并从shell传入和webpack.config.js文件里配置的参数，生产最后的配置结果。
1. 注册所有配置的插件，好让插件监听webpack构建生命周期的事件节点，以做出对应的反应。
1. 从配置的entry入口文件开始解析文件构建AST语法树，找出每个文件所依赖的文件，递归下去。
1. 在解析文件递归的过程中根据文件类型和loader配置找出合适的loader用来对文件进行转换。
1. 递归完后得到每个文件的最终结果，根据entry配置生成代码块chunk。
1. 输出所有chunk到文件系统。

## 4. 如何编写一个 loader ##

loader的入口需要导出一个函数，这个函数要干的事情就是转换一个文件的内容。
函数接收的参数content是一个文件在转换前的字符串形式内容，需要返回一个新的字符串形式内容作为转换后的结果，所有通过模块化倒入的文件都会经过loader。从这里可以看出loader只能处理一个个单独的文件而不能处理代码块。

## 5. 如何编写一个 plugin ##
插件是 webpack 的支柱功能。通过插件可以实现loader所不能实现的功能。在webpack的生命周期中会广播出许多事件，插件会监听这些事件，然后在合适的时机通过webpack提供的api更改输出结果。
###webpack plugin 的2个核心概念 ###

- Compiler: Compiler对象存放着Webpack配置，Webpack从启动到退出只存在一个Compiler对象，可在整个编译生命周期访问，它是全局唯一的，可以简单地把它理解为 Webpack 实例。

- Compilation: Compilation对象代表一次编译，包含了当前的模块资源，编译生成资源，变化的文件等。在开发模式的时候，每次文件变化就会触发一次。通过 Compilation 能读取到 Compiler 对象，同时也提供了很多事件回调供插件做扩展。

Compiler 和 Compilation 都继承自 Tapable，可以直接在 Compiler 和 Compilation 对象上广播和监听事件。

- 分发事件：compiler.apply('event-name', params);
- 监听事件：compiler.plugin('event-name', function(params) {})


Compilation同上
#### 注意点 ####
- 只要能拿到 Compiler 或 Compilation 对象，就能广播出新的事件，所以在新开发的插件中也能广播出事件，给其它插件监听使用。
- 传给每个插件的 Compiler 和 Compilation 对象都是同一个引用。也就是说在一个插件中修改了 Compiler 或 Compilation 对象上的属性，会影响到后面的插件。
- 有些事件是异步的，这些异步的事件会附带两个参数，第二个参数为回调函数，在插件处理完任务时需要调用回调函数通知 Webpack，才会进入下一处理流程。



    compiler.plugin('emit',function(compilation, callback) {
      // 支持处理逻辑
    
      // 处理完毕后执行 callback 以通知 Webpack 
      // 如果不执行 callback，运行流程将会一直卡在这不往下执行 
      callback();
    });


### webpack生命周期的事件节点（提供给插件监听） ###
- should-emit
- **done**： 在成功构建并且输出了文件后，Webpack 即将退出时发生；
- additional-pass(异步)
- **before-run**(异步)
- **run**(异步)
- **emit**(异步): 发生 emit 事件时所有模块的转换和代码块对应的文件已经生成好，
需要输出的资源即将输出，emit 事件是修改 Webpack 输出资源的最后时机。
- **after-emit(异步)**
- this-compilation
- compilation
- normal-module-actory
- context-module-factory
- **before-compile(异步)**
- **compile**
- **make(异步)**
- **after-compile(异步)**
- **watch-run(异步)**: 文件发生变化时触发；
- failed: 在构建出现异常导致构建失败，Webpack 即将退出时发生；
- invalid
- watch-close

参见`webpack/lib/Compiler.js`

## 6. 常用打包优化手段 ##
