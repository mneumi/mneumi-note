## component

注册全局组件，具体看 **组件化** 一文



## directive

注册全局指令，具体看 **自定义指令** 一文



## mixin

注册全局mixin，具体看 **混入mixin** 一文



## provide

全局提供者，具体看 **提供与注入** 一文



## use

使用插件，具体看 **自定义插件** 一文



## mount

挂载应用到节点上

```js
import { createApp } from 'vue';

const app = createApp({});

app.mount('#my-app');
```



## unmount

卸载应用

```js
import { createApp } from 'vue';

const app = createApp({});

app.mount('#my-app');

// 挂载5秒后，应用将被卸载
setTimeout(() => app.unmount('#my-app'), 5000);
```



## config

设置应用配置的对象

```js
import { createApp } from 'vue';

const app = createApp({});

app.config = {...};
```

具体看 **总结：应用配置** 一文