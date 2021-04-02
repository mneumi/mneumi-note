## Object基础

### 创建Object

#### 使用构造方法

```js
const person = new Object();
```

如果不给构造函数传递参数，则可以省略后面的那一对圆括号，但不推荐这样做

```js
const person = new Object; // ok，但不推荐
```

#### 使用字面量

在使用对象字面量定义对象时，不会调用 Object 构造方法

```js
const person = {};
```

#### Object.create

```js
Object.create(原型对象[, descriptors]);
```

示例

```js
const person1 = {
    name: "alice",
    age: 18
};

// 等价于

const person2 = Object.create(Object, {
    name: {
        value: "alice"
    },
    age: {
        value: 18
    }
});
```

#### Object.fromEntries

这是Object.entries的逆运算

接收二维数组或者一个Map，用于创建对象

```js
Object.fromEntries(二维数组/Map);
```

示例

```js
const arr = [["name", "alice"], ["age", 18]];

const person = Object.fromEntries(arr);

console.log(person);
```

```js
const m = new Map([["name", "alice"], ["age", 18]]);

const person = Object.fromEntries(m);

console.log(person);
```

### 键值对要求

对象中的键值只能是字符串，但符合标识符命名规范时，可以不加引号

值可以是任意类型

```js
const obj = {
    name: "js",
    "full-name": "JavaScript"
};
```

### 访问对象

通过 `.` 运算符：属性名要符合标识符命名规范

通过 `[]` 运算符：可以使用变量，或使用不符合命名规范的属性名，用 `[]` 可替代所有的 `.`

通常，除非必须使用变量来访问属性或者属性名不符合规范，否则我们建议使用 `.` 运算符

```js
const obj = {
    name: "js",
    "full-name": "JavaScript"
};

const objName = obj.name;

console.log(obj.name); // 通过 . 运算符
console.log(obj[objName]); // 通过变量
console.log(obj["full-name"]); // 通过不符合命名规范的属性名
```



## Object属性

### 添加与删除

```js
const obj = {};

obj.name = "obj"; // 添加属性

delete obj.name; // 删除属性
```

属性添加还可以使用属性描述符，详见下面的内容

### 计算属性

键可以使用`[]`，在`[]`可写表达式

表达式的结果会调用`toString`转为字符串，因为属性只支持字符串

```js
const name = "ES6";
const es5_obj = {
    ES5: 'ECMAScript 5'
}
const es6_obj = {
    // ES6: 'ECMAScript 6'
    [name]: 'ECMAScript 6' 
}
```

```js
const obj = {};

const my = {
  [obj]: 123
}

// 如何取到123？
console.log(my[obj.toString()]) = 123;

// 实际等价于
const my = {
  [obj.toString()]: 123
}
```

### 检测属性存在

| 检测方式       | 特点               |
| -------------- | ------------------ |
| in操作符       | 检查自身以及原型链 |
| hasOwnProperty | 只检查自身         |

```js
const person = {
    name: "alice",
    age: 20
};

console.log("name" in person); // true
console.log(person.hasOwnProperty("name")); // true

console.log("toString" in person); // true
console.log(person.hasOwnProperty("toString")); // false
```

### 简洁表达

#### 简化键与值同名的表达

```js
const o = 1;
const k = 2;

const es5 = {
    o: o,
    k: k
};

const es6 = {
    o,
    k
};
```

#### 简化函数属性的定义

```js
// ES5 写法
const es5_method = {
    hello: function() {}
}
// ES6 写法
const es6_method = {
    hello() {}
}
```



## Object属性描述符

### 属性描述符介绍

对象的每一个属性都有自己的特征，一共有6个，统称为属性描述符（**descriptor**)

| 特征         | 说明                                                         | 默认值    |
| ------------ | ------------------------------------------------------------ | --------- |
| value        | 属性的值                                                     | undefined |
| writeable    | 属性是否可修改；严格模式下，会报错，非严格模式不报错，但还是不能修改 | true      |
| enumerable   | 属性是否可遍历                                               | true      |
| configurable | 是否可删除或重新defineProperty；严格模式下，会报错，非严格模式不报错，但还是不能删除 | true      |
| get          | 取值函数，即getter                                           | undefined |
| set          | 存值函数，即setter                                           | undefined |

