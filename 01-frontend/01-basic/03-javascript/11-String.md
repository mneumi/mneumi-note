## 创建字符串

### 使用构造函数

```js
const str = new String("Hello");
```

### 使用字面量

使用字面量创建字符串时，不会调用构造函数

```js
const str = "Hello";
```



## 字符串的不可变性

字符串是不可变的：如果对字符串的某个索引赋值，不会有任何错误，但是也没有任何效果



## 转义字符

| 转义字符 | 含义                            |
| -------- | ------------------------------- |
| \n       | 换行                            |
| \t       | 制表符                          |
| \b       | 退格                            |
| \r       | 回车                            |
| \f       | 换页                            |
| \\\      | 反斜杠                          |
| \\'      | 单引号                          |
| \\"      | 双引号                          |
| \\`      | 反引号                          |
| \xnn     | 以十六进制表示nn字符            |
| \unnnn   | 以十六进制表示nnnn的unicode字符 |



## 字符串API

### 获取字符串长度

使用属性`length`

```js
const str = "Hello World";
console.log(str.length); // 11
```

### 获取特定字符

#### charAt()

方法以单字符字符串的形式返回给定位置的那个字符，相当于 `[]`

```js
const stringValue = "hello world"; 
console.log(stringValue.charAt(1));   // e
```

#### charCodeAt()

方法以单字符字符串的形式返回给定位置的那个字符的字符编码

```js
const stringValue = "hello world"; 
console.log(stringValue.charCodeAt(1));   // 101
```

#### fromCharCode()

String对象的静态方法，方法由字符的字符编码返回单字符字符串

```js
console.log(String.fromCharCode(101)); // e
```

### 拼接字符串

#### + 操作符

```js
const string1 = "Hello";
const string2 = "World";
const string3 = string1 + " " + string2;
```

#### concat

```js
const stringValue = "hello "; 
const result = stringValue.concat("world"); 
console.log(result);          //"hello world" 
console.log(stringValue);      //"hello"
```

### 搜索子串

#### indexOf

可以接收可选的第二个参数，表示从字符串中的哪个位置开始搜索

```js
const stringValue = "hello world"; 
console.log(stringValue.indexOf("o")); //4 
console.log(stringValue.indexOf("o", 5)); //7
```

#### lastIndexOf

可以接收可选的第二个参数，表示从字符串中的哪个位置开始搜索

```js
const stringValue = "hello world"; 
console.log(stringValue.lastIndexOf("o")); //7 
console.log(stringValue.lastIndexOf("o", 4)); //4 
```

### 获取子串

#### slice

参数

* 参数1：开始位置下标
* 参数2：结束位置下标（默认值为字符串长度）

返回值：结果字符串[ 开始位置下标，结束位置下标 )

```js
const str = "Hello World";
str.slice(1,5); // "ello"
```

#### substring

参数：

* 参数1：开始位置下标
* 参数2：结束位置下标（默认值为字符串长度）

返回值：结果字符串[ 开始位置下标，结束位置下标 )

注意：如果参数2不填，则默认值是字符串结尾；所有负值参数都转换为 0

```js
const str = "Hello World";
str.substring(1,5); // "ello"
```

#### substr

参数

* 参数1：开始位置下标
* 参数2：字符串长度（默认值是字符串长度）

返回值：结果字符串[ 开始位置下标，开始位置下标 + 字符串长度 )

注意：该方法不是规范中的标准方法，但绝大多数浏览器支持

```js
const str = "Hello World";
str.substr(1,5); // "ello "
```

#### 对比总结

| 方法      | 参数               | 特点                    |
| --------- | ------------------ | ----------------------- |
| slice     | 开始下标，结束下标 | 前闭后开，支持负数      |
| substring | 开始下标，结束下标 | 前闭后开，负数全部转为0 |
| substr    | 开始下标，字符长度 | 负数长度转为0           |

### 字符串去除首尾空格

#### trim

去除两侧空格

```js
const str = "   Hello World   ";
const result = str.trim();
console.log(result); // "Hello World"
```

#### trimStart, trimLeft

去除左侧空格，trimLeft是别名

```js
const str = "   Hello World   ";
console.log(str.trimStart()); // "Hello World   "
console.log(str.trimLeft()); // "Hello World   "
```

#### trimEnd, trimRight

去除右侧空格，trimRIght是别名

```js
const str = "   Hello World   ";
console.log(str.trimEnd()); // "   Hello World"
console.log(str.trimRight()); // "   Hello World"
```

### 字符串填充

#### padStart

当字符串长度不够时，在字符串左边填充特定字符使字符串长度达标

```js
const str = "Hello";
console.log(str.padStart(10,"*")); //*****Hello
```

#### padEnd

当字符串长度不够时，在字符串右边边填充特定字符使字符串长度达标

```js
const str = "Hello";
console.log(str.padEnd(10,"*")); //Hello*****
```

### 字符串判断

#### startsWith

判断字符串是否以某个子串开头

```js
const str = "https://www.google.com";
console.log(str.startsWith("http")); // true
console.log(str.startsWith("ssh")); // false
```

#### endsWith

判断字符串是否以某个子串结尾

```js
const str = "https://www.google.com";
console.log(str.endsWith("com")); // true
console.log(str.endsWith("org")); // false
```

#### includes

判断字符串是否含有某个字符，第二个参数是起始下标

```js
const str = "Hello World";
console.log(str.includes("llo W")); // true
console.log(str.includes("JavaScript")); // false
console.log(str.includes("llo W", 7)); // false
```

### 字符串大小写转换

toLowerCase、toLocaleLowerCase、toUpperCase、toLocaleUpperCase

```js
var stringValue = "hello world"; 
alert(stringValue.toLocaleUpperCase());  //"HELLO WORLD" 
alert(stringValue.toUpperCase());        //"HELLO WORLD" 
alert(stringValue.toLocaleLowerCase());  //"hello world" 
alert(stringValue.toLowerCase());        //"hello world" 
```

### 字符串切割

split(分隔字符串)

```js
const str = "1-2-3-4-5-6";
const result = str.split("-");
console.log(result); // [1,2,3,4,5,6]
```

### 字符串比较

localeCompare(字符串)

这个方法比较两个字符串，并返回下列值中的一个 

* 如果字符串在字母表中应该排在字符串参数之前，则返回一个负数
* 如果字符串等于字符串参数，则返回 0
* 如果字符串在字母表中应该排在字符串参数之后，则返回一个正数

```js
var stringValue = "yellow";        
alert(stringValue.localeCompare("brick")); //1 
alert(stringValue.localeCompare("yellow")); //0 
alert(stringValue.localeCompare("zoo")); //-1
```

### 重复字符串

语法：str.repeact(次数)

作用：能够将某个字符串重复n次，返回新的字符串

```js
console.log("*".repeat(5)); // *****
```

### 原生字符串

语法：String.raw``

作用：不进行任何转义

注意：raw函数是String类的静态方法，且是模板字符串函数

```js
console.log(String.raw`\n\t\r\"`); // \n\t\r\"
```

### toString特殊用法

通过传入参数，可以得到数值的二进制，八进制，十六进制或者其他有效进制基数

```js
const num = 10;
console.log(num.toString()); // "10"
console.log(num.toString(2)); // "1010"
console.log(num.toString(8)); // "12"
console.log(num.toString(10)); // "10"
console.log(num.toString(16)); // "a"
```



## 模式匹配

字符串模式匹配是指对字符串使用正则表达式

### replace

作用：替换子串

语法

```js
str.replace(regexp|substr, newSubStr|function)
```

返回值：新字符串

示例1

```js
const str = "colour: red";

