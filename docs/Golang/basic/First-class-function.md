> Go语言拥有**头等函数（First-class Function）**,头等函数是指函数可以被当作变量一样使用，即函数可以被当作参数传递给其他函数，可以作为函数的返回值，还可以被赋值给一个变量

## 把函数赋值给变量

```go
func main() {
   bookFunc := func() {
      fmt.Println("学习Java")
   }
   bookFunc()
   fmt.Printf("bookFunc的类型是 %T \n", bookFunc)
}
```

该程序的运行结果：

```go
// 学习Java
// bookFunc的类型是 func() 
```

## 将函数作为参数

```go
// 函数的参数为一个返回值为string类型的函数
func printRes(show func(author, book string) string) {
   fmt.Println(show("李白", "静夜思"))
}

func main() {
   printRes(func(author, book string) string {
      return author + ": " + book
   })
}
```

## 将函数作为返回值

```go
// 函数的返回值为一个返回值为string类型的函数
func Show() func(author, book string) string {
   return func(author, book string) string {
      return author + ": " + book
   }
}

func main() {
   f := Show()
   fmt.Println(f("曹雪芹", "红楼梦"))
}
```

## 闭包

**闭包** (Closure)是匿名函数的一个特例，当一个匿名函数所访问的变量定义在函数体的外部时候，就称这样的匿名函数为闭包

```go
func startAt(x int) func(y int) int {
	b := func(y int) int {
		return x + y
	}
	return b
}

func main() {
	at := startAt(2)
	fmt.Println(at)        // 0xbfce40
	fmt.Printf("%p\n", at) // 0xbfce40
	fmt.Println(at(2))     // 4
}
```

