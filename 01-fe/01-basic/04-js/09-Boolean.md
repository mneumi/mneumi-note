## Boolean类型介绍

Boolean类型只有两个值：true和false



## 创建Boolean类型对象

### 使用字面量

使用字面量创建Boolean类型对象时，不会调用构造函数

```js
let a = true;
let b = false;
```

### 使用构造函数

```js
let a = new Boolean(true);
let b = new Boolean(false);
```



## Boolean类型转换

### 隐式转换

当其他类型用于条件表达式时，系统会将其自动转为Boolean类型

规则如下：除了 `0`，`""`，`false`，`null`，`undefined` , `NaN`六个值为 `false`外，其他皆为`true`

注意：`[]`为true

### 显式转换

#### 方法1：使用构造函数Boolean()

```js
let a = "Hello World";
let b = null;

let aa = new Boolean(a);
let bb = new Boolean(b);

console.log(aa); // true;
console.log(bb); // false
```

#### 方法2：使用两次"非"运算符

```js
let a = "Hello World";
let b = null;

console.log(!!a); // true;
console.log(!!b); // false
```