## 客户端

简单版

```go
func main() {
    resp, err := http.Get("http://www.baidu.com")
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()
    
    b, err := httputil.DumpResponse(resp, true)
    if err != nil {
        panic(err)
    }
    
    fmt.Printf("%s\n", b)
}
```

进阶版

```go
func main() {
    // 创建 request
    request, err := http.NewRequest(
    	http.MethodGet,
        "http://www.baidu.com",
        nil
    )
    request.Header.Add("User-Agent", "Mozilla/5.0 xxx")
    
    // 发送 request
    resp, err := http.DefaultClient.Do(request)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    // 读取 response
    b, err := httputil.DumpResponse(resp, true)
    if err != nil {
        panic(err)
    }
    fmt.Printf("%s\n", b)
}
```

