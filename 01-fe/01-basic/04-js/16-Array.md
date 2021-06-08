## Array类型介绍

### 数组是什么

数组是一种数据结构，能够顺序存储对象

### 特点

与其他语言不同的是，ECMAScript 数组的每一项可以保存任何类型的数据

数组多可以包含2<sup>32</sup>个项，这几乎已经能够满足任何编程需求了



## Array创建

创建数组的基本方式有两种

### 字面量

使用数组字面量表示法时，不会调用 Array 构造函数

```js
let arr = ["red", "blue", "green"];
```

### 构造函数

```js
let arr = new Array(); // 空数组
let arr = new Array("red", "blue", "green"); // 创建数组，并赋初始值
```

#### 注意点（坑）

如果参数只有一个，且为数值时，会创建一个长度为数值的数组，且值都为undefined

```js
let arr = new Array(3); // 指定数组长度为3，值都为 undefined，即 [undefined, undefined, undefined]
```

### Array.of

Array.of与Array构造函数功能完全一样，唯一的区别就是避免参数只有一个数值时的坑

```js
let arr = Array.of(3); // [3]
```

### Array.from

参数需要是伪数组对象或可迭代对象

```js
let str = "123456";
let arr = Array.from(str); // [1,2,3,4,5,6]
```



## 检测Array

无法使用 `typeof `来检测对象是否是数组，因为就算是数组，typeof的返回值也是`object`

```js
let arr = [];
console.log(typeof arr); // object
```

方法一：instanceof

```js
let arr = [];
console.log(arr instanceof Array); // true
```

方法二：Arrya.isArray，最好的方法

```js
var arr = [];
console.log(Array.isArray(arr); // true
```



## length属性

数组的length属性很有特点：它不是只读的，可以设置该属性

### 实现清空数组的效果

令 length 为 0 可以清空数组

```js
let arr = [1,2,3];
console.log(arr[1]); // 2

arr.length = 0;

console.log(arr[1]); // undefined
```

### 实现添加元素的效果

从数组的末尾移除项或向数组中添加新项， 空的位置 undefined 填充

```js
var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组 
colors.length = 4; 
alert(colors[3]); //undefined

var colors = ["red", "blue", "green"];    // 创建一个包含 3 个字符串的数组 
colors[colors.length] = "black";          //（在位置 3）添加一种颜色 
colors[colors.length] = "brown";          //（在位置 4）再添加一种颜色  

var colors = ["red", "blue", "green"];     // 创建一个包含 3 个字符串的数组 
colors[99] = "black";                    // （在位置 99）添加一种颜色 
alert(colors.length); // 100

// 向colors数组的位置99插入了一个值，数组新长度就是100 (99+1)
// 而位置3到位置98实际上都是不存在的，所以访问它们都将返回 undefined
```



## 修改Array元素方法 [本身改变]

### 栈方法和队列方法

| 方法                           | 功能         |
| ------------------------------ | ------------ |
| Array.prototype.push(value)    | 数组尾加元素 |
| Array.prototype.pop()          | 数组尾弹元素 |
| Array.prototype.unshift(value) | 数组头加元素 |
| Array.prototype.shift()        | 数组头弹元素 |

### splice

作用：修改数组本身

语法：返回值为空数组

```js
splice(开始下标，[删除元素个数]，[...要插入的元素]) // 返回值始终为一个空数组
```

示例

```js
let arr = [0,1,2,3,4,5];
arr.splice(0,2); // [2,3,4,5]
arr.splice(1,0,666); // [2,666,4,5]
arr.splice(0,arr.length,9,8,7,6,5,4,3,2,1); // [9,8,7,6,5,4,3,2,1]
```

### 使用length属性

```js
const arr = [1,2,3];
arr[arr.length] = 100; // [1,2,3,100]
arr.length = 0; // 数组清空
```



## 替换Array元素方法 [本身改变]

### fill

作用：使用元素重复填充数组

语法：

* [startIndex, endIndex) 前闭后开区间

* startIndex默认为0，endIndex默认为数组长度

```js
Array.prototype.fill(元素, [startIndex], [endIndex])
```

示例

```js
let arr = [0,1,2,3,4,5,6,7,8];

arr.fill(666, 3, 7);

console.log(arr); // [0,1,2,666,666,666,666,7,8]
```

### copyWithin

作用：从数组的指定位置拷贝元素到数组的另一个指定位置中

语法：

* [startIndex, endIndex) 前闭后开区间
* startIndex默认为0，endIndex默认为数组长度

```js
Array.prototype.copyWithin(targetIndex, [startIndex], [endIndex])
```

示例

```js
let arr1 = ["apple", "orange", "mongo", "cherry", "banana"];
arr1.copyWithin(0, 2, 3); // ["mongo", "orange", "mongo", "cherry", "banana"]

let arr2 = ["apple", "orange", "mongo", "cherry", "banana"];
arr2.copyWithin(0, 1, 4); // ["orange", "mongo", "cherry", "cherry", "banana"]
```



## Array排序和倒置方法 [本身改变]

