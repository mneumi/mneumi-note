## source-map是什么

source-map能够提供源代码与构建后代码的映射关系

如果构建后代码出错，则可以通过映射代码追踪查找错误



## 开启方式

开启source-map很简单，只需要配置`devtool`属性即可

```js
module.exports = {
    devtool: "source-map"
}
```



## source-map规则与选项

### 规则

| 规则       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| source-map | 外联；错误信息，源代码位置，能看到源代码，精确到列，产生.map文件 |
| inline     | 改为内联，即将.map作为dataURL嵌入到js文件内部                |
| hidden     | 隐藏源代码                                                   |
| eval       | 使用eval包裹块代码                                           |
| nosources  | 隐藏源代码和错误信息                                         |
| cheap      | 不包含列信息                                                 |
| module     | 将loader的source-map也包含进来                               |

### 开发环境选择

速度比较（快到慢）：eval > inline > cheap

调试更友好：source-map > cheap-source-module-source-map > cheap-source-map

结论：eval-source-map / eval-cheap-module-source-map

### 生产环境选择

内联会使构建后的文件体积变大，所以不考虑内联

nosource-source-map：全部隐藏

hidden-source-map：只隐藏源代码

结论：source-map / cheap-module-source-map