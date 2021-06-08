## class

### class名称字符串

```html
<!-- classString: "main" -->
<div :class="classString">
</div>
```

### class对象

```html
<!-- classObject: { red: true, green: false } -->
<div :class="classObject">  
</div>
```

### class数组

```html
<!-- classArray: ["red", "green"] -->
<div :class="classArray">  
</div>
```

### 对象与数组混合

```html
<!-- classArrayAndObject: ["red", "green", { brown: false, pink: true }] -->
<div :class="classArrayAndObject">
</div>
```

### 父子组件属性

```html
<!-- 父组件 -->
<div>
  <demo class="green"></demo>  
</div>
  
<!-- 子组件 -->
<div :class="$attrs.class">
</div>
```



## style

### 对象模式

CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名

```html
<!-- styleObject: { color: "orange", fontSize: "16px" } -->
<div :style="styleObject">    
</div>
```

### 数组模式

`v-bind:style` 的数组语法可以将多个样式对象应用到同一个元素上

```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```