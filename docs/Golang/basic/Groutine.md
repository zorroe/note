> Go语言的协程始于其他函数或方法一起并发运行的工作方式。协程可看作是轻量级线程。与线程相比，创建一个协程的成本很小。因此在Go应用中，常常会看到很多协程并发运行

## 启动一个协程

在调用函数或者方法时，在前面加上关键字`go`，就可以让一个新的Go协程并发运行

```go
// 定义一个函数
func functionName(parameterList) {
    code
}

// 执行一个函数
functionName(parameterList)

// 开启一个协程执行这个函数
go functionName(parameterList)
```



