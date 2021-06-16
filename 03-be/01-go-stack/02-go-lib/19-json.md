如果值为空，则省略的tag

```go
type Order struct {
    ID string `json:"id"`
    Name string `json:"name,omitempty"`
}
```

json除了结构体，还可以使用map

```go
m := make(map[string]interface{})

err := json.Unmarshal([]byte(res), &m)

fmt.Printf("%+v/n", m["data"].([]interface{})[2].(map[string]interface{})["count"] )
```

使用匿名结构体，简化操作

```go
m := struct{
    Data []struct {
        Count int `json:"count"`
    } `json:"data"`
} {}

json.Unmarshal([]byte(res), &m)

fmt.Printf("%+v\n", m.Data[2].Count)
```

