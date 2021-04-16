> 来源：http://javascript-puzzlers.herokuapp.com/

### 1.

```js
["1", "2", "3"].map(parseInt)
```

```js
A. ["1", "2", "3"]
B. [1, 2, 3]
C. [0, 1, 2]
D. other
```

```js
D

what you actually get is [1, NaN, NaN] because parseInt takes two parameters (val, radix) and map passes 3 (element, index, array)
```

### 2. 

```js
[typeof null, null instanceof Object]
```

```js
A. ["object", false]
B. [null, false]
C. ["object", true]
D. other
```

```js
答案：D

typeof will always return "object" for native non callable objects.
```

### 3. 

```js
[ [3,2,1].reduce(Math.pow), [].reduce(Math.pow) ]
```

```js
A. an error
B. [9, 0]
C. [9, NaN]
D. [9, undefined]
```

```js
A

Per spec: reduce on an empty array without an initial value throws TypeError
```

### 4. 

```js
var val = 'smtg';
console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');
```

```js
A. Value is Something
B. Value is Nothing
C. NaN
D. other
```

```js
D

it actually prints 'Something' the + operator has higher precedence than the ternary one.
```

### 5. 

```js
var name = 'World!';
(function () {
    if (typeof name === 'undefined') {
        var name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
})();
```

```js
A. Goodbye Jack
B. Hello Jack
C. Hello undefined
D. Hello World
```

```js
A

The var declaration is hoisted to the function scope, but the initialization is not.
```

### 6. 

```js
var END = Math.pow(2, 53);
var START = END - 100;
var count = 0;
for (var i = START; i <= END; i++) {
    count++;
}
console.log(count);
```

```js
A. 0
B. 100
C. 101
D. other
```

```js
D

it goes into an infinite loop, 2^53 is the highest possible number in javascript, and 2^53+1 gives 2^53, so i can never become larger than that.
```

### 7. 

```js
var ary = [0,1,2];
ary[10] = 10;
ary.filter(function(x) { return x === undefined;});
```

```js
A. [undefined × 7]
B. [0, 1, 2, 10]
C. []
D. [undefined]
```

```js
C

Array.prototype.filter is not invoked for the missing elements
```

### 8. 

```js
var two   = 0.2
var one   = 0.1
var eight = 0.8
var six   = 0.6
[two - one == one, eight - six == two]
```

```js
A. [true, true]
B. [false, false]
C. [true, false]
D. other
```

```js
C

JavaScript does not have precision math, even though sometimes it works correctly.
```

### 9.  

```js
function showCase(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase(new String('A'));
```

```js
A. Case A
B. Case B
C. Do not know!
D. undefined
```

```js
C

switch uses === internally and new String(x) !== x
```

### 10.  

```js
function showCase2(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase2(String('A')); 
```

```js
A. Case A
B. Case B
C. Do not know!
D. undefined
```

```js
A

String(x) does not create an object but does return a string, i.e. typeof String(1) === "string"
```

### 11. 

```js
function isOdd(num) {
    return num % 2 == 1;
}
function isEven(num) {
    return num % 2 == 0;
}
function isSane(num) {
    return isEven(num) || isOdd(num);
}
var values = [7, 4, '13', -9, Infinity];
values.map(isSane);
```

```js
A. [true, true, true, true, true]
B. [true, true, true, true, false]
C. [true, true, true, false, false]
D. [true, true, false, false, false]
```

```js
C

Infinity % 2 gives NaN, -9 % 2 gives -1 (modulo operator keeps sign so it's result is only reliable compared to 0)
```

### 12. 

```js
parseInt(3, 8)
parseInt(3, 2)
parseInt(3, 0)
```

```js
A. 3, 3, 3
B. 3, 3, NaN
C. 3, NaN, NaN
D. other
```

```js
D

3 doesn't exist in base 2, so obviously that's a NaN, but what about 0? parseInt will consider a bogus radix and assume you meant 10, so it returns 3.
```

### 13.

```js
Array.isArray( Array.prototype )
```

```js
A. true
B. false
C. error
D. other
```

```js
A

Array.prototype is an Array. Go figure.
```

### 14.

```js
var a = [0];
if ([0]) {
  console.log(a == true);
} else {
  console.log("wut");
}
```

```js
A. true
B. false
C. "wut"
D. other
```

```js
B

[0] as a boolean is considered true. Alas, when using it in the comparisons it gets converted in a different way and all goes to hell.
```

### 15. 

```js
[]==[]
```

```js
A. true
B. false
C. error
D. other
```

```js
B

== is the spawn of satan.
```

### 16.

```js
'5' + 3
'5' - 3
```

```js
A. "53", 2
B. 8, 2
C. error
D. other
```

```js
A

Strings know about + and will use it, but they are ignorant of - so in that case the strings get converted to numbers.
```

### 17.

```js
1 + - + + + - + 1
```

