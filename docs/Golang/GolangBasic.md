## 开发环境搭建

> 在进行开发之前学会如何搭建环境对于学习所有编程语言都是至关重要的

### 源码包下载

Go源码包下载地址：https://golang.google.cn/dl/

### 安装

对于win平台，下载之后，双击，一路next即可

```shell
go version
```

配置GOPATH

![image-20220710110504118](https://cdn.jsdelivr.net/gh/zorroe/img-host@main/img/2022-07-10-16-00-20-73a45812bbff41079e5886b9aaba983a-2022-07-10-11-05-04-73a45812bbff41079e5886b9aaba983a-image-20220710110504118-f5f5af-96e9f6.png)

### IDE安装

* 下载GoLand：https://www.jetbrains.com/go/

## 第一个Go程序

> 开始”Hello Go吧“

```go
// hello.go

package main   // 声明 main 包

import "fmt"   // 导入 fmt 包，打印字符串时需要用到

func main(){   // 声明 main 主函数
    fmt.Println("Hello, Go!")  // 打印 Hello Go!
}
```

## 变量与常量

### 变量

> 给一段指定的内存空间起名，方便操作这段内存。

* **方法1**：声明一个变量，默认值为0

  ```go
  package main
  
  import "fmt"
  
  func main(){
      // 方法一：声明一个变量, 默认的值是0
      var a int
      fmt.Println("a = ", a)
      fmt.Printf("a的类型是: %T\n", a)
  }
  ```

* **方法2**：声明一个变量，并初始化一个值

  ```go
  package main
  
  import "fmt"
  
  func main(){
      // 方法二：声明一个变量, 初始化一个值
      var b int = 100
      fmt.Printf("b = %d, type of b = %T\n", b, b)
  
      var bb string = "从0到Go语言微服务架构师"
      fmt.Printf("bb = %s, bb的类型是: %T\n", bb, bb)
  }
  ```

* **方法3**：在初始化的时候，可以省去数据类型，通过值去自动匹配当前变量的数据类型

  ```go
  package main
  
  import "fmt"
  
  func main(){
  
      // 方法三：在初始化的时候，可以省去数据类型，通过值去自动匹配当前变量的数据类型
      var c = 100
      fmt.Printf("c = %d, type of c = %T\n", c, c)
  
      var cc = "Go语言微服务架构师核心22讲"
      fmt.Printf("cc = %s, cc的类型是: %T\n", cc, cc)
  }
  ```

* **方法4**：短声明，只能在函数内

  ```go
  package main
  
  import "fmt"
  
  func main(){
      
      // 注: 短声明是在函数或方法内部使用, 不支持全局变量声明！！！！
      e := 100
      fmt.Printf("e = %d, e的类型是: %T\n", e, e)
  
      f := "Go语言极简一本通"
      fmt.Printf("f = %s, f的类型是: %T\n", f, f)
  }
  ```

* 多变量声明

  ```go
  package main
  
  import "fmt"
  
  func main(){
  	// 声明多个变量
      var xx, yy int = 100, 200
      var kk, wx = 300, "write_code_666(欢喜哥)"
  
      var (
          nn int = 100
          mm bool = true
      )
  }
  ```

### 常量

> 常量（constant）表示固定的值，在计算机程序运行时，不会被程序修改

* 使用`const`关键字定义一个常量。常量定义的时候要赋值

  ```go
  package main
  
  import "fmt"
  
  func main(){
      // 常量
      const length int = 10
      // length = 100  // 常量是不允许被修改的
      fmt.Println("length = ", length)
  }
  ```

* `const`定义枚举类型

  ```go
  package main
  
  import "fmt"
  
  // const来定义枚举类型
  const (
      BEIJING = 0
      SHANGHAI = 1
      SHENZHEN = 2
  )
  
  func main() {
      fmt.Println("BEIJING = ", BEIJING)      // 0
      fmt.Println("SHANGHAI = ", SHANGHAI)    // 1
      fmt.Println("SHENZHEN = ", SHENZHEN)    // 2
  }
  ```

* iota

  ```go
  const (
  	BEIJING = 10 * iota // iota = 0
  	SHANGHAI			// iota = 1
  	SHENZHEN			// iota = 2
  )
  
  func main() {
  	fmt.Println(BEIJING)  // 10
  	fmt.Println(SHANGHAI) // 20
  	fmt.Println(SHENZHEN) // 30
  }
  ```

### 关键字

> Go语言中有25个关键字

|    break     |     default     |    func    |  interface  |   select   |
| :----------: | :-------------: | :--------: | :---------: | :--------: |
|   **case**   |    **defer**    |   **go**   |   **map**   | **struct** |
|   **chan**   |    **else**     |  **goto**  | **package** | **switch** |
|  **const**   | **fallthrough** |   **if**   |  **range**  |  **type**  |
| **continue** |     **for**     | **import** | **return**  |  **var**   |

## 基本数据类型

> 在静态类型语言（C++/Java/Golang等）中规定在创建一个变量或者常量时，必须要指定出相应的数据类型，否则无法给变量分配内存

### 整型

* 有符号整型：`int8`、`int16`、`int32`、`int64`、`int`
* 无符号整型：`uint8`、`uint16`、`uint32`、`uint64`、`uint`

![11011651636282_.pic](https://cdn.jsdelivr.net/gh/zorroe/img-host@main/img/2022-07-10-16-00-28-bfebcdb12db8025069f86bb786e6ebf8-e6c9d24ely1h1w8do4sznj20td0cugn5-6df282.jpg)

> * 除非对整型的大小有特定的需求，否则你通常应该使用`int`表示整型宽度，在32位系统下是32位，在64位系统下是64位
> * 对于 `int8` ， `int16` 等这些类型后面有一个数值的类型来说，它们能表示的数值个数是固定的。所以，在有的时候：例如在二进制传输、读写文件的结构描述(为了保持文件的结构不会受到不同编译目标平台字节长度的影响)等情况下，使用更加精确的 `int32` 和 `int64` 是更好的。

### 浮点型

> 浮点型表示存储的数据是实数，如3.145。

|  类型   | 字节数 |     说明      |
| :-----: | :----: | :-----------: |
| float32 |   4    | 32 位的浮点型 |
| float64 |   8    | 64 位的浮点型 |

> 浮点数能表示的数值很大，但是会有精度上的损失
>
> * `float32`大约能表示小数点后6位
> * `float64`大约能表示小数点后15位

### 字符

> 字符串中的每一个元素叫做**字符**，定义字符使用**<u>单引号</u>**

| 类 型 | 字 节 数 |                            说 明                             |
| :---: | :------: | :----------------------------------------------------------: |
| byte  |    1     | 表示`UTF-8`字符串的单个字节的值，表示的是`ASCII`码表中的一个字符,`uint8`的别名类型 |
| rune  |    4     |           表示单个`unicode`字符，`int32`的别名类型           |

* byte类型只能表示$2^8$个值，若响表示其他值，比如中文，就需要使用`rune`类型

### 字符串

>字符串在 Go 语言中是以基本数据类型出现的，定义字符串使用**<u>双引号</u>**

常见的转义字符

| 转 义 字 符 |                   含 义                   |
| :---------: | :---------------------------------------: |
|    `\r`     |          回车符 return，返回行首          |
|    `\n`     | 换行符 new line, 直接跳到下一行的同列位置 |
|    `\t`     |                制表符 TAB                 |
|    `\'`     |                  单引号                   |
|    `\"`     |                  双引号                   |
|    `\\`     |                  反斜杠                   |

定义多行字符串的方法

* 双引号书写字符串被称为字符串字面量（string literal），这种字面量不能跨行。
* 多行字符串需要使用反引号“`”，多用于内嵌源码和内嵌数据。

### 布尔类型

> `true`或者`false`
>
> 在Go中，true不等于1，false不等于0

### 复数类型

>复数型用于表示数学中的复数，如 1+2j、1-2j、-1-2j 等。在 Go 语言中提供了两种精度的复数类型：`complex64` 和`complex128`，分别对应`float32`和`float64`两种浮点数精度

|    类 型     | 字 节 数 |                        说 明                        |
| :----------: | :------: | :-------------------------------------------------: |
| `complex64`  |    8     | 64 位的复数型，由`float32`类型的实部和虚部联合表示  |
| `complex128` |    16    | 128 位的复数型，由`float64`类型的实部和虚部联合表示 |

### `fmt`格式输出

| **格式** |                           **含义**                           |
| :------: | :----------------------------------------------------------: |
|    %%    |                         一个%字面量                          |
|    %b    | 一个二进制整数值(基数为 2)，或者是一个(高级的)用科学计数法表示的指数为 2 的浮点数 |
|  **%c**  | **字符型。可以把输入的数字按照 ASCII 码相应转换为对应的字符** |
|  **%d**  |                **一个十进制数值(基数为 10)**                 |
|  **%f**  |            **以标准记数法表示的浮点数或者复数值**            |
|    %o    |               一个以八进制表示的数字(基数为 8)               |
|  **%p**  | **以十六进制(基数为 16)表示的一个值的地址，前缀为`0x`,字母使用小写的 a-f 表示** |
|    %q    | 使用 Go 语法以及必须时使用转义，以双引号括起来的字符串或者字节切片[]byte，或者是以单引号括起来的数字 |
|  **%s**  | **字符串。输出字符串中的字符直至字符串中的空字符（字符串以`\0`结尾，这个`\0`即空字符）** |
|  **%t**  |           **以 `true` 或者 `false` 输出的布尔值**            |
|  **%T**  |                **使用 Go 语法输出的值的类型**                |
|    %x    |  以十六进制表示的整型值(基数为十六)，数字 a-f 使用小写表示   |
|    %X    |  以十六进制表示的整型值(基数为十六)，数字 A-F 使用小写表示   |

## 容器类型

### 数组

> **数组** 是一个由 **固定长度** 的特定类型元素组成的序列，一个数组可以由零个或多个元素组成。数组的长度是固定的

#### 数组声明

可以使用 `[n]Type` 来声明一个数组。其中 `n` 表示数组中元素的数量， `Type` 表示每个元素的类型。

```go
// 一维数组  
a := [3]int{1, 2, 3}
b := [...]int{1, 2, 3, 4, 5}
// 下标1为100，下标3为300，其他为默认值
c := [5]int{1: 100, 3: 300}
// 结构体数组
d := [...]struct {
    name string
    age  int
}{
    {"jack", 17},
    {"Lucy", 20},
}
// 二维数组
e := [2][3]int{{1, 2, 3}, {4, 5, 6}}
f := [...][3]int{{1, 2, 3}, {4, 5, 6}}
```

#### 数组长度

使用内置的 `len` 函数将返回数组中元素的个数，即数组的长度。

```go
func main(){
    a := [3]int{1, 2, 3}
    fmt.Println("数组的长度：",len(a))
}
```

#### 数组遍历

```go
for index, val := range arr {
   fmt.Printf("index:%d, val:%d\n", index, val)
}
```

#### 数组是值类型

Go 中的数组是值类型而不是引用类型。当数组赋值给一个新的变量时，该变量会得到一个原始数组的一个副本。如果对新变量进行更改，不会影响原始数组。

```go
func main() {
   arr := [...]int{10, 20, 30}
   copy := arr
   copy[0] = 0
   fmt.Println(arr)  // [10 20 30]
   fmt.Println(copy) // [0 20 30]
}
```

### 切片

>切片是对数组的一个连续片段的引用，所以切片是一个引用类型。切片 本身不拥有任何数据，它们只是对现有数组的引用，每个切片值都会将数组作为其底层的数据结构。slice 的语法和数组很像，只是没有固定长度而已。

#### 创建切片

使用 `[]Type` 可以创建一个带有 `Type` 类型元素的切片。

```go
// 声明整型切片
var numList []int

// 声明一个空切片
var numListEmpty = []int{}
```

使用 `make` 函数构造一个切片，格式为 `make([]Type, size, cap)`

```go
numList := make([]int, 3, 5)
```

对数组进行片段截取创建一个切片

```go
func main() {
   arr := [...]int{10, 20, 30, 40, 50}
   s1 := arr[1:4]   // 左闭右开
   fmt.Println(arr) // [10 20 30 40 50]
   fmt.Println(s1)  // [20 30 40]
}
```

#### 切片的长度和容量

一个 slice 由三个部分构成：**指针** 、 **长度** 和 **容量** 。

* **指针**指向第一个 slice 元素对应的底层数组元素的地址，要注意的是 slice 的第一个元素并不一定就是数组的第一个元素。
* **长度**对应 slice 中元素的数目；长度不能超过容量
* **容量**是从 slice 的开始位置到底层数据的结尾位置。简单的讲，容量就是从创建切片索引开始的底层数组中的元素个数，而长度是切片中的元素个数。

```go
func main() {
	s1 := make([]int, 3, 5)
	fmt.Println(len(s1)) // 长度为3
	fmt.Println(cap(s1)) // 容量为5
}
```

slice 是引用类型，如果不赋值的话，他的默认值是`nil`

```go
var numList []int
fmt.Println(numList == nil) // true
```

切片之间不能比较，因此我们不能使用 `==` 操作符来判断两个 slice 是否含有全部相等元素。特别注意，如果你需要测试一个 slice 是否是空的，使用 `len(s) == 0` 来判断，而不应该用 `s == nil` 来判断。

#### 切片元素的修改

切片自己不拥有任何数据。它只是底层数组的一种表示。对切片所做的任何修改都会反映在底层数组中。

```go
func modifySlice() {
   var arr = [...]string{"Go", "Java", "PHP", "Python"}
   s := arr[1:]
   fmt.Println(s)   // [Java PHP Python]
   fmt.Println(arr) // [Go Java PHP Python]
   s[0] = "Ruby"
   fmt.Println(s)   // [Ruby PHP Python]
   fmt.Println(arr) // [Go Ruby PHP Python]
}
```

#### 追加切片元素

当新的元素被添加到切片时，如果容量不足，会创建一个新的数组，现有数组会被复制到这个新数组中，并返回新的引用，**新切片的容量是旧切片容量的两倍**

```go
func appendSlice() {
   s := []string{"Go"}
   fmt.Println(cap(s)) // 1
   //追加一个元素
   s = append(s, "Java")
   fmt.Println(cap(s)) // 2
   //追加两个元素
   s = append(s, "C", "C++")
   fmt.Println(cap(s)) // 4
   //追加一个切片
   s = append(s, []string{"Rust", "PHP"}...)
   fmt.Println(cap(s)) // 8
   fmt.Println(s)
}
```

#### 多维切片

类似于数组，切片也可以多个维度

```go
func mSlice() {
	var s = [][]string{
		{"1", "Java"},
		{"2", "Python"},
		{"3", "Go"},
	}
	fmt.Println(s)
}
```

### Map

在 Go 语言中，`map` 是散列表(哈希表)的引用。它是一个拥有键值对元素的**无序集合**，在这个集合中，键是唯一的，可以通过键来获取、更新或移除操作。无论这个散列表有多大，这些操作基本上是通过常量时间完成的。所有可比较的类型，如 `int` ，`string` 等，都可以作为 `key` 。

#### 创建Map

使用 `make` 函数传入键和值的类型，可以创建 map 。具体语法为 `make(map[KeyType]ValueType)` 。

```go
// 创建一个键类型为 string 值类型为 int 名为 scores 的 map
map1 := make(map[string]int)
map2 := make(map[string]string)
```

我们也可以用 map 字面值的语法创建 map ，同时还可以指定一些最初的 key/value ：

```go
var map3 map[string]string = map[string]string{
		"name":   "Jack",
		"gender": "female",
		"score":  "80",
	}
fmt.Println(map3)
```

```go
map4 := map[string]string{
		"name":   "Jack",
		"gender": "female",
		"score":  "80",
}
fmt.Println(map4)
```

#### Map操作

```go
func mapMethod() {
	var map1 = map[string]string{
		"name":   "Jack",
		"gender": "female",
		"score":  "80",
	}
	fmt.Println(map1) // map[gender:female name:Jack score:80]
	// 添加元素
    map1["email"] = "131@qq.com"
	fmt.Println(map1) // map[email:131@qq.com gender:female name:Jack score:80]
	// 修改元素
    map1["email"] = "222@153.com"
	fmt.Println(map1["email"]) // 222@153.com
	// 删除元素
    delete(map1, "email")
	fmt.Println(map1) // map[gender:female name:Jack score:80]
	// 判断元素是否存在
    _, exist := map1["email"]
	fmt.Println(exist) // false
	// 取元素
    value, exist := map1["name"]
	fmt.Println(value, exist) // Jack true
    // 遍历元素
	for key, value := range map1 {
		fmt.Println(key, value)
	}
}
```

#### Map是引用类型

当 `map` 被赋值为一个新变量的时候，它们指向同一个内部数据结构。因此，改变其中一个变量，就会影响到另一变量。

```go
func mapByReference() {
   var steps = map[string]string{
      "name":   "Jack",
      "gender": "female",
      "score":  "80",
   }
   steps2 := steps

   steps2["name"] = "Lucy"
   steps2["gender"] = "male"
   steps2["score"] = "99"

   fmt.Println(steps)  // map[gender:male name:Lucy score:99]
   fmt.Println(steps2) // map[gender:male name:Lucy score:99]
}
```