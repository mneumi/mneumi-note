## BigInt类型介绍

`BigInt`是`JS`最新的基本数据类型，用于存储比最大安全整数值还大或比最小安全整数值还小的整数

* 最大安全值 Number.MAX_SAFE_INTEGER：2<sup>53</sup> - 1
* 最小安全值 Number.MIN_SAFE_INTEGER：- (2<sup>53</sup> - 1)



## BigInt类型创建

### 字面量创建

在数字后加符号`n`

```js
const num = 9007199254740991n;
```

### 构造函数创建

类似于Symbol，BigInt无构造函数，是一个内置对象

构造函数中可以传数值或字符串

```js
const num1 = BigInt(9007199254740991);
const num2 = BigInt("9007199254740991");
```

可以使用十六进制、八进制或二进制

```js
const num1 = BigInt("0x1fffffffffffff");
const num2 = BigInt("377777777777777777");
const num3 = BigInt("0b11111111111111111111111111111111111111111111111111111")
```

对于无法转换的值，会报错

```js
BigInt(10.2); // RangeError
BigInt(null); // TypeError
BigInt("abc"); // SyntaxError
```



## BigInt类型运算

### 支持

支持 `+`，`-`，`*`，`**`

支持`>>`，`<<`，`&`，`|`，`~`，`^`

### 不支持

不支持 `>>>`，因为 `BigInt` 都是有符号的

不支持单目`+`

不支持除法

不能使用Math对象的方法



## BigInt与Number联系

### 计算

不能和Number类型进行混合运算，需要转换为统一类型

### 转换

BigInt转为Number时有可能会丢失精度

### 比较

Number与BigInt可以进行比较

```js
1n < 2
// ↪ true

2n > 1
// ↪ true

2 > 2
// ↪ false

2n > 2
// ↪ false

2n >= 2
// ↪ true
```

与Number相等关系：严格不等，宽容相等

```js
let num1 = 1;
let num2 = 1n;

console.log(num1 == num2); // true, 宽容相等
console.log(num1 === num2); // false, 严格不等
```

### typeof

与Number类型不同，无论是使用字面量创建还是构造函数创建，typeof返回值均为`bigint`

```js
typeof BigInt("1"); // bigint
typeof 1n; // bigint
```