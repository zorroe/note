> **通道（channel）**，就是一个管道，可以想象成Go协程之间通信的管道，它是一种队列式的数据结构，遵循先入先出的规则

## 通道的声明

每个通道只能传递一种数据类型的数据，声明规则如下，通道的零值为`nil`

```go
var channel_name chan channel_type
```

例如下面，定义一个`string`类型的通道。

```go
var ch chan string  
```

## 通道的初始化

声明完通道之后，通道的值为`nil`，不能直接使用，必须先使用`make`函数对通道进行初始化操作。

```go
ch = make(chan channel_type)
```

例如：

```go
ch = make(chan string)
```

也可以使用简短声明，一次性定义一个通道

```go
ch := make(chan string)
```

## 使用通道发送和接收数据

往通道中写入数据

```go
channel_name <- data
```

从通道中取出数据

```go
value := <- channel_name
```

例如：

```go
func func1(c chan string) {
	c <- "Java微服务"
}

func main() {
	// 创建一个通道
	cs := make(chan string)
	fmt.Println("开始学习")
	// 开启协程
	go PrintChan(cs)
	res := <-cs
	fmt.Println(res)
	fmt.Println("微服务学习结束")
}
```

这个程序模拟两个协程并发调用的场景，在`main`函数中，创建了一个通道，在`main`函数中先打印“开始学习”，然后开启协程运行`func1`函数，而`main`函数通过协程接收数据，主协程发生阻塞等待通道`ch`发送的数据，在`func1`中，通道被写入数据，当写入完成时，主协程接收数据，解除阻塞状态，然后打印从通道接收到的数据。最后主协程打印“微服务学习结束”

* 发送与接收默认是阻塞的

从上面例子可以得到：如果通道接收数据没接收完，主协程是不会继续执行下去的。当把数据发送到通道时，会在发送数据的语句处发生阻塞，直到有其他协程从通道读取到数据，才会解除阻塞。以此类推，当读取通道的数据时，如果没有其他协程写入到这个通道，那么读取过程就会一直保持阻塞

## 通道的关闭

对于一个使用完的通道，我们要将其关闭

```go
close(channel_name)
```

对于一个已经关闭的通道如果再次关闭会导致报错，我们可以在接收数据的时候，判断通道是否已经关闭，从通道读取数据返回的第二个值表示是否关闭，`false`表示已经关闭，`true`表示未关闭

```go
value, ok := <- channel_name
```

## 通道的容量与长度

使用`make`函数创建通道的时候，传入的第二个参数表示容量

* 容量为0时表示通道中不能存放数据，在传入数据的时候，必须有人接收，否则会报错。此时的通道称为无缓冲通道（**无缓冲通道**）
* 容量为1时表示通道中只能缓存一个数据，若通道中已有一个数据，此时再往里面传入数据会造成阻塞，利用这一点可以实现锁（**缓冲通道**）
* 容量大于1时表示通道中可以存放多个数据，可以用于多个协程之间的通信管道，共享资源（**缓冲通道**）

使用`cap`和`len`获取通道的容量与长度

```go
func main() {
	c := make(chan string, 5)
	fmt.Println("初始化后")
	fmt.Println("cap:", cap(c)) // 5
	fmt.Println("len:", len(c)) // 0
	c <- "Jack"
	c <- "Lucy"
	fmt.Println("传入两个数据后")
	fmt.Println("cap:", cap(c)) // 5
	fmt.Println("len:", len(c)) // 2
	<-c
	fmt.Println("取出一个数据后")
	fmt.Println("cap:", cap(c)) // 5
	fmt.Println("len:", len(c)) // 1
}
```

## 单向通道

上述通道为双向通道，既可输入数据，也可输出数据

* `<-chan`表示只读通道

* `chan<-`表示只写通道

例如：

```go
func counter(out chan<- int) {
   for i := 0; i < 100; i++ {
      out <- i
   }
   close(out)
}

func squarer(out chan<- int, in <-chan int) {
   for i := range in {
      out <- i * i
   }
   close(out)
}
func printer(in <-chan int) {
   for i := range in {
      fmt.Println(i)
   }
}

func main() {
   ch1 := make(chan int)
   ch2 := make(chan int)
   go counter(ch1)
   go squarer(ch2, ch1)
   printer(ch2)
}
```

## 使用通道做锁

当通道容量为1时，只能缓存一个数据。可以用作锁

例如：

```go
func increment(bc chan bool, x *int) {
   bc <- true
   *x += 1
   <-bc
}

func main() {
	num := 0
	bc := make(chan bool, 1)
	for i := 0; i < 10000; i++ {
		increment(bc, &num)
	}
	time.Sleep(time.Second)
	fmt.Println(num)
}
```