```js
A. 2
B. 1
C. error
D. other
```

```js
A

Great fun.
```

### 18. 

```js
var ary = Array(3);
ary[0]=2
ary.map(function(elem) { return '1'; });
```

```js
A. [2, 1, 1]
B. ["1", "1", "1"]
C. [2, "1", "1"]
D. other
```

```js
D

The result is ["1", undefined × 2], as map is only invoked for elements of the Array which have been initialized.
```

### 19.

```js
function sidEffecting(ary) {
  ary[0] = ary[2];
}
function bar(a,b,c) {
  c = 10
  sidEffecting(arguments);
  return a + b + c;
}
bar(1,1,1)
```

```js
A. 3
B. 12
C. error
D. other
```

```js
D

The result is 21, in javascript variables are tied to the arguments object so changing the variables changes arguments and changing arguments changes the local variables even when they are not in the same scope.
```

### 20.

```js
var a = 111111111111111110000,
    b = 1111;
a + b;
```

```js
A. 111111111111111111111
B. 111111111111111110000
C. NaN
D. Infinity
```

```js
B

Lack of precision for numbers in JavaScript affects both small and big numbers.
```

### 21.

```js
var x = [].reverse;
x();
```

```js
A. []
B. undefined
C. error
D. window
```

```js
D

[].reverse will return this and when invoked without an explicit receiver object it will default to the default this AKA window
```

### 22.

```js
Number.MIN_VALUE > 0
```

```js
A. false
B. true
C. error
D. other
```

```js
B

Number.MIN_VALUE is the smallest value bigger than zero, -Number.MAX_VALUE gets you a reference to something like the most negative number.
```

### 23.

```js
[1 < 2 < 3, 3 < 2 < 1]
```

```js
A. [true, true]
B. [true, false]
C. error
D. other
```

```js
A

This is parsed as (1 > 2) > 3 and (3 > 2) > 1. Than it's implicit conversions at work: true gets intified and is 1, while false gets intified and becomes 0.
```

### 24.

```js
// the most classic wtf
2 == [[[2]]]
```

```js
A. true
B. [true, false]
C. error
D. other
```

```js
A

both objects get converted to strings and in both cases the resulting string is "2"
```

### 25.

```js
3.toString()
3..toString()
3...toString()
```

```js
A. "3", error, error
B. "3", "3.0", error
C. error, "3", error
D. other
```

```js
C

3.x is a valid syntax to define "3" with a mantissa of x, toString is not a valid number, but the empty string is.
```

### 26.

```js
(function(){
  var x = y = 1;
})();
console.log(y);
console.log(x);
```

```js
A. 1, 1
B. error, error
C. 1, error
D. other
```

```js
C

y is an automatic global, not a function local one.
```

### 27.

```js
var a = /123/,
    b = /123/;
a == b
a === b
```

```js
A. true, true
B. true, false
C. false, false
D. other
```

```js
C

Per spec Two regular expression literals in a program evaluate to regular expression objects that never compare as === to each other even if the two literals' contents are identical.
```

### 28.

```js
var a = [1, 2, 3],
    b = [1, 2, 3],
    c = [1, 2, 4]
a ==  b
a === b
a >   c
a <   c
```

```js
A. false, false, false, true
B. false, false, false, false
C. true, true, false, true
D. other
```

```js
A

Arrays are compared lexicographically with > and <, but not with == and ===
```

### 29.

```js
var a = {}, b = Object.prototype;
[a.prototype === b, Object.getPrototypeOf(a) === b]
```

```js
A. [false, true]
B. [true, true]
C. [false, false]
D. other
```

```js
A

Functions have a prototype property but other objects don't so a.prototype is undefined.
Every Object instead has an internal property accessible via Object.getPrototypeOf
```

### 30.

```js
function f() {}
var a = f.prototype, b = Object.getPrototypeOf(f);
a === b
```

```js
A. true
B. false
C. null
D. other
```

```js
B

f.prototype is the object that will become the parent of any objects created with new f while Object.getPrototypeOf returns the parent in the inheritance hierarchy.
```

### 31. 

```js
function foo() { }
var oldName = foo.name;
foo.name = "bar";
[oldName, foo.name]
```

```js
A. error
B. ["", ""]
C. ["foo", "foo"]
D. ["foo", "bar"]
```

```js
C

name is a read only property. Why it doesn't throw an error when assigned, I do not know.
```

### 32.

```js
"1 2 3".replace(/\d/g, parseInt)
```

```js
A. "1 2 3"
B. "0 1 2"
C. "NaN 2 3"
D. "1 NaN 3"
```

```js
D

String.prototype.replace invokes the callback function with multiple arguments where the first is the match, then there is one argument for each capturing group, then there is the offset of the matched substring and finally the original string itself. so parseInt will be invoked with arguments [1, 0], [2, 2], [3, 4].
```

### 33.

