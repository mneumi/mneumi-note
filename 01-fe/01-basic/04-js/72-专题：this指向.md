## 初识this

### 其他语言中的this

在主流的编程语言中，几乎都有`this`这个关键字（Objective-C 和 Python中使用的是`self`）

在这些语言中，`this`只会出现在类方法中，`this`非常明显且固定，就是指向类的实例对象

所以在这些语言中，`this`的指向几乎没有任何疑惑和学习难度，非常浅显易懂

### JavaScript中的this

但是在 JavaScript 中，`this`的指向与其他主流语言都不同，`this`的指向是在程序运行过程中进行绑定的

所以`this`的指向不固定，情况也比较多，学习理解起来比较困难

### this出现在哪里？

既然`this`情况复杂，那么首先就要确定在`JavaScript`中`this`这个关键字会出现在哪里

* 出现在全局作用域下
* 出现在函数中
* 出现在eval语句中（eval很少被使用，所以不进行讨论总结）



## this绑定规则

## 规则一：默认绑定

什么情况下使用默认绑定呢？独立函数调用

独立的函数调用我们可以理解成函数没有被绑定到某个对象上进行调用

```js
// 1.案例一
function foo() {
    console.log(this);
}

foo();

// 2.案例二
function test1() {
    console.log(this);
    test2();
}

function test2() {
    console.log(this);
    test3();
}

function test3() {
    console.log(this);
}

test1();

// 3.案例三
function foo(func) {
    func();
}

var obj = {
	name: 'why',
    bar: function() {
        console.log(this);
    }
}

foo(obj.bar);
```



## 规则二：隐式绑定

另外一种比较常见的调用方式是通过某个对象进行调用的：也就是它的调用位置中，是通过某个对象发起的函数调用

```js
// 1.通过对象调用
function foo() {
    console.log(this); // obj对象
}

var obj = {
    name: 'alice',
    foo
}

obj.foo();

// 2
function foo() {
    console.log(this);
}

var obj1 = {
    name: 'obj1',
    foo
}

var obj2 = {
    name: 'obj2',
    obj1
}

obj2.obj1.foo();

// 3
function foo() {
    console.log(this);
}

var obj1 = {
    name: 'obj1',
    foo
}

// 将obj1的foo赋值给bar
var bar = obj1.foo;
bar();
```



## 规则三：显式绑定

隐式绑定有一个前提条件：必须在调用的对象内部有一个对函数的引用（比如一个属性）

如果没有这样的引用，在进行调用时，会报找不到该函数的错误，正是通过这个引用，间接的将this绑定到了这个对象上

如果我们不希望在 **对象内部** 包含这个函数的引用，同时又希望在这个对象上进行强制调用，该怎么做呢

* JavaScript所有的函数都可以使用call和apply方法（这个和Prototype有关）
  * 它们两个的区别这里不再展开
  * 其实非常简单，第一个参数是相同的，后面的参数，apply为数组，call为参数列表
* 这两个函数的第一个参数都要求是一个对象，这个对象的作用是什么呢？就是给this准备的
* 在调用这个函数时，会将this绑定到这个传入的对象上
* 因为上面的过程，我们明确的绑定了this指向的对象，所以称之为 **显式绑定**

```js
function foo() {
    console.log(this);
}

foo.call(window); // window
foo.call({name: 'why'}); // {name: 'why'}
foo.call(123); // Number对象，存放时123
```

如果我们希望一个函数总是显示的绑定到一个对象上，可以怎么做呢？

```js
var obj = {
    name: 'why'
}

function bind(func, obj) {
    return function() {
        return func.apply(obj, arguments);
    }
}

var bar = bind(foo, obj);

bar(); // obj对象

///////////////////////////

function foo() {
    console.log(this);
}

var obj = {
    name: 'why'
};

var bar = foo.bind(obj);

bar(); // obj对象
```



## 内置函数的绑定思考

有些时候，我们会调用一些JavaScript的内置函数，或者一些第三方库中的内置函数

* 这些内置函数会要求我们传入另外一个函数
* 我们自己并不会显示的调用这些函数，而且JavaScript内部或者第三方库内部会帮助我们执行
* 这些函数中的this又是如何绑定的呢

```js
setTimeout(function() {
    console.log(this); // window
}, 1000);

/////

var names = ['abc', 'zxc', 'qwe'];
var obj = { name: 'why' };
names.forEach(function(item) {
    console.log(this); // 三次obj对象
}, obj);

/////

var box = document.querySelector('.box');
box.onclick = function() {
    console.log(this === box); // true
}
```

