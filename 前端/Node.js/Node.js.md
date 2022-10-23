# Node.js

## node.js的简介

> 简单的说 Node.js 就是运行在服务端的 JavaScript。
>
> Node.js 是一个基于 Chrome JavaScript 运行时建立的一个平台。
>
> Node.js 是一个事件驱动 I/O 服务端 JavaScript 环境，基于 Google 的 V8 引擎，V8 引擎执行 Javascript 的速度非常快，性能非常好。

## node.js的第一个hello world服务器

```js
const http = require('http')
http.createServer(function (request,response) {
    // 发送http头部
    // http 状态码
    // 内容类型 text/plain
    response.writeHead('200',{'Content-Type':'text/plain'})
    //发送响应的数据
    response.end('Hello world')
}).listen(9002)

//始终打印下列信息
console.log("server Run at Http://127.0.0.1")

```

