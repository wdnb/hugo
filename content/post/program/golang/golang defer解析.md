---
title: golang defer解析
date: 2023-12-08T16:53:03+08:00
tags:
  - golang
categories:
  - Program
---
# golang defer解析

## 定义

defer 是 Go 语言提供的一种用于注册延迟调用的机制：让函数或语句可以在当前函数执行完毕后执行。

1. 每次 defer 语句执行的时候，会把函数“压栈”，函数参数会被拷贝下来。
2. 当外层函数退出时，多个defer后进先出（LIFO）的顺序执行，即最后一个defer语句会最先执行。）。
3. 如果 defer 执行的函数为 nil, 那么会在最终调用函数的产生 panic。

**不能在defer里面返回值**
### 什么情况下会调用 defer 延迟过的函数呢？

1. 当函数执行了 `return` 语句后
2. 当函数处于 `panicing` 状态，也就是程序中 `panic` 回溯上来到当前函数范围内时

## 进阶
### defer return拆解

- defer在return之前执行 但return xxx这一条语句并不是一条原子指令
- 函数返回的过程是这样的：先给返回值赋值，然后调用defer表达式，最后才是返回到调用函数中。
```go
// res=0;defer res++;return res  
func defer1() (res int) {  
    defer func() {  
       res++  
    }()  
    return 0  
}
```

```go  
// res=1;defer res++;return res  
func defer2() (res int) {  
    res = 1  
    defer func() {  
       res++  
    }()  
    return res  
}
```

```go
// x=1;res++;return x  
func defer3() int {  
    res := 1  
    defer func() {  
       res++  
    }()  
    return res  
}
  ```

```go
// r=res;defer res++;return r  
func defer4() (r int) {  
    res := 1  
    defer func() {  
       res++  
    }()  
    return res  
}
```

```go
// res=0;new_res++;return res  
func defer5() (res int) { //命名返回值在函数开始时会被初始化为其类型的零值，而匿名返回值则不会
    defer func(res int) { //这里的res是传值 不会更改外部的res  
       res++  
    }(res)  
    return res  
}
```

```go
// r=0;r++;return r  
func defer6() (r int) {  
    defer func(res *int) { //这里的res是传引用 会更改外部的res  
       *res++  
    }(&r)  
    return r  
}
```

### defer 函数对外部变量的引用

在 defer 函数定义时，对外部变量的引用是有两种方式的，分别是作为函数参数和作为闭包引用。

- 作为函数参数，则在 defer 定义时就把值传递给 defer，并被 cache 起来。
- 作为闭包引用的话，则会在defer 函数真正调用时根据整个上下文确定当前的值。

defer 后面的语句在执行的时候，函数调用的参数会被保存起来，也就是复制了一份。真正执行的时候，实际上用到的是这个复制的变量，因此如果此变量是一个“值”，那么就和定义的时候是一致的。如果此变量是一个“引用”，那么就可能和定义的时候不一致。

**defer 后面跟的是闭包，必然是引用类型的变量。**

```go
func main() {
	startedAt := time.Now()
	defer fmt.Println(time.Since(startedAt))//并不能返回1
	
	time.Sleep(time.Second)
}
```

```go
func main() {
	startedAt := time.Now()
	defer func() { fmt.Println(time.Since(startedAt)) }()//返回1
	
	time.Sleep(time.Second)
}
```


## References

* [Go 中的 defer 详解](https://giaogiaocat.github.io/go/defer/)
* [Go 语言设计与实现](https://draveness.me/golang/docs/part2-foundation/ch05-keyword/golang-defer/)
* [深入解析Go](https://tiancaiamao.gitbooks.io/go-internals/content/zh/03.4.html)