## Undefined 类型

### 概念

Undefined 类型是只有一个值的数据类型，即特殊的 undefined

### 创建

在声明了变量但未初始化时， 这个变量的值就是 undefined

```js
var message;
console.log(message); // undefined
console.log(message === undefined); //true 
```

### undefined和未声明是有区别的

```js
let message;

// 确保没有声明过age变量
// let age;

console.log(message); // "undefined"
console.log(age); // 报错
```

### 对于typeof

对未初始化和未声明的变量执行 typeof 操作符都返回了 undefined 值

```js
var message; // 这个变量声明之后默认取得了 undefined 值 
 
// 下面这个变量并没有声明 // var age 
 
console.log(typeof message);     // "undefined" 
console.log(typeof age);         // "undefined" 
```

### 注意事项

无论在什么情况下都没有必要把一个变量的值显式地设置为 undefined，即没必要显式使用 undefined

字面值 undefined 的主要目的是用于判断是否存在



## Null 类型

### 概念

Null 类型是只有一个值的数据类型，即特殊的 null

### 使用

只要意在保存对象的变量还没有真正保存对象，就应该明确地让该变量保存null值，这样做不仅可以体现 null 作为空对象指针的惯例，而且也有助于进一步区分 null 和 undefined

### 注意

对于 typeof，返回的不是 **null**，而是**object**，这是一个Bug，且已经无法修复了

```js
console.log(typeof null); // "object"
```

### 总结

大多数情况下，我们都应该用null，undefined仅仅在判断函数参数是否传递的情况下有用
