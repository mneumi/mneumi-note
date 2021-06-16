## 正则表达式概述

### 定义

正则表达式就是字符串模式匹配的规则

### 作用

验证字符串，替换字符串，提取字符串



## 正则表达式元字符

元字符就是特殊字符，在正则表达式中其特殊作用

元字符分为：边界限定符，可选字符集合，量词，符合符，分组符，预定义字符

### 边界限定符

| 边界限定符 | 说明       |
| ---------- | ---------- |
| ^          | 必须是开头 |
| $          | 必须是结尾 |

### 可选字符集合

在`[]`内可以填写多个字符，只要满足其中一个即可

| 可选字符集合  | 说明                  |
| ------------- | --------------------- |
| [abcxyz]      | a,b,c,x,y,z中任意一个 |
| [A-Z]         | A-Z中任意一个         |
| [a-z]         | a-z中任意一个         |
| [0-9]         | 0-9中任意一个         |
| [0-9a-zA-Z]   | 0-9,a-z,A-Z中任意一个 |
| `^`可用于取反 |                       |
| [^A-Z]        | 除了A-Z中任意一个     |
| [^0-9]        | 除了0-9中任意一个     |

### 量词
| 量词  | 说明            |
| ----- | --------------- |
| *     | 重复0次或多次   |
| ?     | 重复0次或1次    |
| +     | 重复1次或多次   |
| {n}   | 重复n次         |
| {n,}  | 重复n次或更多次 |
| {n,m} | 重复n到m次      |

### 组合/分组符
被`()`包裹的字符看作一个组合/分组

| 例子        | 说明                         |
| ----------- | ---------------------------- |
| \abc{3}\    | c这个字符重复3次             |
| \\(abc){3}\ | abc这个字符串作为整体重复3次 |

### 预定义字符

| 量词 | 说明                                                       |
| ---- | ---------------------------------------------------------- |
| \d   | 匹配0-9，相当于[0-9]                                       |
| \D   | 匹配非0-9，相当于\[^0-9]                                   |
| \w   | 匹配所有大小写字母、数字和下划线，相当于[0-9a-zA-Z_]       |
| \W   | 匹配除了所有大小写字母、数字和下划线，相当于\[^0-9a-zA-Z_] |
| \s   | 匹配空格符，相当于[\n\t\r\v\f]                             |
| \S   | 重复n到m次，相当于\[^\n\t\r\v\f]                           |

### 复合符

`|` : 多个字符或多种表达式，只要满足其一即认为匹配

```go
/^(\d{3}-\d{8})|(\d{4}-\d{8})$/ // 只要满足其中一个条件即可
```



## 正则表达式参数

### 参数书写位置
```shell
/regExpression/[switch]
```

### 参数选值

| 选值 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| g    | 全局匹配（也叫贪婪匹配），就算匹配成功也会继续匹配<br />正则表达式默认是非全局匹配（也叫非贪婪匹配），即一旦匹配成功，则不再继续往后进行匹配 |
| i    | 忽略大小写                                                   |
| gi   | 全局匹配+忽略大小写                                          |



## Regexp包

### 概述

在 `Go` 中，如果想要使用正则表达式，则需要使用到内置的 `Regexp` 包

### 创建Regexp对象

| 方法签名                                   | 注意事项                                                     |
| ------------------------------------------ | ------------------------------------------------------------ |
| Compile(str string) (*Regexp, error)       | 采用最左最短方式搜索<br />如果传入的正则表达式错误，则会返回`error` |
| MustCompile(str string) *Regexp            | 采用最左最短方式搜索<br />如果传入的正则表达式解析错误，会引发`panic` |
| CompilePOSIX(str string)  (*Regexp, error) | 采用最左最长方式搜索<br />采用`POSIX` 语法<br />不支持：`\d、\D、\s、\S、\w、\W` |
| MustCompilePOSIX(str string) *Regexp       | 采用最左最长方式搜索<br />采用`POSIX` 语法<br />不支持：`\d、\D、\s、\S、\w、\W` |

示例代码

```go
package main

import (
  "regexp"
)

func main() {
  reg1, err := regexp.Compile(`[a-z]+`)
  reg2 := regexp.MustCompile(`[a-z]+`)
  reg3, err := regexp.CompilePOSIX(`[a-z]+`)
  reg4 := regexp.MustCompilePOSIX(`[a-z]+`)
}
```

### 验证字符串

