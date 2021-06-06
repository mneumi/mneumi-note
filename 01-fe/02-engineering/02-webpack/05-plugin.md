## plugin

插件用于bundle文件的优化，资源管理和环境变量的注入

作用于整个构建过程

> 任何loaders不可实现的功能都可以由plugins支持



## plugins用法

创建插件对象，放到plugins数组中

```js
const path = require("path");

module.exports = {
    output: {
        filename: "bundle.js"
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: "./src/index.html"
        })
    ]
}
```



## 常用plugins

| 名称                    | 说明                                   |
| ----------------------- | -------------------------------------- |
| html-webpack-plugin     | 创建html文件去承载输出的bundle         |
| clean-webpack-plugin    | 清理构建目录                           |
| uglifyjs-webpack-plugin | 压缩JS                                 |
| zip-webpack-plugin      | 将打包出来的资源生成一个zip包          |
| copy-webpack-plugin     | 将文件或文件拷贝到构建的输出目录       |
| mini-css-extract-plugin | 将css单独打包成一个css文件             |
| commons-chunk-plugin    | 将chunks相同的模块代码提取为公共js文件 |



