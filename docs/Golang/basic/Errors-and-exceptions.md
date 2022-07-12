## 错误

### 内建错误

在Go中。**错误**使用内建的`error`类型表示。`error`类型是一个接口类型。它的定义如下

```go
type error interface {
	Error() string
}
```

`error`有一个签名为`Error() string`的方法。所有实现该接口的类型都可以当作一个类型错误。`Error()`方法给出了错误的描述。`fmt.Println()`在打印错误时，会在内部调用`Error() string`方法来得到该错误的描述。

下面例子用来演示打开文件不存在的错误

```go
func main() {
	file, err := os.Open("./a.txt")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(file.Name(), " Open Successfully")
}
```

当文件不存在时，会报如下错误

```go
open ./a.txt: The system cannot find the file specified.
```

### 自定义错误

使用`errors`包中的`New`函数可以创建自定义错误。下面是实现举例

```go
func area(a, b int) (int, error) {
   if a < 0 || b < 0 {
      return 0, errors.New("长宽不可小于0")
   }
   return a * b, nil
}

func main() {
   a := 100
   b := -10
   area, err := area(a, b)
   if err != nil {
      fmt.Println(err.Error())
      return
   }
   fmt.Println(area)
}
```

### 给错误添加更多信息

上面的程序能够报出自定义错误，但是没有具体说明那个数据出了问题。可以使用`fmt.Errorf()`函数，这个函数返回一个`error`类型。可以用来给错误填入更多信息

```go
func area(a, b int) (int, error) {
	if a < 0 || b < 0 {
		return 0, fmt.Errorf("计算错误，长度%d或者宽度%d不可小于0", a, b)
	}
	return a * b, nil
}

func main() {
	a := 100
	b := -10
	area, err := area(a, b)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(area)
}
```

## 异常

错误和异常是两个不同的概念。错误指的是可能出现问题的地方出了问题，异常指的是不应该出现问题的地方出了问题

### panic

在有些情况，当程序发生异常时，无法继续运行。在这种情况下，我们会使用`panic`来终止程序。当函数发生`panic`时，它会终止运行，在执行完所有的延迟函数后，程序返回到该函数的调用方，这样的过程会一直持续下去，知道当前协程的所有函数都返回退出，然后程序会打印出`panic`信息，接着打印出栈跟踪，最终程序停止。

我们应该尽可能地使用错误，而不是`panic`或`recover`。只有当程序不能继续运行的时候，才应该使用`panic`和`recover`机制。

`panic`有两个合理的用例：

* 发生了一个不能恢复的错误，此时程序不能继续运行。一个例子就是web服务器无法绑定所要求的端口。在这种情况下，就应该使用`panic`因为如果绑定不了端口，什么都做不了
* 发生了一个编程上的错误，假设我们有一个就收指针参数的方法，而其他人使用`nil`作为参数进行调用。在这种情况下，我们可以使用`panic`，因为这是一个编程错误：用`nil`参数调用了一个只能接收合法指针的方法

### 触发panic

下面是内建函数`panic`的签名

```go
func panic(v interface{})
```

当程序终止时，会打印传入`panic`函数

```go
func main() {
	panic("panic error")
}

// panic: panic error                                        
//                                                          
// goroutine 1 [running]:                                    
// main.main()                                               
//         E:/goproject/src/go-study/error/panic01.go:4 +0x27
// 
// Process finished with the exit code 2

```

### 发生panic时的defer

当函数发生`panic`时，会终止运行，在执行完所有的延迟函数后，程序返回到该函数的调用方。这样的过程会一直持续下去，知道当前协程的所有函数都返回退出，然后程序会打印出`panic`信息，接着打印出堆栈追踪信息，最终程序终止

```go
func MyTest() {
   defer fmt.Println("defer MyTest")
   panic("panic MyTest")
}

func main() {
   defer fmt.Println("defer main")
   MyTest()
}
```

程序的运行结果如下

```go
// defer MyTest
// defer main                                                 
// panic: panic MyTest 
```

 ### recover

`recover`是一个内建函数，用于重新获得`panic`协程的控制，下面是内建函数`recover`的签名：

```go
func recover() interface{}
```

`recover`必须在`defer`函数中才能生效，在其他作用域下，他是不工作的。在延迟函数内调用`recover`，可以取到`panic`的错误信息，并且停止`panic`续发事件，程序运行恢复正常，下面是例子

```go
func outOfArray(x int) {
   defer func() {
      // recover() 可以将捕获到的 panic 信息打印
      err := recover()
      if err != nil {
         fmt.Println(err)
      }
   }()
   var array [5]int
   array[x] = 1
}
func main() {
   // 故意制造数组越界 触发 panic
   outOfArray(20)
   // 如果能执行到这句 说明 panic 被捕获了
   // 后续的程序能继续运行
   fmt.Println("main...")
}

// runtime error: index out of range [20] with length 5
// main...
```

虽然程序出发了`panic`，但由于我们使用了`recover()`捕获了`panic`异常，并输出`panic`信息，即使`panic`会导致整个程序退出，但在退出前有`defer`延迟函数，还是得执行完`defer`，然后程序还会继续执行下去

**注**：只有在相同的协程中调用`recover`才管用，`recover`不能恢复不同协程的`panic`。

