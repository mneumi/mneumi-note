## Vue优化技巧

### 基于选项

* data层级不要太深
* 合理使用 computed

### 基于指令

* 合理使用 v-show / v-if
* v-for时要加 key

### 基于组件

* 使用 keep-alive 缓存组件
* 使用 import 加载异步组件
* 自定义事件、DOM事件、定时器及时销毁