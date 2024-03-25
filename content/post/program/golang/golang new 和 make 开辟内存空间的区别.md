---
title: golang new 和 make 的区别
date: 2024-03-20T03:20:03+08:00
tags:
  - golang
categories:
  - Program
---

# golang new 和 make 开辟内存空间的区别

## 总结

### new 函数
`new` 函数用于为指定的类型分配一片零值内存,并返回指向这片内存的指针（指定类型的指针），它适用于任何类型的值,包括用户定义的类型和内置类型。

### make 函数
`make()` 函数用于创建 slice、map 和 channel，返回被初始化的（非零值）引用类型的实例

### `new` 和 `make` 在内存分配 slice、map 和 channel 时的区别

1. **使用 `new`**:
    - `slice := new([]int)` 将分配一个新的 slice 指针,指向 `nil` 值的底层数组结构。
    - `mp := new(map[string]int)` 将分配一个新的 map 指针,指向一个 `nil` 的 map 结构。
    - `ch := new(chan int)` 将分配一个新的 channel 指针,指向 `nil` 的 channel 结构。
2. **使用 `make`**:
    - `slice := make([]int, 0)` 将分配一个新的由 0 个整数元素初始化的 slice 结构。
    - `mp := make(map[string]int)` 将分配并初始化一个新的空 map 结构。
    - `ch := make(chan int)` 将分配并初始化一个新的未缓存的 channel 结构。

## 官方说明

### new
Go has two allocation primitives, the built-in functions `new` and `make`. They do different things and apply to different types, which can be confusing, but the rules are simple. Let's talk about `new` first. It's a built-in function that allocates memory, but unlike its namesakes in some other languages it does not _initialize_ the memory, it only _zeros_ it. That is, `new(T)` allocates zeroed storage for a new item of type `T` and returns its address, a value of type `*T`. In Go terminology, it returns a pointer to a newly allocated zero value of type `T`.

Go 语言有两个用于内存分配的内置函数，分别为 new 和 make。它们用于不同的目的，适用于不同的数据类型，new 函数用于分配内存，但是和一些其他语言中的同名函数不同，它不会初始化内存，只会将分配的内存清零。也就是说，new(T) 会为类型 T 的新项目分配清零的存储空间，并返回该内存空间的地址，即一个 \*T类型的指针值。用 Go 术语来说，**new返回一个指向新分配的类型 T 的零值的指针**。
### make

Back to allocation. The built-in function `make(T,` _args_`)` serves a purpose different from `new(T)`. It creates slices, maps, and channels only, and it returns an _initialized_ (not _zeroed_) value of type `T` (not `*T`). The reason for the distinction is that these three types represent, under the covers, references to data structures that must be initialized before use. A slice, for example, is a three-item descriptor containing a pointer to the data (inside an array), the length, and the capacity, and until those items are initialized, the slice is `nil`. For slices, maps, and channels, `make` initializes the internal data structure and prepares the value for use. For instance,

内置函数 make(T, args) 的用途与 new(T) 不同。它只用于创建切片 (slice)、映射 (map) 和通道 (channel)，并且返回一个初始化的 (而非清零的) 类型 T 的值 (而非 \*T)。之所以要区分这两种函数，是因为这三种类型在底层表示对数据结构的引用，而这些数据结构在使用之前必须进行初始化。例如，slice是一个包含三个元素的描述符，其中包含指向数据 (位于数组内部) 的指针、长度和容量。在这些元素被初始化之前，切片都是 nil 值。对于slice, map, 和 channel，make 会初始化内部数据结构并使其可以被使用。

## References

- [golang中new与make的区别](https://juejin.cn/post/7180326159027011639)
- [new](https://go.dev/doc/effective_go.html#allocation_new)
- [make](https://go.dev/doc/effective_go.html#allocation_make)