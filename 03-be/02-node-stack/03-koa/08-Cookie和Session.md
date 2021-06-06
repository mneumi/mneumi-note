## Cookie

### 设置Cookie

#### 函数签名

```js
ctx.cookies.set(name, value[, options]);
```

#### 配置项options

| 键        | 说明                                                     |
| --------- | -------------------------------------------------------- |
| maxAge    | cookie保存时间，毫秒数                                   |
| expires   | cookie过期日期                                           |
| path      | cookie路径，默认为 /                                     |
| domain    | cookie域名，默认为*，表示所有同域下均可访问              |
| secure    | 默认为false，如果为true，则在https下才能访问             |
| httpOnly  | 默认为true，用于设置是否只有服务器可访问（即js不能访问） |
| overwrite | 默认为false，表示是否覆盖同名的cookie                    |

### 获取Cookie

```js
ctx.cookies.get(keyName);
```

### 注意事项

Cookie中不能设置中文

解决方式：可以使用`base64`等对中文进行编码再存储



## Session

### 概述

koa原生不支持 Session，需要使用第三方中间件，如`koa-session`

### 安装

```shell
npm install koa-session --save
# OR
yarn add koa-session
```

### 使用

```js
const session = require("koa-session");

app.keys = ["some secret key"];

const CONFIG = {
    key: "koa:sess",  // cookie key (default is koa:sess),
    maxAge: 86400000, // cookie 的过期时间 maxAge is ms (default is 1 days)
    overwrite: true,  // 是否可以overwrite，默认为 true
    httpOnly: true,   // cookie是否只有服务器能够访问，默认为 true
    signed: true,     // 签名，默认为true
    rolling: false,   // 在每次请求时强行设置cookie，即能够重置cookie过期时间（默认为false）
    renew: false,     // (boolean) renew session when session is nearly expired
};

app.use(session(CONFIG, app));

// 设置值
ctx.session.username = "alice";

// 获取值
console.log(ctx.session.username);
```

