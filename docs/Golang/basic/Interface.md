在Go语言中，**接口**就是方法签名（Method Signature）的集合。在面向对象的领域里，接口定义一个对象的行为，接口只指定了对象应该做什么，至于如何实现这个行为，由对象本身去确定。当一个类型实现了接口中的所有方法，我们称它实现了该接口。接口制定了一个类型应该具有的方法，并由该类型决定如何实现这些方法

## 接口的定义

使用`type`关键字定义接口

```go
type interface_name interface{
    method
}
```

## 接口的实现

创建类型或者结构体，并为其绑定接口定义的方法，接收者为该类型或结构体，方法名为接口中定义的方法名，这样就说该类型或者结构体实现了该接口。

```go
type Study interface {
   learn()
}

type Student struct {
   name string
   book string
}

func (s Student) learn() {
   //TODO implement me
   fmt.Printf("%s 在读 %s\n", s.name, s.book)
}

func main() {
   student := Student{
      name: "Jack",
      book: "三国演义",
   }
   student.learn()
}
```

这个程序定义一个`Study`接口，接口中有未实现的方法`learn()`，还定义了一个`Student`结构体，其绑定`learn()`方法，隐式实现了`Study`接口

下面的例子使用了指针接受者实现接口。

```go
type Study interface {
	learn()
}

type Student struct {
	name string
	book string
}

type Worker struct {
	name string
	book string
	by   string
}

func (s Student) learn() {
	fmt.Println(s.name, "学习", s.book)
}

func (w *Worker) learn() {
	fmt.Printf("%s 在读 %s,通过方式 %s", w.name, w.book, w.by)
}

func main() {
	var s1 Study
	var s2 Study

	student2 := Student{
		name: "李四",
		book: "《Go语言极简一本通》",
	}
	s1 = student2
	s1.learn()   // 李四 学习 《Go语言极简一本通》

	student3 := Student{
		name: "王五",
		book: "Go语言微服务架构核心22讲",
	}
	s1 = &student3
	s1.learn()   // 王五 学习 Go语言微服务架构核心22讲

	worker1 := Worker{
		name: "老王",
		book: "从0到Go语言微服务架构师",
		by:   "视频",
	}
	s2 = worker1 // error
	s2 = &worker1  // 老王 在读 从0到Go语言微服务架构师,通过方式 视频
	s2.learn()
}
```

该程序定义了结构体 `Student` ，使用其作为值接受者实现 `Study` 接口。`student2` 的类型为 `Student` ， `student2` 赋值给 `s1` ，由于 `Student` 实现了接口变量 `s1` 所以会有输出。而接下来 `s1` 又被赋值为 `&student3` ，同样有输出。接下来的结构体 `Worker` 使用指针接受者实现 `Study` 接口。`worker1` 的类型为 `Worker` ， `s2` 被赋值为 `&worker1` ，所以会有输出。但如果把 `s2` 赋值为 `worker1` 会报错，对于使用指针接受者的方法，用一个指针或者一个可取得地址的值来调用都是合法的。但接口中存储的具体值(Concrete Value)并不能取到地址，因此对于编译器无法自动获取 `worker1` 的地址，于是程序报错。

## 接口实现多态

```
func main() {
   var s Study

   s1 := Student{
      name: "Jack",
      book: "三国演义",
   }
   s = s1
   s.learn()  // Jack 学习 三国演义
   s1.learn() // Jack 学习 三国演义
}
```

## 接口的内部表示

可以把接口的内部看作`(type value)`。`type`是接口底层的数据类型（Concrete Type），而`value`是具体类型的值。

```go
func main() {
	var s Study

	s1 := Student{
		name: "Jack",
		book: "三国演义",
	}
	s = s1
	fmt.Printf("接口类型:%T\n", s)  // 接口类型:main.Student
}
```

## 空接口

**空接口**是特殊形式的接口类型，没有定义任何方法的接口称为空接口，可以说所有类型都至少实现了空接口，空接口表示为`interface{}`。例如使用空接口作为函数形参，可以传输任何类型的参数。

```go
func ShowType(i interface{}) {
	fmt.Printf("类型: %T, 值: %v\n", i, i)
}

func main() {
	str := "Java"
	ShowType(str) // 类型: string, 值: Java
	num := 3.14
	ShowType(num) // 类型: float64, 值: 3.14
```

接口有两个属性，值和类型，对于空接口来说，这两个属性都为`nil`

```go
func main() {
   var i interface{}
   fmt.Printf("Type:%T,Value:%v", i, i)  // Type:<nil>,Value:<nil>
}
```

### 空接口的常用用法

1. 使用`interface{}`作为类型声明一个实例，这个实例可以承载任何类型的值

   ```go
   func main() {
      var i interface{}
   
      i = "Java"
      fmt.Println(i)
      i = 3.14
      fmt.Println(i)
   }
   ```

   也可以用来接收`array`、`slice`、`map`、`struct`。

   ```go
   func main() {
      x := make([]interface{}, 3)
      x[0] = "PHP"
      x[1] = 3.14
      x[2] = [...]int{1, 2, 3}
   
      fmt.Println(x)  // [PHP 3.14 [1 2 3]]
   }
   ```

   空接口可以承载任何值，但是空接口类型的对象不能把值赋给固定类型对象

   当空接口承载数组和切片后，该对象不能再进行切片

   ```go
   func main() {
      var s = []int{1,2,3}
   
      var i interface{} = s
      
      var s2 = i[1:2]  //error
      fmt.Println(i) 
   }
   ```

## 类型断言

类型断言用于提取接口的底层值(Underlying Value)。使用 `interface.(Type)` 可以获取接口的底层值，其中接口 `interface` 的具体类型是 `Type` 。

```go
func assert(i interface{}) {
   value, ok := i.(int)
   fmt.Println(value, ok)
}

func main() {
	var x interface{} = 3
	assert(x) // 3 true

	var y interface{} = "Go"
	assert(y) // 0 false
}
```

## 实现多个接口

```go
type Animal interface {
   eat()
}

type Plate interface {
   grow()
}

type DCXC struct {
}

func (D DCXC) eat() {
   fmt.Println("冬天是虫子吃肉")
}

func (D DCXC) grow() {
   fmt.Println("夏天是草被割")
}

func main() {
	dcxc := DCXC{}
	dcxc.grow() // 夏天是草被割
	dcxc.eat()  // 冬天是虫子吃肉
}
```

## 接口嵌套

```go
type Animal interface {
   eat()
}

type Plate interface {
   grow()
}

type Life interface {
   Animal
   Plate
}

type DCXC struct {
}

func (D DCXC) eat() {
   fmt.Println("冬天是虫子吃肉")
}

func (D DCXC) grow() {
   fmt.Println("夏天是草被割")
}

func main() {
   var l Life

   dcxc := DCXC{}

   l = dcxc
   l.grow() // 夏天是草被割
   l.eat()  // 冬天是虫子吃肉
}
```

 