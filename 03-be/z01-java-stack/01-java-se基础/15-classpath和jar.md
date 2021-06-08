## classpath

### classpath是什么

`classpath`是一个环境变量，类似于`JAVA_HOME`、`PATH`等

`classpath`环境变量的值是一组存放`.class`文件的目录的集合，它用来指示`JVM`如何搜索`.class`文件

### 设置方法

| 设置方法                              | 是否推荐                       |
| ------------------------------------- | ------------------------------ |
| 系统环境变量中设置`classpath`环境变量 | 不推荐，因为这样会污染全局环境 |
| 启动JVM时设置`classpath`变量          | 推荐                           |

设置示例

```shell
java -classpath .:/home/mneumi/libs Main
# 使用 -cp 简写
java -cp .:/home/mneumi/libs Main
```

### 搜索规则

`JVM`根据`classpath`从前往后进行搜索，一旦找到就停止搜索；如果所有路径下都没有找到，就报错

默认值：如果没有设置系统环境变量，也没有传入`-cp`参数，那么JVM默认的`classpath`为`.`，即当前目录

### 最佳实践

* 不要把任何Java核心库添加到classpath中！JVM根本不依赖classpath加载核心库
* 绝大多数情况下，不要设置`classpath`！默认的当前目录`.`对于绝大多数情况都够用了



## jar包

### jar包是什么

`jar`包可以理解为**一个压缩包，里面含有多个.class文件**

`jar`包本质上是一个`zip`压缩包

### jar包作用

如果有很多`.class`文件，散落在各层目录中，肯定不便于管理，如果能把目录打一个包，变成一个文件，就方便多了

`jar`包可以把`package`组织的目录层级，以及各个目录下的所有文件（包括`.class`文件和其他文件）都打成一个jar文件，方便备份，发送和上线

### 使用jar包

如果要执行一个`jar`包中的`class`，可以把jar包放到`classpath`中，这样JVM会自动在`jar`包中去搜索某个类

```shell
java -cp ./other.jar Main
```

### 创建jar包

#### 方式1：手动压缩

因为jar包就是zip包，所以可以先直接压缩为`zip`文件，再把后缀从`.zip`改为`.jar`，一个`jar`包就创建完成

#### 方式2：使用包管理工具，如Maven

使用`Maven`可以方便快捷地创建`jar`包，具体看 **Maven** 相关资料

### MANIFEST.MF文件

`jar`包可以包含一个特殊的`/META-INF/MANIFEST.MF`文件，`MANIFEST.MF`是纯文本文件，指定`Main-Class`和其它信息

* JVM会自动读取这个`MANIFEST.MF`文件，如果存在`Main-Class`，就不必在命令行指定启动的类名

* jar包还可以包含其它jar包，这个时候，就需要在`MANIFEST.MF`文件里配置`classpath`了