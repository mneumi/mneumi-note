## TypeScript的编译工具

TypeScript 是编译型语言，实际运行时，需要先编译为 JavaScript 才可以运行，常用编译器有

| 编译器                    | 说明                    |
| ------------------------- | ----------------------- |
| tsc                       | 微软官方的编译工具      |
| ts-loader                 | webpack相关的编译工具   |
| awesome-typescript-loader | webpack相关的编译工具   |
| babel-loader              | babel官方提供的编译工具 |
| esbuild                   | go语言编写的编译工具    |



## ts-loader使用示例

### 环境搭建

```shell
npm init -y

# 基础工具安装
yarn add typescript webpack webpack-cli ts-loader --dev

# webpack插件安装
yarn add html-webpack-plugin clean-webpack-plugin --dev
yarn add @babel/core @babel/preset-env babel-loader core-js --dev
yarn add webpack-dev-server --dev
```

### 配置

#### webpack.config.js

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  entry: './src/index.ts',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
    environment: {
      arrowFunction: false // 禁止使用箭头函数
    }
  },
  module: {
    rules: [
      {
        test: /\.ts$/,
        use: [
          {
            loader: 'babel-loader',
            options: {
              // 设置预定义的环境
              presets: [
                [
                  "@babel/preset-env",
                  {
                    // 要兼容的目标浏览器
                    targets: {
                      "chrome": "88",
                      "ie": "11"
                    },
                    // 指定corejs版本
                    "corejs": "3",
                    // 使用corejs的方式
                    "useBuiltIns": "usage" // 按需加载
                  }
                ]
              ]
            }
          },
          'ts-loader'
        ],
        exclude: /node_modules/
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      // title: "这是一个自定义的title",
      template: './src/index.html'
    }),
    new CleanWebpackPlugin()
  ],
  resolve: { // 用来设置模块
    extensions: ['.ts', '.js']
  }
};
```

#### tsconfig.json

```json
{
  "compilerOptions": {
    "module": "ES2015",
    "target": "ES2015",
    "strict": true
  }
}
```

#### index.ts

```typescript
function sum(a: number, b: number): number {
    return a + b;
}

console.log(sum(123, 456));
```

#### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TypeScript</title>
</head>
<body>
    This is a HTML template
</body>
</html>
```

#### package.json

```json
{
    "scripts": {
        "start": "webpack serve --open chrome.exe",
        "build": "webpack"
    }
}
```