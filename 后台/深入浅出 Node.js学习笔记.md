# 深入浅出 Node.js学习笔记


## 模块机制

### CommonJS 规范

- module

	- 代表模块自身，一个文件就是一个模块

- exports

	- module 的属性，用于挂载需要导出的对象或方法 deng

- require

	- 

		- 接受一个模块的标识用以引入一个模块的 API到当前上下文中

### Node 的模块实现

- 引入模块的三个步骤

	- 路径分析

	- 文件定位

	- 编译执行

- 模块编译

	- 在编译过程中对获取的 js 进行头尾包装引入 module,export,require,__filename,__dirname

### 模块组成

- 核心模块

	- 细分

		- C/C++(內建模块)

		- JS

			- 编译成 C/C++（即将原 js 代码以字符串形式存储于 C/C++中，并非转换成 C/C++）后载入内存

			- 作用

				- 內建模块的封装层和桥接层

				- 纯粹的功能模块无需和底层打交道

	- 二进制，启动时直接载入内存；引入时只需步骤三

- 文件模块

	- 运行时动态加载

	- 细分

		- JS模块

		- C/C++扩展模块

	- 调用方向主要为 JS 掉 C/C++

- 其他

	- C/C++会被预先编译成.node 文件

### NPM

## 异步I/O

### 异步I/O

- 事件循环

	- 典型的生产者消费者模型

	- 创建：Windows下基于 IOCP，*nix 下基于多线程

- 观察者

	- 每个事件循环中有一个或多个观察者询问判断是否有要处理的事情

	- 事件被传递到观察者，事件循环从观察者处取出事件并处理

- 请求对象

	- JS 发起调用到系统完成 I/O 的中间产物

- 执行回调

### 非I/O的异步 API

- 定时器

	- setTimeout

	- setInterval

- process.nextTick()

	- 回调保存在数组中

	- 属于①

- setImmediate

	- 回调保存在链表里

	- 属于②

- ps：观察者

	- 类别

		- ①idle 观察者

		- ②I/O 观察者

		- ③check 观察者

	- 优先级

		- ①>②>③

## 异步编程

### 函数式编程

- 高阶函数

	- 把函数当做参数或者将函数作为返回值的函数

- 偏函数

	- 创建一个调用另外一个部分参数或者变量已经预置的函数的函数

### 异步编程的优点与难点

- 优点

	- 基于事件驱动的非阻塞 I/O

- 难点

	- 异常处理

		- try/catch 只能捕获当前循环内的异常，对于回调无能为力

		- 解决办法：将异常作为回调函数的第一个参数传回

	- 函数嵌套过深

	- 阻塞代码

	- 多线程编程

	- 异步转同步

### 解决方案

- 事件发布/订阅模式

- Promise/Deffered

- 流程控制库

- 补充：async/await

### 异步并发控制

- bagpipe

- async

## 内存控制

### 内存

- 默认64位1.4G，32位0.7G

- 打开限制

	- 老生代：—max-old-space-size

	- 新生代—max-new-space-size

- 所有 JS 对象都是通过堆进行分配的

- 新生代

	- 存活时间短

- 老生代

	- 存活时间长或者常驻内存

### 垃圾回收

- 相关算法

	- Scavenge算法

		- 主要用于新生代

		- 将存活的对象在 From 和 To之间进行来回复制，以及时清除死对象所占的空间

		- 经历过此算法或者To 空间的n 内存占比超过限制(25%)，该对象就会晋升到老生代

	- Mark-Sweep(标记清除)

		- 主要用于老生代

		- 分为标记和清除两个阶段，标记阶段将活着的对象标记，在清除阶段清除没有被标记的对象

		- 缺点是容易形成n 内存碎片

	- Mark-Compact(标记整理)

		- 整理内存碎片以形成连续的空间

	- Incremental Marking(增量标记)

		- 每次只完成一小部分与应用逻辑交替执行

		- 减少垃圾回收导致的最大停顿时间

	- V8主要使用 Mark-Sweep 算法，在空间不足以对新生代晋升来的对象进行分配时才执行 Mark-Compact

### 内存指标

- process.memoryUsage()查看进程的内存占用情况

