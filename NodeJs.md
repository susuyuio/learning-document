# 深入浅出Node.js

_`高并发、高性能`_

[[CNode开源技术社区]](https://cnodejs.org/)
[[Node官网]](https://nodejs.org/en/)

---

## 简介

- Web服务器的几个要点：**事件驱动**、 **非阻塞I/O**、 **单线程**
- Node的结构与Chrome十分相似：给予事件驱动的 `异步` 架构（服务界面上的交互/服务I/Os）
- Node 特点：
    1. 异步I/O
    2. 事件与回调函数
    3. 单线程
        - 优势
            - 不用处处在一状态的同步问题
            - 没有死锁的存在
            - 没有线程上下文交换所带来的性能开销
        - 弱点
            - 无法利用多核CPU
            - 错误会引起整个应用退出，应用的健壮性值得考验
            - 大量计算占用CPU导致无法继续调用异步I/O
        - Node采用与Web Workers相同的思路来解决单线程中大计算量的问题：`child_process`
            - 通过将计算分发到各个子进程中，分解大量计算，再通过进程之间的事件消息来传递结果，很好地保持应用模型的简单和低依赖
            - 通过Master-Worker的管理方式，也可以很好地管理各个工作进程，以达到更高的健壮性
    4. 跨平台
        - 主要得益于Node再架构层面的改动，它再操作系统与Node上层模块系统之间构建了一层平台层架构，`libuv`。目前libuv已经成为许多系统实现跨平台的基础组件
- 应用场景
    1. I/O密集型
        - Node面向网络且擅长并行I/O，能够有效地组织起更多的硬件资源，从而提供更好的服务
        - I/O密集的优势主要在于Node利用事件循环的处理能力，而不是启动每一个线程为每一个请求服务，资源占用极少

## 模块机制

### `CommonJS`模块规范

- CommonJS对模块的定义主要分为：模块引用、模块定义、模块标识
    1. 模块引用

        ``` js
        var math = require('math');
        ```

    2. 模块定义
        - 在Node中，一个文件就是一个模块，将方法挂载在exports对象上作为属性即可定义导出的方式

        ``` js
        // math.js
        exports.add = function() {...};

        // program.js
        var math = require('math');
        exports.increament = function(val) {
            return math.add(val, 1);
        };
        ```

    3. 模块标识
        - require()参数必须是符合小驼峰命名字符串，或者绝对路径、相对路径

### Node模块实现

- Node中引入模块3步骤：
    1. 路径分析
    2. 文件定位
    3. 编译执行
- Node中模块分2类：
    1. 核心模块（Node提供）
    2. 文件模块（用户编写）
- 优先从缓存加载
    + 浏览器仅仅缓存文件，Node缓存的是编译和执行之后的对象
- 核心模块 > 路径式的文件模块 > 自定义模块
    + 路径分析、文件定位、编译执行
    + 当前文件的路径越深，模块查找耗时会越多，这是自定义模块的加载速度最慢的原因
- 核心模块
    + 分为C/C++编写（src目录） 和 JS编写（lib目录） 两部分
- 模块调用栈
    + JS核心模块指责：
        1. 作为C/C++内建模块的封装层和桥接层供文件模块调用
        2. 纯粹的功能模块，不需要跟底层打交道，但十分重要

#### 包与`NPM`

- 在模块之外，包和NPM则是将模块联系起来的一种机制
- CommonJS包规范：
    1. 包结构（用于组织包中的各种文件）
    2. 包描述文件（用于描述包的相关信息，供外部读取分析）
- 常用功能
    + 对于Node而言，NPM帮助完成了第三方模块的发布、安装和依赖
    + 镜像源安装 
    
        `npm install undersore --registry=http://registry.url`
    + 设置默认源
    
        `npm config set registry http://registry.url`
    + 注册包创建账号
    `npm adduser`
    + 上传包
    `npm publish \<folder\>`
    + 管理包权限

        `npm owner ls <package-name>`

        `npm owner add <user> <package-name>`
        
        `npm owner rm <package-name>`
    + 分析包
        `npm ls`

#### AMD、CMD规范

- AMD规范是CommonJS模块规范的一个延伸
- AMD需要在声明模块的时候指定所有的依赖，通过形参传递依赖到模块内容中
- CMD支持动态引入
``` js
// AMD
define(['dep1','dep2'], function(dep1, dep2){
    return function () {};
});
//CMD
define(function(require, exports, module){
    // The module code goes here.
});
```

## 异步I/O

- **_Node_** 既作为服务端处理客户端带来的大量并发请求，又作为客户端向网络中各个应用进去并发请求
- **_Nginx_** 采用单纯C编写，性能优异。但受限于同步方式的编程语言
- WHY？
    + 用户体验
    + 资源分配
        - 多线程：开销大，有效提升CPU利用率
        - 单线程：已于表达，性能不好
        - 添加硬件是一种提升服务质量的方式，但不是唯一方式
        - Node 利用单线程，远离多线程死锁，状态同步等问题；利用异步I/O，让单线程远离阻塞，更好地利用CPU
        - 为了弥补单线程无法利用多核CPU的缺点，Node提供了类似前端浏览器中Web Worker的子进程，该子进程可以通过工作进程高效地利用CPU和I/O
- 异步I/O实现现状
    + 从实际效果而言，异步和非阻塞都达到了并行I/O的目的。但是从计算机内核I/O而言，异步/同步和阻塞/非阻塞实际上是两回事
    + `轮询:` 应用程序需要重复调用I/O操作来确认是否完成
- Node 的异步I/O
    + 完成整个异步I/O环节的有：事件循环、观察者、请求对象
- 非I/O的异步API
    + setTimeOut()、setInterval()、setImmediate()、process.nextTick()
    + **process.nextTick()** 中的回调函数执行的优先级高于 _setImmediate()_ ，因为观察者的检查优先级**idle** > I/O > _check_
    + process.nextTick()在轮询中将**数组**中的回调函数全部执行完，setImmediate()执行**链表**中的一个回调函数  

### `keyword`

- web服务器
- 浏览器中除了 **V8** 作为JS引擎外，还有 **Webkit布局引擎**
- **Web Workers** 创建工作线程来进行计算，以解决JS大量计算阻塞UI渲染的问题。工作线程为了不阻塞主线程，通过消息传递的方式来传递运行结果，使得工作线程不能访问到主线程中的UI