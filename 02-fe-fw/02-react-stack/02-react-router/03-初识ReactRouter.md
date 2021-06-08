## 概述

### 三大框架

目前前端流行的三大框架, 都有自己的路由实现

| 框架    | 实现                                                         |
| ------- | ------------------------------------------------------------ |
| Angular | ngRouter                                                     |
| React   | react-router、react-keeper（浏览器专用）、react-navigation（RN专用） |
| Vue     | vue-router                                                   |

### 版本问题

React Router的版本4开始，路由不再集中在一个包中进行管理了，而是拆为3个包

| 组成库              | 说明                 |
| ------------------- | -------------------- |
| react-router        | router的核心部分代码 |
| react-router-dom    | 用于浏览器的路由库   |
| react-router-native | 用于原生应用的路由库 |

`React v5`本质就是`React v4`，`React Router v5`出现的原因是更新 `React Router v4` 的小版本时搞错了版本了，于是干脆直接推出了`React Router v5`，这两者没有`breaking change`



## 安装

因为 `react-router-dom` 底层依赖 `react-router`，所以安装 `react-router-dom` 会自动安装 `react-router`的依赖

即只要安装`react-router-dom`即可

```shell
npm install react-router-dom --save
# OR
yarn add react-router-dom
```
