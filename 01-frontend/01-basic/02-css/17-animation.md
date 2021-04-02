## 介绍

### 作用

animation用于定义动画

### 流程

1. 定义帧动画
2. 将帧动画组赋值给元素

### 注意

类似transition，有中间值的属性才有动画，没有中间值的属性没有动画（比如border的形状）

**transform的参考点始终是最开始状态，不能继承之前的状态**

```css
@keyframes try {
    0% {
        transform: translateX(100px); /* 水平向右移动100px */
    }
    
    25% {
        transform: translate(100px, 100px); /* 水平向右移动100px，垂直向下移动100px */
                                            /* 不可以写成 transform: translateY(100px) */
    }
}
```

### CSS3动画对比JS动画优势

* CSS3动画不占用 JavaScript 主线程资源
* CSS3动画可以利用硬件加速
* 浏览器能够对CSS3动画进行优化



## 定义帧动画组

### 关键词

 `@keyframes`, `from`动画开始，`to动画结束`，百分比

```css
@keyframes hd {
    from {
        background: white;
    }
    to {
        background: red;
    }
}
```

```css
@keyframes hd2 {
    0% {
        background: white;
    }
    
    25% {
        transform: scale(2);
    }
    
    65% {
        transform: scale(1);
    }
    
    100% {
        background: red;
    }
}
```

#### 不设置0%，from, 100%, to

如果不设置0%或from，则使用元素的原始状态

如果不设置100%或to，则使用元素的原始状态

#### 同时声明关键帧(叠加声明)

```css
@keyframes hd3 {
    25% {
        transform: translateX(300px);
    }
    50% {
        transform: translate(300px, 300px);
        background: #f1c40f;
        border-radius: 0;
    }
    75% {
        transform: translateY(300px);
    }
    25%, 75% {
        background: #9b59b5;
        border-radius: 50%;
    }
}
```

#### 不同帧动画属性重叠情况

谁在后面，谁权重高

### 常用属性

#### 延迟执行动画

```css
animation-delay: 2s;
```

#### 循环次数

```css
animation-iteration-count: 5; /* 5次 */
```

支持独立设置

```css
animation-name: translate, background, radius;
animation-iteration-count: 5,2,3; /* 分别为5次,2次，3次 */
```

如果想无限循环

```css
animation-iteration-count: -1;
animation-iteration-count: infinite;
```

#### 动画方向

方向1：0%==>100% [normal]

方向2：100%==>0% [reverse]

方向3：0%==>100%==>0% [alternate]

方向4：100%==>0%==>100% [alternate-reverse]

```css
animation-direction: normal | reverse | alternate | alternate-reverse;
```

#### 贝塞尔曲线（动画幅度）

```css
animation-timing-function: ease | ease-in-out | ease-in | ease-out | linear;
animation-timing-function: cubic-bezier(.26, .53, .1, .3);
```

#### 步进幅度

```css
animation-timing-function: steps(4,start)
animation-timing-function: steps(4,end)
animation-timing-function: step-start; /* 相当于 steps(1,start) */
animation-timing-function: step-end; /* 相当于 steps(1,end) */
```

#### 动画暂停与播放

```css
animation-play-state: running | paused;
```

#### 填充模式

设置动画开始与结束时保持什么状态

状态分为：初始状态，初始帧状态，结束帧状态

##### 注意：初始状态不一定等于初始帧状态，比如当使用animation-delay时，初始状态≠初始帧状态

```css
animation-fill-mode: none 效果是开始时与结束时都保持初始状态(默认值)
animation-fill-mode: backwards 效果是开始时保持初始帧状态，结束时保持结束帧状态
animation-fill-mode: forwards 效果是开始时保持初始状态，结束时保持结束帧状态
animation-fill-mode: both 效果是backwards和forwards的结合，即开始时保持初始帧状态，结束时保持结束帧状态
```

#### 组合定义

```css
animation: [name] [duration] [delay] 
```

#### 总结

`name`, `duration`, `delay`, `timing-function`, `direction`, `iteration-count`, `play-state`



## 将帧动画组赋值给元素

### 关键字

animation-name, animation-duration

```css
main {
    width: 100px;
    height: 100px;
    background: white;
    animation-name: hd;
    animation-duration:
}
```

### 一个元素使用多个帧动画

```css
@keyframes hd1 {
    25% {
        transform: translateX(300px);
    }
    50% {
        transform: translate(300px, 300px);
    }
    75% {
        transform: translateY(300px);
    }
    25%, 75% {
        background: #9b59b5;
        border-radius: 50%;
    }
}

@keyframes hd2 {
    50% {
        background: #f1c40f;
        border-radius: 0;
    }
    25%, 75% {
        background: #9b59b5;
        border-radius: 50%;
    }
}
```

```css
div {
    animation-name: hd1, hd2;
    animation-duration: 4s, 2s;
}
```

#### 如果动画数量多于时间，则从头开始计算

```css
div {
    animation-name: hd1, hd2, h3;
    animation-duration: 4s, 2s;
}
/* 帧动画h3的时间为4s */
```



## 纯css3实现轮播图

```css
body {
    width: 100vw;
    height: 100vh;
    background: #2c3e50;
    display: grid;
}

main {
    width: 400px;
    height: 200px;
    align-self: center;
    justify-self: center;
    overflow: hidden;
    position: relative;
}

section {
    width: 1600px;
    height: 200px;
    display: grid;
    grid-tempalate 1fr/repeat(4, 400px);
    animation-name: slide;
    animation-duration: 4s;
    animation-timing-function: steps(4, end);
    animation-iteration-count: infinite;
}

section:hover, section:hover+ul::after {
    animation-play-state: pasued;
}

section>div {
    overflow: hidden;
}

img {
    width: 100%;
}

@keyframes slide {
    transform: translateX(-1600px);
}

ul {
    position: absolute;
    width: 200px;
    left: 50%;
    bottom: 20px;
    transform: translatex(-50%);
    display: grid;
    grid-template: 1fr/repeat(4, 1fr);
    list-style: none;
    justify-items: center;
}

ul::after {
    content: '';
    width: 30px;
    height: 30px;
    border-radius: 50%;
    background: #e74c3c;
    position: absolute;
    top: 0;
    left: 10px;
    z-index: 1;
    animation-name: num;
    animation-duration: 4s;
    animation-timing-function: steps(4,end);
    animation-iteration-count: infinite;
}

@keyframes num {
    to {
        transform: translateX(200px);
    }
}

li {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    background: rgba(0,0,0,.3);
    display: grid;
    justify-items: center;
    align-items: center;
    color: white;
}
```

```html
<body>
    <main>
        <section>
            <div><img src="1.jpg" /></div>
            <div><img src="2.jpg" /></div>
            <div><img src="3.jpg" /></div>
            <div><img src="4.jpg" /></div>
        </section>
        <ul>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
        </ul>
    </main>
</body>
```

