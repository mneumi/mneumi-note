## string ==> number

1. *1 (number)

   如果转换失败，返回NaN

   ```js
   let str = "123";
   let num = str * 1;
   console.log(typeof num); // number
   
   let str2 = "abc";
   let num2 = str2 * 1;
   console.log(typeof num2); // number
   console.log(num2); // NaN
   ```

2. 使用构造函数 (object)

   如果转换失败，返回NaN

   ```js
   let str = "123";
   let num = new Number(str);
   console.log(typeof num); // number
   
   let str2 = "abc";
   let num2 = new Number(str2);
   console.log(typeof num); // number
   console.log(num2); // NaN
   ```

3. parseInt/parseFloat (number)

   只要是数字开头即可，如果转换失败，返回NaN

   ```js
   let str1 = "99";
   let str2 = "99abc";
   let str3 = "abc99";
   
   console.log(parseInt(str1)); // 99
   console.log(parseInt(str2)); // 99
   console.log(parseInt(str3)); // NaN
   
   let str4 = "99.9";
   let str5 = "99.9abc";
   let str6 = "abc99.9";
   
   console.log(parseFloat(str4)); // 99.9
   console.log(parseFloat(str5)); // 99.9
   console.log(parseFloat(str6)); // NaN
   ```

4. 11



## number ==> string

1. +""

   ```js
   let num = 123;
   let str = num + "";
   console.log(typeof str);
   ```

2. 使用构造函数

   ```js
   let num = 123;
   let str = String(num);
   console.log(typeof str);
   ```

3. toString

   ```js
   const num = 99;
   const str = num.toString();
   ```

   

## String ==> Array

split

```js
const str = "Hello";
let arr = str.split("");
console.log(arr);
```

Array.from

```js
const str = "123456";
console.log(Array.from(str));
```





## Array ==> String

join

```js
const arr = [1,2,3,4,5];
const str = arr.join("");
console.log(str);
```



## Array ==> Number

如果是空数组，则为0

如果数组只有一个元素，且可以转为数字，则为转换后的数字

如果数组有两个或以上元素，则为NaN



## Object ==> Number

会将进行Number(obj.valueOf)的操作，如果失败，则NaN



## 其他类型 ==> Boolean

1. 使用构造函数，原理是先调用`Number`转换为数字后，再判断数字进行判断

   ```js
   const a = 0;
   const b = 1;
   
   const c = Boolean(a);
   const d = Boolean(b);
   
   console.log(c,d);
   
   const str1 = "123";
   const str2 = "";
   
   const e = Boolean(str1);
   const f = Boolean(str2);
   
   console.log(e,f);
   ```

2. 使用`!!`

   ```js
   const a = 0;
   const b = 1;
   
   const c = !!a;
   const d = !!b;
   
   console.log(c,d);
   
   const str1 = "123";
   const str2 = "";
   
   const e = !!str1;
   const f = !!str2;
   
   console.log(e,f);
   ```

   

Number 转 String 的方法

① 使用 toString() 方法

* 可以指定进制：num.toString(8)

② 使用 String() 函数

* 如果值有 toString()方法，则调用该方法（没有参数）并返回相应的结果
* 如果值是 null，则返回"null"
* 如果值是 undefined，则返回"undefined"