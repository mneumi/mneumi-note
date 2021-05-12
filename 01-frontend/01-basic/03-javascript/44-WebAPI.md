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

> 参考自阮一峰老师的[博客文章](http://www.ruanyifeng.com/blog/2016/11/intersectionobserver_api.html)

### 概念

检测某个元素是否已经进入视口，即用户能不能看到它

### 作用

可以用于实现**图片懒加载**，取代 `scroll`事件，提高效率和性能

### 用法

#### 构造函数

```js
const intersectionObserver = new IntersectionObserver(callback, option);
// callback：可见性变化时的回调函数
// option：配置对象（可选）
```

#### io对象方法

```js
// 开始观察
intersectionObserver.observe(DOM节点);

// 停止观察
intersectionObserver.unobserve(DOM节点);

// 关闭观察器对象
intersectionObserver.disconnect();
```

#### callback参数

callback的参数为一个数组，存储的是`IntersectionObserverEntry`对象

```js
(entries) => {};
```

#### IntersectionObserverEntry对象

| 属性                | 说明                                   |
| ------------------- | -------------------------------------- |
| time                | 可见性发生变化的时间，单位毫秒         |
| target              | 被观察的对象，DOM节点类型              |
| rootBounds          | 根元素的getBoundingClientRect返回值    |
| boundingClientReact | 目标元素的getBoundingClientReact返回值 |
| intersectionRect    | 目标元素与视口的交叉区域的信息         |
| intersectionRatio   | 目标元素的可见比例                     |

#### option对象

| 属性       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| threshold  | 决定什么时候触发回调函数，它是一个数组，决定交叉比例触发callback，默认值为[0] |
| root       | 指定目标元素所在的容器节点，注意：容器元素必须是目标元素的祖父节点 |
| rootMargin | 定义容器元素的margin值                                       |

### 实例：图片懒加载

```html
<img class="img-lazy-load" data-img="http://demo/1.png" src="default-img" />
<img class="img-lazy-load" data-img="http://demo/2.png" src="default-img" />
<img class="img-lazy-load" data-img="http://demo/3.png" src="default-img" />
```

```js
function lazyImgLoad() {
    // 创建观察器，并设置好回调函数
    const observer = new IntersectionObserver(entries => {
       entries.forEach(element => {
           if (element.intersectionRatio > 0 && element.intersectionRatio <= 1) {
               // 获取DOM节点
               const target = element.target;
               // 获取真正的图片地址
        	   const imgUrl = target.dataset.src;
               // 设置图片地址
               target.src = imgUrl;
               // 取消对该元素的监听
               observer.unobserve(target);
           }
       });
    });
    
    // 获取所有需要懒加载的对象，并注册进观察器
    Array.from(document.querySelectorAll(".img-lazy-load")).forEach((item) => {
        intersectionObserver.observe(item);
    });
}
```

### 实例：无限滚动

使用前提：在列表下方加一个尾栏（class为`scrollFooter`），观察这个尾栏，判断是否需要加载新元素

```js
const intersectionObserver = new IntersectionObserver((entries) => {
    // 如果不可见，证明视口还没有显示到列表，直接返回
    if (entries[0].interscetionRatio < 0) {
        return;
    }
    // 加载元素
    loadItems(10);
    console.log("加载了新的元素");
});

intersectionObserver.observe(document.querySelector(".scrollFooter"));
```



## window.postMessage

TODO