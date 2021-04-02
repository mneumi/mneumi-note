## Node.js 的模块化

### 支持性总结

Node.js完美支持`CommonJS`

Node.js现今（2020年）实验性支持 `ES Module`

Node.js不支持 `AMD`，`CMD`，`UMD`

### 开启ES Module支持

#### 方式1

在`package.json`中配置 `type: module`

```json
{
    type: 'module'
}
```

#### 方式2

文件以 .mjs 结尾，表示使用的是ES Module

```js
// server.mjs
import Koa from 'koa';

const app = new Koa();

app.use(async (ctx) => {
    ctx.body = 'This is ES Module Support With Ext .mjs';
});

app.listen(3000);
```

#### 使用

```shell
node server.mjs
```



## CommonJS 和 ES Module 交互

### CommonJS语法使用ES Module模块

* 通常情况下，`CommonJS`不能加载`ES Module`，因为`ES Module`是需要进行编译期静态分析的，而 `CommonJS` 是运行时调用，即无法提前进行静态分析，所以也就无法在`CommonJS`中使用`ES Module`模块

* 但是这并非绝对的，因为某些平台在实现的时候可以在运行之前对全部代码文件进行静态分析，所以也是有可能会支持的

* 在 Node.js 中是不支持的

### ES Module语法使用CommonJS模块

* 这是可以实现的，因为`ES Module` 在静态分析时就可以解析相应的`CommonJS` 模块
* ES Module在加载CommonJS时，会将其module.exports导出的内容作为default导出方式来使用
* 在 Webpack 和 Node.js 中都支持