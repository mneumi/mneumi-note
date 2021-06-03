## 编译器

### 概述

因为 Protobuf 是一种特殊的协议格式，它支持多种语言，所以使用前需要使用 **编译器(protoc)**，将 proto 文件编译生成对应语言的文件

### 编译器

下载地址：https://github.com/protocolbuffers/protobuf/releases

如果想在全局环境下直接使用 `protoc`，则需要将 `protoc` 程序所在的路径加入 `PATH` 环境变量中（因为这是基础操作，这里不再演示，自己完成即可）

### Go编译插件

因为 protobuf 是通过插件的机制来实现对不同语言的编译功能，所以在用于生成 Go 文件之前，需要先安装对应的 Go 插件

```shell
go get github.com/golang/protobuf/protoc-gen-go
```

下载完成后，需要将 `protoc-gen-go` 工具也放入 `PATH` 变量中，因为 `protoc` 在编译时会使用到该插件



## 使用案例

### proto 文件定义

```protobuf
syntax = "proto3";

option go_package = "./hello";

message Hello {
  string name = 1;
}
```

### 生成命令

```shell
protoc -I . hello.proto --go_out .
```

| 命令解析    | 说明                         |
| ----------- | ---------------------------- |
| protoc      | 表示使用 protoc              |
| -I          | -I 表示 -Input，表示输入文件 |
| .           | 表示当前路径                 |
| goods.proto | 表示使用的 proto 文件        |
| --go_out    | 表示要生成 Go 文件           |
| .           | 表示生成目录                 |

### 依赖安装

protoc 编译器只能根据 proto 文件生成对应的结构体定义，如果想要对结构体变量进行序列化或反序列化，则需要使用 proto 包

```shell
go get google.golang.org/protobuf/proto
```

### 基础使用

```go
package main

import (
	"test/hello"

	"google.golang.org/protobuf/proto"
)

func main() {
	// 创建对象
	myHello := &hello.Hello{
		Name: "Hello World",
	}

	// 序列化
	b, _ := proto.Marshal(myHello)
	println(b)

	// 反序列化
	otherHello := &hello.Hello{}
	proto.Unmarshal(b, otherHello)
	println(otherHello.Name)
}
```

