## JSON介绍

### JSON是什么

JSON全称：JavaScript Object Notation，JavaScript对象表示法

JSON是一种轻量级的数据交换格式，而不是一种语言

### 特点

* 易于人阅读和编写
* 易于机器解析和生成
* 有效地提升网络传输效率

### JSON规定

* 字符集：必须是UTF-8
* 必须使用双引号
* 键名需要用双引号引起来，不能省略双引号



## JSON中的数据类型

| 数据类型 | 说明                              |
| -------- | --------------------------------- |
| number   | 和js一致                          |
| boolean  | 和js一致                          |
| string   | 和js一致                          |
| null     | 和js一致                          |
| object   | 使用字面值 {}，键必须用`""`括起来 |
| array    | 使用字面值 []                     |



## JSON序列化

### 概念

将JS对象转为字符串的过程称为JSON序列化

### 作用

便于网络数据传输，因为后端语言不一定能接收JS对象，但肯定能接收字符串

### 方法

使用 `JSON.stringify`

#### 语法

```js
JSON.stringify(对象, [进行保留的对象/自定义转换函数], [美化缩进空格数量])
```

#### 示例

```js
let obj = {
    name: "user",
    age: 18,
    gender: "male"
}

console.log(JSON.stringify(obj));
console.log(JSON.stringify(obj, null, 4));
console.log(JSON.stringify(obj, ["name", "gender"], 4));
console.log(JSON.stringify(obj, [], 4));
console.log(JSON.stringify(obj, function (key, value) {
    switch(key) {
        case "name": 
            return "f-" + value;
        default:
            return value;
    }
}, 4));
```

#### 自定义返回

为对象添加 `toJSON` 方法

```js
let obj = {
    name: "user",
    age: 18,
    gender: "male",
    toJSON() {
        return {
            title: "hhh"
        }
    }
}
console.log(JSON.stringify(obj, null, 4));
console.log(typeof JSON.stringify(obj, null, 4));
```



## JSON反序列化

### 概念

将JSON字符串转为JS对象的过程称为JSON反序列化

### 方法

使用 `JSON.parse` 方法

#### 语法

```js
JSON.parse(JSON字符串, [自定义解析方法])
```

#### 示例1

```js
let str = `{"name":"alice","age":20}`;
let obj = JSON.parse(str)
```

#### 自定义解析方法

```js
function(key, value) {
    return 字符串
}
```

#### 示例2

```js
let str = `{"name":"alice","age":20}`;

let obj = JSON.parse(str, (key, value) => {
   if (key === "age") {
       return value - 10;
   }
   return value;
});

console.log(obj);
/*
{
  name: "alice",
  age: 10
}
*/
```