---
title: golang 闭包
date: 2023-12-08T16:53:03+08:00
tags:
  - golang
categories:
  - Program
---
# golang 匿名函数&闭包

## 匿名函数

定义: 匿名函数是一种没有名称的函数,可以在声明的同时被调用或赋值给变量。

用处:

- 实现简单的一次性使用的功能,无需定义具名函数。
- 作为高阶函数的参数,例如在`sort.Slice()`中定义排序规则。
- 在goroutine中快速定义并发任务。
- 实现延迟执行的逻辑,如defer语句。

示例:

a. 作为函数参数 (用于回调):
```go
func executeAndPrint(f func() string) {
    fmt.Println(f())
}

func main() {
    executeAndPrint(func() string {
        return "Hello from anonymous function!"
    })
}
```
b. 在 goroutine 中使用:
```go
func main() {
    go func() {
        for i := 0; i < 5; i++ {
            fmt.Printf("Goroutine: %d\n", i)
            time.Sleep(100 * time.Millisecond)
        }
    }()

    time.Sleep(1 * time.Second)
}
```
c. 实现简单的装饰器模式:
```go
func timeTrack(f func()) func() {
    return func() {
        start := time.Now()
        f()
        fmt.Printf("Function took %v\n", time.Since(start))
    }
}

func main() {
    slowFunc := func() {
        time.Sleep(100 * time.Millisecond)
        fmt.Println("Slow function finished")
    }

    timedSlowFunc := timeTrack(slowFunc)
    timedSlowFunc()
}
```

## 闭包

定义: 闭包是一个函数值,它引用了其外部作用域的变量。闭包通常通过在函数内部定义并返回另一个函数来创建。

用处:

- 实现数据封装和信息隐藏。
- 创建函数工厂,生成定制化的函数。
- 实现记忆化,缓存函数的结果。
- 在并发编程中共享状态。

示例:
a. 实现计数器:
```go
func createCounter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}

func main() {
    counter := createCounter()
    fmt.Println(counter()) // 1
    fmt.Println(counter()) // 2
    fmt.Println(counter()) // 3
}
```
b. 实现配置函数:
```go
type Server struct {
    host string
    port int
}

func NewServer(host string, port int) *Server {
    return &Server{host, port}
}

func (s *Server) WithHost(host string) func(*Server) {
    return func(s *Server) {
        s.host = host
    }
}

func (s *Server) WithPort(port int) func(*Server) {
    return func(s *Server) {
        s.port = port
    }
}

func main() {
    server := NewServer("localhost", 8080)
    server.WithHost("example.com")(server)
    server.WithPort(9000)(server)
    fmt.Printf("Server: %+v\n", server)
}
```
c. 实现中间件:
```go
func loggingMiddleware(next http.HandlerFunc) http.HandlerFunc {
    return func(w http.ResponseWriter, r *http.Request) {
        start := time.Now()
        next.ServeHTTP(w, r)
        fmt.Printf("Request to %s took %v\n", r.URL.Path, time.Since(start))
    }
}

func hello(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, World!")
}

func main() {
    http.HandleFunc("/", loggingMiddleware(hello))
    http.ListenAndServe(":8080", nil)
}
```
d. 实现记忆化 (缓存函数结果):
```go
func memoize(f func(int) int) func(int) int {
    cache := make(map[int]int)
    return func(x int) int {
        if val, found := cache[x]; found {
            return val
        }
        result := f(x)
        cache[x] = result
        return result
    }
}

func fibonacci(n int) int {
    if n <= 1 {
        return n
    }
    return fibonacci(n-1) + fibonacci(n-2)
}

func main() {
    memoFib := memoize(fibonacci)
    fmt.Println(memoFib(40)) // 快速计算,结果被缓存
    fmt.Println(memoFib(40)) // 直接从缓存中获取结果
}
```
e. 实现延迟执行:
```go
func delayExecution(f func()) func() {
    var once sync.Once
    return func() {
        once.Do(f)
    }
}

func main() {
    heavyTask := func() {
        fmt.Println("Executing heavy task...")
        time.Sleep(2 * time.Second)
        fmt.Println("Heavy task completed.")
    }

    delayedTask := delayExecution(heavyTask)

    // 第一次调用会执行任务
    delayedTask()

    // 后续调用不会重复执行
    delayedTask()
    delayedTask()
}
```
## References

* [Go 中的闭包](https://giaogiaocat.github.io/go/go-closure/)
* [Closures in Go](https://sunisdown.me/closures-in-go.html "Permalink to Closures in Go")