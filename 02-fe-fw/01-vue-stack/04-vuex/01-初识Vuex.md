## 状态管理

### 全局对象的缺陷

1. 不是响应式的
2. 数据修改无法跟踪
3. 不符合组件开发的原则

### 状态管理工具

1. Vuex
2. Redux
3. Mobx

### 状态管理工具的基本原则

1. 一个类似 Object 的全局数据结构，称为 store
2. 只能调用特定的方法去实现数据的修改，不能直接修改



## 初识Vuex

### Vuex是什么

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**

### Vuex特点

1. 状态存储是响应式的
2. 不能直接改变 store 中的状态，更改途径是显式地进行 `commit mutation`或`dispatch action`

### Vuex核心概念

![vuex](./images/vuex.png)