# 一些问题

### vue双向绑定原理
双向绑定指的是数据和视图之间的绑定，数据变化引起视图自动更新，反之，视图的变化比如表单输入则会反馈给数据。
vue通过数据劫持的方式实现数据到视图的绑定，它借助ES5的defineProperty方法，通过get进行依赖收集，即收集在模板中使用到的数据以添加订阅者。通过set监听数据的改变，并将数据的变化同步到视图。
然后通过事件的形式监听视图的变化然后修改数据。
### 作用域
可以将作用域定义为一套规则，这套规则用来管理搜索引擎如何在当前作用域及嵌套的作用域中根据标识符进行变量查找，即变量查找的规则。

### 作用域链
由当前环境和其上层环境的一系列变量对象组成，它保证了当前环境对符合访问权限的变量和函数的有序访问。
### 原型链
每个构造函数都有一个原型对象，原型对象包含一个指向构造函数的指针（constructor），而实例都包含一个指向原型对象的内部指针（[[Prorotype]]）。如果让原型对象等于另一个类型的实例，那么这个原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进就构成了实例与原型的链条，也就是原型链。
### 闭包
闭包的解释有好多种，以MDN的解释为例：闭包是一种特殊的对象，它由两部分构成：函数以及创建该函数的环境。函数就是被return的函数，环境则是由闭包创建时所在作用域的任何局部变量组成。举个例子，函数a中return了函数b，一旦函数b中有使用到a中的变量就形成了闭包。闭包一般用于模拟私有方法或者保存对一些变量的引用使之常驻内存。因此运用不当容易引起内存泄漏。

### 事件循环

### DOM事件

### WebSocket

### HTTP/2

### CSS初始化

### H5新增API

### ES6新增

### 浏览器输入一个URL到页面显示出来的全过程，尽可能详细

### JS的执行过程

### 偏函数
Partial Application(偏函数应用) 是指使用一个函数并将其应用一个或多个参数，但不是全部参数，在这个过程中创建一个新函数接受余下参数。即将一个 n 元函数转换为一个你 n-x 元的函数，其中 x 为在创建新函数时随旧函数传入的参数个数。bind方法也是偏函数的应用，只不过它会改变 this 的指向。

代码：

```js
function partial(fn) {
  
  var args = Array.prototype.slice.call(arguments, 1) || [];
  var parted = function () {
    args = args.concat(Array.prototype.slice.call(arguments));
    return fn.apply(this, args);
  }
  return parted;
}

function fn1(a, b, c) {
  return a + b + c;
}

var partialFn1 = partial(fn1, 1);

partialFn1(2, 3);
```

### 函数柯里化
Currying(柯里化) 是把一个接受 n 个参数的函数转换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。也就是说每个函数都接受1个参数。即将一个 n 元函数转换为 n 个一元函数。

代码：

```js
function curry(fn) {
  var argsLength = fn.length;
  var args = [];
  var curried = function() {
    args = args.concat(Array.prototype.slice.call(arguments));
    argsLength--;
    var result = argsLength > 0 ? curried : fn.apply(this, args);
    return result;
  }
  return curried;
}

function fn1(a, b, c) {
  return a + b + c;
}

var curryFn1 = curry(fn1);

curryFn1(1)(2)(3);
```

混合柯里化与偏函数应用的实现：

```js
/**
 * 柯里化和偏函数应用
 * 核心是使用闭包保存每次调用传入的参数，
 * 每次调用时将参数的个数与原函数的长度比较，
 * 小于则返回parted继续收集余下参数，
 * 等于则执行原函数，
 * 没有大于的情况
 * 
 * @param {any} fn 
 * @returns 
 */
function partial(fn) {
  var slice = Array.prototype.slice;
  var args = slice.call(arguments, 1) || [];
  var fnLength = fn.length;
  var parted = function () {
    args = args.concat(slice.call(arguments));
    var result = fnLength > args.length ? parted : fn.apply(this, args);
    return result;
  }
  return parted;
}

function fn1(a, b, c) {
  console.log(a, b, c);
  return a + b + c;
}

var partialFn1 = partial(fn1, 1);

partialFn1(2)(3);
```

混合柯里化与偏函数应用的es6实现：

```js
/**
 * 柯里化和偏函数应用
 * 核心是使用闭包保存每次调用传入的参数，
 * 每次调用时将参数的个数与原函数的长度比较，
 * 小于则返回parted继续收集余下参数，
 * 等于则执行原函数，
 * 没有大于的情况
 * 
 * @param {any} fn 
 * @returns 
 */
var partial =(fn, ...rest) => 
  parted = (...args) =>
    fn.length > args.length + rest.length
      ? (arg) => parted(...args, arg)
      : fn(...rest, ...args);
      
function fn1(a, b, c) {
  console.log(a, b, c);
  return a + b + c;
}

var partialFn1 = partial(fn1, 1);

partialFn1(2)(3);
```


### 尾调用
尾调用（Tail Call）是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指某个函数的最后一步是调用另一个函数。尾调用不一定出现在函数尾部，只要是最后一步操作即可。
### 尾调用优化
即只保留内层函数的调用帧。如果所有函数都是尾调用，那么完全可以做到每次执行时，调用帧只有一项，这将大大节省内存。这就是“尾调用优化”的意义。

ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。

这是因为在正常模式下，函数内部有两个变量，可以跟踪函数的调用栈。

    func.arguments：返回调用时函数的参数。
    func.caller：返回调用当前函数的那个函数。
尾调用优化发生时，函数的调用栈会改写，因此上面两个变量就会失真。严格模式禁用这两个变量，所以尾调用模式仅在严格模式下生效。
### 尾递归
函数调用自身，称为递归。如果尾调用自身，就称为尾递归。

递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow）。但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。


###Cookie
Cookie的安全策略和Javascript的同源策略并不完全相同，Cookie只受Domian、Path、Expires、HttpOnly、Secure等属性的影响。
Cookie的传输并不受端口限制。当Cookie属性secure设置为true时，Cookie只能在https中传输，http不会传输。