| 方法                             | 说明                        |
| -------------------------------- | --------------------------- |
| Array.prototype.sort()           | 按照valueOf的值从小到大排序 |
| Array.prototype.sort(自定义函数) | 自定义排序方法              |
| Array.prototype.reverse()        | 数组倒置                    |

```js
function compara(left, right) {
    if (left < right) { return -1; }
    else if (left === right) { return 0; }
    else { return 1; }
}
```



## Array搜索方法 [本身没有改变]

| 方法                                           | 功能                                                        | 特点                              |
| ---------------------------------------------- | ----------------------------------------------------------- | --------------------------------- |
| Array.prototype.indexOf(value)                 | 自开头从左向右搜索value对应的下标，找不到就返回-1           | 无法进行引用类型的查找            |
| Array.prototype.indexOf(value, startIndex)     | 自startIndex下标从左向右搜索value对应的下标，找不到就返回-1 | 无法进行引用类型的查找            |
| Array.prototype.lastIndexOf(value)             | 自最后从右向左搜索value对应的下标，找不到就返回-1           | 无法进行引用类型的查找            |
| Array.prototype.lastIndexOf(value, startIndex) | 自startIndex下标从右向左搜索value对应的下标，找不到就返回-1 | 无法进行引用类型的查找            |
| Array.prototype.includes(value)                | 查找数组是否含有value，有则返回true，否则返回false          | 无法进行引用类型的查找，可包括NaN |
| Array.prototype.find(item => {})               | 传入一个函数，返回数组中符合条件的第一个元素                | 能够进行引用类型的查找            |
| Array.prototype.findIndex(item => {})          | 传入一个函数，返回数组中符合条件的第一个元素的索引          | 能够进行引用类型的查找            |

注意：includes方法可以查找NaN值

```js
let nan = 0 / 0;
let arr = [1,2,3,nan,4,5,6];

console.log(arr.includes(NaN)); // true
```



## Array合并方法 [本身没有改变]

### concat

用于连接两个或多个数组，该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本

```js
Array.prototype.concat(数组2, [数组3], [数组4], [数组5], ...)
```

示例

```js
let arr = [1,2];
let arr2 = [3,4,5];
let arr3 = [6,7,8,9,10];

let arr4 = arr.concat(arr2, arr3);
console.log(arr4); // [1,2,3,4,5,6,7,8,9]
```

### 使用解构赋值

```js
let a = ["zxc", "abc"];
let b = [1,2,3,4,5];
let c = [true, false];
let arr = [...a, ...b, ...c]; // ["zxc","abc",1,2,3,4,5,true,false]
```



## Array切片方法 [本身没有改变]

### slice 

语法：

* 前闭后开区间，[startIndex, endIndex)
* startIndex默认为0，endIndex默认为数组长度

```js
Array.prototype.slice([startIndex], [endIndex])
```

类似于`Go`的`Slice[startIndex:endIndex]`

示例

```js
let arr = [0,1,2,3,4,5,6,7,8,9];

let a = arr.slice(); // [0,1,2,3,4,5,6,7,8,9]
let b = arr.slice(4); // [4,5,6,7,8,9]
let c = arr.slice(5,7); // [5,6]
```



## Array转换方法 [本身没有改变]

### 数组转字符串

| 方法                             | 功能                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| Array.prototype.toString()       | 返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串，为了创建这个字符串会调用数组每一项的 toString() 方法 |
| Array.prototype.toLocaleString() | 返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串，为了创建这个字符串会调用数组每一项的 toString() 方法 |
| join("给定字符串")               | 返回由数组中每一个值之间用给定字符串拼接而成的字符串         |

#### 将可迭代对象，类数组转数组

语法

* arrayLike：想要转换成数组的伪数组对象或可迭代对象
* mapFn：如果指定了该参数，新数组中的每个元素会执行该回调函数
* thisArg：this指向

```js
Array.from(arrayLike[, mapFn[, thisArg]])
```

示例

```js
const str = "123456";
console.log(Array.from(str));
```



## Array清空方法 [本身改变]

1. arr.pop + 循环

   ```js
   let arr = [1,2,3];
   
   while (arr.length) {
       arr.pop();
   }
   ```

2. arr.length = 0

   ```js
   let arr = [1,2,3];
   arr.length = 0;
   ```

3. arr.splice(0, arr.length)

   ```js
   let arr = [1,2,3];
   arr.splice(0, arr.length);
   ```

4. arr = []

   ```js
   let arr = [1,2,3];
   arr = [];
   ```



## Array降维方法 [本身没有改变]

### flat

功能：能够让数组降低n个维度

语法

```js
Array.prototype.flat(n); // n表示降低深度，n默认值为1
```

示例

```js
let arr1 = [1,2,3,4,[5,6]];
let arr2 = arr1.flat(1); // [1,2,3,4,5,6]

let arr3 = [1,2,3,4,[5,6,[7,8,9]]];
let arr4 = arr3.flat(1); // [1,2,3,4,5,6,[7,8,9]]
let arr5 = arr3.flat(2); // [1,2,3,4,5,6,7,8,9]
```

### flatMap

功能：是 `flat`和`map`的结合，即先调用map方法，然后再进行 flat

