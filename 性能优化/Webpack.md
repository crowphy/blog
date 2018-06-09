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
Webpack把你的项目当做一个整体，通过一个给定的主文件找到项目的所有依赖文件，并使用loaders处理它们，最后打包为一个浏览器可识别的JavaScript文件。在编译的时候，要对整个代码进行静态分析，分析出各个模块的类型和它们依赖关系，然后将不同类型的模块提交给适配的加载器来处理。

4. 如何编写一个 loader

5. 如何编写一个 plugin

6. 常用打包优化手段
