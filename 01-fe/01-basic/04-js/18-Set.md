## Set

### 概念

Set是一种数据结构，它保存存储的对象不重复

Set是可迭代对象

### 创建Set

构造函数参数：**空**或者**可迭代对象**

注意：Set构造函数会对参数(可迭代对象)进行展开操作

```js
let s = new Set();
```

```js
let s = new Set([1,2,3,4,5]);
```

展开操作的坑

```js
let s = new Set("123456321");
console.log(s.size) // 6
console.log(s); // ["1", "2", "3", "4", "5", "6"]
```

### 操作

| 操作 | 示例            |
| ---- | --------------- |
| 增   | s.add(value)    |
| 删   | s.delete(value) |
| 含有 | s.has(value)    |
| 数量 | s.size          |
| 清空 | s.clear()       |

#### 遍历

Set中键和值是相同的

```js
s.keys() // 返回迭代器对象，通过next获取{value, done}，其中value为键
s.values() // 返回迭代器对象，通过next获取{value, done}，其中value为值
s.entries() // 返回迭代器对象，通过next获取{value, done}，其中value为数组 [键, 值]
s.forEach
for (const value of s) {}
```

#### 并集

```js
new Set([...a, ...b]);
```

#### 交集

```js
new Set(
	[...a].filter(value => b.has(value))
);
```

#### 差集

```js
new Set(
	[...a].filter(value => !b.has(value))
);
```

### 转换

#### Set转为数组

1. Array.from

   ```js
   let s = new Set([1,2,3,4,5,6]);
   let arr = Array.from(s)
   ```

2. 使用展开语法

   ```js
   let s = new Set([1,2,3,4,5,6]);
   let arr = [...s]
   ```

#### 数组转Set

1. 直接用`new Set`

   ```js
   let arr = [1,2,3,4,5,6];
   let s = new Set(arr);
   ```

#### 题目：数组去重

```js
let array = [1,2,3,4,5,1,2,3,4,5];
array = Array.from(new Set(array));
console.log(array);
```

或者

```js
let array = [1,2,3,4,5,1,2,3,4,5];
array = [...new Set(array)];
console.log(array);
```



## WeakSet

### 概念

WeakSet类似于Set，也是它保存存储的对象不重复

### 区别

WeakSet与Set区别是：WeakSet中只能存引用类型（对象，函数这些都可以）且WeakSet是弱引用存储

```js
let ws = new WeakSet("aaa"); // 报错
```

构造函数的坑

```js
let ws = new WeakSet(["aaa", "bbb"]); // 报错，因为这是构造函数，会迭代这个可迭代对象，把它的每项添加进集合中
```

```js
let ws = new WeakSet();
ws.add(["abc","bbb"]); // 不报错
```

### 重要特性：弱引用

弱引用特性：引用类型添加进WeakSet中时，引用计数不会加1，即当WeakSet使用自己的某个成员时，该成员可能已经被GC回收掉了

系统会定时检查WeakSet中的对象是否存在，如果不存在，将**自动**将元素从WeakSet中移除

#### 使用场景

由于WeakSet的弱引用特性，所以不会发生内存泄漏问题

常用场景：用来存储dom节点，当dom节点由于别的因素被删除时，不用主动去清除WeakSet中的对象，GC会自动清除

#### 注意事项

由于弱引用特性，WeakSet中的元素可能在某一时刻被GC回收掉了，因此不支持遍历相关以及数量相关的操作

* keys
* values
* entries
* for-of
* size：结果为undefined，即没有这个属性