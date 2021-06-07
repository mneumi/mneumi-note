## 初识Gin

### 概述

`Gin` 是一个 `Go` 的 `Web` 框架

### 特点

`API` 友好：`API` 设计非常简洁和精妙，容易入门和使用

性能良好：路由底层使用了 `httprouter`，性能很好

支持中间件：有非常良好的自定义性和扩展性

### 链接

| 功能   | 链接                             |
| ------ | -------------------------------- |
| 源码库 | https://github.com/gin-gonic/gin |
| 文档   | https://gin-gonic.com            |



## 下载与安装

### 使用`GOPATH`方式

运行 `go get` 命令

```shell
$ go get -u github.com/gin-gonic/gin
```

### 使用`GOMODULE`方式

在源代码中写入导入语句

```go
import "github.com/gin-gonic/gin"
```

运行 `go mod tidy` 命令

```shell
$ go mod tidy
```