| 方法签名                                                     | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Match(pattern string, b []byte) (matched bool, err error)    | 接收字节数组判断是否符合正则表达式<br />matched：符合为true，不符合为false<br />err：查找过程中遇到的任何错误 |
| MatchReader(pattern string, r io.RuneReader) (matched bool, err error) | 从Reader接收数据判断是否符合正则表达式<br />matched：符合为true，不符合为false<br />err：查找过程中遇到的任何错误 |
| MatchString(pattern string, s string) (matched bool, err error) | 接收字符串判断是否符合正则表达式<br />matched：符合为true，不符合为false<br />err：查找过程中遇到的任何错误 |

示例代码

```go
package main

import (
  "regexp"
)

func main() {  
  regexp.Match(`^[a-z]+$`, []byte("abcdef")) // true
  regexp.Match(`^[a-z]+$`, []byte("123456")) // false
  
  regexp.MatchString(`^[a-z]+$`, "abcdef") // true
  regexp.MatchString(`^[a-z]+$`, "123456") // false
  
  reader1 := bytes.NewReader([]byte("abcdef"))
  reader2 := bytes.NewReader([]byte("123456"))
  regexp.MatchReader(`^[a-z]+$`, reader1) // true
  regexp.MatchReader(`^[a-z]+$`, reader2) // false
}
```

### 替换字符串

| 方法签名                                                     | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ReplaceAll(src, repl []byte) []byte                          | 在 `src` 中搜索匹配项，并替换为 `repl` 指定的内容            |
| ReplaceAllString(src, repl string) string                    | 在 `src` 中搜索匹配项，并替换为 `repl` 指定的内容            |
| ReplaceAllLiteral(src, repl []byte) []byte                   | 在 `src` 中搜索匹配项，并替换为 `repl` 指定的内容<br />如果 `repl` 中有分组引用符`$`（如`$1`、`$name`），则将`$` 当普通字符处理 |
| ReplaceAllLiteralString(src, repl string) string             | 在 `src` 中搜索匹配项，并替换为 `repl` 指定的内容<br />如果 `repl` 中有分组引用符`$`（如`$1`、`$name`），则将`$` 当普通字符处理 |
| ReplaceAllFunc(src []byte, repl func([]byte) []byte) []byte  | 在 `src` 中搜索匹配项，然后将匹配的内容经过 `repl` 处理后，替换 `src` 中的匹配项<br />如果 `repl` 中有分组引用符`$`（如`$1`、`$name`），则将`$` 当普通字符处理 |
| ReplaceAllStringFunc(src string, repl func(string) string) string | 在 `src` 中搜索匹配项，然后将匹配的内容经过 `repl` 处理后，替换 `src` 中的匹配项<br />如果 `repl` 中有分组引用符`$`（如`$1`、`$name`），则将`$` 当普通字符处理 |

### 提取字符串

使用`Regexp`对象的方法进行提取字符串

| 常用方法签名                                        | 说明                                 |
| --------------------------------------------------- | ------------------------------------ |
| Find(b []byte) []byte                               | 返回第一个匹配的内容                 |
| FindString(s string) string                         | 返回第一个匹配的内容                 |
| FindAll(b []byte, n int) \[][]byte                  | 返回所有匹配的内容                   |
| FindAllString(s string, n int) []string             | 返回所有匹配的内容                   |
| FindIndex(b []byte) []int                           | 返回第一个匹配的位置                 |
| FindStringIndex(s string) loc []int                 | 返回第一个匹配的位置                 |
| FindReaderIndex(r io.RuneReader) (loc []int)        | 返回第一个匹配的位置                 |
| FindAllIndex(b []byte, n int) [][]int               | 并返回所有匹配的位置[首下标，尾下标] |
| FindAllStringIndex(s string, n int) [][]int         | 并返回所有匹配的位置[首下标，尾下标] |
| FindSubmatch(b []byte) [][]byte                     |                                      |
| FindStringSubmatch(s string) []string               |                                      |
| FindAllSubmatch(b []byte, n int) [][][]byte         |                                      |
| FindAllStringSubmatch(s string, n int) [][]string   |                                      |
| FindSubmatchIndex(b []byte) []int                   |                                      |
| FindStringSubmatchIndex(s string) []int             |                                      |
| FindReaderSubmatchIndex(r io.RuneReader) []int      |                                      |
| FindAllSubmatchIndex(b []byte, n int) [][]int       |                                      |
| FindAllStringSubmatchIndex(s string, n int) [][]int |                                      |