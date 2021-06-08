## 初识Stream

### Stream是什么

流是连续字节的一种表现形式和抽象概念

可以想象当从一个文件中读取数据时，文件的二进制（字节）数据会源源不断的被读取到程序中，而这个一连串的字节，就是我们程序中的流

流应该是可读的，也是可写的

### Stream意义

在之前学习文件的读写时，可以直接通过 readFile或者 writeFile方式读写文件，为什么还需要流呢？

直接读写文件的方式，虽然简单，但是无法控制一些细节的操作

比如从什么位置开始读、读到什么位置，一次性读取多少个字节，读到某个位置后，暂停读取，某个时刻恢复读取

或者这个文件非常大，比如一个视频文件，一次性全部读取并不合适

### Stream在Node中的应用

事实上Node中很多对象是基于流实现的

* http模块的Request和Response对象
* process.stdout对象



## Stream种类

Node.js中有四种基本流类型

| 流类型            | 说明                                           | 例子                   |
| ----------------- | ---------------------------------------------- | ---------------------- |
| Writable          | 可以向其写入数据的流                           | fs.createWriteStream() |
| Readable          | 可以从中读取数据的流                           | fs.createReadStream()  |
| Duplex            | 同时为Readable和的流Writable                   | net.Socket             |
| Transform：Duplex | Duplex可以在写入和读取数据时修改或转换数据的流 | zlib.createDeflate()   |



## Readable流

### 问题

在之前读取一个文件的信息的方式是使用`fs.readFile`，如下

```js
fs.readFile("./foo.txt", (err, data) => {
    console.log(data);
});
```

这种方式是一次性将一个文件中所有的内容都读取到程序（内存）中，但是这种读取方式就会出现之前提到的很多问题，比如文件过大、无法调整读取的位置和结束的位置、一次读取的大小等等

### 解决

这个时候，就可以使用 **createReadStream** 这个 **Readable Stream**，参数如下

* start：文件读取开始的位置
* end：文件读取结束的位置
* highWaterMark：一次性读取字节的长度，默认是64kb

### 示例

#### 创建文件的 `Readable Stream`

```js
const reader = fs.createReadStream("./foo.txt", {
    start: 3,
    end: 8,
    highWaterMark: 4
})
```

#### 读取数据

```js
reader.on("data", (data) => {
    console.log(data);
})
```

#### 暂停、恢复流

```js
read.pause();

setTimeout(() => {
    read.resume();
}, 2000);
```

#### 监听开启事件、关闭事件、结束事件

```js
reader.on("open", fd => {
    console.log("文件被打开");
})

reader.on("end", () => {
    console.log("文件读取完毕");
})

reader.on("close", () => {
    console.log("文件被关闭");
})
```



## Writable流

### 问题

在之前读取一个文件的信息的方式是使用`fs.writeFile`，如下

```js
fs.writeFile("./foo.txt", "内容", (err) => {
    console.log(err);
})
```

这种方式相当于一次性将所有的内容写入到文件中，这种方式存在下列问题，比如无法一点点写入内容、无法精确每次写入的位置等等

### 解决

这个时候，就可以使用 **createWriteStream** 这个 **Writable Stream**，参数如下

* flags：默认是 `w`，如果我们希望是追加写入，可以使用 `a` 或者 `a+`
* start：写入的开始位置

### 使用

#### 创建文件的 `Writable Stream`

```js
const writer = fs.createWriteStream("./foo.txt", {
    flags: "a+",
    start: 8
});

writer.write("你好啊", err => {
    console.log("写入成功")
})
```

#### 监听open事件

```js
writer.on("open", () => {
    console.log("文件打开")
})
```

#### 关于关闭事件

并不能监听到 `close` 事件，这是因为写入流在打开后是不会自动关闭的，必须手动关闭，来告诉Node已经写入结束了，关闭后将发射一个`finish`事件

```js
writer.close(); // 手动关闭

writer.on("finish", () => {
    console.log("文件写入结束")
})
```

另外一个非常常用的方法是 end：end方法相当于做了两步操作： write传入的数据和调用close方法

```js
writer.end("Hello World")
```



## pipe

### 概述

正常情况下，可以将读取到的 输入流，手动的放到 输出流中进行写入

```js
const reader = fs.createReadStream("./foo.txt");
const writer = fs.createWriteStream("./bar.txt");

reader.on("data", (data) => {
    console.log("data");
    writer.write(data, (err) => {
        console.log(err);
    });
});
```

### 优化写法

可以通过pipe来轻松完成这样的操作

```js
reader.pipe(writer);
```

