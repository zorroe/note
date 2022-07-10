> 指针也是一种类型，也可以创建变量，称为指针变量。指针变量的类型为`*Type`，该指针指向一个`Type`类型的变量。指针变量存储的是某个变量的内存地址，通过记录这个变量的地址，间接操作该变量

![Pointers-in-Golang](https://cdn.jsdelivr.net/gh/zorroe/img-host@main/img/2022-07-10-20-45-25-8f6d625b6dfa48948d19959fb2aa4a6f-e6c9d24egy1h1wg12o4byj20m80as0t5-43aa7e.jpeg)

## 创建指针

* 首先定义普通变量，再通过获取普通变量的地址创建指针

  ```go
  x := "Java牛逼"
  ptr := &x
  fmt.Println(ptr)  // 0xc00005a250
  fmt.Println(*ptr) // Java牛逼
  ```

* 先创建指针并分配好内存，再给指针指向的内存地址写入对应的值

  ```go
  ptr2 := new(string)
  *ptr2 = "Go也牛逼"
  fmt.Println(ptr2)  // 0xc00005a270
  fmt.Println(*ptr2) // Go也牛逼
  ```

* 首先声明一个指针变量，再从其他变量获取内存地址给指针变量

  ```go
  x2 := "Python还行"
  var p *string
  p = &x2
  fmt.Println(p)  // 0xc00005a290
  fmt.Println(*p) // Python还行
  ```

> `&`操作符可以获取变量的内存地址
>
> `*`操作符可以获取地址指向的变量

## 指针的类型

`*(指向变量值的数据类型)`就是对应的指针类型

```go
func pointerType() {
   str1 := "Jack"
   int1 := 1
   bool1 := false
   float1 := 2.50
   fmt.Printf("type of &str1 is :%T\n", &str1)     // *string
   fmt.Printf("type of &int1 is :%T\n", &int1)     // *int
   fmt.Printf("type of &bool1 is :%T\n", &bool1)   // *bool
   fmt.Printf("type of &float1 is :%T\n", &float1) // *float64
}
```

## 指针的零值

如果指针声明后没有进行初始化，其默认零值是`nil`

```go
func nilPtr() {
   x := "Java天下第一"
   var ptr *string
   fmt.Println(x)    // Java天下第一
   fmt.Println(ptr)  // <nil>
   ptr = &x
   fmt.Println(*ptr) // Java天下第一
}
```

## 函数传递指针参数

在函数中对指针参数所作的修改，在函数返回后会保存相应的修改

```go
func changePtr(value *int) {
	*value = 10
}

func main() {
	val := 0
	println(val) // 0
	changePtr(&val)
	println(val) // 10
}
```

## 指针与切片

切片与指针一样是引用类型，如果我们想通过一个函数改变一个数组的值，可以将该数组的切片当作参数传给函数，也可以将这个数组的指针当作参数传给函数。但 Go 中建议使用切片，因为这么写更加简洁易读。

```go
// 使用数组
func changeArr(value [3]int) {
	value[0] = 888
}

// 使用切片
func changeSlice(value []int) {
	value[0] = 888
}

// 使用数组指针
func changeArray(value *[3]int) {
	(*value)[0] = 888
}

func main() {
	x := [3]int{10, 20, 30}
	changeArr(x)
	fmt.Println(x) // [10 20 30]

	changeSlice(x[:])
	fmt.Println(x) // [888 20 30]

	y := [3]int{100, 200, 300}
	changeArray(&y)
	fmt.Println(y) // [888 200 300]
}
```

## Go中不支持指针运算

```go
func main() {
	num := 10
	ptr := &num
	ptr++  // invalid operation: ptr++ (non-numeric type *int)
}
```