> 平常对属性进行赋值，实际上设置的是属性描述符中的`value`

### 互斥的值

如果设置了`get`或`set`，则不能再设置`value`和`writable`

如果设置了`value`或`writable`，则不能再设置`get`和`set`

### 获取属性描述符

#### 获取单个属性描述符

使用`Object.getOwnPropertyDescriptor`获取对象单个属性的属性描述符

```js
Object.getOwnPropertyDescriptor(对象, 属性名);
```

示例

```js
const person = {
	name: "alice",
	age: 20
};

const descriptor = Object.getOwnPropertyDescriptor(person, "name");

console.log(descriptor);
```

#### 获取批量属性描述符

使用`Object.getOwnPropertyDescriptors`获取对象所有属性的属性描述符

```js
Object.getOwnPropertyDescriptors(对象);
```

示例

```js
const person = {
	name: "alice",
	age: 20
};

const descriptors = Object.getOwnPropertyDescriptors(person);

console.log(descriptors);
```

### 新增/设置属性描述符

#### 单个属性设置

使用 `Object.defineProperty` 新增或设置单个属性

```js
Object.defineProperty(obj, prop, descriptor)
```

| 参数       | 说明                                           |
| ---------- | ---------------------------------------------- |
| obj        | 必填，目标对象                                 |
| prop       | 必填，需要新增或设置的属性的名字（字符串类型） |
| descriptor | 必填，属性描述符                               |

示例

```js
const person = {
    name: "alice",
    ge: 20
};

Object.defineProperty(person, "age", {
    value: "21",
    writable: false,
    enumerable: true
});

Object.defineProperty(person, "gender", {
    value: "male",
    writable: false,
    enumerable: true,
    configurable: true
});

console.log(person);
```

#### 批量属性设置

使用 `Object.defineProperties` 批量新增或设置属性

```js
Object.defineProperties(obj, descriptors)
```

| 参数        | 说明                 |
| ----------- | -------------------- |
| obj         | 必填，目标对象       |
| descriptors | 必填，批量属性描述符 |

示例

```js
const person = {};

Object.defineProperties(person, {
    name: {
        value: "alice",
        writable: false,
        enumrable: true,
        configurable: true
    },
    age: {
        value: "20",
        writable: false,
        enumrable: false,
        configurable: false
    }
});

console.log(person);
```



## Object属性访问器

### 概念

Object属性访问器可以绑定对象的属性中，为对象的赋值和取值添加额外的逻辑和功能

每个对象的每个属性都可以设置两种访问器，分别为赋值访问器和取值访问器，即 setter 和 getter

### 本质

属性访问器的本质是函数

setter函数形式为

```js
function(value) {}
```

getter函数形式为

```js
function() { return value; }
```

### 使用

#### 方式1：使用get, set关键字

注意函数名需要和绑定的属性不要同名，很容易无限递归导致`StackOverflow`

```js
const person = {
    _name: "alice",
    _age: 18,
    set age(value) {
        if (value < 18) {
            this._age = 18;
        } else if (value > 60) {
            this._age = 60;
        } else {
            this._age = value;
        }
    },
    get age() {
    	return this._age;  
    },
    get name() {
        return `name: ${this._name}`
    }
}

console.log(person.name); // name: alice
console.log(person.age); // 18
person.age = 10;
console.log(person.age); // 18
person.age = 100;
console.log(person.age); // 60
```

#### 方式2：使用属性描述符

如果设置了`value`或`writable`，就不能再设置`set`和`get`

如果设置了`set`或`get`，就不能再设置`value`和`writable`

示例1

