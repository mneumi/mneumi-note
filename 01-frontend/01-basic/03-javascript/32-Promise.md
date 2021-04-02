## Promise介绍

### Promise概念

#### 抽象表达

Promise是JavaScript中进行异步编程的解决方案

JavaScript中进行异步编程的解决方案有四种：纯回调，Promise，generator，async/await

#### 具体表达

从语法上来说：Promise是一个构造函数
从功能上来说：Promise对象用来封装一个异步操作并可以获取其结果

### Promise优点

| 使用纯回调                           | 使用Promise                                            |
| ------------------------------------ | ------------------------------------------------------ |
| 回调地狱，代码难以阅读，异常难以处理 | 链式调用，代码易于阅读，异常易于捕获和处理             |
| 回调函数在异步操作执行前必须先定义好 | 回调函数定义的时间更加灵活，可以在异步操作结束后再定义 |

### Promise状态

1. 无法确认进行状态：当Promise处于pending状态，无法得知发展到哪一个阶段（刚刚开始还是即将完成）

#### Promise有三种状态

| 状态      | 说明             |
| --------- | ---------------- |
| pending   | 异步操作执行中   |
| fulfilled | 异步操作执行成功 |
| rejected  | 异步操作执行失败 |

#### Promise状态改变

状态改变只有两种，且一个Promise只能改变一次，状态不可逆

* pending -> fulfilled

* pending -> rejected

### Promise结果

异步操作无论成功还是失败，Promise都会有一个结果

按照约定，成功的结果记为value，失败的结果记为reason



## Promise API

### 构造函数

参数：执行器函数

返回值：一个Promise对象

执行器函数的结构如下

```js
(resolve, reject) => { 
  // 执行异步操作
    
  // 注册异步操作的回调函数
  if (异步操作成功) {
    resolve(成功结果);
  } else if (异步操作失败) {
    reject(失败结果);
  }
}
```

执行器函数中执行异步操作，定义时立即执行

`resolve`用于改变Promise对象状态为`resolved`，同时保存成功结果到Promise对象中

`reject`用于改变Promise对象状态为`rejected`，同时保存失败结果到Promise对象中

### Promise.prototype.then

参数：成功回调函数和失败回调函数

返回值：一个新的Promise对象

```js
const p = new Promise();
// 写法1
p.then(value => {}, reason => {});
// 写法2
p.then(value => {});
// 写法3
p.then(value=>{}).catch(reason=>{})
```

### Promise.prototype.catch

参数：失败的回调函数

返回值：一个新的Promise对象

```js
const p = new Promise();
p.catch(reason => {});
```

本质：语法糖

```js
p.then(undefined, reason => {});
```

### Promise.prototype.finally

参数：空参的回调函数

作用：不管Promise最后的状态，在执行完then或catch的回调函数后，都会执行finally方法指定的回调函数

```js
const p = new Promise((resolve, reject) => {
    setTimeout(()=>{
        resolve("2秒后")
    }, 2000);
});

p.then(value=>console.log(value)).finally(()=>{console.log("finally")});
```

### Promise.resolve

这是一个静态方法，绑定在类上，而不是实例对象

参数：成功的结果

返回值：一个新的Promise对象，其状态为resolved，结果为传入的参数

本质：语法糖

```js
const p = Promise.resolve(value);
// 等价于
const p = new Promise((resolve, reject) => {
  resolve(value);
});
```

### Promise.reject

参数：失败的结果

返回值：一个新的Promise对象，其状态为resolved，结果为传入的参数

本质：语法糖

```js
const p = Promise.reject(reason);
// 等价于
const p = new Promise((resolve, reject) => {
  reject(reason);
});
```

### Promise.all

参数：一个装有Promise对象的数组

返回值：返回一个新的Promise

该Promise的结果为一个数组，该数组内容为每个Promise对象的结果

该Promise的状态是当全部Promise执行成功时，才成功，否则失败

```js
const p1 = Promise.resolve(1);
const p2 = Promise.resolve(2);
const p3 = Promise.resolve(3);

Promise.all([p1, p2, p3]).then(value => {
  console.log(value); // [1,2,3]
})
```