- os.totalmem()和 os.freemem()查看操作系统的内存占用情况

### 内存泄漏

- 原因

	- 队列消费不及时

	- 作用域未及时释放

	- 缓存

- 办法

	- 慎用内存当缓存，采用进程外缓存 eg.redis,memcached

	- 监控队列状态，一旦超出限制，停止生产

- 排查工具

	- v8-profiler

	- node-heapdump

	- node-memwatch

	- node-mtrace

	- dtrace

### 大内存应用

- 采用 stream模块处理大文件

### C++申请，JS 分配

## Buffer

### 类 Array 的对象，主要用于操作字节

### 内存分配由C++层面申请，不占用 V8堆内存

### 可与字符串之间互相转换

- new Buffer(str[, ecoding])

- buf.toString([ecoding][, start][, end])

### Buffer的拼接

- 须注意宽字节编码，使用+拼接会隐式调用toString 操作，可能导致乱码

- 避免乱码

	- setEncoding()

	- 采用push，然后 concat 成一个大 Buffer 后转换

- 在 node 中，非 node 支持的编码方式可以采用第三方库如 iconv 或 iconv-lite完成转换。注意：一定要再收到的数据还是二进制的时候（比如 data 事件中）转换以防止隐式转换

## 网络编程

### TCP

- 面向连接，所有会话基于连接完成，一个套接字只能与一个进程通信

- net 模块创建，Stream 实例

- 服务器事件

	- listening

	- error

	- connection

	- close

- 连接事件

	- data

	- end

	- connect

	- drain：任一端调用 write() 发送数据时触发

	- error

	- close

	- timeout

### UDP

- 一个 UDP 套接字可以与多个进程通信

- dgram 模块创建，EventEmitter实例

- 客户端与服务器端相同

- 事件

	- message

	- listening

	- error

	- close

### HTTP另说

### WebSocket

- ws 的客户端基于事件的编程模型与 node 相差无几

- 实现了客户端与服务器的长连接

- 使用 HTTP 协议完成握手，然后升级为 WS 协议实现数据传输

- 客户端发送的数据须做掩码处理，服务器端则不用，如果数据异常则都会关闭连接

### 网络服务与安全

- HTTPS

## 构建 web 应用（一个简单的 express 实现）

### 基础功能

### 数据上传

### 路由解析

### 中间件

### 页面渲染

## 玩转进程

### child_process

- 创建子进程

	- spawn

	- fork

	- exec

	- execFile

- 进程间通信

	- send

	- message

	- 使用 fork 或其他命令创建子进程后，父子直接会创建 IPC通道，通过 IPC，父子直接才能通过 send,messgae 通信

	- 父进程在创建子进程之前会先创建 IPC 通道并监听，然后才会创建子进程，并通过环境变量（NODE_CHANNEL_FD）告诉子进程该通道的文件描述符，然后子进程根据该文描去连接该 IPC，从而完成父子通信

- 句柄传递

	- 每个连接都会消耗一个文件描述符

	- send 的第二个可选参数就是句柄

	- 句柄可以标识一个服务器端的 socket 对象等

	- 发送句柄可以使得主进程收到 socket 请求后将这个socket直接发给子进程，而不是从新与 子进程建立新的 socket连接，因此可以节省文描

	- 发送句柄的原理：子进程根据句柄的文描重新创建一个服务对象，然后监听到同一个文描上

- 集群稳定之路

	- 进程事件

	- 自动重启

		- 收到异常信号时，在子进程退出前即发出自杀信号以创建一个新的进程顶替

	- 负载均衡

		- 轮询

	- 状态共享

		- 通知进程

### cluster

- child_process 和 net 的组合应用

- 一个主进程只能管理一组工作进程

- 事件

	- fork

	- online

	- listening

	- disconnect

	- exit

	- setup

## 测试

### 单元测试

- 编写可测试代码

	- 单一职责

	- 接口抽象

	- 层次分离

- 测试风格

	- TDD

	- BDD

- mocha

	- TDD

		- suite test

	- BDD

		- describe it

### 性能测试

## 产品化

### 项目工程化

### 部署流程

### 日志

### 监控报警

### 稳定性

- 多机器

- 多机房

- 容灾备份

### 异构共存

