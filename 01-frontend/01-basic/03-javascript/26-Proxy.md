## Proxy类型介绍

### Proxy概念

对象或函数代理，相当于套了一层壳，使用和访问所代理的对象或函数时，都需要经过代理层

### Proxy分类

分为对象代理，函数代理和数组代理三类

### Proxy作用

通过对整个对象或函数进行代理，提供提供拦截 JavaScript 操作的方法



## 代理对象和代理数组

### 语法

```js
new Proxy(需要进行代理的对象/数组, 捕获器对象)
```

### 捕获器对象属性

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

### 注意

set需要返回boolean，否则设置肯定不会成功，因为不写return true时，默认返回undefined，即相当于return false

### 示例

```js
let obj = { name: "姓名" };

const proxy = new Proxy(obj, {
    get(target, property) {
        console.log(property);
    },
    set(target, property, value) {
    	console.log(value);
        return true
	},
    has(target, property) {
        return true;
    }
})
```

```js
let obj = { // 供应商，原始数据
  time: '2017-03-11',
  name: 'net',
  _r: 123
};

let monitor = new Proxy(obj, {
  // 代理对象属性的读取
  get(target, key) {
    return target[key].replace('2017', '2018');
  }

  // 代理对象属性的设置
  set(target, key, value) {
    if (key === 'name') {
      return target[key] = value;
    } else {
      return target[key];
    }
  }

  // 拦截 key in object 操作
  has(target, key) {
    if (key === 'name') {
      return true;
    } else {
      return false;
    }
  }

  // 拦截 delete
  deleteProperty(target, key) {
    if (key.indexof('_') > -1) {
      delete target[key];
        return true;
      } else {
        return false;
    }
  }

  // 拦截Object.keys, Object.getOwnPropertySymbols, Object.getOwnPropertyNames
  ownKeys(target) {
    return Object.keys(target).filter(item => item != 'time');
  }
});

console.log(monitor.time);
monitor.time = '2019';
monitor.name = 'test';
console.log(monitor.time);
delete monitor.time;
console.log(monitor);
delete monitor._r;
console.log(monitor);
console.log(Object.keys(monitor));
```



## 代理函数

### 语法

```js
new Proxy(需要代理的函数，捕获器对象)
```

### 捕获器对象属性

| 属性                       | 返回值 | 说明                 |
| -------------------------- | ------ | -------------------- |
| apply(func, thisArg, args) | any    | 函数调用操作的捕捉器 |

### 示例

```js
function factorial(num) {
  return num === 1 ? 1 : num * factorial(num-1);
}

let proxy = new Proxy(factorial, {
    apply(func, thisArg, args) {
        console.time("run");

        let a = func.apply(thisArg, args);
        console.log(a);

        console.timeEnd("run");
    }
})

proxy.apply({}, [5]);
```
