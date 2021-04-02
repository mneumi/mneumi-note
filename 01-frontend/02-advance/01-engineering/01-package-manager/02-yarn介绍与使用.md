## 初识yarn

### yarn简介

`yarn`发布于2016年10月，由`Facebook`推出

### yarn特点

| 特点 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| 快速 | yarn 会缓存它下载的每个包，所以无需重复下载，它还能并行化操作以最大化资源利用率，安装速度之快前所未有 |
| 安全 | yarn会在每个安装包被执行前校验其完整性                       |
| 可靠 | yarn 使用格式详尽而又简洁的 lockfile文件 和确定性算法来安装依赖，能够保证在一个系统上的运行的安装过程也会以同样的方式运行在其他系统上 |

### yarn安装

```shell
npm install yarn -g
```

#### 注意

由于yarn的全局安装位置与npm不同，则要配置yarn的全局安装路径到环境变量中，否则全局安装的包无法使用

查看yarn的全局安装路径

```shell
yarn global dir
yarn global bin
```

#### 设置淘宝镜像

```shell
yarn config set registry https://registry.npm.taobao.org
```



## yarn常用命令

| 常用命令                 | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| yarn                     | 安装依赖                                                     |
| yarn add 包名            | 安装到生产依赖                                               |
| yarn add 包名 -D / --dev | 安装到开发依赖                                               |
| yarn global add 包名     | 全局安装                                                     |
| yarn remove 包名         | 移除依赖                                                     |
| yarn global remove 包名  | 移除全局依赖                                                 |
| yarn cache clean         | 清空yarn缓存                                                 |
| yarn upgrade             | 升级依赖                                                     |
| yarn info 包名           | 查看包的详细信息                                             |
| yarn init                | 初始化项目的package.json文件                                 |
| yarn cache dir           | 查看缓存的目录                                               |
| yarn global bin          | 查看bin全局安装路径                                          |
| yarn global dir          | 查看dir全局安装路径                                          |
| yarn create              | 运行时会自动安装或者更新需要的包，同时在它的名字前加上`create-`前缀 |
