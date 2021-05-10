## 代码入侵

直接在代码中写死 Mock 数据，或者请求本地的 JSON 文件

**优点**：无

**缺点：**

1. 和其他方案比 Mock 效果不好。
2. 与真实 Server 环境的切换非常麻烦，一切需要侵入代码切换环境的行为都是不好的。



## 请求拦截

代表：[Mock.js](http://mockjs.com/)

示例：

```js
Mock.mock(/\\/api\\/visitor\\/list/, 'get', {
  code: 2000,
  msg: 'ok',
  'data|10': [
    {
      'id|+1': 6,
      'name': '@csentence(5)',
      'tag': '@integer(6, 9)-@integer(10, 14)岁 @cword("零有", 1)基础',
      'lesson_image': "<https://images.pexels.com/3737094/pexels-photo-3737094.jpeg>",
      'lesson_package': 'L1基础指令课',
      'done': '@integer(10000, 99999)',
    }
  ]
})
```

**优点：**

1. 与前端代码分离
2. 可生成随机数据

**缺点：**

1. 数据都是动态生成的假数据，无法真实模拟增删改查的情况。
2. 只支持 ajax，不支持 fetch。

(想要了解 ajax 和 fetch 区别的同学来点[我](https://zhuanlan.zhihu.com/p/24594294))



## 接口管理工具

**代表：**[rap](https://github.com/thx/RAP), [swagger](https://swagger.io/),[moco](https://github.com/dreamhead/moco), [yapi](https://github.com/YMFE/yapi)

**优点：**

1. 配置功能强大，接口管理与 Mock 一体，后端修改接口 Mock 也跟着更改，可靠。

缺点：

1. 配置复杂，依赖后端，可能会出现后端不愿意出手，或者等配置完了，接口也开发出来了的情况。
2. 一般会作为大团队的基础建设而存在， 没有这个条件的话慎重考虑。



## 本地 node 服务器

**代表：**[json-server](https://github.com/typicode/json-server)

**优点：**

1. 配置简单，json-server 甚至可以 0 代码 30 秒启动一个 REST API Server。
2. 自定义程度高，一切尽在掌控中。
3. 增删改查真实模拟。

**缺点：**

1. 与接口管理工具相比，无法随着后端 API 的修改而自动修改。



## 在线 mock 平台

> https://jsonplaceholder.com/users