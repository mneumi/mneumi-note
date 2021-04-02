## Reflect介绍

Reflect 是一个内置的对象，它提供拦截 JavaScript 操作的方法（元编程）

无构造函数，即不能进行new操作，类似于Math对象



## Reflect使用

### 理论

Reflect的API与Proxy完全一样

区别就是Reflect的是静态方法

### API列表(13个)

| 属性                                         | 返回值                  | 说明                                                         |
| -------------------------------------------- | ----------------------- | ------------------------------------------------------------ |
| get(target, property)                        | any                     | 属性读取操作的捕捉器                                         |
| set(target, property, value)                 | boolean                 | 属性设置操作的捕捉器                                         |
| has(target, property)                        | boolean                 | in 操作符的捕捉器                                            |
| deleteProperty(target, property)             | boolean                 | delete 操作符的捕捉器                                        |
| construct(target, argArray, newTarget)       | object                  | new 操作符的捕捉器                                           |
| defineProperty(target, property, descriptor) | boolean                 | Object.defineProperty 的捕捉器                               |
| getOwnPropertyDescriptor(target, property)   | Descriptor \| undefiend | Object.getOwnPropertyDescriptor 的捕获器                     |
| getPrototypeOf(target)                       | object \| null          | Object.getPrototypeOf 的捕捉器                               |
| setPrototypeOf(target, value)                | boolean                 | Object.setPrototypeOf的捕捉器                                |
| ownKeys(target)                              | Array                   | Object.getOwnPropertyNames和Object.getOwnPropertySymbols 的捕获器 |
| isExtensible(target)                         | boolean                 | Object.isExtensible 的捕获器                                 |
| preventExtensions(target)                    | boolean                 | Object.preventExtensions 的捕获器                            |
| apply(target, thisArg, args)                 | any                     | 函数调用操作的捕捉器                                         |

### 示例

```js
let obj = { // 供应商，原始数据
  time: "2017-03-11",
  name: "net",
  _r: 123
};

console.log(Reflect.get(obj, "time"));
Reflect.set(obj,"name","test");
console.log(Reflect.get(obj, "name");
console.log(Reflect.has(obj, "_r");
```