```js
function f() {}
var parent = Object.getPrototypeOf(f);
f.name // ?
parent.name // ?
typeof eval(f.name) // ?
typeof eval(parent.name) //  ?
```

```js
A. "f", "Empty", "function", "function"
B. "f", undefined, "function", error
C. "f", "Empty", "function", error
D. other
```

```js
C

The function prototype object is defined somewhere, has a name, can be invoked, but it's not in the current scope.
```

### 34.

```js
var lowerCaseOnly =  /^[a-z]+$/;
[lowerCaseOnly.test(null), lowerCaseOnly.test()]
```

```js
A. [true, false]
B. error
C. [true, true]
D. [false, true]
```

```js
C

the argument is converted to a string with the abstract ToString operation, so it is "null" and "undefined".
```

### 35.

```js
[,,,].join(", ")
```

```js
A. ", , , "
B. "undefined, undefined, undefined, undefined"
C. ", , "
D. ""
```

```js
C

JavaScript allows a trailing comma when defining arrays, so that turns out to be an array of three undefined.
```

### 36.

```js
var a = {class: "Animal", name: 'Fido'};
a.class
```

```js
A. "Animal"
B. Object
C. an error
D. other
```

```js
D

The answer is: it depends on the browser. class is a reserved word, but it is accepted as a property name by Chrome, Firefox and Opera. It will fail in IE. On the other hand, everybody will accept most other reserved words (int, private, throws etc) as variable names too, while class is verboten.
```

### 37.

```js
var a = new Date("epoch")
```

```js
A. Thu Jan 01 1970 01:00:00 GMT+0100 (CET)
B. current time
C. error
D. other
```

```js
D

You get "Invalid Date", which is an actual Date object (a instanceof Date is true). But invalid. This is because the time is internally kept as a Number, which in this case it's NaN.
```

### 38.

```js
var a = Function.length,
    b = new Function().length
a === b
```

```js
A. true
B. false
C. error
D. other
```

```js
B

It's false. Function.length is defined to be 1. On the other hand, the length property of the Function prototype object is defined to be 0.
```

### 39.

```js
var a = Date(0);
var b = new Date(0);
var c = new Date();
[a === b, b === c, a === c]
```

```js
A. [true, true, true]
B. [false, false, false]
C. [false, true, false]
D. [true, false, false]
```

```js
B

When Date is invoked as a constructor it returns an object relative to the epoch (Jan 01 1970). When the argument is missing it returns the current date. When it is invoked as a function, it returns a String representation of the current time.
```

### 40.

```js
var min = Math.min(), max = Math.max()
min < max
```

```js
A. true
B. false
C. error
D. other
```

```js
B

Math.min returns +Infinity when supplied an empty argument list. Likewise, Math.max returns -Infinity.
```

### 41.

```js
function captureOne(re, str) {
  var match = re.exec(str);
  return match && match[1];
}
var numRe  = /num=(\d+)/ig,
    wordRe = /word=(\w+)/i,
    a1 = captureOne(numRe,  "num=1"),
    a2 = captureOne(wordRe, "word=1"),
    a3 = captureOne(numRe,  "NUM=2"),
    a4 = captureOne(wordRe,  "WORD=2");
[a1 === a2, a3 === a4]
```

```js
A. [true, true]
B. [false, false]
C. [true, false]
D. [false, true]
```

```js
C

Regular expressions in JavaScript if defined using the /g flag will carry a state across matches, even if they are actually used on different strings (the lastIndex property). This means a3 will be null as the regular expression was applied starting from the index of the last matched string, even if it was a different one.
```

### 42.

```js
var a = new Date("2014-03-19"),
    b = new Date(2014, 03, 19);
[a.getDay() === b.getDay(), a.getMonth() === b.getMonth()]
```

```js
A. [true, true]
B. [true, false]
C. [false, true]
D. [false, false]
```

```js
D

JavaScript inherits 40 years old design from C: days are 1-indexed in C's struct tm, but months are 0 indexed. In addition to that, getDay returns the 0-indexed day of the week, to get the 1-indexed day of the month you have to use getDate, which doesn't return a Date object.
```

### 43.

```js
if ('http://giftwrapped.com/picture.jpg'.match('.gif')) {
  'a gif file'
} else {
  'not a gif file'
}
```

```js
A. 'a gif file'
B. 'not a gif file'
C. error
D. other
```

```js
A

String.prototype.match silently converts the string into a regular expression, without escaping it, thus the '.' becomes a metacharacter matching '/'.
```

### 44.

```js
function foo(a) {
    var a;
    return a;
}
function bar(a) {
    var a = 'bye';
    return a;
}
[foo('hello'), bar('hello')]
```

```js
A. ["hello", "hello"]
B. ["hello", "bye"]
C. ["bye", "bye"]
D. other
```

```js
B

Variabled declarations are hoisted, but in this case since the variable exists already in the scope, they are removed altogether. In bar() the variable declaration is removed but the assignment remains, so it has effect.
```