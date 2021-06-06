## SPA

### SPA是什么

* SPA全称是 Single Page Appliation：单页应用，是一种Web应用程序或网站

* SPA在和用户交互的时候，不会跳转到其他的页面

* SPA由 JS 实现 URL 变化和动态变换 HTML 的内容

### SPA优点

* 速度快，第一次下载完成静态文件，跳转不需要再次下载

* 体验好，整个体验趋于无缝，更倾向于原生应用

* 为前后端分离提供了实践场所

### SPA框架

* Ember.js
* Angular

* React Router

* Vue Router



## Vue Router

### Vue Router是什么

Vue Router 是 [Vue.js](http://cn.vuejs.org/) 官方的路由管理器，它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌

主要功能有：

* 嵌套的路由/视图表
* 模块化的、基于组件的路由配置
* 路由参数、查询、通配符
* 基于 Vue.js 过渡系统的视图过渡效果
* 细粒度的导航控制
* 带有自动激活的 CSS class 的链接
* HTML5 历史模式或 hash 模式，在 IE9 中自动降级
* 自定义的滚动条行为

### Vue Router的安装

#### 通过脚手架安装

在`vue create`时选择使用 VueRouter

#### 通过命令安装

```shell
npm install vue-router --save
# OR
yarn add vue-router
```
