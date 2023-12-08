---
title: golang知识大纲
date: 2023-12-08T15:43:00
tags:
  - golang
categories:
  - Programe
draft: "true"
---
golang faq 
>https://learnku.com/go/wikis/38175

golang 常见面试题
>https://zhuanlan.zhihu.com/p/471490292

在 Golang 中，申明变量和分配内存是两个不同的概念。

Go 语言中所有的函数调用都是传值的

# 申明变量
在 Golang 中，申明变量并不会立即分配内存。变量的内存分配时间取决于变量的类型和作用域。
##

# 分配内存
### `=` 和 `:=` 的区别？
#### =是赋值变量，:=是定义变量。

### 指针的作用
#### 如果指针名字为p，那么可以说“p指针指向变量x”，或者说“p指针保存了x变量的内存地址”
#### 它所指向的地址在32位或64位机器上分别**固定**占4或8个字节
#### golang指针不能偏移和位运算 如：&i<<1 &i++


goroutine+channel工作池
https://developer.aliyun.com/article/618494

gin前缀树
gin基于前缀树实现

综合易用性和性能，一般推荐使用 strings.Builder 来拼接字符串。
https://geektutu.com/post/hpg-string-concat.html

# golang 数据类型

Go语言将数据类型分为四类：
- 基础类型 （数字、字符串和布尔型）
- 复合类型 （数组和结构体——是通过组合简单类型，来表达更加复杂的数据结构）
- 引用类型（引用类型包括指针、切片、字典、函数、通道，虽然数据种类很多，但它们都是对程序中一个变量或状态的间接引用）
- 接口类型（接口类型是对其它类型行为的抽象和概括；因为接口类型不会和特定的实现细节绑定在一起，通过这种抽象的方式我们可以让我们的函数更加灵活和更具有适应能力）

## 基础类型

## 复合类型 

## 引用类型

## 复合数据类型

### slice
slice唯一合法的比较操作是和nil比较
```
if summer == nil { /* ... */ }
```

# 错误

## error是接口类型
nil意味着函数运行成功，non-nil表示失败
```
type error interface {  
   Error() string  
}
```

## 错误处理5个策略

## 传播错误
使用fmt.Errorf格式化错误信息并返回
当错误最终由main函数处理时，错误信息应提供清晰的从原因到后果的因果链
```
    return nil, fmt.Errorf("parsing %s as HTML: %v", url,err)
```

## 重新尝试失败的操作
应当注意限制重试的时间间隔或者重试次数

## 输出错误信息并结束程序
这种策略只应在main中执行，库函数应仅向上传播错误

## 仅输出错误信息
不影响程序运行的可直接输出
```
if err := Ping(); err != nil {
    log.Printf("ping failed: %v; networking disabled",err)
}
```
## 直接忽略掉错误


## CSP

参考资料
>https://zhuanlan.zhihu.com/p/313763247

Communicating Sequential Process golang借用csp模型 process和channel这两个概念。process是在go语言上的表现就是 goroutine 是实际并发执行的实体，每个实体之间是通过channel通讯来实现数据共享。

-   **Go协程goroutine: 是一种轻量线程，它不是操作系统的线程，而是将一个操作系统线程分段使用，通过调度器实现协作式调度。是一种绿色线程，微线程，它与Coroutine协程也有区别，能够在发现堵塞后启动新的微线程。**
-   **通道channel: 类似Unix的Pipe，用于协程之间通讯和同步。协程之间虽然解耦，但是它们和Channel有着耦合。**

### Goroutines

Goroutine 是实际并发执行的实体，它底层是使用协程(coroutine)实现并发，coroutine是一种运行在用户态的用户线程，类似于 greenthread，go底层选择使用coroutine的出发点是因为，它具有以下特点：

特点：

-   **用户空间 避免了内核态和用户态的切换导致的成本**
-   **可以由语言和框架层进行调度**
-   **更小的栈空间允许创建大量的实例**


### channel


```
chan T // 声明一个双向通道
chan<- T // 声明一个只能用于发送的通道
<-chan T // 声明一个只能用于接收的通道COPY
```

### 参考资料
#### [一文读懂什么是进程、线程、协程](https://www.cnblogs.com/Survivalist/p/11527949.html)

#### [一文让你明白CPU上下文切换](https://zhuanlan.zhihu.com/p/52845869)

####  [深入理解Linux的CPU上下文切换](https://cloud.tencent.com/developer/article/1897179)


### 如何才能确保自定义类型实现了接口？

你可以尝试像下面这样用 `T` 的零值或 `T` 的指针来进行赋值，编译器将为你检查类型 `T` 是否实现了 接口 `I`。
```
type T struct{}
var _ I = T{}       // 检查 T 是否实现了接口 I
var _ I = (*T)(nil) // 检查 *T 是否实现了接口 I
```

### 为什么 nil 错误值不等于 nil?[#](https://learnku.com/go/wikis/38175#e9276e)


### 为什么 Go 不支持隐式的数值转换？

### 为什么 Map、切片和通道是引用，而数组是值？

## 指针和内存分配

### 什么时候函数参数是传值？
Go 语言中的所有传递都是传值,函数接收到的永远都是参数的一个副本,
传递一个 `int` 值给一个函数，函数收到的是这个 `int` 值得副本，传递指针值，获得的是指针值的副本，而不是指针指向的数据。



### new 和 make 的区别？[#](https://learnku.com/go/wikis/38175#1ef5c5)
简单地说： `new` 请求分配内存，返回指针，而 `make` 只用于初始化slice，map，channel。

-   make 只能用来分配及初始化类型为 slice、map、channel 的数据。new 可以分配任意类型的数据；
-   new 分配返回的是指针，即类型 \*Type。make 返回引用，即 Type；
-   new 分配的空间被清零。make 分配空间后，会进行初始化；

### string和[]byte 对比和转换

#### [String和[]byte的转换](https://www.51cto.com/article/670312.html)

#### [golang string和[]byte的对比](https://zboya.github.io/post/golang_byte_slice_and_string/)


