1.
Q:EACCES
A:sudo

2.
Date.parse：注意此方法的浏览器兼容性，chrome的容错要比webkit的好，在chrome下正常的，在webkit的可能就不行。
Date.parse('19 Jan 2018 00:00:00')两种都行
但是Date.parse('2018-01-19 00:00:00')在webkit下就会返回NaN

3.
Q:ECONNREFUSED
A:端口被占用

4.Q:服务器端渲染时，引用第三方组件库时经常会出现：

    Error: window is not defined
    ReferenceError: window is not defined
        at ...
        at ...
        at e.exports (...)
        at Object. (...)
        at p (...)
        at Object.e.exports.render.e (...)
        at p (...)
        at Object. (...)
        at p (...)
        at e.__esModule.default (...)
A:因为`style-loader`当前不支持局部导出（local export），而直接引用这些组件库会经过`style-loader`的处理
