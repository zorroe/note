>**函数**是基于功能或逻辑进行封装的可复用的代码结构。将一段功能复杂、很长的一段代码封装成多个代码片段(即函数)，有助于提高代码可读性和可维护性。

## 函数的声明

函数的声明语法如下

```go
func function_name(parameter_list) (result_list) {
    //函数体
}
```

例如

```go
// 无返回值
func printHello() {
   fmt.Println("Hello")
}

// 函数返回一个无名变量，返回值列表的括号可以省略
func sum1(x int, y int) int {
   return x + y
}

// 参数的类型一致，只在最后一个参数后面添加类型
func sum2(x, y int) int {
   return x + y
}
```

## 可变参数

### 多个类型一致的参数

在参数类型前面加 `...` 表示一个切片，用来接收调用者传入的参数。

> 如果该函数下有其他类型的参数，这些其他参数必须放在参数列表的前面，切片必须放在最后。

```go
// 多个类型一致的参数
func show(args ...string) int {
   sum := 0
   for index, arg := range args {
      fmt.Printf("%d:%s\n", index, arg)
      sum++
   }
   return sum
}
```

### 多个类型不一致的参数

如果传多个参数的类型都不一样，可以指定类型为 `...interface{}`

```go
// 多个类型不一致的参数
func printType(args ...interface{}) {
   for _, arg := range args {
      switch arg.(type) {
      case int:
         fmt.Println("int")
      case string:
         fmt.Println("string")
      case bool:
         fmt.Println("bool")
      default:
         fmt.Println("default")
      }
   }
}
```

## 解序列

使用 `...` 可以用来解序列，将函数的可变参数（即切片）一个一个取出来，传递给另一个可变参数的函数

```go
// 使用...解序列
func func1() {
   var s []string
   s = append(s, []string{"Tank", "Monika", "Lucy"}...)
   fmt.Println(s)
}
```

## 返回值

Go可以有多个返回值

```go
// Go函数可以有多个返回值
func returnFunc1() (string, int) {
   return "jack", 22
}
```

Go支持返回带有变量名的值，上面一个函数等于下面

```go
func returnFunc2() (name string, age int) {
   name = "Jack"
   age = 18
   return
}
```

## 匿名函数

没有名字的函数就是匿名函数，只有函数体，没有函数名。一般定义后立即使用

```go
func main() {
	func(name string) {
		fmt.Println(name)
	}("Jack")
}
```

## 内部方法与外部方法

在Go中，函数通过函数名首字母大小写实现控制对方法的访问权限

* 当方法的首字母为大写的时候，这个方法对所有包都是public，其他包可以随意访问
* 当方法的首字母为小写的时候，这个方法是private，其他包无法访问