```js
const person = {
    _name: "alice",
    _age: 18
};

Object.defineProperty(person, "name", {
    // value: "alice", // 不能设置这个值
    // writable: true, // 不能设置这个值
    enumerable: true,
    configurable: true,
    get: function() {
        return this._name;
    },
    set: function(value) {
        this._name = value;
    }
});

console.log(person.name);
```

### 优先级

属性访问器比属性优先级更高

```js
const person = {
    age: 18,
    get age() {
        return 20;
    }
};

console.log(person.age); // 20
```



## Object遍历

### 前置知识

由**属性修饰符**中的**enumerable**决定一个属性是否可枚举

在Object的遍历中，存在多种遍历方法，其中有些方法能够遍历全部属性，即包括可枚举和不可枚举的属性，而有些方法只能遍历可枚举的属性

### for-in

for-in循环只能遍历可枚举属性，遍历过程获取的是**键**

```js
const person = {
    name: "alice",
    age: 20,
    gender: "female"
};

Object.defineProperty(person, "age", { // 设置 age 属性为不可枚举的
    enumerable: false
});

for (const key in person) {
    console.log(`key: ${key}, value: ${person[key]}`)
}

// 输出结果为
// key: name, value: alice
// key: gender, value: female
```

### Object.keys

获取对象中**可枚举**属性的**键**组成的数组

```js
const person = {
    name: "alice",
    age: 20,
    gender: "female"
};

Object.defineProperty(person, "age", { // 设置 age 属性为不可枚举的
    enumerable: false
});

const keysArray = Object.keys(person);

keysArray.forEach(item => console.log(item));

// 输出结果为
// name
// gender
```

### Object.values

获取对象中**可枚举**属性的**值**组成的数组

```js
const person = {
    name: "alice",
    age: 20,
    gender: "female"
};

Object.defineProperty(person, "age", { // 设置 age 属性为不可枚举的
    enumerable: false
});

const valuesArray = Object.values(person);

valuesArray.forEach(item => console.log(item));

// 输出结果为
// alice
// female
```

### Object.entries

获取对象中**可枚举**属性的键值对组成的数组

数组中的每一项也为一个数组，长度为2，形式为 **[键, 值]**

```js
const person = {
    name: "alice",
    age: 20,
    gender: "female"
};

Object.defineProperty(person, "age", { // 设置 age 属性为不可枚举的
    enumerable: false
});

const entriesArray = Object.entries(person);

entriesArray.forEach(item => console.log(item));

// 输出结果为
// [name, alice]
// [gender, female]
```

### getOwnPropertyNames

用于获取对象中**所有**属性的**键**组成的数组，与`Object.keys`的区别是可以获取到**不可枚举属性**

注意：Symbol类型计算属性无法获取

例子：获取到一个对象中**不可枚举**属性名组成的数组

```js
const person = {
    name: "alice",
    age: 20,
    gender: "female",
    height: 165,
    hobby: "movie"
};

// 设置 age, height, hobby 三个属性是不可枚举的
Object.defineProperties(person, {
    age: {
        enumerable: false
    },
    height: {
    	enumerable: false  
    },
    hobby: {
        enumerable: false
    }
});

const properties = Object.getOwnPropertyNames(person); // 获取所有属性的键组成的数组
const keys = Object.keys(person); // 获取所有可枚举属性的键组成的数组

const result = properties.filter(item => !keys.includes(item));

console.log(result); // [age, height, hobby]
```

### getOwnPropertySymbols

用于获取对象中键为Symbol计算属性的数组

特点：只能获取Symbol计算属性

```js
const sym = Symbol.for("sym");

const person = {
    name: "alice",
    age: 20,
    gender: "female",
    [sym]: "我是symbol计算属性的值"
};

const symbolArray = Object.getOwnPropertySymbols(person);

console.log(symbolArray);

// 输出结果为
// [Symbol(sym)]
```



## Object封装

### 扩展

扩展指添加属性，不包括删除属性

扩展禁止只能禁止一层，子级对象不会被禁止

