## DOM介绍

### HTML文档与DOM的关系

HTML文档本质上只是一个字符串，并不能直接由它渲染出页面

浏览器会解析HTML文档，根据HTML文档字符串生成一个树状模型，该树状模型上的每个节点与HTML文档的标签、属性和文本一一对应

该树状模型称为DOM（文档对象模型 - Document Object Model），浏览器会使用DOM完成页面的渲染

### 常用DOM节点

文档本身：document(根节点)

html标签：document.documentElement

body标签：document.body

### DOM元素

DOM中由标签映射出来的节点可以称为DOM元素，即DOM元素是特殊的DOM节点

DOM元素拥有属性和方法，用户可以通过操作这些属性和方法来改变页面的样式和行为

![](./images/image-20201012023246454.png)

### 获取DOM元素方法

| 方法                     | 示例                                        | 返回值                                       | 找不到         | 备注                 |
| ------------------------ | ------------------------------------------- | -------------------------------------------- | -------------- | -------------------- |
| 根据id获取元素           | document.getElementById("id值")             | 一个元素对象，因为id是唯一的                 | 返回undefined  | document对象特有方法 |
| 根据类名获取元素em       | em.getElementsByClassName("类名")           | 元素对象的集合，以伪数组的形式存储           | 返回空的伪数组 |                      |
| 根据标签名获取元素em     | em.getElementsByTagName("标签名")           | 元素对象的集合，以伪数组的形式存储           | 返回空的伪数组 |                      |
| 根据选择器获取第一个元素 | em.querySelector("选择器")                  | 指定选择器的第一个元素对象                   | 返回undefined  |                      |
| 根据选择器获取所有元素   | em.querySelectorAll("选择器")               | 指定选择器的所有元素对象，以伪数组的形式存储 | 返回空的伪数组 |                      |
| 特殊标签元素的获取       | document.body<br />document.documentElement | body标签元素<br />html标签元素               |                |                      |
| 通过节点操作获取         | em.parentNode                               | 父节点                                       |                |                      |



## DOM元素的操作

### 操作元素的内容

| 内容                       | 说明                              |
| -------------------------- | --------------------------------- |
| element.innerText          | 不识别html标签，不保留空格和换行  |
| element.innerHTML          | 识别html标签，保留空格和换        |
| element.insertAdjacentText | 类似于innerText，但能调整插入位置 |
| element.insertAdjacentHTML | 类似于innerHTML，但能调整插入位置 |

- `'beforebegin'`: 插入到标签开始前
- `'afterbegin'`: 插入到标签开始标记之后
- `'beforeend'`: 插入到标签结束标记前
- `'afterend'`: 插入到标签结束标记后

```js
beforeBtn.addEventListener('click', function() {
  para.insertAdjacentText('afterbegin',textInput.value);
});

afterBtn.addEventListener('click', function() {
  para.insertAdjacentText('beforeend',textInput.value);
});
```

### 操作元素的样式

| 样式              | 说明                                                 |
| ----------------- | ---------------------------------------------------- |
| element.style     | 样式，样式名采用驼峰命名法，改变少量样式             |
| element.className | 类名，改变大量样式，用className的原因是class是保留字 |

> 排他思想：如果有一组元素，只要它们之中的一个有某种样式，其他没有，则需要先使用循环将它们的样式全部清除，再给特点元素添加

获取元素高级样式（如伪元素选择器）的值的方法：使用`document.getComputedStyle`

```js
document.getComputedStyle(dom元素，样式值)
```

示例代码

```js
document.getComputedStyle(document.querySelect("div"), "::first-letter").color;
```

### 操作元素的属性

| 属性         | 说明     |
| ------------ | -------- |
| element.src  | 资源地址 |
| element.href | 链接地址 |
| element.alt  | 失效提示 |
| element.id   | id值     |

### 操作表单元素的属性

