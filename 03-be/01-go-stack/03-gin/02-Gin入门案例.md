## 入门案例

`Gin` 框架版本的`Hello World`

```go
package main

import "github.com/gin-gonic/gin" // 导入Gin框架

func main() {
  engine := gin.Default() // 创建Gin使用默认配置的引擎

  engine.GET("/helloworld", func(ctx *gin.Context) { // 设置路由以及路由处理函数
    ctx.String(200, "Hello World")
  })

  engine.Run(":8080") // 启动引擎（启动服务器）
}
```

访问`127.0.0.1:8080/helloworld`即可看到浏览器显示`Hello World`