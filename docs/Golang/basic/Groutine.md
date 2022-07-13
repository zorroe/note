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

下面是举例：

```go
func PrintInfo() {
   fmt.Println("学习Go语言")
}

func main() {
   // 开启一个协程执行 PrintInfo 函数
   go PrintInfo()
   // 使主协程休眠 1 秒
   time.Sleep(1 * time.Second)
   // 打印 main
   fmt.Println("main")
}
```

主函数运行子啊一个特殊的协程之上，这个协程称之为**主协程**

## 启动多个协程

```go
func PrintNum(num int) {
   for i := 0; i < 3; i++ {
      fmt.Println(num)
      // 避免观察不到并发效果 加个休眠
      time.Sleep(100 * time.Millisecond)
   }
}

func main() {
   // 开启 1 号协程
   go PrintNum(1)
   // 开启 2 号协程
   go PrintNum(2)
   // 使主协程休眠 1 秒
   time.Sleep(time.Second)
}
```

