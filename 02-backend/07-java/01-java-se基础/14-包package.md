## 初识包package

### 概述

Java定义了一种名字空间，称之为**包(package)**

Java中，一个类总是属于某个包，类名只是一个简写，真正的完整类名是**包名.类名**

在Java虚拟机执行的时候，JVM只看完整类名，因此只要包名不同类就不同

### 作用

在Java中，**包package**的作用是**避免命名冲突**

### 写法

在编写**Java**源文件，定义`class`的时候，需要在第一行使用**package**关键字声明这个`class`属于哪个包

```java
package 包名;
```

包可以是多层结构，用`.`隔开，例如：`java.util`

```java
package xxx.yyy.zzz;
```

**包没有父子关系**：`java.util` 和 `java.util.zip` 是不同的包，两者没有任何继承关系

### 默认包

没有定义包名的`class`，使用的是**默认包**

默认包非常容易引起名字冲突，因此不推荐不写包名的做法



## 包package目录结构

### 概述

Java源文件的**存放目录结构**必须能够对应上**包名层级关系**

编译后的`.class`文件**存放目录结构**也需要能够对应上**包名层级关系**

### 案例

#### 包名层级关系

小明的`Person`类存放在包`ming`下面，因此完整类名是`ming.Person`

小红的`Person`类存放在包`hong`下面，因此完整类名是`hong.Person`

小军的`Arrays`类存放在包`mr.jun`下面，因此完整类名是`mr.jun.Arrays`

#### 存放目录

假设以`package_sample`作为根目录，`src`作为源码目录，那么所有文件结构如下

```
package_sample
└─ src
    ├─ hong
    │  └─ Person.java
    │  ming
    │  └─ Person.java
    └─ mr
       └─ jun
          └─ Arrays.java
```

编译后的`.class`文件假设存放在`bin`目录下，那么目录结构如下

```
package_sample
└─ bin
   ├─ hong
   │  └─ Person.class
   │  ming
   │  └─ Person.class
   └─ mr
      └─ jun
         └─ Arrays.class
```



## 包package作用域

### 概述

不用`public`、`protected`、`private`修饰的字段和方法就是包作用域

### 作用

一个类的包作用域下的方法可以访问同一个包作用域下的其他字段和方法

### 例子

假设`Person`类定义在`hello`包下面

```java
package hello;

public class Person {
    // 包作用域:
    void sayHello() {
        System.out.println("Hello!");
    }
}
```

`Main`类也定义在`hello`包下面，则`Main`中可以直接使用`Person`类中`sayHello`方法

```java
package hello;

public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        p.sayHello(); // 可以调用，因为Main和Person在同一个包
    }
}
```

### 作用域总结

| 作用域                             | 实现                | 说明                 |
| ---------------------------------- | ------------------- | -------------------- |
| public作用域                       | 使用 public 修饰    | 任何场合都可以调用   |
| protected作用域                    | 使用 protected 修饰 | 本类以及子类能够调用 |
| private作用域                      | 使用 private 修饰   | 本类可以调用         |
| package作用域（也叫default作用域） | 不使用权限修饰符    | 本包中可以调用       |



## import语句

### 概述

**import语句**的作用是简化包名

### 使用其他类的四种方式

#### 方式1：使用完整类名

直接使用完整类名，不使用导入语句

结论：每次写完整类名比较痛苦，且不好维护**（不推荐，不主流）**

```java
// Person.java
package ming;

public class Person {
    public void run() {
        mr.jun.Arrays arrays = new mr.jun.Arrays();
    }
}
```

#### 方式2：使用import语句，导入特定类

先使用import语句，导入想要的`class`，然后再用

结论：比较方便，且较好维护**（推荐，主流）**

```java
// Person.java
package ming;

// 导入完整类名:
import mr.jun.Arrays;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}
```

#### 方式3：使用import语句，导入全部类

在写`import`的时候，可以使用`*`，表示把这个包下面的所有`class`都导入进来（但不包括子包的`class`）

结论：一般不推荐这种写法，因为在导入了多个包后，很难看出使用的类属于哪个包**（不太推荐，不太主流）**

```java
// Person.java
package ming;

// 导入mr.jun包的所有class:
import mr.jun.*;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}
```

#### 方式4：使用 import static语句，导入静态字段和静态方法

`import static`语句可以导入一个类的静态字段和静态方法

结论：`import static`很少使用**（不推荐，不主流）**

```java
package main;

// 导入System类的所有静态字段和静态方法:
import static java.lang.System.*;

public class Main {
    public static void main(String[] args) {
        // 相当于调用System.out.println(…)
        out.println("Hello, world!");
    }
}
```

### 默认导入

* Java编译器也会默认自动`import`当前`package`的其他`class`，所以需要手动`import`同包的其他类

* Java编译器，会默认使用以下导入语句

```java
import java.lang.*; // 这是为啥能够直接使用 String 等类的原因
```

### 匹配顺序

#### 匹配概述

Java编译器最终编译出的`.class`文件只使用**完整类名**

因此在代码中，当编译器遇到一个`class`名称时，会按以下规则进行匹配

* 如果是完整类名，就直接根据完整类名查找这个`class`
* 如果是简单类名，按下面的顺序依次查找：
  * 查找当前`package`是否存在这个`class`
  * 查找`import`的包是否包含这个`class`
  * 查找`java.lang`包是否包含这个`class`
* 如果按照上面的规则还无法确定类名，则编译报错

#### 匹配例子

```java
// Main.java
package test;

import java.text.Format;

public class Main {
    public static void main(String[] args) {
        java.util.List list; // ok，使用完整类名 -> java.util.List
        Format format = null; // ok，使用import的类 -> java.text.Format
        String s = "hi"; // ok，使用java.lang包的String -> java.lang.String
        System.out.println(s); // ok，使用java.lang包的System -> java.lang.System
        MessageFormat mf = null; // 编译错误：无法找到MessageFormat: MessageFormat cannot be resolved to a type
    }
}
```

### 冲突问题

如果有两个`class`名称相同，例如，`mr.jun.Arrays`和`java.util.Arrays`，那么只能`import`其中一个，另一个必须写完整类名。



## 最佳实践

### 保证包名唯一

为了避免名字冲突，需要确定唯一的包名

常用的、推荐的做法是使用倒置的域名来确保唯一性，例如

```java
// 域名倒置，保证唯一
package org.apache;
package org.apache.commons.log;
```

### 不要和JDK的包重名

注意不要和JDK常用包重名，即自己的包不要使用下列这些名字

```java
// 不要使用类似下列包名
package java.util;
package java.text;
package java.math;
```

### 不要和lang包中的类重名

注意不要和`java.lang`包的类重名，即自己的类不要使用下列这些名字

```java
x // 不要使用类似下列类名
class String {}
class System {}
class Runtime {}
```