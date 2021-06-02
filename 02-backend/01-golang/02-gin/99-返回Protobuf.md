```protobuf
syntax = "proto3";

option go_package = ".;proto";

message Teacher {
	string name = 1;
	repeated string course = 2;
}
```



```go
import (
	"github.com/gin-gonic/gin"
    "net/http"
    "Test/gin_start/ch6/proto"
)

func main() {
    router := gin.Default()
    
    router.GET("/someProtoBuf", returnProto)
}

func returnProto(c *gin.Context) {
    user := &proto.Teacher {
        Name: "booo",
        Course: []string {
            "nodejs",
            "go",
            "微服务",
        }
    }
    
    c.ProtoBuf(http.StatusOK, user)
}
```



## PureJSON

通常情况下，JSON会将特殊的HTML字符替换为对应的 unicode 字符，如 `<` 会替换为 `\u003q`

如果想要原样输出，则使用 `PureJSON`

```go
c.PureJSON(200, gin.H{
    "html": "<b>Hello World</b>"
})
```