注意：flat的深度为1，且不能设置

语法

```js
Array.prototype.flatMap((value,key,arr)=>{}, thisArg)
```

示例

```js
let arr = [1,2,3];

let result = arr.flatMap(item => {
    return [item,[item]];
})

console.log(result);
// reuslt结果为
// [1,[1],2,[2],3,[3]]
```



## Array迭代方法 [本身没有改变]

### 前置知识

#### forEach，map，filter，every，some

回调函数格式都为

```js
function (当前值, [当前值下标], [数组本身])
```

forEach，map，filter，every，some的格式都为

```js
(回调函数, [thisArg])
```

::: warning 注意

如果省略了 thisValue，"this"的值为 "undefined"

:::

::: danger 警告

forEach中的函数是**并行异步执行的，并不是按顺序执行的**，所以不要在`forEach`中使用`await`

```js
var arr = [1,2,3]

arr.forEach(async (v,i,a)=>{

　　await Promise()

})
```

:::

#### reduce，reduceRight

回调函数格式都为

```js
function (累加值，当前值, [下标], [数组本身])
```

reduce，reduceRight的格式都为

```js
(回调函数, [累加值初始值])
```

::: warning 注意

注意：如果省略掉累加值初始值，则累加值初始值为第一个元素，且从第二个元素开始遍历

:::

### forEach

作用：遍历数组

回调函数返回值：无

最终返回值：无

语法

```js
Array.prototype.forEach((当前值, [当前值下标], [数组本身])=>{
    
}, thisArg)
```

示例

```js
let arr1 = ["apple", "orange", "mongo", "cherry", "banana"];

let arr2 = arr.forEach((item) => {
    console.log(item);
    return item;
});

console.log(arr2); // undefined
```

### map

作用：遍历数组

回调函数返回值：元素

最终返回值：返回每次函数调用的结果组成的数组

区别：与forEach区别是map返回新数组，而forEach无返回值

语法

```js
Array.prototype.map((当前值, [当前值下标], [数组本身])=>{
    return [新数组元素]
}, thisArg)
```

示例

```js
let arr = [1,3,5,7,9];

let arr2 = arr.map((value,index) => {
	return value + index;    
})

console.log(arr2); // [1,4,7,10,13]
```

### filter

作用：遍历数组，过滤元素

回调函数返回值：布尔值

最终返回值：返回使回调函数为true的元素组成的新数组

语法

```js
Array.prototype.filter((当前值, [当前值下标], [数组本身])=>{
    return true/false;
}, thisArg)
```

示例

```js
let arr1 = [1,2,3,4,5,6,7,8,9];

let arr2 = arr1.filter((value)=>{
    return value % 2 === 0;
})

console.log(arr2); // [2,4,6,8]
```

### every

作用：遍历数组，判断元素是否都满足某个条件

回调函数返回值：布尔值

最终返回值：布尔值；当数组中所有元素都满足回调函数时，返回true；一旦不满足，则中止遍历(短路操作)，直接返回false

语法

```js
Array.prototype.every((当前值, [当前值下标], [数组本身])=>{
    return true/false;
}, thisArg)
```

示例

```js
let arr1 = [1,2,3,4,5];
let arr2 = [111,2,3,4,5];

function fn(value) {
    return value <= 5);
}

let a = arr1.every(fn); // true
let b = arr2.every(fn); // false
```

### some

作用：遍历数组，判断是否某个元素满足条件

回调函数返回值：布尔值

最终返回值：布尔值；当数组中某个元素都满足回调函数时，中止遍历(短路操作)，直接返回true；只有当全部不满足时返回false

语法

```js
Array.prototype.some((当前值, [当前值下标], [数组本身])=>{
    return true/false;
}, thisArg)
```

示例

```js
let arr1 = [1,2,3,4,5];
let arr2 = [111,2,3,4,5];

function fn(value) {
    return value > 5;
}

let a = arr1.some(fn); // false
let b = arr1.some(fn); // true
```

### reduce

作用：遍历数组，使用累加值

回调函数返回值：新的累加值

最终返回值：最终累加值

语法

```js
Array.prototype.reduce((累加值, [当前值], [当前值下标], [数组本身])=>{
    return 新的累加值;
}, [initValue])
```

注意：如果没有 `initValue`，则累加值为第一个元素，且从第二个元素开始遍历

示例

```js
let arr = [1,2,3,4,5,6,7,8,9];

let sum = arr.reduce((total,value)=>{
    return total + value
});

console.log(sum); // 45
```

### reduceRight

作用：从右向左遍历数组，使用累加值

回调函数返回值：新的累加值

最终返回值：最终累加值

语法

```js
Array.prototype.reduce((累加值, [当前值], [当前值下标], [数组本身])=>{
    return 新的累加值;
}, [initValue])
```

注意：如果没有 `initValue`，则累加值为最后一个元素，且从倒数第二个元素开始遍历

示例

```js
let arr = [1,2,3,4,5,6,7,8,9];

let v = arr.reduceRight((accumulate, value) => {
    return accumulate - value;
});

console.log(v); // -27
```