## Map

### 概念

Map是一种数据结构，实现键值对存储

#### 与普通对象区别

普通对象的键只能是字符串

```js
let obj = {
    666: "数字",
    "666": "字符串"
}
console.log(Object.keys(obj).length); // 1
```

而Map的键可以是任意类型（对象，函数也可以）

```js
function a(){}
let m = new Map();
m.set(a, 123);
```

### 创建Map

#### 空参数

```js
new Map();
```

#### 非空参数

接收一个数组，数组中每一项也为数组，子数组索引0元素为键，索引1元素为值

```js
new Map([['name',"姓名"],['age',18]]);
```

### 操作 

| Map操作 | 示例              |
| ------- | ----------------- |
| 增/改   | m.set(key, value) |
| 删      | m.delete(key)     |
| 查      | m.get(key)        |
| 含有    | m.has(key)        |
| 清空    | m.clear()         |

#### 遍历

```js
m.keys() // 返回迭代器对象，通过next获取{value, done}，其中value为键
m.values() // 返回迭代器对象，通过next获取{value, done}，其中value为值
m.entries() // 返回迭代器对象，通过next获取{value, done}，其中value为数组 [键，值]
m.forEach()
for (const value of m) {}
```

### 转换

#### Map ==> Array

展开语法

```js
let m = new Map([['name',"姓名"],['age',18]]);
let arr = [...m];
let arr = [...m.keys()];
let arr = [...m.values()];
let arr = [...m.entries()]; // 等价于 [...m]
```

#### Array ==> Map

使用构造函数

```js
new Map([['name',"姓名"],['age',18]]);
```



## WeakMap

### 概念

WeakMap类似于Map，是一种键值对的数据结构

### 区别

WeakMap与Map区别是：WeakMap中只能存引用类型（对象，函数这些都可以）且WeakMap是弱引用存储

```js
let ws = new WeakSet("aaa"); // 报错
```

### 重要特性：弱引用

弱引用特性：引用类型添加进WeakMap中时，引用计数不会加1，即当WeakMap使用自己的某个成员时，该成员可能已经被GC回收掉了

系统会定时检查WeakMap中的对象是否存在，如果不存在，将**自动**将元素从WeakMap中移除

#### 使用场景

由于WeakMap的弱引用特性，所以不会发生内存泄漏问题

常用场景：用来存储dom节点，当dom节点由于别的因素被删除时，不用主动去清除WeakMap中的对象，GC会自动清除

#### 注意事项

由于弱引用特性，WeakMap中的元素可能在某一时刻被GC回收掉了，因此不支持遍历相关以及数量相关的操作

* keys
* values
* entries
* for-of
* size：结果为undefined，即没有这个属性