| 属性             | 说明                                    |
| ---------------- | --------------------------------------- |
| element.value    | 表单控件的值                            |
| element.disable  | 改变表单控件的可用状态，true/false      |
| element.type     | 改变表单控件的类型，比如显示输入的密码  |
| element.checked  | 改变radio和checkbox类型的控件的选中状态 |
| element.selected | 改变select-option类型控件的选中状态     |

### 操作元素属性的万能方法

通过万能方法能够设置和获取自定义的属性

* 获取元素属性：element.getAttribute("属性名")

* 设置元素属性：element.setAttribute("属性名","属性值")

> 注意：如果要设置类，则直接用class，而不是className

* 移除元素属性：element.removeAttribute("属性名")

### 自定义元素属性

作用：自定义属性的作用是临时保存数据到页面

约定：自定义属性以"data-"开头

设置自定义属性

* 方法一：使用element.setAttribute("属性名","属性值")设置自定义属性

* 方法二：直接在标签内写自定义属性

  ```html
  <div id="id" data-index="1"></div>
  ```

获取自定义属性

* 方法一：使用element.getAttribute("属性名","属性值")获取自定义属性的值

* 方法二：通过dom元素的dataset属性获取

  dataset是一个集合，存放了所有以"data-"开头的自定义属性

  如果自定义属性名以"-"连接，则需要使用驼峰命名法

  ```js
  const dom = document.getElementById("id");
  const index = dom.dataset.index;
  console.log(index);
  ```



## DOM元素offset, client, scroll系列

### DOM元素偏移量offset系列

#### 概念

offset就是偏移量的意思，使用offset相关属性可以动态地得到该元素的位置（偏移）、大小等

获得元素距离定位父元素的距离，获得元素自身的大小（宽度高度）

#### 属性

| offset系列属性       | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| element.offsetParent | 返回作为该元素带有定位的父级元素，如果父级都没有定位，则返回body【返回的是dom对象】 |
| element.offsetTop    | 返回元素相对带有定位的父级元素上方的偏移                     |
| element.offsetLeft   | 返回元素相对带有定位的父级元素左边框的偏移                   |
| element.offsetWidth  | 返回自身包括padding,边框、内容区的宽度，不带单位             |
| element.offsetHeight | 返回自身包括padding,边框、内容区的高度，不带单位             |

#### 图示属性

![](./images/image-20201012023318132.png)

#### 注意

返回的值都是不带单位的，就是单纯的数值

没有offsetRight和offsetBottom

#### offset和style区别

| offset                                  | style                             |
| --------------------------------------- | --------------------------------- |
| offset可以得到任意样式表中的样式值      | style只能得到行内样式表中的样式值 |
| offset系列获取的数值是不带单位的        | style获取的样式是带单位的字符串   |
| offsetWidth包含padding + border + width | style.width不包含padding和border  |
| offsetWidth是只读属性                   | style是可读可写属性               |

### DOM元素可视区client系列

#### 概念

使用client系列的属性可以获取元素的相关信息

通过client系列的相关属性可以动态地得到该元素的边框大小，元素大小等

#### 属性

| client系列属性       | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| element.clientTop    | 返回元素上边框的大小                                         |
| element.clientLeft   | 返回元素左边框的大小                                         |
| element.clientWidth  | 返回自身包括padding,content的宽度，不含边框，返回数值不带单位 |
| element.clientHeight | 返回自身包括padding,content的高度，不含边框，返回数值不带单位 |

#### 图示属性

![](./images/image-20201012023330704.png)

#### 注意

返回的值都是不带单位的，就是单纯的数值

没有clientRight，没有clientBottom

### DOM元素滚动scroll系列

#### 概念

scroll系列相关属性可以动态获取元素的大小、滚动距离等

#### 属性

