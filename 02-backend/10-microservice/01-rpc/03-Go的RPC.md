## Go的RPC

### 概述

Go语言自带有 rpc 库，名称为 `net/rpc`

### 基础使用

#### 服务端

使用方法

1. Go的RPC方法：只能有两个可序列化的参数，其中第二个参数是指针类型，并且返回一个error类型，同时必须是公开的方法

2. `rpc.Register`函数调用会将对象类型中所有满足RPC规则的对象方法注册为RPC函数，所有注册的方法会放在命名空间之下

3. 最后需要建立一个唯一的TCP链接，并且通过rpc.ServeConn函数在该TCP链接上为对方提供RPC服务

示例代码

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

#### 客户端

使用方法

1. 通过`rpc.Dial`拨号RPC服务，创建客户端

2. 通过`client.Call`调用具体的RPC方法

   第一个参数：用点号链接的RPC服务名字和方法名字

   第二/三个参数：分别我们定义RPC方法的两个参数`

示例代码

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

### 序列化协议改为JSON

#### 服务端

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

#### 客户端

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

### 网络协议改为HTTP

#### 服务端

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

#### 客户端

使用 postman 之类的工具调用即可



## 封装GO的RPC

### 需求分析

Go 默认情况下的 rpc 调用不太好用，比如

* 命名空间与服务名是个字符串
* 服务端启动服务麻烦，需要手动创建连接，注册服务麻烦
* 客户端调用很麻烦，需要手动创建连接，传递的参数比较难用

使用 **代理模式** 解决，即将与业务无关的代码封装到一个代理中，使我们只要专注于业务代码即可

### 服务端代理类

需要解决的问题：

* 提供服务接口
* 注册服务函数
* 创建服务器

#### ServerProxy

```go
package server_proxy

import (
	"fmt"
	"net"
	"net/rpc"
)

const HelloServiceName = "handler/HelloSerivce"

type HelloServicer interface {
	Hello(request string, reply *string) error
}

func RegisterHelloService(srv HelloServicer) error {
	return rpc.RegisterName(HelloServiceName, srv)
}

func RunServer(host string, port int) {
	listener, _ := net.Listen("tcp", fmt.Sprintf("%s:%d", host, port))

	for {
		conn, _ := listener.Accept()
		go rpc.ServeConn(conn)
	}
}
```

#### Server

```go
package server

import (
	"server_proxy"
)

type HelloServicer struct{}

func (h *HelloServicer) Hello(request string, reply *string) error {
	*reply = "Hello " + request
	return nil
}

func main() {
	err := server_proxy.RegisterHelloService(&HelloServicer{})

	if err != nil {
		panic("注册服务失败")
	}

	server_proxy.RunServer("lcalhost", "12345")
}
```

### 客户端代理类

需要解决的问题：

* 提供本地方法调用
* 自动完成服务名称注册
* 自动完成远程调用
* 维护客户端

#### ClientProxy

```go
package client_proxy

import "net/rpc"

const HelloServiceName = "handler/HelloSerivce"

type HelloServiceStub struct {
	*rpc.Client
}

func NewHelloServiceClient(host string, port int) HelloServiceStub {
    conn, err := rpc.Dial("tcp", fmt.Sprintf("%s:%d", host, port))

	if err != nil {
		panic("connect error")
	}

	return HelloServiceStub{conn}
}

func (c *HelloServiceStub) Hello(request string, reply *string) error {
	err := c.Call(HelloServiceName+".Hello", request, reply)

	if err != nil {
		return err
	}

	return nil
}
```

#### Client

```go
package main

import (
	"fmt"

	"client_proxy"
)

func main() {
	client := client_proxy.NewHelloServiceClient"localhost", 12345)

	var reply string
	err := client.Hello("alice", &reply) // 这就像本地调用一样
    
	if err != nil {
		panic(err)
	}
    
	fmt.Println(reply)
}
```

### 思考

1. 这些proxy能否自动生成（为多种语言生成）
2. 这些概念proxy、handler都在grpc中有对应
3. 以上都能满足，这就是grpc (protobuf)
