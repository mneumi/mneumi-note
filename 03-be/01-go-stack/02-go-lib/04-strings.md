## 字符串操作

### 求字符串长度

如果使用`len`求长度，结果为字节的长度

因为使用 utf8 动态编码，所以一个汉字为3个字节

```go
var name string = "mneumi:你好啊"
fmt.Println(len(name)) // 16 = 7 + 3 * 3  
```

求真正长度，先转换为 `[]rune`

```go
var name string = "mneumi:你好啊"
fmt.Println(len([]rune(name))
```

### 转义字符

反引号中不会进行转义

| 转义字符 | 说明                              |
| -------- | --------------------------------- |
| \n       | 换行 LF，将当前位置移动到下一行   |
| \r       | 回车 CR，将当前位置移动到本行开头 |
| \t       | 水平制表 HT                       |
| \\\      | 表示一个反斜杠字符 \              |
| \\"      | 代表一个双引号字符                |
| \\'      | 代表一个单引号字符                |
| \?       | 代表一个问号                      |

### 是否包含子串

```go
strings.Contains(name, "123")

strings.Index(name, "123")
```

### 统计子串出现次数

```go
fmt.Println(strings.Count(name, "123"))
```

### 判断是否含有前缀或后缀

```go
fmt.Println(strings.HasPrefix(name, "o"))

fmt.Println(strings.HasSuffix(name, "o"))
```

### 大小写转换

```go
strings.ToUpper("bobby")
strings.ToLower("bobby")
```

### 字符串比较

字符比较的是ascii的比较，返回-1，0，1

```go
strings.Compare("ab", "aa")
```

### 去掉空格或指定字符串

```go
strings.TrimSpace("    bobby   ")
strings.TrimLeft("    bobby   ", "b")
strings.TrimRight("    bobby   ", 'b')
```

### 字符串切割

```go
strings.Split("bobby imooc", " ")
```

### 合并数组内容为字符串

```go
arrs := strings.Split("bobby imooc", " ")strings.Join(arrs, "-")
```

### 字符串替换

```go
strings.Replace("bobby 18 电话", "18", "19", 1)strings.ReplaceAll("bobby 18 电话", "18", "19") // 相当于strings.Replace("bobby 18 电话", "18", "19", -1)
```

### 格式化字符串

fmt.Printf

fmt.Sprintf

```go
%v 缺省格式%#v 带go语法打印%T 类型打印%d 十进制%+d 带符号十进制%4d Pad空格，左补%-4d Pad空格，右补%b 二进制%o 八进制%X 十六进制%s 字符串%6s	宽度为6，不够则左补%-6s 宽度为6，不够则右补%c 字符%q 有引号的字符%U Unicode%#U Unicode%e 科学计数法%f 十进制小数
```



## 含中文字符串的坑

下标取值的坑

```go
name := "bobby:哈哈哈"fmt.Printf("%c\n", name[0]) // bfmt.Printf("%c\n", name[6]) // 理论上是 哈，但不是，而是一个特殊字符// 因为与 len 类型，Go中对字符串取下标，按字节取
```

for循环遍历的坑，因为与 len 类型，Go中对字符串取下标，按字节取

```go
name := "bobby:哈哈哈"for i := 0; i<len(name); i++ {    fmt.Printf("%c\n", name[i])}
```

统一解决方法：使用 []rune



## 类型转换

strconv.Itoa(42)

strconv.Atoi("42")

strconv.ParseBool("true")

strconv.ParseFloat("3.14159", 64)

strconv.ParseInt("-42", 10, 64)

strconv.ParseUint("42", 10, 64)

strconv.FormatBool(true)

strconv.FormatFloat(3.1415, "E", -1, 64)

* f: -dddd.dddd
* b: -ddddp+-ddd指数为2进制
* e: -d.ddde+-dd十进制指数
* E: -d.dddE+-dd十进制指数
* g：指数很大时用e，否则用f
* G：指数很大时用E，否则用F
* prec：控制精度

strconv.FormatInt(-42, 16)

strconv.FormatUint(42, 16)