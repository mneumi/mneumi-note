## 初识nest

### nest介绍

nest 是一个 Node.js 服务器端应用程序的框架

### nest特点

* nest内部使用TypeScript编写，能够提供优秀的TypeScript支持，编写起来非常舒服

* nest集成了许多库和功能，能够实现很多常见的功能，而无需导入第三方库，开发和维护非常舒服

* nest提供IOC容器，并通过TypeScript中的装饰器提供AOP编程能力（即提供了IOC+AOP的开发模式）

### nest底层

nest 底层对 Express 和 Fastify 框架进行了集成和封装，从而可以轻松使用每个平台的无数第三方模块

nest 在 Express 和 Fastify 框架上提供了一层通用的API接口，用户可以根据实际选择底层框架

* Express：更多的扩展，性能较差（默认框架）
* Fastify：更高的性能，但扩展较少



## nest使用

### 安装 nest 命令行工具

```shell
yarn global add @nestjs/cli
```

### 创建项目

```shell
nest new <project-name>
```

### 运行项目

```shell
yarn run start:dev
```

浏览器访问项目 http://127.0.0.1:3000



## nest工程目录

```shell
|- src/                          # 存放源代码的目录
    |- app.module.ts             # app module
    |- app.controller.ts         # app module的controller
    |- app.service.ts            # app module的service 
    |- app.controller.spec.ts    # app module的controller的测试文件
    |- main.ts                   # 程序入口文件

|- test/                         # 存放测试代码的目录
    |- app.e2e-spec.ts           # app的端对端测试文件
    |- jest-e2e.json             # jest的端对端测试配置文件

|- README.md                     # 项目介绍文件
|- nest-cli.json                 # CLI工具配置文件
|- package.json                  # 项目依赖配置文件
|- yarn.lock                     # 项目依赖版本锁定文件
|- tsconfig.json                 # typescript的配置文件
|- tsconfig.build.json           # typescript的构建配置文件
|- .eslintrc.js                  # eslint的配置文件
|- .gitignore                    # git的忽略管理文件
|- .prettierrc                   # prettier的配置文件
```

