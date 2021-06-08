## Web框架

### 什么是Web框架

Web框架是指将Web服务器开发中常用的基础功能都实现并且提供给上层调用的框架

### Web框架作用

原生http在进行很多处理时，会较为复杂，比如有URL判断、Method判断、参数处理、逻辑代码处理等，都需要我们自己来处理和封装，并且所有的内容都放在一起，会非常的混乱

所有需要有框架将常用的基础功能都实现，并提供好用的API给开发者使用

### 主流web框架

目前在Node中比较流行的Web服务器框架是express、koa、nest

express早于koa出现，并且在Node社区中迅速流行起来，可以基于express快速、方便的开发自己的Web服务器，并且可以通过一些实用工具和中间件来扩展自己功能



## 初识Express

### Express是什么

Express 是基于 Node.js 平台，快速、开放、极简的 Web 开发框架

### Express核心

Express核心是**中间件**

Express内部集成了常用的中间件，如：静态资源服务，路由，参数解析等

### Express安装

方式1：通过express提供的脚手架，直接创建一个应用的骨架

```shell
# 安装脚手架
npm install -g express-generator

# 创建项目
express express-demo

# 安装依赖
npm install

# 启动项目
node bin/www
```

方式2：从零搭建自己的express应用结构

```js
const express = require("express");

const app = express();

app.get("/home", (req, res, next) => {
    res.end("Hello Home");
    next();
})

app.post("/login", (req, res, next) => {
    res.end("Hello Login")
    next();
})

app.listen(8000, () => {
    console.log("服务器启动成功")
})
```



