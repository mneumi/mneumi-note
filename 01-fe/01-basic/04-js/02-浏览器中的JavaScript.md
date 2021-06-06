## 浏览器中 JavaScript 的组成部分

1. ECMAScript：由ECMA-262定义并提供核心功能
2. DOM - Document Object Model：提供与网页内容进行交互的方法和接口
3. BOM - Browser Object Model：提供与浏览器交互的方法和接口



## 引入JavaScript

将JavaScript引入到HTML的方法主要是通过`script`标签

### 外部脚本

引入外部脚本时，使用的是`HTTP`的`GET`请求，不受同源策略限制

```html
<script src="https://example.com/foo.js"></script>
```

### 行内脚本

```html
<script>
  console.log("Hello World");
</script>
```

### 不能两者混用

如果script标签使用的外部脚本，那么就不应该再写行内脚本，如果两者都提供，则浏览器只会下载并使用外部脚本，忽略行内脚本

```html
<script src="https://example.com/foo.js">
  alert("这行代码会被忽略");
</script>
```



## script标签

### script标签的正确写法

符合规范的写法

```html
<script type="text/javascript" src="example.js"></script>
```

不符合规范的写法

```html
<script type="text/javascript" src="example.js" />
```

### script标签的位置

按照传统的做法，所有`<script>`元素都应该放在页面的`<head>`元素中这种做法的目的就是把所有外部文件（包括 CSS 文件和 JavaScript 文件）的引用都放在相同的地方

但将 `<script>` 全部放到 `<header>` 标签中有存在这样的问题：必须等到全部JavaScript代码都被下载、解析和执行完成以后，才能开始呈现页面的内容（因为浏览器在遇到body标签时才开始渲染内容）

这对于那些需要很多JavaScript代码的页面来说，这无疑会导致浏览器在呈现页面时出现明显的延迟，而延迟期间的浏览器窗口中将是一片空白

解决方法：现代 Web 应用程序一般都把全部JavaScript引用放在body元素中页面内容的后面，这样，在解析包含的 JavaScript代码之前，页面的内容将完全呈现在浏览器中。而用户也会因为浏览器窗口显示空白页面的时间缩短而感到打开页面的速度加快了

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Example HTML Page</title>
    </head>
    <body>
        <!-- 这里是页面内容 -->
        <script src="https://example.com/foo.js"></script>
        <script src="https://example.com/bar.js"></script>
    </body>
</html> 
```

###  script标签的注意事项

在使用script标签嵌入 JavaScript 代码时，记住不要在代码中的任何地方出现`</script>`字符串，例如，浏览器在加载下面所示的代码时就会产生一个错误：

```html
<script type="text/javascript">
    function sayScript(){
        alert("</script>");
    }
</script>
```

而通过转义字符“/”可以解决这个问题，例如：

```html
<script type="text/javascript">
    function sayScript(){
        alert("<\/script>");
    }
</script> 
```

###  行内脚本与外部脚本对比

使用外部脚本更好

1. 更好的可维护性
2. 外部脚本文件可缓存
3. 更好的封装性和解耦

### type属性

type属性的默认值是`text/javascript`，所以可以省略type属性，这个属性基本没有作用，不推荐写

```html
<script type="text/javascript"></script>

<!-- 不写 type 也可以 -->
<script></script>
```

###  src属性

带有 src 属性的script标签不应该在其`<script>`和`</script>`之间再包含额外的 JavaScript 代码

如果包含了嵌入的代码，则只会下载并执行外部脚本文件，嵌入的代码会被忽略

src 属性可以包含来自外部域的 JavaScript 文件，即能够实现跨域（JSONP的实现原理）

```html
<script src="https://example.com/index.js"></script>
```

### defer属性

在`<script>`元素中设置defer 属性，相当于告诉浏览器立即下载，但延迟执行——脚本会被延迟到整个页面都解析完毕后再运行

理解：立即下载，同步执行，但延迟执行

执行时机：页面解析完毕之后，DOMContentLoaded事件之前

顺序：多个defer外部脚本之间也是按照顺序执行的，即第一个推迟的脚本会在第二个推迟的脚本之前执行

注意：只对外部脚本生效，即如果既写了外部脚本，又写了行内脚本，则浏览器会忽略行内脚本

```html
<!-- 同步与顺序的体现 -->
<script src="https://example.com/defer1.js" defer></script> <!-- 第1个推迟执行的脚本，也是第4个执行的脚本 -->
<script src="https://example.com/foo.js"></script> <!-- 第1个执行的脚本 -->
<script src="https://example.com/defer2.js" defer></script> <!-- 第2个推迟执行的脚本，也是第5个执行的脚本 -->
<script>
  console.log("第二个执行的脚本")
