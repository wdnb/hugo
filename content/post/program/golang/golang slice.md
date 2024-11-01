---
title: golang slice
date: 2024-07-24T15:27:00+08:00
tags:
  - golang
categories:
  - Program
---
# golang slice
## Golang 数组和切片的区别

**数组（Array）：**

1. Array（数组）的本质：

- 固定长度：数组的长度在定义时就已确定，且不可更改。
- 值类型：数组是值类型，意味着当你将一个数组赋值给另一个变量时，会复制整个数组。
- 内存连续：数组在内存中是连续存储的。
- 类型的一部分：数组的长度是其类型的一部分，例如 [5]int 和 [10]int 是不同的类型。
示例：
```go
var a [5]int
b := a  // 完整复制数组 a 到 b
```
**切片（Slice）：**

Slice 本质上是一个结构体，包含三个字段：

- 指针：指向底层数组的指针
- 长度：当前 slice 中元素的数量
- 容量：slice 可以扩展到的最大长度

Go 语言的 runtime 包中定义了 slice 的结构：
```go
type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
```
特征：

- 动态长度：slice 的长度可以在运行时改变。
- 引用类型：slice 是引用类型，传递 slice 时传递的是 slice 结构体的副本，但它们指向相同的底层数组。
- 可以动态增长：通过 append 函数，slice 可以自动增长。
- 共享底层数组：多个 slice 可以共享同一个底层数组。

示例：
```go
s := make([]int, 5, 10)
// s 是一个 slice，长度为 5，容量为 10
```
3. Array 和 Slice 的关系：

- Slice 通常基于 Array 创建。
- Slice 可以看作是数组的一个"视图"。
- 可以从一个数组创建多个 slice，它们共享底层数组。
### 一些常见的 slice 相关问题：
1. 容量与长度混淆： slice 有长度（len）和容量（cap）两个属性，混淆这两个概念可能导致意外行为。
```go
s := make([]int, 5, 10)
fmt.Println(len(s), cap(s)) // 输出：5 10
```
2. 在函数间传递 slice： 将 slice 传递给函数时，函数接收的是 slice 的引用。修改 slice 的元素会影响原始 slice，但是追加元素可能不会。
```go
func modify(s []int) {
    s[0] = 100      // 会修改原始 slice
    s = append(s, 1) // 不会修改原始 slice
}
```
3. slice 的扩容： 当 append 操作超过 slice 的容量时，Go 会创建一个新的底层数组，这可能导致意外的行为。
```go
s := make([]int, 0, 5)
s2 := s
s = append(s, 1, 2, 3, 4, 5, 6)
fmt.Println(s, s2) // s 和 s2 现在指向不同的底层数组
```
4.  使用 append 返回值： 始终使用 append 的返回值，否则可能丢失数据。
```go
s := make([]int, 0, 5)
append(s, 1) // 错误：丢失了追加的元素
s = append(s, 1) // 正确
```
5.  从数组创建 slice： 从数组创建的 slice 与原数组共享底层存储，修改 slice 会影响原数组。
```go
a := [5]int{1, 2, 3, 4, 5}
s := a[1:3]
s[0] = 10
fmt.Println(a) // 输出：[1 10 3 4 5]
```
6.  nil slice 和空 slice： nil slice 和空 slice 是不同的，虽然它们的长度都是 0。
```go
var s1 []int         // nil slice
s2 := []int{}        // 空 slice
fmt.Println(s1 == nil, s2 == nil) // true false
```
7.  多维 slice 的坑： 创建多维 slice 时，内部的 slice 可能共享底层数组，导致意外的数据修改。
```go
s := make([][]int, 3)
for i := range s {
    s[i] = make([]int, 2)
    s[i][0] = i
    s[i][1] = i + 1
}
// 正确方式：为每个内部 slice 单独分配内存
```
8.  在循环中追加元素： 在遍历 slice 的同时追加元素可能导致无限循环或跳过元素。
```go
s := []int{1, 2, 3}
for i := 0; i < len(s); i++ {
    s = append(s, s[i])
    // 危险：可能导致无限循环
}
```
9.  使用 copy 函数： copy 函数只会复制最小的 slice 长度，可能导致部分数据未被复制。
```go
src := []int{1, 2, 3, 4, 5}
dst := make([]int, 3)
copied := copy(dst, src)
fmt.Println(dst, copied) // 输出：[1 2 3] 3
```
10. slice 作为 map 的键： slice 不能直接作为 map 的键，因为它们不是可比较的类型。
```go
m := make(map[[]int]string) // 编译错误
```
 11. 在已有切片的基础上进行切片，不会创建新的底层数组。因为原来的底层数组没有发生变化，内存会一直占用，直到没有变量引用该数组。因此很可能出现这么一种情况，原切片由大量的元素构成，但是我们在原切片的基础上切片，虽然只使用了很小一段，但底层数组在内存中仍然占据了大量空间，得不到释放。比较推荐的做法，使用 `copy` 替代 `re-slice`。 
 ```go
 func lastNumsBySlice(origin []int) []int {  
	return origin[len(origin)-2:]  
}  
  
func lastNumsByCopy(origin []int) []int {  
	result := make([]int, 2)  
	copy(result, origin[len(origin)-2:])  
	return result  
}
```
上述两个函数的作用是一样的，取 origin 切片的最后 2 个元素。

- 第一个函数直接在原切片基础上进行切片。
- 第二个函数创建了一个新的切片，将 origin 的最后两个元素拷贝到新切片上，然后返回新切片。

### Go Slice Tricks Cheat Sheet
#### Copy
![[Pasted image 20240724175623.png]]
#### Append
![[Pasted image 20240724175633.png]]
### Delete
![[Pasted image 20240724175648.png]]

#### Delete(GC)
![[Pasted image 20240724175656.png]]
#### Insert
![[Pasted image 20240724175703.png]]
#### Filter
![[Pasted image 20240724175709.png]]
#### Push
![[Pasted image 20240724175722.png]]
![[Pasted image 20240724175738.png]]
#### Pop
![[Pasted image 20240724175808.png]]
![[Pasted image 20240724175815.png]]
## References
- [切片(slice)性能及陷阱](https://geektutu.com/post/hpg-slice.html)
- [Go Slice Tricks Cheat Sheet](https://ueokande.github.io/go-slice-tricks/)