### Promise.race

参数：一个装有Promise对象的数组

返回值：返回一个新的Promise

该Promise的状态以第一个执行成功的Promise的状态为准

该Promise的结果为第一个执行完毕的Promise的结果

```js
const p1 = Promise.reject(1);
const p2 = Promise.resolve(2);
const p3 = Promise.resolve(3);

Promise.all([p1, p2, p3]).then(value => {
  console.log(value); // [1,2,3]
})
```

### Promise.allSettled

参数：一个Promise对象数组

返回值：为一个Promise，且状态肯定是成功的，该Promise的value为一个数组，里面是每个Promise的执行结果状态和值

与 Promise.all区别：Promise.all的状态不一定成功，只有全部Promise成功，且当失败时，值为失败的值



## resolve和reject

### 本质作用

这两个函数的作用都是为了**改变Promise状态，并将结果存储进Promise中**

resolve：将Promise状态改为成功，并将结果存储进Promise中

* 如果此时已经注册了回调函数，则执行
* 如果此时还没有注册回调函数，则先不管，等待then函数的调用

reject：将Promise状态改为失败，并将结果存储进Promise中

* 如果此时已经注册了回调函数，则执行
* 如果此时还没有注册回调函数，则先不管，等待catch函数的调用

### resolve注意事项

如果一个Promise（记为A）中resolve了另外一个Promise（记为B），则相当于把A的resolve和reject作为所B的then的成功回调和失败回调

```js
resolveA(b);
// 相当于
b.then(resolveA,rejectA);
```

示例

```js
// 定义一个 Promise，该Promise每次执行，有一半概率resolev，一半概率reject
const p = new Promise((resolve,reject)=>{
    // 生成一个随机数，大小区间为 [0.0,1.0)
	const random = Math.random();
    
    setTimeout(()=>{
    	if (random < 0.5) {
            resolve("成功");
        } else {
            reject("拒绝");
        }
	},1000);
});

// 定义一个 Promise，它resolve上一个Promise
new Promise((resolve, reject) => {
    resolve(p);
    // 相当于：p.then(resolve, reject)
})
    .then(value => console.log("success: ", value))
    .catch(reason => console.log("failed: ", reason));
```

### reject注意事项

如果Promise的状态为reject，则必须进行错误处理（即使用catch函数），否则会报错



## then和catch

### 特点

then和catch中的回调函数都会放到任务队列中，属于**微任务**

### 返回值

1. then和catch肯定返回一个新的Promise对象

   ```js
   const p1 = new Promise((resolve, reject)=>{
       resolve("成功");
   });
   
   // then返回一个Promise对象
   const p2 = p1.then(value => console.log(value));
   
   console.log(p2 instanceof Promise); // true
   
   // then返回一个Promise对象
   const p3 = p1.catch(reason => console.log(reason));
   
   console.log(p3 instanceof Promise); // true
   ```

2. 如果then/catch内部返回非Proimse对象，则新Promise的状态为`fulfilled`，`value`为返回的值

   ```js
   const p1 = new Promise((resolve, reject)=>{
       resolve("成功");
   });
   
   // 显式返回值
   const p2 = p1.then(value => value + 123); // 返回 成功123
   
   console.log(p2); // 状态为fulfilled，值为 成功123
   
   // 默认返回值
   const p3 = p1.then(value => console.log(value)); // 默认返回 undefined
   
   console.log(p3); // 状态为 fulfilled，值为 undefined
   ```

   catch函数中也一样

   ```js
   const p1 = new Promise((resolve, reject)=>{
       reject("失败");
   });
   
   // 显式返回值
   const p2 = p1.catch(value => value + 123); // 返回 失败123
   
   console.log(p2); // 状态为fulfilled，值为 失败123
   
   // 默认返回值
   const p3 = p1.catch(value => console.log(value)); // 默认返回 undefined
   
   console.log(p3); // 状态为 fulfilled，值为 undefined
   ```