### v8引擎对setTimeout

```js
var fn = 接收函数;
fn.apply(window);

box.onclick = function() {
    console.log(this === box); // true
}
fn.apply(box);
```





## new绑定

JavaScript中的函数可以当做一个类的构造函数来使用，也就是使用new关键字

使用new关键字来调用函数是，会执行如下的操作

* 创建一个全新的对象
* 这个新对象会被执行Prototype连接
* 这个新对象会绑定到函数调用的this上（this的绑定在这个步骤完成）
* 如果函数没有返回其他对象，表达式会返回这个新对象

```js
// 创建Person
function Person(name) {
    console.log(this); // Person {}
    this.name = name;
}

var p = new Person('why');
console.log(p);
```



## 规则优先级

学习了四条规则，接下来开发中我们只需要去查找函数的调用应用了哪条规则即可，但是如果一个函数调用位置应用了多条规则，优先级谁更高呢？ 

**默认规则的优先级最低**

毫无疑问，默认规则的优先级是最低的，因为存在其他规则时，就会通过其他规则的方式来绑定this

**显示绑定优先级高于隐式绑定**

代码测试

**new绑定优先级高于隐式绑定**

代码测试

**new绑定优先级高于bind**

new绑定和call、apply是不允许同时使用的，所以不存在谁的优先级更高

new绑定可以和bind一起使用，new绑定优先级更高

代码测试



## this规则之外 – 忽略显示绑定

我们讲到的规则已经足以应付平时的开发，但是总有一些语法，超出了我们的规则之外

如果在显示绑定中，我们传入一个null或者undefined，那么这个显示绑定会被忽略，使用默认规则

```js
function foo() {
    console.log(this);
}

var obj = {
    name: 'why'
}

foo.call(obj); // obj对象
foo.call(null); // window
foo.call(undefined); // window

var bar = foo.bind(null);
bar(); // window
```



## this规则之外 - 间接函数引用

另外一种情况，创建一个函数的 间接引用，这种情况使用默认绑定规则

赋值(obj2.foo = obj1.foo)的结果是foo函数

foo函数被直接调用，那么是默认绑定

```js
function foo() {
    console.log(this);
}

var obj1 = {
    name: 'obj1',
    foo
}

var obj2 = {
    name: 'obj2'
}

obj1.foo(); // obj1对象
(obj2.foo = obj1.foo())(); // window
```



## this规则之外 – ES6箭头函数

在ES6中新增一个非常好用的函数类型：箭头函数

箭头函数不使用this的四种标准规则（也就是不绑定this），而是根据外层作用于来决定this

```js
var obj = {
    data: [],
    getData: function() {
        var _this = this;
        setTimeout(function() {
            // 模拟获取到的数据
            var res = ['abc', 'zxc', 'qwe'];
            _this.data.push(...res);
        }, 1000)
    }
}

//////////

var obj = {
    data: [],
    getData: function() {
        setTimeout(() => {
            // 模拟获取到的数据
            var res = ['abc', 'zxc', 'qwe'];
            this.data.push(...res);
        }, 1000)
    }
}

// 思考：如果getData也是一个箭头函数，那么setTimeout中的回调函数中的this指向谁呢？

var obj = {
    data: [],
    getData: () => {
        setTimeout(() => {
            console.log(this); // window
        }, 1000)
    }
}
```



## 面试题

### 面试题1

```js
var name = 'window';

var person = {
    name: 'person',
    sayName: function() {
        console.log(this.name);
    }
};

function sayName() {
    var sss = person.sayName;
    sss(); // window
    person.sayName(); // person
    (person.sayName)(); // person
    (b = personsayName)(); // window
}
sayName();
```

### 面试题2

```js
var name = 'window';

var person1 = {
    name: 'person1',
    foo1: function() {
        console.log(this.name);
    }
    foo2: () => console.log(this.name),
    foo3: function() {
        return function() {
            console.log(this.name);
        }
    },
    foo4: function() {
        return () => {
            console.log(this.name);
        }
    }
}

var person2 = { name: 'person2' };
person1.foo1(); // person1
person1.foo1.call(person2); //person2

person1.foo2(); // window
person1.foo2.call(person2); // window

person1.foo3()(); // window
person1.foo3.call(person2)(); // window
person1.foo3().call(person2); // person2

person1.foo4()(); // person1
person1.foo4.call(person2)(); // person2
person.foo4().call(person2); // person1
```

### 面试题3

