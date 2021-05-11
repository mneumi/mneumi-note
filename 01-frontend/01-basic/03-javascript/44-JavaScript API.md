## onbeforeunload

### 概述

检测离开页面时的API

### 作用

提醒用户保存信息、进行相关取消操作等

### 示例代码

```js
window.onbeforeunload = function() {
    alert("确定要离开吗？")
}
```



## Intersection Observer

### 概念

检测某个元素是否已经进入视口，即用户能不能看到它

### 作用

可以用于实现**图片懒加载**，从而取代 `scroll`事件，提高效率和性能

### 用法

#### 构造函数

```js
const io = new IntersectionObserver(callback, option);
// callback：可见性变化时的回调函数
// option：配置对象（可选）
```





## window.postMessage

