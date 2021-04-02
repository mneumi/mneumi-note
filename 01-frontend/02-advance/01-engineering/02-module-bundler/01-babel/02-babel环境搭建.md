## 环境搭建

```shell
yarn add @babel/cli -D                        
yarn add @babel/core -D                       
yarn add @babel/preset-env -D                 
yarn add @babel/plugin-transform-runtime -D

yarn add @babel/polyfill                     
yarn add @babel/runtime                                   
```



## .babelrc 配置

```js
{
    "presets": [
        [
            "@babel/preset-env"
        ]
    ],
    "plugins": [
        
    ]
}
```



## 编译

```shell
npx babel src/index.js
```



## presets

常用场景的plugins进行打包

```js
@babel/preset-env
@babel/preset-flow
@babel/preset-react
@babel/preset-typescript
```



## babel-polyfill

polyfill：补丁，垫片

如 indexOf

polyfill的集合——core.js和regenerator

* core.js不支持 生成器语法，而 regenerator支持生成器语法

babel-polyfill即core.js和regenerator的集合

### 注意

babel 7.4 之后会废弃babel-polyfill，推荐直接使用core-js和regenerator

但babel-polyfill的概念仍然可能会在面试中问到

### 使用

```js
import "@babel/polyfill"
```

### 按需加载

babel-polyfill体积较大，不需要完全加载

配置如下(.babelrc)

```js
{
    "presets": [
        [
            "@babel/preset-env",
            {
                "useBuiltIns": "usage",
                "corejs": 3
            }
        ]
    ]
}
```



## babel-runtime

babel-polyfill问题：会污染全局环境

* 如果做一个独立的web系统，没问题
* 如果做一个lib，则不行

babel-runtime作用：避免命名污染全局

babel-runtime配置

```js
{
    "presets": [
        [
            "@babel/preset-env"
        ]
    ],
    "plugins": [
        "@babel/plugin-transform-runtime",
        {
            "absoluteRuntime": false,
            "corejs": 3,
            "helpers": true,
            "regenerator": true,
            "useESModules": false
            
        }
    ]
}
```