3. 如果then/catch内部发生错误，或throw Error，则新Promise的状态为`rejected`，`reason`为错误值

   ```js
   const p1 = new Promise((resolve, reject) => {
       resolve("成功");
   });
   
   const p2 = p1.then((value) => {
       throw new Error("throw Error"); // throw Error
   });
   
   console.log(p2); // 状态为rejected，值为 "Error: throw Error"
   ```

   ```js
   const p1 = new Promise((resolve, reject) => {
      resolve("成功");
   });
   
   const p2 = p1.then((value)=>abc+123); // 使用未定义变量，发生错误
   
   console.log(p2); // 状态为rejected，值为 "ReferenceError: abc is not defined"
   ```

   catch函数中也一样

   ```js
   const p1 = new Promise((resolve, reject) => {
       reject("失败");
   });
   
   const p2 = p1.catch((value) => {
       throw new Error("throw Error"); // throw Error
   });
   
   console.log(p2); // 状态为rejected，值为 "Error: throw Error"
   ```

   ```js
   const p1 = new Promise((resolve, reject) => {
      reject("失败");
   });
   
   const p2 = p1.catch((value)=>abc+123); // 使用未定义变量，发生错误
   
   console.log(p2); // 状态为rejected，值为 "ReferenceError: abc is not defined"
   ```

4. 如果then/catch内部返回Promise对象，则返回的就是该Promise对象

   ```js
   const p1 = new Promise((resolve,reject)=>{
   	resolve("成功"); 
   });
   
   const p2 = p1.then(value => { 
       return new Promise((resolve,reject)=>{ // 返回一个新的Promise，状态为fulfilled
          resolve(1);
       });
   });
   
   console.log(p2); // 状态为 fulfilled，值为 1
   
   const p3 = p1.then(value => {
       return new Promise((resolve,reject)=>{ // 返回一个新的Promise，状态为rejected
           reject(2);
       });
   });
   
   console.log(p3); // 状态为 rejected，值为 2
   
   const p4 = p1.then(value => {
       return new Promise((resolve,reject)=>{}); // 返回一个新的Promise，状态为pending
   });
   
   console.log(p4); // // 状态为 pending，值为undefined
   ```

   catch函数中也一样

   ```js
   const p1 = new Promise((resolve,reject)=>{
   	reject("失败"); 
   });
   
   const p2 = p1.catch(value => { 
       return new Promise((resolve,reject)=>{ // 返回一个新的Promise，状态为fulfilled
          resolve(1);
       });
   });
   
   console.log(p2); // 状态为 fulfilled，值为 1
   
   const p3 = p1.catch(value => {
       return new Promise((resolve,reject)=>{ // 返回一个新的Promise，状态为rejected
           reject(2);
       });
   });
   
   console.log(p3); // 状态为 rejected，值为 2
   
   const p4 = p1.catch(value => {
       return new Promise((resolve,reject)=>{}); // 返回一个新的Promise，状态为pending
   });
   
   console.log(p4); // // 状态为 pending，值为undefined
   ```

5. then/catch内部甚至能返回自己，不会报错

   ```js
   const p1 = new Promise((resolve, reject) => {
       resolve("成功");
   });
   
   const p2 = p1.then(value => {
       return p1; // 返回自己，不会报错
   });
   
   p2.then(v => console.log(v));
   ```

   catch函数中也一样

   ```js
   const p1 = new Promise((resolve, reject) => {
       reject("失败");
   });
   
   const p2 = p1.catch(reason => {
       return p1; // 报错，不能返回自己
   });
   
   p2.catch(reason => {
       console.log(reason);
   });
   ```

### 返回值总结

1. then和catch肯定返回一个新的Promise对象
2. then和catch中可以返回任意值（甚至能返回自己，不会报错）
3. 如果then/catch内部返回非Proimse对象，则新Promise的状态为`fulfilled`，`value`为返回的值
4. 如果then/catch内部发生错误，或throw Error，则新Promise的状态为`rejected`，`reason`为错误值
5. 如果then/catch内部返回Promise对象，则返回的就是该Promise对象



## Promise核心理解

### 前提

异步任务执行完毕前必须注册回调函数，因为根据事件循环模型，在异步任务执行完毕后，会将回调函数放任务队列中，所以如果异步任务执行完毕前不注册回调函数，那么程序将无法获得异步操作的结果

