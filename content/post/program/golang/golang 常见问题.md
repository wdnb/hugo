---
title: golang 常见问题
date: 2024-07-015T15:41:00+08:00
tags:
  - golang
categories:
  - Program
---
# golang 常见问题

## 整数溢出问题

#### 无符号类型的溢出

当无符号类型溢出时，它们的行为是“环绕”性质的，从最大值回到最小值。因为所有位都用于表示数值，超出最大值的进位会被截断，只保留低位部分。

示例：`uint8` 类型的加法溢出
```go
package main

import "fmt"

func main() {
    var a uint8 = 250
    var b uint8 = 10
    var c uint8 = a + b

    fmt.Printf("uint8 溢出: %d + %d = %d\n", a, b, c)  // 输出: uint8 溢出: 250 + 10 = 4
}

```
#### 有符号类型的溢出
	
有符号类型使用补码表示法，因此它们在溢出时的行为与无符号类型不同。补码表示法的特点是，最小负数（如 `-128`）的二进制表示为最高位为 1 的数值。当溢出时，结果会“环绕”到最小负数或者最大正数。

示例：`int8` 类型的加法和减法溢出
```go
package main

import "fmt"

func main() {
    var a int8 = 127
    var b int8 = 1
    var c int8 = a + b

    fmt.Printf("int8 加法溢出: %d + %d = %d\n", a, b, c)  // 输出: int8 加法溢出: 127 + 1 = -128

    var d int8 = -128
    var e int8 = 1
    var f int8 = d - e

    fmt.Printf("int8 减法溢出: %d - %d = %d\n", d, e, f)  // 输出: int8 减法溢出: -128 - 1 = 127
}

```

## byte和rune

- **`byte`** 是 `uint8` 的别名，范围是 0 到 255，用于处理原始二进制数据和 ASCII 字符。
- **`rune`** 是 `int32` 的别名，范围是 `-2147483648` 到 `2147483647`，用于处理 Unicode 字符。
- 使用 `byte` 和 `rune` 可以根据不同的需求高效地处理字符和文本数据。


## tag是如何解析的

使用 `reflect` 包首先获取 `User` struct 的类型 (`reflect.Type`) ，然后遍历每个字段并使用 `Field.Tag.Get("json")` 来获取标签的值。

```go
package main

import (
    "fmt"
    "reflect"
)

type User struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

func main() {
    user := User{Name: "Alice", Age: 30}
    t := reflect.TypeOf(user)
    for i := 0; i < t.NumField(); i++ {
        field := t.Field(i)
        fmt.Printf("Field: %s, Tag: %s\n", field.Name, field.Tag.Get("json"))
    }
}
```

3、for range 的时候它的地址会发生变化么？
for 循环遍历 slice 有什么问题？

defer recover 的问题？(主要是能不能捕获)


7、 golang 中解析 tag 是怎么实现的？反射原理是什么？(问的很少，但是代码中用的多)
8、调用函数传入结构体时，应该传值还是指针？ （Golang 都是值传递）
9、silce 遇到过哪些坑？
10、go struct 能不能比较？
11、Go 闭包