| 方法                           | 功能                   | 备注                                   |
| ------------------------------ | ---------------------- | -------------------------------------- |
| Object.preventExtensions(对象) | 禁止对象扩展(添加属性) | 严格模式下报错报错，非严格模式下不报错 |
| Object.isExtensible(对象)      | 查看对象是否可扩展     |                                        |

示例

```js
"use strict"; // 使用严格模式，开启报错功能

const person = {
    name: "alice"
};

Object.preventExtensions(person); // 设置为不可扩展

console.log(Object.isExtensible(person)); // false，表示不可扩展

delete person["name"]; // ok，不报错，因为禁止扩展不包括删除属性

person["age"] = 20; // 报错
```

### 封闭

封闭是指，对象不能添加，删除属性，批量设置configurable: false

封闭只能封闭一层，子级对象不会封闭

| 方法                  | 功能             | 备注                                   |
| --------------------- | ---------------- | -------------------------------------- |
| Object.seal(对象)     | 封闭对象         | 严格模式下报错报错，非严格模式下不报错 |
| Object.isSealed(对象) | 判断对象是否封闭 |                                        |

示例

```js
"use strict"; // 使用严格模式，开启报错功能

const person = {
    name: "alice"
};

Object.seal(person); // 封闭对象

console.log(Object.isSealed(person)); // true，表示对象是封闭的

delete person["name"]; // 报错

person["age"] = 20; // 报错
```

### 冻结

冻结是指，对象不能添加，删除和修改属性，批量设置configurable: false和writable: fasle

冻结只能冻结一层，子级对象不会冻结

| 方法                  | 功能             | 备注                                   |
| --------------------- | ---------------- | -------------------------------------- |
| Object.freeze(对象)   | 冻结对象         | 严格模式下报错报错，非严格模式下不报错 |
| Object.isFrozen(对象) | 判断对象是否冻结 |                                        |

示例

```js
"use strict"; // 使用严格模式，开启报错功能

const person = {
    name: "alice"
};

Object.seal(person); // 封闭对象

console.log(Object.isSealed(person)); // true，表示对象是封闭的

delete person["name"]; // 报错

person["age"] = 20; // 报错
```



## Object拷贝

### 浅拷贝

#### 方式1：循环

```js
const person = {
    name:"alice",
    age:20
};

const obj = {};

for (let key in person) {
    obj[key] = person[key];
}

console.log(obj);
```

#### 方式2：Object.assign

不会拷贝继承的属性，不会拷贝不可枚举的属性

```js
const person = {
    name:"alice",
    age:20
};

const obj = {};

Object.assign(obj, person);

console.log(obj);
```

#### 方式3：展开语法

```js
const person = {
    name:"alice",
    age:20
};

const obj = {...person};

console.log(obj);
```

### 深拷贝

#### 方式1：JSON（乞丐版）

使用 JSON.stringify 以及 JSON.parse 

缺点：不可以拷贝 undefined ， function， RegExp，Date 等类型的值

```js
let hd = {
    name: "后盾人",
    methods: {
        GET: "GET方法",
        POST: "POST方法"
    }
}

let obj = JSON.parse(JSON.stringify(hd));
```

#### 方式2：使用第三方库

 `Lodash` 或 `underscore`等库均有深拷贝的实现

#### 方式3：递归实现

详细看**《自己实现深拷贝》**一文



## Object原型

### 设置原型对象

非标准方法可以直接设置对象的`__proto__`属性，但这是非标准的

标准方法可以使用`Object.setPrototypeOf`设置原型对象

```js
Object.setPrototypeOf(obj, prototype);
```

示例

```js
const person1 = {
    name: "alice"
};

const person2 = {};

Object.setPrototypeOf(person2, person1); // 设置person2的原型对象为person1

console.log(person2.name); // 查找到原型链上person1的name属性
```

### 获取原型对象

非标准方法可以使用对象的`__proto__`属性获取原型对象

标准方法可以使用`Object.getPrototypeOf`获取原型对象

```js
Object.getPrototypeOf(obj);
```

