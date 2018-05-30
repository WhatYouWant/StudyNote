#Node.js学习笔记
Node.js就是运行在服务端的JavaScript。

##Node.js应用由哪几部分组成
1.引入required模块：可以使用require指令来载入node.js模块。
2.创建服务器：服务器可以监听客户端的请求，类似于apache等http服务器。
3.接收请求与响应请求：服务器很容易创建，客户端可以使用浏览器或终端发送http请求，服务器接收请求后返回响应数据。
##NPM使用介绍
NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见使用场景如下：
1.允许用户从NPM服务器下载别人编写的第三方包到本地使用。
2.允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
3.允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

#使用npm命令安装模块
$npm install <Module Name>
例如使用Node.js web框架模块 express:
$npm install express

#Node.js REPL(交互式解释器)
Node.js REPL(Read Eval Print Loop:交互式解释器) 表示一个电脑的环境，类似 Window 系统的终端或 Unix/Linux shell，我们可以在终端中输入命令，并接收系统的响应。
Node自带了交互式解释器，可以执行以下任务：
1.读取 - 读取用户输入，解析输入了JavaScript数据结构并存储在内存中。
2.执行 - 执行输入的数据结构。
3.打印 - 输出结果。
4.循环 - 循环操作以上步骤直到用户两次按下ctrl+c按钮退出。
启动node终端
$ node

#Node.js回调函数
Node.js 异步编程的直接体现就是回调。
异步编程依托于回调来实现，但不能说使用了回调后程序就异步化了。
回调函数在完成任务后就会被调用，Node 使用了大量的回调函数，Node 所有 API 都支持回调函数。
回调函数一般作为参数的最后一个参数出现：

```
function foo1(name, age, callback) { }
function foo2(value, callback1, callback2) { }
```

#Node.js事件循环
Node.js 是单进程单线程应用程序，但是因为 V8 引擎提供的异步执行回调接口，通过这些接口可以处理大量的并发，所以性能非常高。
Node.js 几乎每一个 API 都是支持回调函数的。
Node.js 基本上所有的事件机制都是用设计模式中观察者模式实现。
Node.js 单线程类似进入一个while(true)的事件循环，直到没有事件观察者退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数.

#事件驱动程序
Node.js使用事件驱动模型，当web server接收到请求，就把它关闭然后进行处理，然后去服务下一个web请求。
当这个请求完成，它被放回处理队列，当到达队列开头，这个结果被返回给用户。
这个模型非常高效可扩展性非常强，因为webserver一直接受请求而不等待任何读取操作。（这也被称之为非阻塞IO或者事件驱动IO）
在事件驱动模型中，会生成一个主循环来监听事件，当检测到事件时触发回调函数。
![](https://www.runoob.com/wp-content/uploads/2015/09/event_loop.jpg)

有点类似于观察者模式，事件相当于一个主题Subject，而所有注册到这个事件上的处理函数相当于观察者Observer。


