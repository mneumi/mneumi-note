## Date类型介绍

Date对象用于操作日期和时间



## 创建Date对象

使用Date构造函数创建Date对象

### 无参构造函数

在调用 Date 构造函数而不传递参数的情况下，新创建的对象自动获得当前日期和时间

```js
const nowDate = new Date();
```

### 含参构造函数

#### 字符串参数

```js
const d = new Date("2020-01-02 12:30:00"); // 2020年1月2号 12点30分0秒
```

#### 数字参数

注意月份是从0开始算的

```js
const d = new Date(2020, 1, 2, 12, 30, 00); // 2020年2月2号 12点30分0秒
```

#### 时间戳参数

```js
const timestamp = Date.now();
const d = new Date(timestamp);
```

### 注意

构造函数new时，返回的是Date对象

```js
let d = new Date();

console.log(d instanceof String); // false
console.log(d instanceof Date); // true
```

构造函数不new时，返回的是字符串

```js
let d = Date();

console.log(d instanceof String); // true
console.log(d instanceof Date); // false
```



## 获取时间戳

### Date.now

获取当前时间的时间戳(毫秒)

```js
Date.now();
```

### Date.parse

由日期时间字符串获取时间戳(毫秒)

```js
Date.parse("2020-01-02 12:30:00");
```

### Date.UTC

日期时间数字 ==> 时间戳(毫秒)

注意：月份从0开始计算

```js
Date.UTC(2019, 9, 30, 18, 10, 22);
```



## Date实例对象常用方法

| 方法            | 功能                       |
| --------------- | -------------------------- |
| d.getFullYear() | 年                         |
| d.getMonth()    | 月，**1月是0**（**大坑**） |
| d.getDate()     | 日                         |
| d.getHours()    | 时                         |
| d.getMinutes()  | 分                         |
| d.getSeconds()  | 秒                         |
| d.getDay()      | 星期，**星期天是0**        |
| d.getTime()     | 时间戳                     |




## 相互转换

### Date对象 ==> 时间戳

1. `*1`法(参与数字运算)

    ```js
    const d = new Date();
    console.log(d * 1)
    ```

2. 使用Number构造函数

   ```js
   const date = new Date();
   let timestamp = new Number(date); 
   ```

3. 使用valueOf

   ```js
   const date = new Date();
   let timestamp = date.valueOf(); 
   ```

4. 使用getTime

   ```js
   const date = new Date();
   let timestamp = date.getTime(); 
   ```
   
5. 使用Date.now

    返回当前时间毫秒数

    ```js
    let timestamp = Date.now();
    ```

### 时间戳 ==> Date对象

使用构造函数

```js
let timestamp = (new Date()).getTime();
console.log(timestamp);

let date = new Date(timestamp);
console.log(date);
```

