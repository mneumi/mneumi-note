## 初识Vue

### Vue是什么

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**

Vue用于构建视图，是一个**视图层**框架

### Vue特点

* 渐进式框架：非强侵入式框架
* 容易上手：学习成本低，学习曲线平缓
* 生态丰富：拥有包括路由、状态管理在内的丰富生态
* 高效：20kB min+gzip 运行大小，高效虚拟dom



## 安装Vue

### 使用CDN

```js
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

### 使用VueCLI

具体看 **Vue CLI ** 内容

#### 安装 Vue CLI

```shell
npm install @vue/cli -g
# OR
yarn global add @vue/cli
```

#### 使用 Vue CLI创建项目

```shell
vue --version # 检查版本，判断是否安装成功
vue create [项目名]
```



## 体验案例

### 案例1：Hello World

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>
  new Vue({
      el: "#root",
      template: "<div>Hello World</div>"
  });
</script>
</html>
```

### 案例2：计数器counter

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>
  new Vue({
    el: "#root",
    template: "<div>{{ count }}</div>",
    data() {
      return {
        count: 1
      }
    },
    mounted() {
      setInterval(() => {
        this.count += 1; // this.$data.count
      }, 1000)
    }
  });
</script>
</html>
```

### 案例3：反转字符串

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>
  new Vue({
    el: "#root",
    data() {
      return {
        content: "Hello World"
      }
    },
    methods: {
      handleBtnClick() {
        this.content = this.content.split("").reverse().join("");
      }
    },
    template: `
      <div>
        {{content}}
        <button v-on:click="handleBtnClick">反转</button>
      </div>
    `,
  });
</script>
</html>
```

### 案例4：显示与隐藏

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>
  new Vue({
    data() {
      return {
        content: "Hello World",
        show: true
      }
    },
    methods: {
      handleBtnClick() {
        this.show = !this.show;
      }
    },
    template: `
      <div>
        <span v-if="show">{{content}}</span>
        <button v-on:click="handleBtnClick">显示/隐藏</button>
      </div>
    `,
  }).$mount("#root");
</script>
</html>
```

### 案例5：TodoList

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>
  const vm = new Vue({
    data() {
      return {
        list: [],
        inputValue: ""
      }
    },
    methods: {
      handleAddItem() {
        this.list.push(this.inputValue);
        this.inputValue = "";
      }
    },
    template: `
      <div>
        <input v-model="inputValue" />
        <button v-on:click="handleAddItem">添加</button>
        <ul>
          <li v-for="item of list">
            {{ item }}  
          </li>
        </ul>
      </div>
    `,
  });
    
  vm.$mount("#root");
</script>
</html>
```

