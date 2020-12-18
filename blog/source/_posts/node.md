---
title: node
comments: true
date: 2018-09-14 20:43:25
tags: 
- Node
- Express
---



## fs模块

文件系统（fs 模块）模块中的方法均有异步和同步版本

异步的方法函数最后一个参数为回调函数，回调函数的第一个参数包含了错误信息(error)。

建议大家使用异步方法，比起同步，异步方法性能更高，速度更快，而且没有阻塞。

1. 读文件

   ```js
   // 引入包
   const fs = require("fs");
   
   // file - 文件名或文件描述符。
   // options - 默认编码为 utf8
   // callback - 回调函数，回调函数包含错误信息参数和读取数据(err, data)。
   
   // 异步读取
   fs.readFile('file.txt', [options], function (err, data) {
      if (err) {
          return console.error(err);
      }
      console.log("异步读取: " + data.toString());
   });
   
   // 同步读取
   var data = fs.readFileSync('input.txt');
   console.log("同步读取: " + data.toString());
   ```

2. 写文件

   ```js
   const fs = require("fs");
   
   // 异步写入
   // file - 文件名或文件描述符。
   // data - 要写入文件的数据，可以是 String(字符串) 或 Buffer(缓冲) 对象。
   // options - 包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ，flag 为 'w'
   // callback - 回调函数，回调函数只包含错误信息参数(err)，在写入失败时返回
   
   fs.writeFile('file.txt', [options],  function(err) {
      if (err) {
          return console.error(err);
      }
      console.log("数据写入成功！");
   });
   
   // 同步读取
   var result = fs.writeFileSync('input.txt');
   console.log("同步写入: " + result);
   ```

## HTTP模块

http 模块主要用于搭建 HTTP 服务端和客户端，使用 HTTP 服务器或客户端功能必须调用 http 模块

1. 使用http模块启动web服务

   ```js
   // 引入包
   var http = require('http');
   // 创建服务器
   var server = http.createServer( function (request, response) {
         //  发送响应数据
         response.end();
      });
   // 监听8080端口，控制台会输出以下信息
   server.listen(8080, () => {
       console.log('Server running at http://127.0.0.1:8080/');
   })
   ```

2. 处理静态资源

   ```js
   var http = require('http');
   var fs = require('fs');
   var path = require('path');
   var server = http.createServer( function (request, response) {
         //  利用path包可以拼接请求资源绝对路径、解析请求资源后缀
         const extname = path.extname(pathname)
         const filePath = path.join(__dirname, 'public', pathname)
         fs.readFile(filePath, 'utf8', (err, data) => {
         if (err) {
           response.statusCode = 404
           response.end('Not Found')
         }
         //  根据不同得文件类型设置响应头
         if (extname === '.html') {
           response.setHeader('Content-type', 'text/html;charset=utf8;')
         } else if (extname === '.js') {
           response.setHeader('Content-type', 'text/javascript;charset=utf8;')
         } else if (extname === '.css') {
           response.setHeader('Content-type', 'text/css;charset=utf8;')
         }
         //  发送响应数据
         response.end();
       })
      });
   server.listen(8080, () => {
       console.log('Server running at http://127.0.0.1:8080/');
   })
   ```

3. 处理get接口

   ```js
   var http = require('http');
   var url = require('url');
   var server = http.createServer( function (request, response) {
         if(request.method === 'GET') {
             //  利用url包可解析出请求的请求路径和query参数，为true后可直接把查询参数解析为对象
             const { pathname, query } = url.parse(request.url, true)
             consoele.log(pathname, query)
             //  发送响应数据
         	  response.end();
         }
      });
   server.listen(8080, () => {
       console.log('Server running at http://127.0.0.1:8080/');
   })
   ```

4. 处理post接口

   ```js
   var http = require('http');
   var qs = require('querystring');
   var server = http.createServer( function (request, response) {
         if(request.method === 'POST') {
         	let rsData = ''
       	request.on('data',rs => {
         		rsData += rs
       	})
       	request.on('end', () => {
               //  利用querystring包的parse方法可以解析出请求体传来的参数
         		let { name, content } = qs.parse(rsData)
         		console.log(name, content)
         		//  发送响应数据
         	    response.end();
       	})
         }
      });
   server.listen(8080, () => {
       console.log('Server running at http://127.0.0.1:8080/');
   })
   ```

## express框架

Express 是一个保持最小规模的灵活的 Node.js Web 应用程序开发框架，为 Web 和移动应用程序提供一组强大的功能。使用您所选择的各种 HTTP 实用工具和中间件，快速方便地创建强大的 API。

Express 框架核心特性：

- 可以设置中间件来响应 HTTP 请求。
- 定义了路由表用于执行不同的 HTTP 请求动作。
- 可以通过向模板传递参数来动态渲染 HTML 页面。

1. 初始化目录

   ```js
   npm init --yes
   ```

2. 安装

   ```js
   npm install express
   ```

3. 使用express创建web服务

   ```js
   // 引如包
   const express = require('express')
   // 创建web服务
   const app = express()
   // 路由一个api
   app.get('/', (req, res) => res.send('Hello World!'))
   // 监听端口
   app.listen(port, [callback])
   ```

4. 处理静态资源

   ```js
   // 第一个可选path 第二个静态资源路径
   app.use([path], express.static('path'))
   ```

5. get接口处理

   ```js
   // 无url参数
   app.get('/get-no-params', (req, res) => {
     console.log(req.query)
     res.send({
       code: 200,
       message: '请求成功'
     })
   })
   // 有url参数
   app.get('/get-params', (req, res) => {
     console.log(req.query)
     res.send({
       code: 200,
       message: '请求成功',
       data: {
         ...req.query,
       }
     })
   })
   ```

6. post接口处理

   ```js
   const express = require('express')
   const bodyParser = require('body-parser')
   // Multer 会添加一个body对象以及file或files对象到express的request 对象中。 
   // body对象包含表单的文本域信息，file或files对象包含对象表单上传的文件信息。
   const multer = require('multer')
   
   const app = express()
   
   app.use(express.static('public'))
   // 处理post类型传参：键值对、表单、json三种数据
   app.use(bodyParser.urlencoded({ extended: false }))
   const upload = multer({ dest: 'upload/'})
   app.use(bodyParser.json())
   
   //  处理普通键值对接口
   app.post('/post-params', (req, res) => {
     console.log(req.body)
     res.send({
       code: 200,
       message: '请求成功',
       data: {
         ...req.body,
       }
     })
   })
   //  处理表单数据文件接口
   //  cover为表单name值
   app.post('/post-file', upload.single('cover'), (req, res) => {
     console.log(req.file)
     console.log(req.body)
     res.send({
       code: 200,
       message: '上传成功',
       data: {
         ...req.body,
         url: req.file.path
       }
     })
   })
   //  处理json数据接口
   app.post('/post-json', (req, res) => {
     console.log(req.body)
     res.send({
       code: 200,
       message: '请求成功',
       data: {
         ...req.body
       }
     })
   })
   ```

## Ps  $.ajax发送post请求小结

contentType: 键值对、表单文件、json三种数据

1. contentType: 'application/x-www-form-urlencoded'
2. contentType: 'multipart/formdata'
3. contentType: 'application/json'