| scroll系列属性       | 作用                                         |
| -------------------- | -------------------------------------------- |
| element.scrollTop    | 返回被卷去的上侧距离，返回数值不带单位       |
| element.scrollLeft   | 返回被卷去的侧左距离，返回数值不带单位       |
| element.scrollWidth  | 返回自身实际宽度，不含边框，返回数值不带单位 |
| element.scrollHeight | 返回自身实际高度，不含边框，返回数值不带单位 |

#### 图示属性

![](./images/image-20201012023347821.png)

#### 注意

返回的值都是不带单位的，就是单纯的数值

没有scrollRight和scrollBottom

### getBoundingClientRect

#### 定义

`Element.getBoundingClientRect()` 方法返回元素的大小及其相对于视口的位置

#### 属性

返回值是一个`DOMRect`对象，常用属性如下

| 常用属性 | 说明                   |
| -------- | ---------------------- |
| top      | 元素顶部距离视口的距离 |
| left     | 元素左侧距离视口的距离 |
| bottom   | 元素底部距离视口的距离 |
| right    | 元素右侧距离视口的距离 |

![](./images/image-20200925171823869.png)



## DOM节点

### 节点基本属性

节点类型：nodeType：标签节点：1、属性节点：2、文本节点：3、注释：8、document：9

节点名称：nodeName

节点值：nodeValue

### 节点操作之获取父节点

#### node.parentNode

获取最近的父节点，如果找不到则返回null

### 节点操作之获取子节点

#### node.childNodes

获取一级子节点组成的数组，找不到则返回空数组

包含元素节点，文本节点和属性节点，可以通过nodeType进行手动过滤

#### node.children

获取一级子元素节点组成的数组，找不到则返回空数组

只包含dom元素节点

#### node.firstChild

获取第一个子节点，找不到则返回null

不管是元素节点，文本节点和属性节点

#### node.lastChild

获取最后一个子节点，找不到则返回null

不管是元素节点，文本节点和属性节点

#### node.firstElementChild

获取第一个子元素节点，找不到则返回null

兼容性写法：node.children[0]

#### node.lastElementChild

获取最后一个子元素节点，找不到则返回null

兼容性写法：node.children[node.children.length - 1]

### 节点操作之获取兄弟节点

#### node.nextSibling

获取下一个兄弟节点，找不到则返回null

包含元素节点，文本节点和属性节点，可以通过nodeType进行手动过滤

#### node.nextElementSibling

获取下一个兄弟元素节点，找不到则返回null

### 节点操作之创建节点

#### document.createElement

documnet.createElement("tagName")

创建由tagName指定的HTML元素节点

#### node.innerHTML

作用：使用拼接字符串的方式创建新节点

例子：node.innerHTML += "\<div>123\</div>"

建议：不推荐使用拼接字符串的方法来创建多个节点，因为由于字符串的不变性，需要使用大量内存，所以性能会很差，推荐使用字符串数组的join方法来创建多个节点

#### document.write

作用：直接将内容写入到文档流

例子："\<div>123\</div>"

特点/缺点：当文档加载完成，会先清空文档，再把参数写入body内容的开头，即会导致整个页面的重绘

结论：没有实际意义

#### 性能对比

innerHTML和documnet.createElement和document.write性能对比

* 首先因为document.write会导致页面重绘，所以性能肯定是最差的

* innerHTML在创建大量元素时，性能会比document.createElement性能要高一些，前提是不使用拼接字符串，而是使用字符串数组的join方法

### 节点操作之添加节点

#### node.appendChild(child)

在node节点后追加child子节点

#### node.insertBefore(child, 指定元素)

将一个新节点添加到父接节点的指定子节点之前

### 节点操作之删除节点

#### node.removeChild(child)

删除父节点某个子节点

### 节点操作之复制节点

#### node.cloneNode()

复制当前的节点，没有参数或参数为false，则为浅拷贝，即只拷贝标签本身，不拷贝标签内的内容，如果参数为true，则为深拷贝，即复制节点本身以及里面的所有节点
