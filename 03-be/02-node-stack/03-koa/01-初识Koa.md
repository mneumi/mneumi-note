## 初识Koa

### Koa是什么

Koa 是基于 Node.js 的下一代 Web 框架

### Koa特点

* 由 Express 原班人马打造，更小、更富表现力、更健壮

* Koa的核心代码只有1600+行，是一个更加轻量级的框架，没有捆绑任何中间件，可以根据需要安装和使用中间件
* Koa旨在为Web应用程序和API提供更小、更丰富和更强大的能力
* 使用 async/await，抛弃回调函数，避免异步的过度嵌套，相对于express具有更强的异步处理能力
* 增强错误处理：try...catch



## 对比express

express是完整和强大的，其中内置了非常多好用的功能

koa是简洁和自由的，它只包含最核心的功能，并不会对使用其他中间件进行任何的限制

express和koa框架他们的核心其实都是中间件



## Koa初体验

### 初始化项目

```shell
npm init
```

### 安装Koa2

```shell
npm install koa --save
# OR
yarn add koa
```

### Hello World

```js
// 引入 Koa 库
const Koa = require("koa");

// 创建 Koa 应用
const app = new Koa();

// 注册中间件
app.use((ctx, next) => {
	ctx.body = "Hello World";
    next();
});

// 监听端口
app.listen(3000, () => {
    console.log("server start")
});
```

### 自动重启

#### 安装 nodemon

```shell
npm install nodemon --save-dev
# OR
yarn add nodemon -D
```

#### 使用 nodemon

```
nodemon index.js
```