console.log(str.replace("colour", "color")); // color: red
```

示例2

```js
const str = "H123o H456o H789o H000o H999o";
const result = str.replace(/(\d+)/g, "66666");

console.log(result); // "H66666o H66666o H66666o H66666o H66666o";
```

示例3

```js
function replacer(match, p1, p2, p3, offset, string) {
  // p1 is nondigits, p2 digits, and p3 non-alphanumerics
  return [p1, p2, p3].join(' - ');
}
var newString = 'abc12345#$*%'.replace(/([^\d]*)(\d*)([^\w]*)/, replacer);
console.log(newString);  // abc - 12345 - #$*%
```

### match

作用：检索返回一个字符串匹配正则表达式的的结果

语法

```js
str.match(regexp)
```

返回值：

* 如果使用g标志，则将返回与完整正则表达式匹配的所有结果，但不会返回捕获组
* 如果未使用g标志，则仅返回第一个完整匹配及其相关的捕获组（`Array`）

示例

```js
var str = 'For more information, see Chapter 3.4.5.1';
var re = /see (chapter \d+(\.\d)*)/i;
var found = str.match(re);

console.log(found);

// logs [ 'see Chapter 3.4.5.1',
//        'Chapter 3.4.5.1',
//        '.1',
//        index: 22,
//        input: 'For more information, see Chapter 3.4.5.1' ]

// 'see Chapter 3.4.5.1' 是整个匹配。
// 'Chapter 3.4.5.1' 被'(chapter \d+(\.\d)*)'捕获。
// '.1' 是被'(\.\d)'捕获的最后一个值。
// 'index' 属性(22) 是整个匹配从零开始的索引。
// 'input' 属性是被解析的原始字符串。
```

### search

作用：搜索字符串是否满足正则表达式的子串

语法

```js
str.search(regexp)
```

返回值

* 如果匹配成功，则返回最开始索引值
* 如果匹配失败，则返回-1

示例

```js
const str = "hey JudE";
const re = /[A-Z]/g;
const re2 = /[.]/g;
console.log(str.search(re)); // returns 4, which is the index of the first capital letter "J"
console.log(str.search(re2)); // returns -1 cannot find '.' dot punctuation
```



## 模板字符串

字面量中可以套字面量

语法：必须用反引号，不能用单引号或双引号，值写在`${}`中

```js
const start = "start";
const end = "end";

const array = [
    {v: 1}, 
    {v: 2}, 
    {v: 3}
];

const ret = `${start} ${array.map(item => `${item.v}`)} ${end}`;
console.log(ret); // start 1 2 3 end
```

特点：模板字符串支持多行写法

```js
const str = `
第一行
第二行
第三行
`;
```



## 标签模板字符串

### 原理

需要配合模板字符串使用，功能是解析模板字符串为字符串数组

```js
let name = "alice";
let age = 18;

let ret = foo`姓名：${name}，年龄：${age}，你好`

function foo(strings, ...vars) {
    console.log(strings); // 数组
    console.log(vars); // 数组
    
    return strings.map((item,index) => `${item}=${index}`).join("")
}

document.write(ret);
```

strings的长度一定大于变量数量，因为它包括前后的两个`""`

```js
let name = "alice";
let age = 18;

let ret = foo`${name}`

function foo(strings, ...vars) {
    console.log(strings); // 数组
    console.log(vars); // 数组
}
```

使用案例：React中的`styled-components`使用到了标签模板字符串

### 应用

`String`类默认提供了一个标签模板字符串：`String.raw`，它的功能是不进行转义

```js
console.log(`\u00A9`); // ©
console.log(String.raw`\u00A9`); // \u00A9
```