示例

```js
const person1 = {
    name: "alice"
};

const person2 = {};

Object.setPrototypeOf(person2, person1); // 设置person2的原型对象为person1

console.log(Object.getPrototypeOf(person2) === person1); // true
```

### 判断是否是原型对象

```js
obj.isPrototypeOf(obj);
```

示例

```js
const person1 = {
    name: "alice"
};

const person2 = {};
const person3 = {};

Object.setPrototypeOf(person2, person1); // 设置person2的原型对象为person1

console.log(person1.isPrototypeOf(person2)); // true
console.log(person3.isPrototypeOf(person2)); // false
```



## Object判断

### 方法1：typeof运算符

不好用，对于引用类型除了函数，其他的对象均返回`object`

且存在Bug：`typeof null 为 object`

```js
typeof null; // object, 这是bug
typeof 123;  // number
typeof {};   // object
```

### 方法2：instanceof运算符

返回值为bool类型，原理是递归判断对象的`prototype`属性，看是否含有待判断对象

注意：自动包装属性返回值是false

```js
console.log(null instanceof Object); // false
            
console.log(123 instanceof Number); // false

const n = new Number(123);
console.log(n instanceof Number); // true
```

### 方法3：Object.is

```js
Object.is(对象,对象);
```

效果基本等价于 `===` 操作符，只有两个地方不同

1. `+0不等于-0`
2. NaN等于自身

示例

```js
console.log(Object.is("111","111")); // true

console.log(Object.is(+0,-0)); // false
console.log((+0) === (-0)); // true

console.log(Object.is(NaN,NaN)); // true
console.log(NaN === NaN) // false
```



## 对象合并

```js
Object.assign(合并进的对象, ...被合并的对象)
```

注意：会改变`合并进的对象`，且如果存在重复属性，则后面的值会覆盖前面的值

```js
const obj1 = {a:1, v:11};
const obj2 = {b:2, v:22};
const obj3 = {c:3, v:33};

Object.assign(obj1,obj2,obj3);

console.log(obj1); // a:1, b:2, c:3, v33
```



## Object总结

### Object静态方法

| 静态方法                      | 功能                               |
| ----------------------------- | ---------------------------------- |
| Object.create                 | 创建Object，指定其原型和属性描述符 |
| Object.fromEntries            | 由二维数组或Map创建Object          |
| Object.defineProperty         | 设置对象某个属性的属性描述符       |
| Object.defineProperties       | 批量设置对象属性的属性描述符       |
| Object.getPropertyDescriptor  | 获取对象某个属性的属性描述符       |
| Object.getPropertyDescriptors | 获取对象全部属性的属性描述符       |
| Object.keys                   | 获取可枚举属性的键组成的数组       |
| Object.values                 | 获取可枚举属性的值组成的数组       |
| Object.entries                | 获取可枚举属性的键和值组成的数组   |
| Object.getOwnPropertyNames    | 获取所有属性的键组成的数组         |
| Object.getOwnPropertySymbols  | 获取所有Symbol属性的键组成的数组   |
| Object.preventExtensions      | 禁止扩展                           |
| Object.isExtensible           | 判断是否可扩展                     |
| Object.seal                   | 封闭对象                           |
| Object.isSealed               | 判断对象是否封闭                   |
| Object.freeze                 | 冻结对象                           |
| Object.isForzen               | 判断对象是否冻结                   |
| Object.setPrototypeOf         | 设置对象的原型对象                 |
| Object.getPrototypeOf         | 获取对象的原型对象                 |
| Object.assgin                 | 合并对象                           |
| Object.is                     | 类似于 `===` 操作符                |

### Object实例方法

实例方法很少，只有`obj.hasOwnProperty`和`obj.isPrototypeOf`



## 绝对全局对象

### 作用

不管在啥环境：浏览器，Node.js，globalThis都会指向全局对象

### 本质

| 具体    | 指向的对象 |
| ------- | ---------- |
| 浏览器  | window     |
| node.js | global     |

