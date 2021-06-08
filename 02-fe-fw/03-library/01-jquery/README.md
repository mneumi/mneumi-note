## JavaScript 库

### 概述

JavaScript库，即library，它是一种**封装好特定功能的集合**

### 常见的JavaScript库

`jQuery`，`Prototype`，`YUI`，`Dojo`，`ExtJS`，`zepto`



## 初识jQuery

### 概述

`jQuery`是一个快速、简洁的 JavaScript 库，其宗旨是`Write Less, Do More`

`jQuery`封装了 JavaScript 常用的功能代码，涵盖了 DOM 操作，事件处理，动画设计，Ajax交互等功能

### 特点

* 轻量级，核心文件大小为几十kb
* 跨浏览器兼容
* 链式编程，隐式迭代
* 支持插件扩展
* 免费且开源

### 版本介绍

| 版本 | 说明                              |
| ---- | --------------------------------- |
| 1.x  | 兼容 IE 6 7 8，不再更新维护       |
| 2.x  | 不兼容 IE 6 7 8，不再更新维护     |
| 3.x  | 不兼容 IE 6 7 8，官方主力维护版本 |



## 引入jQuery

### 下载与CDN

[官方下载网址](https://jquery.com)

[常用CDN](https://www.bootcdn.cn/jquery/)

### 通过 script 标签

```html
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.js"></script>
```

### 通过npm

```shell
npm install jquery --save
# OR
yarn add jquery
```

```js
const jquery = require("jquery");
const $ = require("jquery");
```



## 入门使用

### jQuery顶级对象 $

`$`是`jQuery`的别称，可以在代码中使用 `jQuery`代替`$`，但一般为了方便，通过都是直接使用 `$`

多库共存问题：jQuery使用`$`作为标识符，而其他的`JavaScript`库也可能使用`$`作为标识符，这会造成冲突，此时使用 `jQuery`，而不是 `$`来解决冲突

### jQuery入口函数

`DOMContentLoaded`事件：等待DOM加载完成

```js
$(function() {
    // code here，此处是DOM加载完成的入口
});

// 等价于下列的写法

jQuery(function() {
    // code here，此处是DOM加载完成的入口
});

// 等价于下列的写法

$(document).ready(function() {
    // code here，此处是DOM加载完成的入口
});

// 等价于下列的写法

jQuery(document).ready(function() {
    // code here，此处是DOM加载完成的入口
});
```

`JavaScript`入口函数

```js
window.onload = function() {
    // code here，此处是页面全部加载完成的入口
};
```

### jQuery对象和DOM对象

#### DOM对象转为jQuery对象

使用 `$(选择器)` 来获取 jQuery 对象

```js
const obj1 = $("div");
const obj2 = $("span");
const obj3 = $("#main");
const obj4 = $(DOM对象)
```

#### jQuery对象转为DOM对象

使用 `[]` 索引或者 `get` 索引

```js
$("div")[index];
$("div").get(index);
```



## 选择器

### 概述

原生 JS 获取元素的方式有很多，且兼容情况不一样，因此，jQuery封装了统一的选择元素的方法

### 基础选择器

| 选择器     | 示例            | 说明                     |
| ---------- | --------------- | ------------------------ |
| ID选择器   | $("#id")        | 获取指定ID的元素         |
| 全选选择器 | $("*")          | 匹配所有元素             |
| 类选择器   | $(".class")     | 获取同一类class的元素    |
| 标签选择器 | $("div")        | 获取同一类标签的所有元素 |
| 并集选择器 | $("div,p,li")   | 选取多个元素             |
| 交集选择器 | $("li.current") | 交集元素                 |
| 子代选择器 | $("ul>li")      | 获取亲子级元素           |
| 后代选择器 | $("ul li")      | 获取所有后代元素         |

### 筛选选择器

| 筛选选择器 | 示例          | 说明                                       |
| ---------- | ------------- | ------------------------------------------ |
| :first     | $("li:first") | 获取第一个li元素                           |
| :last      | $("li:last")  | 获取最后一个li元素                         |
| :eq(index) | $("li:eq(2)") | 获取到的li元素中，选择索引号为2的li元素    |
| :odd       | $("li:odd")   | 获取到的li元素中，选择索引号为奇数的li元素 |
| :even      | $("li:even")  | 获取到的li元素中，选择索引号为偶数的li元素 |

```js
$(function() {
    $("ul li:first").css("color", "red");
    $("ul li:last").css("color", "green");
    $("ul li:eq(2)").css("color", "blue");
    $("ul li:odd").css("color", "purple");
    $("ul li:even").css("color", "orange");
})
```

### 筛选方法

| 筛选方法               | 示例                           | 说明                               |
| ---------------------- | ------------------------------ | ---------------------------------- |
| **parent()**           | $("li").parent();              | 查找父级                           |
| **children(selector)** | $("ul").children("li")         | 相当于`$("ul>li")`，子级选择器     |
| **find(selector)**     | $("ul").find("li")             | 相当于`$(ul li)`，后代选择器       |
| **siblings(selector)** | $(".first").siblings("li")     | 查找兄弟节点，不包括自己本身       |
| **eq(index)**          | $("li").eq(2)                  | 相当于`$("li:eq(2)")`              |
| nextAll([expr])        | $(".first").nextAll()          | 查找当前元素之后所有同辈元素       |
| prevAll([expr])        | $(".first").prevAll()          | 查找当前元素之前所有同辈元素       |
| hasClass(class)        | $("div").hasClass("container") | 检查当前的元素是否含有某个特定的类 |



## 显示与隐藏操作

### 概述

`jQuery`能够设置元素的 `display` 属性为 `block` 或者 `hidden` 来显示或隐藏元素

对应的方法为：`show`和`hide`

### 示例代码

```js
$(function() {
    $("card").show();
    setTimeout(() => {
        $("card").hide();
    }, 2000);
});
```



## 样式操作

### style操作

#### 核心API

```js
// key-value形式
$("选择器").css("属性", "值");

// 样式对象形式
$("选择器").css({ /*样式对象 */ });
```

#### 使用

参数只写属性名，功能是返回属性值

```js
$(this).css("color");
```

参数是属性名和值，则是设置对应的样式

```js
$(this).css("color", "red");
```

参数是样式对象

```js
$(this).css({"color": "white", "font-size": "20px"});
```

### class操作

#### 添加class

```js
$("div").addClass("current");
```

#### 移除class

```js
$("div").removeClass("current");
```

#### 切换class

```js
$("div").toggleClass("current");
```

#### 对比原生className

原生 JS 中 className 会覆盖元素理的class

jQuery中的class操作只针对特定的class，不会影响原有的 class



## 内容与文本值操作

### 元素内容

#### 概述

相当于原生JS中的`innerHTML`

#### 获取元素的内容

API：`html()`

```js
const innerHTML = $("#main").html();

console.log(innerHTML);
```

#### 设置元素的内容

API：`html("内容")`

```js
$("#main").html("<h2>Hello jQuery</h2>");
```

### 文本内容

#### 概述

相当于原生JS中的`innerText`

#### 获取文本内容

API：`text()`

```js
const innerText = $("#main").text();

console.log(innerText);
```

#### 设置文本内容

API：`text("内容")`

```js
$("#main").text("Hello jQuery");
```

### 表单内容

#### 概述

针对元素的内容和表单的值操作，相当于原生JS中的 `value`

#### 获取表单内容

API：`val()`

```js
$("input").val();
```

#### 设置表单内容

API：`val(值)`

```js
$("input").val("123");
```



## 属性操作

### 元素固有属性值

#### 概述

所谓元素固有属性就是元素本身自带的属性，是DOM标准中写明的

#### 获取属性值

API：`prop("属性名")`

```js
$("a").prop("href");
```

#### 设置属性值

API：`prop("属性名", "属性值")`

```js
$("#main").prop("title", "newTitle");
```

### 自定义属性值

#### 概述

自定义属性是指用户自己给元素添加的属性

#### 获取属性值

API：`attr("属性名")`，类似于原生JS中的 `getAttribute()`

```js
$("#main").prop("key");
```

#### 设置属性值

API：`attr("属性名", "属性值")`，类似于原生JS中的 `setAttribute()`

```js
$("#main").prop("key", "fdsfdjjsg");
```

### data属性

#### 概述

data属性是指用户自定义的，以`data-`开头的属性

#### 获取data属性

API：`data("属性名")`

```js
$("#main").data("name");
```

#### 设置data属性

API：`data("属性名", "属性值")`

```js
$("#main").data("name", "mneumi");
```



## 元素操作

### 遍历元素

#### 方式1：$(选择器).each

注意：domElem是DOM对象，而不是jQuery对象，如果想使用jQuery的方法，则需要转为jQuery对象

```js
$(选择器).each(function (index, domElem) {
    // code here
    const jqElem = $(domElem); // 将 DOM 对象转为 jQuery 对象
});
```

#### 方式2：$.each

`$.each`方法可以用于遍历任何对象，主要用于数组和对象的处理

注意：domElem是DOM对象，而不是jQuery对象，如果想使用jQuery的方法，则需要转为jQuery对象

```js
$.each(arr, function (index, domElem) {
    // code here
    const jqElem = $(domElem); // 将 DOM 对象转为 jQuery 对象
});
```

### 创建元素

`$()`中写`HTML`标签即可

```js
$("<li></li>"); // 动态创建了一个 <li>
```

### 添加元素

#### 内部添加

```js
element.append("内容"); // 相当于 appendChild
element.prepend("内容"); // 相当于 prependChild
```

#### 外部添加

```js
element.after("内容"); // 把内容放在目标元素的后面
element.before("内容"); // 把内容放在目标元素的前面
```

### 删除元素

```js
element.remove(); // 删除匹配的元素本身
element.empty(); // 清空所有子节点
element.html(""); // 清空元素内容
```



## 尺寸操作

### 概述

| API                                  | 说明                                                  |
| ------------------------------------ | ----------------------------------------------------- |
| width / height()                     | 取得匹配元素宽度和高度值，只算 width / height         |
| innerWidth() / innerHeight()         | 取得匹配元素宽度和高度值，包括padding                 |
| outerWidth() / outerHeight()         | 取得匹配元素宽度和高度值，包括padding，border         |
| outerWidth(true) / outerHeight(true) | 取得匹配元素宽度和高度值，包括padding，border，margin |

如果参数为空，则获取相应值，返回的是纯数字，不带`px`

如果参数为数字，则修改对应值，参数不必写单位

### 示例代码

```js
$("#main").width();
$("#main").innerHeight(300);
$("#main").outerWidth(200);
$("#main").outerHeight(true);
$("#main").outerHeight(false, 500);
```



## 位置操作

### offset

#### 概述

offset方法设置或返回被选元素**相对于文档**的偏移坐标，**跟父级没有关系**，与**offset系列**不同

#### 属性

| 属性          | 说明                   |
| ------------- | ---------------------- |
| offset().left | 获取距离文档左侧的距离 |
| offset().top  | 获取距离文档顶部的距离 |

#### 设置元素的偏移

```js
offset({ top: 10, left: 30 });
```

### position

#### 概述

position方法用于返回被选元素**相对于带有父级定位**的偏移坐标，如果父级都没有定位，则以文档为准

注意：只能获取，不能设置偏移

#### 示例代码

```js
$("#main").position()
```

### scrollTop/scrollLeft

#### 概述

`scrollTop / scrollLeft` 获取或设置元素被卷去的头部和左侧

#### 示例代码

```js
$(function() {
    const boxTop = $(".container").offset().top;
    $(window).scroll(function() {
        if ($(document).scrollTo() >= boxTop) {
            $(".back").fadeIn();
        } else {
            $(".back").fadeOut();
        }
     })
})
```

```js
$(function() {
    $(document).scrollTop(100); // 发生变化的一行
    
    const boxTop = $(".container").offset().top;
    $(window).scroll(function() {
        if ($(document).scrollTo() >= boxTop) {
            $(".back").fadeIn();
        } else {
            $(".back").fadeOut();
        }
     })
})
```



## 深拷贝/合并操作

### 概述

如果想把某个对象拷贝（或合并）给另外一个对象使用，则可以使用 `$.extend` 方法

### 语法

```js
$.extend([deep], target, object1, [objectN]);
```

| 参数    | 说明                                                |
| ------- | --------------------------------------------------- |
| deep    | 可选，如果为true，则为深拷贝，默认为false，即浅拷贝 |
| target  | 要拷贝的目标对象                                    |
| object1 | 待拷贝到的第一个对象                                |
| objectN | 待拷贝到的第N个对象                                 |

### 示例代码

```js
const settings = { validate: false, limit: 5, name: "foo" }; 
const options = { validate: true, name: "bar" }; 

$.extend(settings, options);

console.log(settings);
// { validate: true, limit: 5, name: "bar" }
```



## 事件处理

### 事件注册

#### 单个处理函数注册

```js
element.事件名(function(event) {})
```

```js
$("div").click(function(event) { /* 事件处理程序 */ })
```

#### 多个处理函数注册：字符串模式

```js
element.on(events, [selector], fn)
```

| 参数     | 说明                                              |
| -------- | ------------------------------------------------- |
| events   | 一个或多个用空格分隔的事件类型，如 click，keydown |
| selector | 元素的子元素选择器，实际触发对象                  |
| fn       | 回调函数，即处理函数                              |

示例代码：事件委托

```js
// click 是绑定到 ul 上的，但是触发的对象是 ul 里面的 li
$("ul").on("mouseover mouseout", "li", function(event) {
    alert(event);
});
```

#### 多个处理函数注册：对象模式

```js
element.on({
    事件名1: 事件处理函数,
    事件名2: 事件处理函数,
    事件名3: 事件处理函数
});
```

示例代码

```js
$("div").on({
    mouseenter: function(event) {
        $(this).css("background", "skyblue");
    },
    click: function(event) {
        $(this).css("background", "purple");
    },
    mouseleave: function(event) {
        $(this).css("background", "blue");
    }
})
```

### 事件解绑

`off`方法可以移除通过`on`方法添加的事件处理程序

#### 函数签名

```js
off([事件名], [委托对象])
```

#### 示例代码

```js
$("p").off(); // 解绑 p 元素所有的事件处理程序
$("p").off("click"); // 解绑 p 元素上的click事件的处理程序
$("p").off("click", "li"); // 解绑 p 元素所有的事件处理程序
```

### 只触发一次的事件

如果有的事件只想触发一次，可以使用`one`来绑定时间

```js
$("p").one("click", function(event) {
    alert("just execute once");
});
```

### 事件自动触发

有些事件希望自动触发，比如轮播图的自动播放功能

#### 方式1：配合定时器使用`click`事件

#### 方式2：使用`trigger`方法

```js
element.trigger("eventName");
// 示例
$("p").trigger("click");
```

#### 方式3：使用`triggerHandler`方法

特点：不会触发元素的默认行为

```js
element.triggerHanler("eventName");
// 示例
$("p").triggerHanler("click");
```



## 隐式迭代

### 概述

`jQuery`会自动给匹配到的所有元素进行遍历，执行相应方法，这个过程称为**隐式迭代**

### 获取元素索引号

由于`jQuery`会进行隐式迭代，对于这些元素的操作，需要依赖其索引号，所以可以使用`index`来获取元素对应的索引号

```js
$(function() {
	$("#left li").mouseover(function() {
        const currentIndex = $(this).index();
        console.log(currentIndex);
    }) 
});
```

### 示例代码

```html
<div>Line 1</div>
<div>Line 2</div>
<div>Line 3</div>
<ul>
    <li>Li One</li>
    <li>Li Two</li>
    <li>Li Three</li>
</ul>
<div>Line 4</div>
<div>Line 5</div>
```

```js
// 给所有的 div 设置背景颜色为 red
$("div").css("background", "red");

// 给所有的 li 设置 color 为 orange
$("ul li").css("color", "orange");
```



## 动画效果

### 显示隐藏

#### 函数签名

`show` / `hide` / `toggle`

```js
show([speed, [easing], [fn]])
hide([speed, [easing], [fn]])
toggle([speed, [easing], [fn]])
```

#### 参数说明

| 参数   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| speed  | 三种预设字符串 `slow/nomal/fast` 或者表示动画时长的毫秒数，如1000 |
| easing | 切换效果，默认为 `swing`，可用参数 `linear`                  |
| fn     | 回调函数，在动画完成时执行的函数，每个元素执行一次           |

### 淡入淡出

#### 函数签名

`fadeIn` / `fadeOut` / `fadeToggle` / `fadeTo`

```js
slideUp([speed, [easing], [fn]])
slideDown([speed, [easing], [fn]])
slideToggle([speed, [easing], [fn]])
slideToggle([[speed], opcity, [easing], [fn]])
```

#### 参数说明

| 参数   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| speed  | 三种预设字符串 `slow/nomal/fast` 或者表示动画时长的毫秒数，如1000 |
| easing | 切换效果，默认为 `swing`，可用参数 `linear`                  |
| fn     | 回调函数，在动画完成时执行的函数，每个元素执行一次           |
| opcity | 透明度，取值 0-100                                           |

### 滑动

#### 函数签名

`slideUp` / `slideDown` / `slideToggle`

```js
slideUp([speed, [easing], [fn]])
slideDown([speed, [easing], [fn]])
slideToggle([speed, [easing], [fn]])
```

#### 参数说明

| 参数   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| speed  | 三种预设字符串 `slow/nomal/fast` 或者表示动画时长的毫秒数，如1000 |
| easing | 切换效果，默认为 `swing`，可用参数 `linear`                  |
| fn     | 回调函数，在动画完成时执行的函数，每个元素执行一次           |

### 自定义动画

#### 函数签名

```js
animate(params, [speed], [easing], [fn])
```

#### 参数说明

| 参数   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| params | 想要改变的样式属性，以对象形式传递，必填                     |
| speed  | 三种预设字符串 `slow/nomal/fast` 或者表示动画时长的毫秒数，如1000 |
| easing | 切换效果，默认为 `swing`，可用参数 `linear`                  |
| fn     | 回调函数，在动画完成时执行的函数，每个元素执行一次           |

#### 示例代码

```js
$(function() {
    $("button").click(function() {
        $("div").animate({
            left: 500,
            top: 300,
            opacity: .4,
            width: 500
        }, 500)
    })
})
```

### 动画队列

动画或者效果一旦触发就会执行，如果多次触发，就会造成多个动画或者效果排队执行

停止动画或效果：`stop()`

注意：`stop()`写道动画或者效果的前面，相当于停止结束上一次的动画

```js
$(".nav>li").hover(function() {
    // stop 方法必须写到动画的前面
    $(this).children("ul").stop().slideToggle();
})
```



## jQuery插件

### 概述

jQuery本身功能是对DOM操作的封装，功能具有普适性，但不能对某些领域提供很好的帮助

为了解决某些领域的增强，可以使用 jQuery 插件来增强 jQuery 的功能

### 使用前提

jQuery插件是对 jQuery 的增强，所以在引入插件之前，需要先引入jQuery本身

### 使用步骤

1. 引入相关文件（jQuery文件和插件文件）
2. 参考插件的示例HTML、CSS和JS代码

### 常用的插件网站

1. jQuery插件库：http://www.jq22.com
2. jQuery之家：http://www.htmleaf.com



## 案例分析

### 案例1

#### 效果

Tab栏切换

#### 实现分析

* 点击 li 元素，为当前 li 添加 current 类，其他兄弟移除 current 类
* 点击的同时，获取当前 li 的索引号
* 让下部里面相应索引号的item显示，其他的item隐藏

#### 实现代码

```js
$(function() {
    $(".tab_list li").click(function() {
        $(this).addClass("current").siblings().removeClass("current");
        const index = $(this).index();
        $(".tab_con .item").eq(index).show().siblings().hide();
    })
})
```

### 案例2

#### 效果

带有动画的返回顶部

#### 实现分析

* 核心原理：使用animate动画返回顶部

* animate动画函数中有`scrollTop`属性，可以设置位置

#### 实现代码

```js
$(".back").click(function() {
    $("body, html").stop().animate({
        scrollTop: 0
    })
})
```

### 案例3

#### 效果

鼠标划过某个元素，在其他区域显示该元素的信息

#### 实现分析

* 核心原理：鼠标经过盒子的某个li，就让内容区盒子相应图片显示，其余的图片隐藏

* 通过 index() 获取索引号，通过 eq 获取对应的元素

* 显示元素show()，隐藏元素hide()

#### 实现代码

```js
$(function() {
    $("#left li").mouseover(function() {
        const index = $(this).index();
        $("#content div").eq(index).show();
        $("#content div").eq(index).siblings().hide();
    })
})
```

### 案例4

#### 效果

手风琴（折叠卡片）效果

#### 实现分析

* 鼠标经过某个 li 时，有两步操作

* 当前 li 宽度变为 224px，同时里面的小图片淡出，大图片淡入

* 其他 li 宽度变为 69px，小图片淡入，大图片淡出

#### 实现代码

```js
$(function() {
    $(".container li").mouseenter(function() {
        $(this).stop().animate({
            width: 224
        }, 200).find(".small").stop().fadeOut().siblings(".big").stop().fadeIn();
        
        $(this).siblings("li").stop().animate({
            width: 69
        }).find(".small").stop().fadeIn().siblings(".big").stop.fadeOut();
    })
})
```