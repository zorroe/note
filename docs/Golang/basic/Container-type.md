## 数组

> **数组** 是一个由 **固定长度** 的特定类型元素组成的序列，一个数组可以由零个或多个元素组成。数组的长度是固定的

### 数组声明

可以使用 `[n]Type` 来声明一个数组。其中 `n` 表示数组中元素的数量， `Type` 表示每个元素的类型。

> * [n]Type表示数组
>
> * []Type表示切片
>
> * 长度是数组类型的一部分，因此，`num[5] int`和 `num[10]int`是不同的类型

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

### 数组长度

使用内置的 `len` 函数将返回数组中元素的个数，即数组的长度。

```go
func main(){
    a := [3]int{1, 2, 3}
    fmt.Println("数组的长度：",len(a))
}
```

### 数组遍历

```go
for index, val := range arr {
   fmt.Printf("index:%d, val:%d\n", index, val)
}
```

### 数组是值类型

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

## 切片

>切片是对数组的一个连续片段的引用，所以切片是一个引用类型。切片 本身不拥有任何数据，它们只是对现有数组的引用，每个切片值都会将数组作为其底层的数据结构。slice 的语法和数组很像，只是没有固定长度而已。

### 创建切片

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

### 切片的长度和容量

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

### 切片元素的修改

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

### 追加切片元素

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

### 多维切片

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

## Map

在 Go 语言中，`map` 是散列表(哈希表)的引用。它是一个拥有键值对元素的**无序集合**，在这个集合中，键是唯一的，可以通过键来获取、更新或移除操作。无论这个散列表有多大，这些操作基本上是通过常量时间完成的。所有可比较的类型，如 `int` ，`string` 等，都可以作为 `key` 。

### 创建Map

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

### Map操作

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

### Map是引用类型

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