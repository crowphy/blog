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
## 3. 运行原理
### 大致流程 ###
1. 解析webpack配置参数，合并从shell传入和webpack.config.js文件里配置的参数，生产最后的配置结果。
1. 注册所有配置的插件，好让插件监听webpack构建生命周期的事件节点，以做出对应的反应。
1. 从配置的entry入口文件开始解析文件构建AST语法树，找出每个文件所依赖的文件，递归下去。
1. 在解析文件递归的过程中根据文件类型和loader配置找出合适的loader用来对文件进行转换。
1. 递归完后得到每个文件的最终结果，根据entry配置生成代码块chunk。
1. 输出所有chunk到文件系统。
### webpack的生命周期 ###


## 4. 如何编写一个 loader ##

## 5. 如何编写一个 plugin ##

## 6. 常用打包优化手段 ##
