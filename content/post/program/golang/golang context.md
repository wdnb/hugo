---
title: golang context
date: 2024-07-26T15:37:00+08:00
tags:
  - golang
categories:
  - Program
---
# golang context 

## context 结构

```go
type Context interface {
    Deadline() (deadline time.Time, ok bool)
    Done() <-chan struct{}
    Err() error
    Value(key interface{}) interface{}
}
```

这个接口定义了四个方法：

- Deadline(): 返回 context 的截止时间（如果有的话）。
- Done(): 返回一个 channel，当 context 被取消时，这个 channel 会被关闭。
- Err(): 返回 context 被取消的原因。
- Value(): 返回与 key 关联的值。

## context 使用场景和用途

a. 请求链路传递：在微服务架构中，context 可以用于在不同服务之间传递请求相关的信息，如请求 ID、用户认证信息等。

b. 超时控制：可以使用 context.WithTimeout 或 context.WithDeadline 来设置操作的最大执行时间，防止长时间阻塞。

c. 取消操作：通过 context.WithCancel 创建可取消的 context，可以在需要时取消长时间运行的操作，如 HTTP 请求或数据库查询。

d. 值传递：使用 context.WithValue 可以在调用链中传递键值对，但通常建议仅用于传递请求作用域的数据。

e. goroutine 管理：可以使用 context 来管理一组 goroutine，确保它们在主操作结束时正确退出。

f. API 边界：在设计 API 时，将 context 作为第一个参数传入函数，可以提供一种统一的方式来处理超时、取消和请求作用域的值。

g. 数据库操作：在执行数据库查询时，可以使用 context 来设置超时或在需要时取消操作。

h. HTTP 服务器：在处理 HTTP 请求时，可以使用 context 来确保在客户端断开连接时及时停止相关的处理流程。