```js
var name = 'window';
function Person(name) {
    this.name = name;
    this.foo1 = function() {
        console.log(this.name);
    }
    this.foo2 = () => console.log(this.name);
    this.foo3 = function() {
        return function() {
            console.log(this.name);
        };
    };
    this.foo4 = function() {
        return () => {
            console.log(this.name);
        }
    }
}

var person1 = new Person('person1');
var person2 = new Person('person2');

person1.foo1(); // person1
person1.foo1.call(person2); // person2

person1.foo2(); // person1
person1.foo2.call(person2); // person1

person1.foo3()(); // window
person1.foo3.call(person2)(); // window
person1.foo3().call(person2); // person2

person1.foo4()(); // person1
person1.foo4.call(person2)(); // person2
person1.foo4().call(person2); // person1
```



### 面试题4

```js
var name = 'window';

function Person(name) {
    this.name = name;
    this.obj = {
        name: 'obj',
        foo1: function() {
            return function() {
                console.log(this.name);
            }
        },
        foo2: function() {
            return () => {
                console.log(this.name);
            }
        }
    }
}

var person1 = new Person('person1');
var person2 = new Person('person2');

person1.obj.foo1()(); // window
person1.obj.foo1.call(person2)(); // window
person1.obj.foo1().call(person2); // person2

person1.obj.foo2()(); //obj
person1.obj.foo2.call(person2)(); // person2
person1.obj.foo2().call(person2); // obj
```





===========================================================================

 ## 补充

直接使用箭头函数或使用bind



================================================



#### this指向问题

普通函数：谁调用指向谁

箭头函数：无自己的this，用词法作用域的this

类中的函数

* 构造函数：指向实例本身
* 普通函数：谁调用指向谁

定时器中的回调函数的this

* 因为setTimeout和setInterval是属于window对象的，即回调函数调用者是window，所以定时器回调函数的this指向window



### 箭头函数

#### 箭头函数的this

箭头函数没有自己的this，箭头函数的this在定义时决定，定义时会一层一层地往外查找this，直到遇到有this，箭头函数中的this就是这个查找到的this，即箭头函数中的this指向与箭头函数定义时最近的this指向相同

##### 情况枚举

箭头函数位于全局环境：this指向Window或undefined(严格模式)

箭头函数位于普通函数内

* 如果普通函数位于对象/类中，则this指向该对象/类实例
* 如果普通函数位于全局环境中，则this指向Window或undefined(严格模式)
* 如果普通函数使用了apply/call/bind等方法，则this指向这些方法指定的对象

##### 例子

```js
const obj = { name: "张三" };

function fn() {
    console.log(this.name);
    return () => {
        console.log(this.name);
    }
}

const resFn = fn.call(obj);
resFn();
```

在这个例子中，箭头函数定义的环境是fn函数，则箭头函数的this指向与fn函数的this指向相同

fn调用时，fn函数的this指向的是obj(因为使用了call方法)，所以箭头函数的this也指向obj

上述代码的输出结果是

```
张三
张三
```

##### this的使用场景

箭头函数可以固定this指向，箭头函数中的this在定义时就能确定，不随调用者不同而发生改变，这个功能常常用于将函数作为参数传递时使用

```js
setTimeout(function() {
  console.log(this); // Window 或 undefined(严格模式)
}, 1000);

let obj = {
  fn() {
    setTimeout(function() {
      console.log(this); // Window 或 undefined(严格模式)
    }, 1000);
   
    setTimeout(() => {
      console.log(this); // obj
    }, 1000);
  }
}
```



可以使用常量绑定this

```js
const self = this;
```



使用箭头函数绑定 this

apply call bind 方法详解

特殊写法

```js
const obj = {};
function foo() {
    
}.bind(obj);
```

bind有**两次**传参机会：定义时传参，使用时传参



### 普通函数的this

函数的this跟函数定义无关，不同于Java的静态绑定，JS中是动态绑定的

### 直接使用

this指向window || undefined

```js
function show() {
    console.log(this);
}

show(); // window || undefined
```

### 挂载到对象上，然后对象执行执行方法

this指向对象

```js
let arr = [1,2,3];
arr.fn = show;
arr.fn();
```

### 定时器

定时器的回调函数如果是普通函数，则一定是指向window的，因为定义器对象对象属于window

```js
setTimeout(show, 100);
```

### forEach也有二个参数 thisArg

```js
let arr = [1,2,3];

arr.forEach(function(item) {
    console.log(this, item);
},88);

// 88 1
// 88 2
// 88 3
```



