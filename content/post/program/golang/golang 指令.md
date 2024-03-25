---
title: golang 指令
date: 2023-12-08T16:53:03+08:00
tags:
  - golang
categories:
  - Program
---
# golang 指令
## 打印
- 打印变量类型
```go
fmt.Println(reflect.TypeOf(x).String())
fmt.Printf("%T", x)
```

- 格式化输出

1.  **%v 只输出所有的值**
2.  **%+v 先输出字段类型，再输出该字段的值**
3.  **%#v 先输出结构体名字值，再输出结构体（字段类型+字段的值）**

```go
fmt.Printf("a:%#v\n",a)
```
