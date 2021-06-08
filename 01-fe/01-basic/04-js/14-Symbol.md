## Symbol类型介绍

Symbol类型是基础数据类型

Symbol函数产生的值是唯一的，即能够进行唯一标识

Symbol值可以当作特殊的字符串，配合计算属性使用



## 声明Symbol对象

### 注意

Symbol不是构造函数，没有constructor，而是普通的函数，所以不能用new关键字

### Symbol函数

```js
let s1 = Symbol();
let s2 = Symbol("标识信息");
```

### Symbol.for函数

```js
let s3 = Symbol.for("标识信息");
```

### 区别

Symbol.for是在全局注册的，而Symbol不是在全局注册的



## 获取Symbol对象

使用`标识信息`来获取已经声明了的Symbol对象

```js
let s1 = Symbol("abc");
let s2 = Symbol("abc");

console.log(s1 === s2); // true
```



## 获取标识信息

### 方法1：通过属性

```js
let s = Symbol("这是标识信息");
console.log(s.description); // 这里是描述
```

### 方法2：通过方法keyFor

只能获取由`Symbol.for`创建的Symbol对象的信息

```js
let s = Symbol.for("这是标识信息");
console.log(Symbol,keyFor(cms));
```

注意：通过Symbol()创建的Symbol的描述无法通过keyFor获取，这是因为keyFor只能获取全局注册的Symbol，而Symbol()创建的Symbol不是在全局注册的



## Symbol对象特性

### 无法给Symbol对象绑定属性

```js
let s = Symbol();
s.name = "name";
console.log(s.name); // undefinded
```

### 无法直接访问到对象中的Symbol属性

利用该特性，能够对对象的属性进行保护

```js
let s = Symbol("这是一个Symbol类型");
let obj = {
    name: "哈哈",
    [s]: "111"
}

console.log(s); // 找不到[s]属性
```



## 遍历Symbol属性

由于Symbol属性是受保护的，所以一般的`for-in`，`for-of`以及`Object.keys`，`Object.entries`是获取不到的，需要使用特定的函数

### Object.getOwnPropertySymbols

```js
let s = Symbol("这是描述信息");

let obj = {
    name: "obj",
    [s]: "Symbol属性"
}

console.log(Object.getOwnPropertySymbols());
```

### Reflect.ownKeys

```js
let s = Symbol("这是描述信息");

let obj = {
    name: "obj",
    [s]: "Symbol属性"
}

console.log(Reflect.ownKeys(obj));
```



## Symbol使用场景

### 解决字符串重名问题

```js
let user1 = "李四";
let user2 = "王五";
let user3 = "李四"; // 重名

let grade = { // 存在重名问题
    [user1]: 88,
    [user2]: 99,
    [user3]: 100
}
```

解决

```js
let user1 = {
    name: "李四",
    key: Symbol()
};
let user2 = {
    name: "李四",
    key: Symbol()
};
let user3 = {
    name: "李四",
    key: Symbol()
};

let grade = { // 存在重名问题
    [user1.key]: 88,
    [user2.key]: 99,
    [user3.key]: 100
}
```

### 进行属性保护

```js
let s = Symbol("这是一个Symbol类型");
let obj = {
    name: "哈哈",
    [s]: "111"
}

console.log(s); // 找不到[s]属性
```



## 常用内置符号

### 介绍

ES6引入了一批常用内置符号，用于暴露语言内部行为，最常见用途就是冲洗定义它们，从而改变原生结构的行为

这些内置符号都是以 Symbol 工厂函数字符串属性的形式存在

### 特点

所有的内置符号的属性都是不可写，不可枚举，不可配置的

### Symbol.toStringTag

该符号作为一个属性表示：**”一个字符串，该字符串用于创建对象的默认字符串描述“**

```js
const s = new Set();

console.log(s); // Set(0) {}
console.log(s.toString()); // [object Set]
console.log(s[Symbol.toStringTag]); // Set

//////////////////////////////////////////

class Foo {}
const foo = new Foo();

console.log(foo); // Foo {}
console.log(foo.toString()); // [object Object]
console.log(foo[Symbol.toStringTag]); // undefined

//////////////////////////////////////////

class Bar {
    constructor() {
        this[Symbol.toStringTag] = "Bar";
    }
}
const bar = new Bar();

console.log(bar); // Bar {}
console.log(bar.toString()); // [object Bar]
console.log(bar[Symbol.toStringTag]); // Bar
```

