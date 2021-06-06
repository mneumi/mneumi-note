## path模块介绍

### 概述

path模块用于对路径和文件进行处理，提供了很多好用的方法

### 特点

解决不同平台下，路径分隔符不同的问题

> Linux和Mac下用的时 `/`，在 Windows 下用的是 `\`

自动解析层级关系

> 能够处理`./`和`../`的层级关系



## 常用API

### resolve

获取绝对路径

```js
const path = require('path');

console.log(path.resolve('/foo/bar', './baz'));      // '/foo/bar/baz'
console.log(path.resolve('/foo/bar', '/tmp/file/')); // '/tmp/file'

console.log(path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif'));
// 如果当前工作目录为 /home/myself/node
// 则返回 '/home/myself/node/wwwroot/static_files/gif/image.gif'
```

### join

单纯将各个path片段连接在一起，不一定返回绝对路径

```js
const path = require('path');

console.log(path.join('/a', '/b'));    // Outputs '/a/b'
console.log(path.resolve('/a', '/b')); // Outputs '/b'
```

### dirname

获取路径中的目录

### basename

获取路径中文件名

### extname

获取路径中文件名的扩展名

```js
const path = require('path');

const filePath = '/User/mneumi/abc.txt';

console.log(path.dirname(filePath));  // /User/mneujmi
console.log(path.basename(filePath)); // abc.txt
console.log(path.extname(filePath));  // .txt
```
