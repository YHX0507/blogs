---
title: Cookie和Session
comments: true
date: 2018-10-11 17:23:07
tags: 
- cookie
- session
---

# Cookie

## 什么是 Cookie？

- Cookie 是一些数据, 存储于你电脑上的文本文件中。

- 当 web 服务器向浏览器发送 web 页面时，在连接关闭后，服务端不会记录用户的信息。


## Cookie 的作用就是用于解决 "如何记录客户端的用户信息":

- 当用户访问 web 页面时，他的名字可以记录在 cookie 中。

- 在用户下一次访问该页面时，可以在 cookie 中读取用户访问记录。

## 设置Cookie

### 原生方法

- 设置单个cookie

```js
res.setHeader('set-cookie','属性名=属性值');
```

- 设置多个cookie

```js
res.setHeader('set-cookie',['属性名=属性值','属性名=属性值']);
```

### Express方法

```js
res.cookie('属性名','属性值',{属性});
//	这里属性可以设置cookie有限期
{
    expires: new Date(new Date() + 1000*5)
}
```

## 服务器获取Cookie

```js
res.headers.cookie
// 注意这里查询到的为'a=1;b=2'字符串
```

### 原生方法解析

- 通过replace方法替换`；`为`&`后利用`queryString`包解析为对象

- 通过`cookie-parser`包解析

```js
const cookieParser = require('cookie-parser');
app.use(cookieParser())
// cookie会保存在req对象中，req.cookies会解析为cookie对象
```

## 删除Cookie

```js
res.clearcookie('属性名');
```

# Session

## Express框架处理session

```js
//	安装
npm install express-session
//  引入包
const session = require('express-session')
//	使用包
app.use(session(conf))
```

## 设置Session

```js
req.session.属性名 = 属性值
```

## 获取Session

```js
req.session.属性名
```

## 删除Session

```js
req.session.destroy()
```