### 本质

Promise本质是能够将**异步任务的执行**与**异步任务实际回调函数的注册**进行**分离**

即触发异步任务时不需要立即注册**实际的**回调函数，而是注册一个**使用resolve和reject将异步任务成功和失败的结果值存储进Promise对象中的回调函数**，从而可以在之后的任意时刻再绑定实际的回调函数，从而解决了回调地狱的问题

### 总结

* 无论是否使用Promise，都必须在异步操作完成前注册回调函数
* 如果不使用Promise，则需要注册真正回调函数，可能会发生回调地狱的问题
* 如果使用Promise，则只需要注册使用`resolve`或`reject`的回调函数即可，不需要注册真正回调函数，解决回调地狱问题



## Promise FAQ

### 改变Promise状态的方法？

一共有三种改变Promise状态的方法

* resolve(value): 如果当前是pending就会变为resolved

* reject(reason): 如果当前是pending就会变为rejected

* 抛出异常: 如果当前是pending就会变为rejected

### 能否绑定多个回调函数？

一个Promise指定了多个成功/失败的回调函数，都会调用吗？

可以指定多个成功/失败的回调函数，都会进行调用

```js
const p = new Promise((resolve, rejecet) => {
  resolve("Promise resolved result");
});

p.then(value=>{
  console.log("1 > ", value);
});
p.then(value=>{
  console.log("2 > ", value);
});
p.then(value=>{
  console.log("3 > ", value);
});

// 输出结果
// 1 > Promise resolved result
// 2 > Promise resolved result
// 3 > Promise resolved result
```

### 改变Promise状态和指定回调函数谁先谁后？

都有可能，这两种情况都允许发生

如果先指定回调函数，则Promise会保存回调函数，当状态发生改变时，回调函数就会调用，得到数据

如果先改变状态，则当指定回调时，回调函数就会调用，得到数据

### then返回的新Promise的状态是什么？

简单表达：由then()中回调函数执行的结果决定

详细表达：

* 回调函数执行如果抛出异常，新Promise状态为rejected，结果为抛出的异常
* 回调函数执行如果返回的是非Promise的任意值（包括undefined），新Promise状态为resolved，结果为返回的值
* 如果返回的是一个Promise，则此Promise的状态和结果就是新Promise的状态和结果

### Promise如何串连多个连续的任务？

Promise的then会返回一个新的Promise，可以形成链式调用

### Promise链式调用过程中异常如何捕获？

当使用Promise的then链式调用时，可以在最后指定失败的回调

前面任何操作出现了异常，都会传递到最后失败的回调中进行处理

### 如何中断Promise链式调用？

在回调函数中返回一个pending状态的Promise

```js
p.then(value => {
  return new Promise(() => {});  
})
```



## thenAble对象

如果对象有这样的一个属性函数，称该对象是`thenAble`的

```js
then(resolve, reject)
```

则该对象可以作为一个Promise对象使用

```js
const obj = {
    then(resolve, reject) {
        setTimeout(()=>{
            resolve("这是 thenAble 对象");
        }, 5000);
    }
}

Promise.resolve(obj).then(value=>{
    console.log(value);
})
```

等价于

```js
const p = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve("这是 thenAble 对象");
    }, 5000);
});

Promise.resolve(p).then(value=>{
    console.log(value);
})
```

更加本质化

```js
const p = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve("这是 thenAble 对象");
    }, 5000);
});

const p2 = new Promise((resolve,reject)=>{
    resolve(p)
})

p2.then(value => {
    console.log(value);
})
```



## 高级用法

### Promise式定时器

```js
function timeout(delay) {
    return new Promise((resolve) => {
        setTimeout(()=>{resolve()}, delay);
    })
}

timeout(2000).then(() => {
    console.log("我在2秒后输出");
})
```

### 能进行取消的Promise

```js
function withCancel(promise, token) {
    return Promise.race([promise, new Promise((resolve, reject) => {
        token = function () {
            reject("cancel")
        };
    })]);
}

const token = {};
const enhancePromise = withCancel(promsie, token);

token.cancel();
```

