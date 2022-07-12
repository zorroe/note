> **select**语句用在多个发送/接收通道操作中进行选择
>
> * `select`语句会一直阻塞，直到发送/接收操作准备就绪
> * 如果有多个通道准备完毕，`select`会随机地选取其中之一执行

## select语法

```go
select {
    case expression1:
        code
    case expression2:
    	code
    default:
    	code
}
```

例如

```go
func main() {
	// 创建3个通道
	ch1 := make(chan string, 1)
	ch2 := make(chan string, 1)
	ch3 := make(chan string, 1)

	// 往通道1发送数据
	ch1 <- "Hello Jack"
	// 往通道2发送数据
	ch2 <- "Hello Mia"
	// 往通道3发送数据
	ch3 <- "Hello Lucy"

	select {
	// 如果从通道1接收到数据
	case message1 := <-ch1:
		fmt.Println("ch1 received:", message1)
	// 如果从通道2接收到数据
	case message2 := <-ch2:
		fmt.Println("ch2 received:", message2)
	// 如果从通道3接收到数据
	case message3 := <-ch3:
		fmt.Println("ch3 received:", message3)
	default:
		fmt.Println("无数据")
	}
}
```

这三个case里面的语句都有可能被打印出来（只打印一个case），具体取决于哪一个通道首先接收到数据

## select的应用

每个任务执行的时间不同，使用`select`语句等待相应的通道发出响应。`select`会选择首先响应完成的`task`，而忽略其他的响应。所有使用这种方法，我们可以做多个task，并给用户返回最快的`task`结果。

例如

```go
func task1(ch chan string) {
   time.Sleep(time.Second * 5)
   ch <- "Hello, Jack"
}
func task2(ch chan string) {
   time.Sleep(time.Second * 7)
   ch <- "Hello, Mia"
}
func task3(ch chan string) {
   time.Sleep(time.Second * 2)
   ch <- "Hello, Amy"
}

func main() {
   // 创建3个通道
   ch1 := make(chan string, 1)
   ch2 := make(chan string, 1)
   ch3 := make(chan string, 1)
   go task1(ch1)
   go task2(ch2)
   go task3(ch3)
   select {
   case message := <-ch1:
      fmt.Println("ch1 received:", message)
   case message := <-ch2:
      fmt.Println("ch2 received:", message)
   case message := <-ch3:
      fmt.Println("ch3 received:", message)
   }
}
```

这段程序没有`default`分支，因为如果有了`default`分支，还没有从通道中得到数据，`select`语句就会直接执行`default`分支然后退出了

## 超时处理

如果上面程序中，三个通道一直没有得到数据，且没有`default`分支，那么`select`将会一直阻塞造成死锁。这个时候我们可以设置一个超时时间

```go
func makeTimeOut(ch chan bool, t int) {
	time.Sleep(time.Second * time.Duration(t))
	ch <- true
}

func main() {
	// 创建3个通道
	ch1 := make(chan string, 1)
	ch2 := make(chan string, 1)
	ch3 := make(chan string, 1)
	timeout := make(chan bool, 1)

	go makeTimeOut(timeout, 10)

	select {
	case message := <-ch1:
		fmt.Println("ch1 received:", message)
	case message := <-ch2:
		fmt.Println("ch2 received:", message)
	case message := <-ch3:
		fmt.Println("ch3 received:", message)
	case <-timeout:
		fmt.Println("Timeout, exit!")
	}
}
```