</script>
<script src="https://example.com/bar.js"></script> <!-- 第3个执行的脚本 -->
<script src="https://example.com/defer3.js" defer></script> <!-- 第3个推迟执行的脚本，也是第6个执行的脚本 -->

<!-- 混用时情况 -->
<script src="https://example.com/defer1.js" defer>
  console.log("行内与外部脚本混用时，script标签会忽略外部脚本与defer属性，只执行行内脚本");
</script>
```

在现实当中，延迟脚本并不一定会按照顺序执行，也不一定会在 DOMContentLoaded 事件触发前执行，因此最好只包含一个延迟脚本。

### async属性

在`<script>`元素中设置async属性，相当于告诉浏览器立即下载，并立即异步执行——每次执行的顺序可能不一样

理解：立即下载，立即异步执行

执行时机：下载完毕后，立即执行

顺序：由于是异步执行，所以无法保证顺序

场景：由于不能保证顺序，所以异步脚本不要依赖其他的脚本，也不要含有修改DOM的操作

异步加载脚本

```html
<script async="async" src="example1.js"></script>
<script async="async" src="example2.js"></script>
```

不能保证上述的`example1.js`和`example2.js`的执行顺序

### integrity属性

作用：用于比对接收到的资源与integrity指定的加密签名以验证资源的完整性，如果接收到的资源与这个属性指定的签名不匹配，则页面会报错

场景：这个属性可以用于确保内容分发网络（CDN）不会被篡改和提供恶意资源

```html
<script></script>
```

### crossorigin属性

作用：用于设定相关请求的CORS设置，默认不使用CORS

可选值：

* crossorigin="anonymous"，使用CORS，且不携带凭据
* crossorigin="use-credentials"，使用CORS，且携带凭据

```html
<!-- 不使用 CORS -->
<script src="example1.js"></script>

<!-- 使用 CORS，不携带凭据 -->
<script src="example1.js" crossorigin="anoymous"></script>

<!-- 使用 CORS，携带凭据 -->
<script src="example1.js" crossorigin="use-credential"></script>
```



## 动态加载脚本

可以使用 JavaScript 创建 script 标签，从而动态加载脚本

这种加载方式的script标签，默认会设置async属性，即异步加载

```js
const script = document.createElement("script");
script.src = "https://example.com/foo.js";
document.body.appendChild(script);
```

显式取消异步加载

```js
const script = document.createElement("script");
script.src = "https://example.com/foo.js";
script.async = false;
document.body.appendChild(script);
```



## noscript标签

早期浏览器都面临一个特殊的问题，即当浏览器不支持JavaScript时如何让页面平稳地退化

对这个问题的最终解决方案就是创造一个\<noscript>元素，用以在不支持 JavaScript 的浏览器中显示替代的内容

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Example HTML Page</title>
        <script type="text/javascript" defer="defer" src="example1.js"></script>
        <script type="text/javascript" defer="defer" src="example2.js"></script>
    </head>
    <body>
        <noscirpt>
            您的浏览器不支持脚本，请开启脚本功能或更换更高级的浏览器
        </noscript>
    </body>
</html> 
```

包含在noscript元素中的内容只有在下列情况下才会显示出来

1. 浏览器不支持脚本
2. 浏览器支持脚本，但脚本被禁用
