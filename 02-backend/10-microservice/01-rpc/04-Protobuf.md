## 初识Protobuf

### 概述

Protobuf是由 google 发明的一种序列化/反序列化的通信协议，可以类比JSON

### 特点

* 压缩比高
* 支持多种主流语言（Go，JS，Python，Java，C++等）

### 后缀名

`.proto`

### 对比 JSON

| 对比项   | JSON                                                         | Protobuf                                  |
| -------- | ------------------------------------------------------------ | ----------------------------------------- |
| 定义协议 | 使用接口文档                                                 | 使用 proto 文件                           |
| 定义对象 | 不同语言不同实现<br />Go：使用结构体+tag<br />JS：使用对象   | 根据 proto 文件自动生成定义，直接使用即可 |
| 序列化   | 不同语言不同实现<br />Go：json.Marshal<br />JS：JSON.stringify | 使用 proto 工具包的函数                   |
| 反序列化 | 不同语言不同实现<br />Go：json.Unmarshal<br />JS：JSON.parse | 使用 proto 工具包的函数                   |



## 入门案例

### 示例代码

```protobuf
syntax = "proto3";

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}
```

### 代码解析

#### syntax

用于指定 proto 的版本，目前 proto 有 2 和 3 两个主流版本，如果没有定义 syntax，则默认为 proto2

```protobuf
syntax = "proto3";
```

#### message

在protobuf中，基本单位为 `message`，它用于定义`通信信息` 的结构，形如

```protobuf
message 消息名 {
	字段类型 字段名称 = 标识号;
}
```



## 注释

可以使用 C 风格的双斜杠（//） 语法格式给 Protobuf 文件添加注释，如

```protobuf
message SearchRequest { // 查询消息
  string query = 1; // 查询关键字
  int32 page_number = 2;  // 查询页码
  int32 result_per_page = 3;  // 每页返回结果数目
}
```



## 标识号

### 概述

消息定义中，每个字段都有唯一的一个数字标识符，这些标识符是用来在消息的二进制格式中识别各个字段的，一旦开始使用就不能够再改变

### 要求

[1,15]之内的标识号在编码的时候会占用一个字节，[16,2047]之内的标识号则占用2个字节，所以应该为那些频繁出现的消息元素保留 [1,15]之内的标识号，即要为将来有可能添加的、频繁出现的标识号预留一些标识号

最小的标识号可以从1开始，最大到2^29 - 1, or 536,870,911；不可以使用其中的 [19000－19999] 的标识号， Protobuf协议实现中对这些进行了预留，如果非要在.proto文件中使用这些预留标识号，编译时就会报警

### 保留标识号

如果某些字段名或标识号被弃用，不推荐直接使用注释或删除的方式处理，因为这有可能导致 proto 文件解析发生错误

正确的做法是声明保留，在声明保留后，如果后续重复使用到这些字段名或标识号时，编译器会抛出警告

注意：不要在同一行reserved声明中同时声明域名字和标识号，要分为多行处理

```protobuf
message Foo {
  reserved 2, 15, 9 to 11;
  reserved "foo", "bar";
}
```



## 数据类型与默认值

### 基础类型与默认值

| proto Type       | 说明                                                         | proto 默认值     | Go Type          |
| ---------------- | ------------------------------------------------------------ | ---------------- | ---------------- |
| bool             | bool                                                         | false            | bool             |
| string           | 一个字符串必须是UTF-8编码或者7-bit ASCII编码的文本           | ""               | string           |
| int32            | 使用变长编码，对于负值的效率很低<br />如果域有可能有负值，请使用sint64替代 | 0                | int32            |
| float            | 低精度浮点型                                                 | 0                | float32          |
| double           | 高精度浮点型                                                 | 0                | float64          |
| bytes            | 任意顺序的字节数据                                           | 空的bytes        | []byte           |
| 分隔：以下不常用 | 分隔：以下不常用                                             | 分隔：以下不常用 | 分隔：以下不常用 |
| uint32           | 使用变长编码                                                 | 0                | uint32           |
| uint64           | 使用变长编码                                                 | 0                | uint64           |
| sint32           | 使用变长编码，这些编码在负值时比int32高效的多                | 0                | int32            |
| sint64           | 使用变长编码，有符号的整型值，编码时比通常的int64高效        | 0                | int64            |
| fixed32          | 是4个字节，如果数值总是比总是比228大的话，这个类型会比uint32高效 | 0                | uint32           |
| fixed64          | 总是8个字节，如果数值总是比总是比256大的话，这个类型会比uint64高效 | 0                | uint64           |
| sfixed32         | 总是4个字节                                                  | 0                | int32            |
| sfixed64         | 总是8个字节                                                  | 0                | int64s           |

### 枚举类型

### 映射类型



## 定义服务

### 概述

Protobuf 可以定义服务，一般用于配合 `gRPC` 使用

### 定义形式

```protobuf
syntax = "proto3";

service 服务名 {
	rpc 函数名(参数message) returns (返回message);
}
```

### 示例代码

```protobuf
syntax = "proto3";

service HelloService {
	rpc SayHello(Name) returns (Ret);
}

message Name {
	string name = 1;
}

message Ret {
	string ret = 1;
}
```



## 选项

### 概述

在定义 proto 文件时能够标注一系列的options

options并不改变整个文件声明的含义，但却能够影响特定环境下处理方式

### 示例代码

Go语言下定义生成的文件的包的名称

```protobuf
syntax = "proto3";

option go_package = "xxx/v1/user"; // Go语言下定义生成的文件的包的名称
```



## 导入其他包



## proto的生成文件

### 概述

当用protocol buffer编译器来运行.proto文件时，编译器将生成所选择语言的代码

### 生成文件

| 语言        | 生成文件                          | 消息对应                                  |
| ----------- | --------------------------------- | ----------------------------------------- |
| Go          | 一个.pb.go文件                    | 每一个消息有一个对应的结构体              |
| Java        | 一个.java文件                     | 一个特殊的Builder类（用来创建消息类接口） |
| C#          | 一个.cs文件                       | 每一个消息有一个对应的类                  |
| C++         | 一个.h文件和一个.cc文件           | 每一个消息有一个对应的类                  |
| Objective-C | 一个pbobjc.h文件和一个pbobjcm文件 | 每一个消息有一个对应的类                  |
| Python      | 多个模块                          | 每个消息类型生成一个含有静态描述符的模块  |
| JS          |                                   |                                           |



## 注意事项

### 保证Proto文件一致

最好只使用一份proto文件，如果使用多份，则需要保证Proto文件相同

### 序号不能乱改

注意序号一定要相同，如果弃用，这使用 `reserved` 关键字进行保留