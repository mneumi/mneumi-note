## 概述

### 作用

`tsconfig.json`是 `TypeScript`的编译配置文件，`TypeScript`可以根据它的信息来对代码进行特定模式的编译

### 创建

```shell
tsc --init
```

### 特点

虽然`tsconfig.json`是`json`格式的文件，但`TypeScript`支持其写注释

### 使用

```shell
tsc # 直接使用 tsc，不要指定文件

tsc demo.ts # 如果指定编译文件，则不会使用 tsconfig.json 配置
```



## 常用配置

### 基础配置

| 配置    | 说明                       |
| ------- | -------------------------- |
| include | 指定哪些文件需要进行编译   |
| exclude | 指定哪些文件不需要进行编译 |
| extends | 继承配置文件               |
| files   | 指定需要进行编译的文件     |

### 编译配置

> 编译配置都写在 compileOptions 中

#### 生成目标配置

| 配置      | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| target    | 指定编译的ES版本，默认转es3，选项：es3 es5 es6 es2015 es2016 ... es2020 esnext |
| module    | 模块化方案：'es2015' 'commonjs' 'umd' 'amd' 'system' 'none' 'es2015' 'es2020' 'esnext' |
| lib       | 指定生成的库用于什么环境                                     |
| outDir    | 指定编译后生成文件存储的路径                                 |
| outFile   | 将全局作用域中代码合并为一个文件，如果想要合并模块文件，则需要指定 module 为 system，一般用打包工具 |
| rootDir   | 根目录                                                       |
| baseUrl   | 项目的根路径                                                 |
| sourceMap | 是否生成sourceMap，默认falsee                                |

#### 编译过程检查配置

| 配置                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| allowJs                | 是否对 .js 文件进行编译，默认false                           |
| checkJs                | 是否检查 .js 文件的语法，默认false                           |
| removeComment          | 是否移除注释，默认false                                      |
| noEmit                 | 不生成编译文件，默认false，作用：只有ts检查语法，不用于生成编译文件（比如使用webpack进行打包） |
| noEmitOnError          | 当有错误时，不生成编译文件，默认false，作用：避免有安全隐患的代码生成 |
| noUnusedLocals         | 如果存在没有使用的变量时，将会报错                           |
| noUnusedParameters     | 如果存在没有使用的函数时，将会报错                           |
| experimentalDecorators | 是否开启实验特性：装饰器                                     |
| incremental            | 是否开启增量编译                                             |

#### 严格模式配置

| 配置             | 说明                                      |
| ---------------- | ----------------------------------------- |
| strict           | 所有严格检查的总开关                      |
| alwaysStrict     | 设置编译后文件是否使用严格模式，默认false |
| noImplicitAny    | 不允许隐式any，默认false                  |
| noImplicitThis   | 不允许不明确类型的this                    |
| strictNullChecks | 严格检查空值                              |

### 常用配置一览

```typescript
{
    "include": ['./src/**/*'],        // ** 表示任意目录， * 表示任意文件
    "exclude": ['./src/hello/**/*'],  // 默认值：["node_modules", "bower_component", "jspm_packages"]
    "extends": "./config/base",       // 继承基础配置文件 ./config/base
    "files": ["core.ts", "sys.ts"],   // 指定需要进行编译的文件，与include区别是：include能够指定目录
    "compileOptions": {               // 编译器的选项
        "target": "ES5",              // 指定编译的ES版本，默认转es3，选项：es3 es5 es6 es2015 es2016 ... es2020 esnext
        "module": "es2015",           // 模块化方案：'es2015' 'commonjs' 'umd' 'amd' 'system' 'none' 'es2015' 'es2020' 'esnext'
        "moduleResolution": "node",   // 决定编译器的工作方式，支持"node"和"classic"模式
        "lib": [],                    // 指定生成的库用于什么环境
        "outDir": "./dist",           // 指定编译后生成文件存储的路径
        "outFile": "./dist/app.js",   // 将全局作用域中代码合并为一个文件，如果想要合并模块文件，则需要指定 module 为 system，一般用打包工具
            
        "allowJs": false,             // 是否对 .js 文件进行编译，默认false
        "skipLibCheck": true,         // 忽略对库文件的检查
        "esModuleInterop": true,      // 允许我们使用commonjs的方式import默认文件
        "checkJs": false,             // 是否检查 .js 文件的语法，默认false
        "removeComment": false,       // 是否移除注释，默认false
        "noEmit": false,              // 不生成编译文件，默认false，作用：只有ts检查语法，不用于生成编译文件
        "noEmitOnError": false,       // 当有错误时，不生成编译文件，默认false，作用：避免有安全隐患的代码生成
            
        "strict": false,              // 所有严格检查的总开关
        "alwaysStrict": false,        // 设置编译后文件是否使用严格模式，默认false
        "noImplicitAny": false,       // 不允许隐式any，默认false
        "noImplicitThis": false,      // 不允许不明确类型的this，默认false
        "strictNullChecks": false,    // 严格检查空值
        "resolveJsonModule": true,    // 支持导入JSON文件
        "jsx": "react"                // 允许编译器支持编译React代码
    }
}
```



## 自动编译

### 编译单个文件

编译文件时，使用 `-w` 指令后，tsc会自动监视文件变化，一旦变化，将自动编译

```shell
tsc app.ts -w
tsc app.ts --watch
```

### 自动编译整个项目

先设置 tsconfig.json

```typescript
{
  "compilerOptions": {
    "module": "ES2015",
    "target": "ES2015",
    "strict": true
  }
}
```

然后在根目录下执行命令，就可以自动编译整个目录下所有的ts文件

```shell
tsc
tsc -w
tsc --watch
```



## 补充说明

### 避免strictNullChecks

在明确值存在时，可以使用`!`在末尾修饰，告知编译器，开发者保证值存在，不用进行约束

```typescript
// 如果开启了strictNullChecks，则会报错，因为编译器觉得不一定能取到值
const dom1 = document.getElementById('dom1'); 

// 在末尾加上 ! ，告知编译器，开发者保证一定能取到值
const dom1 = document.getElementById('dom1'); 
```

