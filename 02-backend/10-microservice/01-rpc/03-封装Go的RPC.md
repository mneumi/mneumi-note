## 概述

### 需求分析

Go 默认情况下的 rpc 调用不太好用，比如

* 命名空间与服务名是个字符串
* 服务端启动服务麻烦，需要手动创建连接，注册服务麻烦
* 客户端调用很麻烦，需要手动创建连接，传递的参数比较难用

### 解决方法

使用 **代理模式** 解决，即将与业务无关的代码封装到一个代理中，使我们只要专注于业务代码即可



## 服务端代理类

### 概述

需要解决的问题：

* 提供服务接口
* 注册服务函数
* 创建服务器

### ServerProxy

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

### Server

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

	server_proxy.RunServer("localhost", "12345")
}
```



## 客户端代理类

### 概述

需要解决的问题：

* 提供本地方法调用
* 自动完成服务名称注册
* 自动完成远程调用
* 维护客户端

### ClientProxy

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

### Client

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



## 思考

1. 这些proxy能否自动生成（为多种语言生成）
2. 这些概念proxy、handler都在grpc中有对应
3. 以上都能满足，这就是grpc (protobuf)