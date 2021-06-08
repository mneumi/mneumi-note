## StrictMode介绍

StrictMode 是一个用来突出显示应用程序中潜在问题的组件，它为其后代元素触发额外的检查和警告

StrictMode 与 Fragment 一样，StrictMode 不会渲染任何可见的 UI

严格模式检查仅在开发模式下运行，它们不会影响生产构建



## 使用方式

### 导入

```jsx
import { StrictMode } from "react";
```

### 使用

使用 StrictMode 组件包裹要进行严格检查的组件

```jsx
ReactDOM.render((
  <StrictMode>
    <App />
  </StrictMode>
), document.getElementById("app"));
```

### 示例

可以为应用程序的任何部分启用严格模式

```jsx
<Header />
<React.StrictMode>
  <div>
  	<ComponentOne />
    <ComopnentTwo />
  </div>
</React.StrictMode>
<Footer />
```

不会对 Header 和 Footer 组件运行严格模式检查

ComponentOne 和 ComponentTwo 以及它们的所有后代元素都将进行检查



## 严格在哪里

下列情况会发生警告

1. 使用了不安全的生命周期

2. 使用过时的ref API

3. 使用废弃的findDOMNode方法

   在之前的React API中，可以通过findDOMNode来获取DOM，不过已经不推荐使用了，可以自行学习

4. 使用了过时的context API

   * 早期的Context是通过static属性声明Context对象属性，通过getChildContext返回Context对象等方式来使用Context的
   * 目前这种方式已经不推荐使用，大家可以自行学习了解一下它的用法

5. 存在副作用

   * 组件的constructor会被调用两次
   * 这是严格模式下故意进行的操作，让你来查看在这里写的一些逻辑代码被调用多次时，是否会产生一些副作用
   * 在生产环境中，是不会被调用两次的
