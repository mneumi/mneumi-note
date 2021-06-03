## 概述

Go语言自带有 rpc 库，名称为 `net/rpc`



## 基础使用

### 服务端

#### 使用方法

1. Go的RPC方法：只能有两个可序列化的参数，其中第二个参数是指针类型，并且返回一个error类型，同时必须是公开的方法

2. `rpc.Register`函数调用会将对象类型中所有满足RPC规则的对象方法注册为RPC函数，所有注册的方法会放在命名空间之下

3. 最后需要建立一个唯一的TCP链接，并且通过rpc.ServeConn函数在该TCP链接上为对方提供RPC服务

#### 示例代码

```go
package main

import (
	"net"
	"net/rpc"
)

type HelloService struct{}

func (s *HelloService) Hello(request string, reply *string) error {
	*reply = "hello " + request
	return nil
}

func main() {
	_ = rpc.RegisterName("HelloService", &HelloService{})

	listener, err := net.Listen("tcp", ":12345")

	if err != nil {
		panic("监听端口失败")
	}

	for {
		conn, err := listener.Accept()

		if err != nil {
			panic("建立链接失败")
		}

		go rpc.ServeConn(conn)
	}
}
```

### 客户端

#### 使用方法

1. 通过`rpc.Dial`拨号RPC服务，创建客户端

2. 通过`client.Call`调用具体的RPC方法

   第一个参数：用点号链接的RPC服务名字和方法名字

   第二/三个参数：分别我们定义RPC方法的两个参数`

#### 示例代码

```go
package main

import (
	"fmt"
	"log"
	"net/rpc"
)

func main() {
	client, err := rpc.Dial("tcp", "localhost:12345")

	if err != nil {
		log.Fatal("dialing:", err)
	}

	var reply string

	err = client.Call("HelloService.Hello", "hello", &reply)

	if err != nil {
		log.Fatal(err)
	}

	fmt.Println(reply)
}
```



## 序列化协议改为JSON

### 服务端

```go
package main

import (
	"net"
	"net/rpc"
	"net/rpc/jsonrpc"
)

type HelloService struct{}

func (s *HelloService) Hello (request string, reply *string) error {
	*reply = "hello " + request
	return nil
}

func main() {
	rpc.RegisterName("HelloService", new(HelloService))
    
	listener, err := net.Listen("tcp", ":12345")
    
	if err != nil {
		panic("启动错误")
	}
    
	for {
		conn, err := listener.Accept()
        
		if err != nil {
			panic("接收")
		}
        
		go rpc.ServeCodec(jsonrpc.NewServerCodec(conn))
	}
}
```

### 客户端

```go
package main

import (
	"fmt"
	"net"
	"net/rpc"
	"net/rpc/jsonrpc"
)

func main() {
	conn, err := net.Dial("tcp", "localhost:12345")
    
	if err != nil {
		panic("连接错误")
	}
    
	client := rpc.NewClientWithCodec(jsonrpc.NewClientCodec(conn))
    
	var reply string
	err = client.Call("HelloService.Hello", "imooc", &reply)
    
	if err != nil {
		panic("调用错误")
	}
    
	fmt.Println(reply)
}
```



## 网络协议改为HTTP

### 服务端

```go
package main

import (
	"io"
	"net/http"
	"net/rpc"
	"net/rpc/jsonrpc"
)

type HelloService struct{}

func (s *HelloService) Hello(request string, reply *string) error {
	*reply = "hello " + request
	return nil
}

func main() {
	rpc.RegisterName("HelloService", new(HelloService))

	http.HandleFunc("/jsonrpc", func(w http.ResponseWriter, r *http.Request) {
		var conn io.ReadWriteCloser = struct {
			io.Writer
			io.ReadCloser
		}{
			ReadCloser: r.Body,
			Writer:     w,
		}

		rpc.ServeRequest(jsonrpc.NewServerCodec(conn))
	})

	http.ListenAndServe(":12345", nil)
}
```

### 客户端

使用 postman 之类的工具调用即可
