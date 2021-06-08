## 初识npm

### npm全称

Node Package Manager：Node的包管理器

### npm 与 node关系

npm是node官方推出的包管理器，专门用于管理包

### npm安装

安装node时，将自动安装npm



## 开发依赖和生产依赖

### 开发依赖

开发依赖指只在开发时使用的，语法检查，压缩代码，扩展css前缀，临时服务器

比如：水泥，胶水，切割机，地板，瓷砖

### 生产依赖

生产依赖指在生产环境中必不可少的

比如：地板，瓷砖



## npm常用命令

| 常用命令                               | 说明                                                 |
| -------------------------------------- | ---------------------------------------------------- |
| npm -v                                 | 查看npm版本                                          |
| npm init                               | 初始化项目的package.json文件                         |
| npm install / i 包名                   | 安装指定的包                                         |
| npm install / i 包名 --save / -S       | 安装指定的包，并添加到项目的生产依赖中               |
| npm install / i 包名 --save-dev / -D   | 安装指定的包，并添加到项目的开发依赖中               |
| npm install / i 包名 -g                | 全局安装                                             |
| npm install / i                        | 安装项目中所有的依赖                                 |
| npm install / i 报名 --registry=[地址] | 安装时指定仓库地址                                   |
| npm uninstall/ r 包名                  | 删除指定的包                                         |
| npm uninstall/ r -g 包名               | 全局卸载                                             |
| npm aduit fix                          | 检测项目依赖中的问题，并且尝试修复，可以处理安全问题 |
| npm search / s 包名                    | 搜索指定的包                                         |
| npm view 包名 versions                 | 查看npm仓库中某个包的所有版本                        |
| npm view 包名 version                  | 查看npm仓库中某个包的最新版本                        |
| npm info 包名                          | 查看某个包的信息                                     |
| npm ls                                 | 查看当前仓库依赖树上所有包的版本信息                 |
| npm config set [key] [value]           | 设置npm特定配置                                      |
| npm config get [key]                   | 获取npm特定配置                                      |
| npm cache clean                        | 清空npm缓存                                          |
| npm rebuild                            | 重新安装依赖                                         |
| rm -rf node_modules && npm install     | 重新安装依赖                                         |
| npm config get cache                   | 查看缓存地址                                         |
| npm config get prefix                  | 查看npm安装路径                                      |



## npx

### npx是什么

> npx 在 npm 5.2 版本后才有

npx是安装node时，附带安装的一个包管理工具

### npx作用

* 避免全局安装模块：下载到临时目录，然后使用，使用完自动删除

* 调用项目内部安装的模块：自动在node_modules中寻找项目内的模块

  示例：通过 npx 调用 webpack

  ```shell
  node_modules/.bin/webpack --version
  
  # 等价于下列用法
  
  npx webpack --version
  ```

  为什么script脚本中能够使用webpack命令？

  ```json
  {
      "scripts": {
          "build": "webpack webpack.config.js"
      }
  }
  ```

  因为在运行脚本时，node会自动将 node_module/.bin 目录加入到环境变量中，调用完后再将环境变量恢复原样（node_module/.bin：存储项目安装的可执行文件）



## cnpm

### cnpm介绍

`cnpm`是由淘宝团队维护的一个完整 `npmjs.org` 镜像，可以用此代替官方版本(只读)，同步频率目前为 **10分钟** 一次以保证尽量与官方服务同步

### cnpm使用

#### 方式1：直接安装cnpm

```shell
npm install cnpm -g --registry=https://registry.npm.taobao.org
```

以后使用`cnpm`替代`npm`

#### 方式2：替换npm仓库地址为淘宝镜像地址（推荐）

```shell
npm config set registry https://registry.npm.taobao.org
```

查看是否更改成功

```shell
npm config get registry
```

以后所有npm命令时，都是从淘宝的镜像服务器中下载
