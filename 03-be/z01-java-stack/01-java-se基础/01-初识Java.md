## 初识Java

### 概述

Java 是由 `Sun` 公司于1995年5月推出的编程语言和虚拟机平台的总称，后来 `Sun` 公司被 `Oracle` 公司收购，Java 也成为 `Oracle` 公司的产品

Java 之父是 **詹姆斯·高斯林 (James_Gosling)**

### 体系

#### Java SE

Java Platform Standard Edition，java平台标准版

标准版，Java SE包含标准的JVM和标准库，它是整个Java平台的核心

#### Java EE

Java Platform Enterprise Edition，java平台企业版

企业版，Java EE在Java SE的基础上加上了大量的API和库，以便方便开发Web应用、数据库、消息服务等

Java EE的应用使用的虚拟机和Java SE完全相同

Java EE是进一步学习Web应用所必须的，Spring等框架都是Java EE开源生态系统的一部分

#### Java ME

Java Platform Micro Edition，java平台微型版

微型版，Java ME是一个针对嵌入式设备的**瘦身版**，Java SE的标准库无法在Java ME上使用，Java ME的虚拟机也是**瘦身版**

Java ME并不流行，现在Android开发成为了移动平台的标准之一，没有特殊需求，不建议学习和使用Java ME

#### 关系图

![](./images/image-20210323162530876.png)

### 特点

* 跨平台
* 高性能
* 介于编译型语言和解释型语言之间



## JDK与JRE

### JRE

Java Runtime Environment，Java运行时环境

JRE就是运行Java字节码的虚拟机

### JDK

Java Development Kit，Java开发工具集

JDK除了包含JRE，还提供了编译器、调试器等开发工具，可以将Java源码编译成Java字节码

### 关系图

![](./images/image-20210323163004474.png)



## 配置JDK

### 下载地址

https://www.oracle.com/java/technologies/javase-downloads.html

### 安装

直接解压，或者执行安装程序即可

### 设置环境变量

| 环境变量  | 说明                                          |
| --------- | --------------------------------------------- |
| JAVA_HOME | 将JAVA_HOME设置为JDK根路径                    |
| PATH      | 将JAVA_HOME/bin添加到PATH中，方便调用开发工具 |

### 测试JDK是否配置成功

输入下列命令，如果看到版本号信息，则说明JDK配置成功

```shell
java --version
```

输出版本信息结果类似

```
java 15.0.2 2021-01-19
Java(TM) SE Runtime Environment (build 15.0.2+7-27)
Java HotSpot(TM) 64-Bit Server VM (build 15.0.2+7-27, mixed mode, sharing)
```

### bin工具

| bin工具 | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| java    | 用于执行Java字节码文件，本质是JVM执行字节码文件              |
| javac   | 用于将Java源文件编译为Java字节码文件                         |
| jar     | 用于打包Java字节码文件，类似于压缩包，常用于备份、传输和发布 |
| jdb     | Java调试器                                                   |
| javadoc | 用于从Java源码中自动提取注释并生成文档                       |



## 第一个Java程序

### 编写源文件

Java源文件后缀名为`.java`，假设文件名为`Hello.java`

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

### 编译和执行

#### 流程

Java源文件本质上是一个文本文件

需要先用`javac`把`Hello.java`编译成字节码文件`Hello.class`

```shell
javac Hello.java
```

然后用`java`命令执行这个字节码文件

```shell
java Hello # 注意不用加后缀名 .class，加了反而会报错
```

#### 示意图

![](./images/image-20210323163904299.png)



## Java语言类型

### 概述

Java某种意义上说，既是编译型语言，也是解释型语言

### 说明

Java的源文件(.java)需要经过**编译**，变为字节码文件(.class)

字节码文件(.class)需要虚拟机JVM**解释**执行