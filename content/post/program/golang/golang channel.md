---
title: golang channel
date: 2024-07-26T15:37:00+08:00
tags:
  - golang
categories:
  - Program
---
# golang channel 

## go channel 的底层实现原理 （数据结构）

1. 基本结构

channel的核心数据结构是runtime.hchan，它包含以下主要字段：

```go
type hchan struct {
    qcount   uint           // 当前队列中的元素数量
    dataqsiz uint           // 环形队列的大小
    buf      unsafe.Pointer // 指向环形队列的指针
    elemsize uint16         // 每个元素的大小
    closed   uint32         // 标记channel是否已关闭
    elemtype *_type         // 元素类型
    sendx    uint           // 发送操作处理到的位置
    recvx    uint           // 接收操作处理到的位置
    recvq    waitq          // 等待接收的goroutine队列
    sendq    waitq          // 等待发送的goroutine队列
    lock     mutex          // 互斥锁，用于并发控制
}
```
2. 环形队列

channel内部使用环形队列（circular queue）来存储数据。这个队列由buf字段指向，大小由dataqsiz决定。sendx和recvx用于跟踪发送和接收操作的位置。

3. 等待队列

recvq和sendq是两个等待队列，分别用于存储等待接收和发送的goroutine。这些队列中的每个元素都是一个sudog结构，包含了等待的goroutine信息。

4. 锁机制

channel使用mutex来保证并发安全。每次对channel进行操作时，都会先获取这个锁。

5. 操作实现

- 发送操作：
    1. 尝试直接发送给一个等待的接收者
    2. 如果没有等待的接收者，且缓冲区未满，将数据放入缓冲区
    3. 如果缓冲区已满，则将当前goroutine放入sendq等待队列
- 接收操作：
    1. 尝试直接从一个等待的发送者接收
    2. 如果没有等待的发送者，且缓冲区非空，从缓冲区取数据
    3. 如果缓冲区为空，则将当前goroutine放入recvq等待队列

6. 关闭channel

当channel关闭时，会将closed字段设置为1，并唤醒所有等待的goroutine。

这种实现方式使得channel能够高效地支持多个goroutine之间的通信，同时保证了操作的原子性和并发安全。

## channel在不同操作下的行为：

| Channel 状态       | 读取操作 (<-ch)           | 写入操作 (ch <- value)            | 关闭操作 (close(ch))               |
| ---------------- | --------------------- | ----------------------------- | ------------------------------ |
| nil channel      | 永久阻塞                  | 永久阻塞                          | panic: close of nil channel    |
| 已关闭的 channel     | 立即返回零值，第二个返回值为 false  | panic: send on closed channel | panic: close of closed channel |
| 有数据的 channel（未满） | 立即返回一个值               | 成功写入并立即返回                     | 成功关闭；之后读取将先读完数据再返回零值           |
| 有数据的 channel（已满） | 立即返回一个值               | 阻塞直到有空间可写或 channel 关闭         | 成功关闭；之后读取将先读完数据再返回零值           |
| 空的 channel（非nil） | 阻塞直到有数据可读或 channel 关闭 | 成功写入并立即返回                     | 成功关闭；之后读取将立即返回零值               |

总结：

1. 对nil channel的操作除了关闭会panic外，读和写都会导致永久阻塞。
2. 对已关闭的channel，读取会立即返回零值，写入和再次关闭都会导致panic。
3. 对于有数据的channel，其行为取决于channel的容量和当前的数据量。

一些额外的注意事项：

- 多次关闭同一个channel会导致panic，所以在多个goroutine操作同一个channel时需要特别小心。
- 向已关闭的channel发送数据会引发panic，但从已关闭的channel接收数据是安全的。
- 使用select语句可以非阻塞地操作channel，这在需要处理多个channel或需要超时机制时非常有用。

这些行为特性使得Go的channel成为一个强大而安全的并发通信工具。正确理解和使用这些特性可以帮助我们编写更健壮的并发程序。