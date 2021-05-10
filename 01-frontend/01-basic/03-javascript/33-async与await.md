## async与await

### 介绍

async/await是一种异步编程方案

### async的使用

在函数定义前使用，用于告诉 JS 解释器这是一个含有异步操作的函数

```js
async function show() {}
```

 被`async`修饰的函数，其返回值均为Promise对象

* 返回非Promise对象：返回值的Promise对象状态为`resolved`，结果为返回值本身
* 出现异常/抛出异常：返回值的Promise对象状态为`rejected`，结果为异常变量
* 返回Promise对象：返回值的Promise对象状态和结果由该Promise对象决定

箭头函数也可以用 async

```js
async () => {}
```

### await的使用

await的作用是：求值 + 等待

await必须写在async函数中，但async函数中可以没有await

await右侧的表达式一般为Promise对象，但也可以是其他的值 

* 如果表达式是Promise对象，await返回的是promise成功的值 

* 如果表达式是其他值，直接将此值作为await的返回值

### 错误处理

#### 方法1：try...catch

如果await的promise失败了，就会抛出异常，需要通过try...catch来捕获处理

即 `try await catch`能够捕获到状态为`rejected`的错误

```js
async function show() {          
  expression1;
  expression2;

  try {
    let data = await expressionA;         
  }  catch (e) {
    onError(e);
  } finally {
  	onFially();
  }

  expression3;
}
```

#### 方法2：使用catch函数

因为asyn函数返回Promise对象，所以可以直接catch

缺点：错误处理和函数定义分离了

```js
async foo() {}
foo().catch(reason=>{})
```

### 本质

async是Promise的语法糖，返回值会被包装为一个Promise

### 注意

async可以和类结合使用，前提：类要有then方法

```js
class User {
    then(resolve, reject) {
        resolve(111);
    }
}

async function get() {
    const value = await new User();
    console.log(value);
}

get();
```

