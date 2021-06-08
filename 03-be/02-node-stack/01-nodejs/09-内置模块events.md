## 初识events模块

Node核心内置模块，用于实现事件的发布订阅模式

Node中的核心API都是基于异步事件驱动的

在这个体系中，某些对象（发射器（Emitters））发出某一个事件，可以监听这个事件（监听器 Listeners），并且传入的回调函数，这个回调函数会在监听到事件时调用



## 常用API

| 常用API                                          | 说明                                                      |
| ------------------------------------------------ | --------------------------------------------------------- |
| emitter.on(eventName, listener)                  | 监听事件，也可以使用`addListener`                         |
| emitter.off(eventName, listener)                 | 移除事件监听，也可以使用`removeListener`                  |
| emitter.emit(eventName[, ...args])               | 发出事件，可以携带参数                                    |
| emitter.once(eventName, listener)                | 监听事件一次，触发后取消监听                              |
| emitter.removeAllListeners([eventName])          | 移除特定/所有的监听器                                     |
| emitter.prependListener(eventName, listener)     | 将监听事件添加到最前面                                    |
| emitter.prependOnceListener(eventName, listener) | 将监听事件添加到最前面，且只监听一次                      |
| emitter.eventNames()                             | 返回当前 EventEmitter对象注册的事件字符串数组             |
| emitter.setMaxListeners(num)                     | 修改当前 EventEmitter对象的最大监听器数量，默认是10       |
| emitter.getMaxListeners()                        | 返回当前 EventEmitter对象的最大监听器数量                 |
| emitter.listenerCount(eventName)                 | 返回当前 EventEmitter对象某一个事件名称，监听器的个数     |
| emitter.listeners(eventName)                     | 返回当前 EventEmitter对象某个事件监听器上所有的监听器数组 |



## 示例代码

### emitter.emit(eventName[, ...args])

发出事件，可以携带参数

```js
const EventEmitter = require('events');

const bus = new EventEmitter();

function clickHandlerOne(...args) {
    console.log('监听到click事件 One: ', ...args);
}

function clickHandlerTwo(...args) {
    console.log('监听到click事件 Two: ', ...args);
}

bus.on('click', clickHandlerOne);
bus.addListener('click', clickHandlerTwo);

setTimeout(() => {
    bus.emit('click', 'Hello World');
    bus.off('click', clickHanlerOne);
    bus.emit('click', 'Hello World');
    bus.removeListener('click', clickHanlerTwo);
    bus.emit('click', 'Hello World');
});
```

### emitter.removeAllListeners([eventName])

移除特定/所有的监听器

```js
emitter.once('click', (...args) => {
    console.log("a监听到事件", ...args);
});

emitter.prependListener('click', (...args) => {
    console.log("b监听到事件", ...args);
});

emitter.prependOnceListener('click', (...args) => {
    console.log("c监听到事件", ...args);
});

emitter.removeAllListeners('click');
emitter.removeAllListeners();
```

### emitter.listeners(eventName)

返回当前 EventEmitter对象某个事件监听器上所有的监听器数组

```js
console.log(emitter.eventNames());
console.log(emitter.setMaxListeners(100));
console.log(emitter.getMaxListeners());
console.log(emitter.listenerCount('click'));
console.log(emitter.listeners('click'));
```