> **结构体(struct)** 是一种聚合的数据类型，是由零个或多个任意类型的值聚合成的实体。每个值称为结构体的成员。学过 C 或 C++ 的人都知道结构体，但在 Go 中，没有像 C++ 中的 `class` 类的概念，只有 `struct` 结构体的概念，所以也没有继承。

## 结构体的声明

声明格式如下

```go
type 结构体名 struct{
    属性1名 属性1类型
    属性2名 属性2类型
    ...
}
```

例如：

```go
type Lesson struct {
	name   string // 课程名称
	target string // 学习目标
	spend  int    // 学习时间
}
```

这个结构体也可以简化为

```go
type Lesson struct {
	name, target string
	spend        int
}
```

## 创建结构体

```go
func main() {
	// 使用每个字段名称对字段进行初始化
	lesson1 := Lesson{
		name:   "Java学习",
		target: "熟练掌握Java开发",
		spend:  20,
	}
	
	// 不使用字段名称，直接按字段声明的顺序对字段初始化
	lesson2 := Lesson{"Python学习", "熟练掌握Python开发", 30}
	fmt.Println(lesson2)
}
```

## 匿名结构体

不使用type声明新类型

```go
func main() {
    lesson3 := struct {
        name, target string
        spend        int
    }{name: "Java学习",
      target: "熟练掌握Java开发",
      spend:  20}
    fmt.Println(lesson3)
}
```

## 结构体的零值

```go
type Lesson struct {
	name, target    string
	spend             int
}

func main() {
	// 不初始化结构体
	var lesson = Lesson{}
	fmt.Println("lesson ", lesson)  // {  0} 
}
```

## 初始化结构体

```go
type Lesson struct {
	name, target    string
	spend             int
}

func main() {
	// 赋部分值，未指定字段按照类型赋默认值
	var lesson5 = Lesson{
		name:   "PHP学习",
		target: "全面掌握PHP",
	}
	fmt.Println(lesson5) // {PHP学习 全面掌握PHP 0}

	var lesson6 = Lesson{
		name:   "PHP学习",
		target: "全面掌握PHP",
		spend:  50,
	}
	fmt.Println(lesson6) // {PHP学习 全面掌握PHP 50}
}
```

## 访问结构体的字段

使用`.`操作符可以**访问结构体的字段**，也可以**给字段赋值**

```go
type Lesson struct {
	name, target    string
	spend             int
}

func main() {
	var lesson6 = Lesson{
		name:   "PHP学习",
		target: "全面掌握PHP",
		spend:  50,
	}
	fmt.Println(lesson6.name)   // PHP学习
	fmt.Println(lesson6.target) // 全面掌握PHP
	fmt.Println(lesson6.spend)  // 50
}
```

## 指向结构体的指针

`lesson7`是一个指向结构体`Lesson`的指针，用`(*lesson7).name`访问`lesson7`的`name`字段，`lesson7.name`代替`(*lesson7).name`的解引用访问

```go
type Lesson struct {
	name, target    string
	spend             int
}

func main() {
    lesson7 := &Lesson{
		name:   "PHP学习",
		target: "全面掌握PHP",
		spend:  50,
	}

	fmt.Println(lesson7.name)    // PHP学习
	fmt.Println((*lesson7).name) // PHP学习
}
```

## 匿名字段

下面的结构体定义了两个匿名字段，匿名字段的默认名称就是它的类型，所以这个结构体`Lesson2`有两个字段，分别为`string`和`int`

```go
type Lesson2 struct {
   string
   int
}

func main() {
    lesson8 := Lesson2{
        "Java",
        80,
	}
	fmt.Println(lesson8)        // {Java 80}
	fmt.Println(lesson8.string) // Java
	fmt.Println(lesson8.int)    // 80
}
```

## 结构体嵌套

```go
type Author struct {
   name string
   age  int
}

type Book struct {
   name   string
   price  float32
   author Author
}

func main() {
    book1 := Book{
    name:  "骆驼祥子",
    price: 29,
	author: Author{
			name: "老舍",
			age:  99,
		},
	}
	fmt.Println(book1)  // {骆驼祥子 29 {老舍 99}}
}
```

## 字段提升

结构体中如果有匿名的结构体类型字段，则该结构体里的字段称为**提升字段（Promoted Fields）**。提升字段可以被外部结构体直接访问，如下`book2.age`，字段相同时需要使用内部结构体，如下`book2.Author.name`

```go
type Author struct {
   name string
   age  int
}

type Book2 struct {
   name   string
   price  float32
   Author
}

func main() {
    book2 := Book2{
        name:  "骆驼祥子",
        price: 29,
	}
	book2.Author = Author{
		name: "老舍",
		age:  99,
	}
	fmt.Println(book2)             // {骆驼祥子 29 {老舍 99}}
	fmt.Println(book2.name)        // 骆驼祥子
	fmt.Println(book2.price)       // 29
	fmt.Println(book2.Author.name) // 老舍
	fmt.Println(book2.Author.age)  // 99
	fmt.Println(book2.age)         // 99
}
```

## 比较结构体

```go
func main() {
    lesson01 := Lesson{name: "Java", target: "Good", spend: 30}
    lesson02 := Lesson{name: "Java", target: "Good", spend: 30}
    fmt.Println(lesson01 == lesson02) // true
    fmt.Println(lesson01.name == lesson02.name &&
                lesson01.target == lesson02.target &&
                lesson01.spend == lesson02.spend) // true    
}
```

## 结构体可以包含方法

```go
type Lesson struct {
   name   string // 课程名称
   target string // 学习目标
   spend  int    // 学习时间
}
/*
   func (接受者 结构体类型) 方法名(参数列表) 返回值列表{
      逻辑执行体
   }
*/
func (l Lesson) showLessonInfo() {
   fmt.Printf("name:%s\n", l.name)
   fmt.Printf("target:%s\n", l.target)
   fmt.Printf("spend:%d\n", l.spend)
}

func main() {
    lesson01 := Lesson{name: "Java", target: "Good", spend: 30}
    lesson01.showLessonInfo()  // name:Java
                               // target:Good
                               // spend:30
}
```

## 方法的参数传递

当绑定结构体的方法中需要改变实例的属性的时候，必须使用结构体指针作为方法的接收者

```go
type Lesson struct {
	name   string // 课程名称
	target string // 学习目标
	spend  int    // 学习时间
}

func (l Lesson) addTimeByVal(time int) {
	l.spend += time
}

func (l *Lesson) addTimeByRef(time int) {
	l.spend += time
}

func main() {
    lesson9 := Lesson{name: "Java", target: "Good", spend: 30}
    fmt.Println(lesson9)  // {Java Good 30}
    lesson9.addTimeByVal(10)
    fmt.Println(lesson9)  // {Java Good 30}
    lesson9.addTimeByRef(10)
    fmt.Println(lesson9)  // {Java Good 40}    
}
```

## Tag标签

### Tag的使用

在字段上增加一个属性，这个属性是用反引号括起来的一个字符串，我们称之为 **Tag(标签)** 。例如：

```go
type Book struct {
   Author string `json:"author"`
   Name   string `json:"name"`
   Price  int    `json:"price,omitempty"`
}
```

结构体的 `Tag` 可以是任意的字符串面值，但是通常是一系列用空格分隔的 `key:"value"` 键值对序列；因为值中含有双引号字符，因此成员 `Tag` 一般用原生字符串面值的形式书写。一般我们常用在 `JSON` 的数据处理方面。

`json` 开头键名对应的值用于控制 `encoding/json` 包的编码和解码的行为，并且 `encoding/…` 下面其它的包也遵循这个约定。`Tag` 中 `json` 对应值的第一部分用于指定 `JSON` 对象的名字，比如将 Go 语言中的 `TotalCount` 成员对应到 `JSON` 中的 `total_count` 对象。

上面的例子中 `gender` 字段的 `Tag` 还带了一个额外的 `omitempty` 选项，表示当 Go 语言结构体成员为空或零值时不生成该 `JSON` 对象（这里 `false` 为零值）。例如：

```go
type Book struct {
   Author string `json:"author"`
   Name   string `json:"name"`
   Price  int    `json:"price,omitempty"`
}

func main() {
   book1 := Book{
      Author: "Lucia",
      Name:   "Java从入门到放弃",
      Price:  40,
   }
   bookJson1, err1 := json.Marshal(book1)
   if err1 != nil {
      panic(err1)
   }
   fmt.Printf("%s\n", bookJson1)  // {"author":"Lucia","name":"Java从入门到放弃","price":40}

   book2 := Book{
      Author: "Jack",
      Name:   "PHP入土为安",
   }
   bookJson2, err2 := json.Marshal(book2)
   if err2 != nil {
      panic(err2)
   }
   fmt.Printf("%s\n", bookJson2)  // {"author":"Jack","name":"PHP入土为安"}
}
```

### Tag的获取

使用反射的方法获取 Tag 步骤如下：

1. 获取字段
2. 获取 Tag
3. 获取键值对

其中获取字段有三种方式，而获取键值对有两种方式。

```go
// 三种获取 field
field := reflect.TypeOf(obj).FieldByName("Name")
field := reflect.ValueOf(obj).Type().Field(i)  // i 表示第几个字段
field := reflect.ValueOf(&obj).Elem().Type().Field(i)  // i 表示第几个字段

// 获取 Tag
tag := field.Tag

// 获取键值对
labelValue := tag.Get("label")
labelValue,ok := tag.Lookup("label")
// Get 当没有获取到对应 Tag 的内容，会返回空字符串
```

例如

```go
func main() {
    p := reflect.TypeOf(book1)
    name, _ := p.FieldByName("Name")
    tag := name.Tag
    fmt.Println("Name tag : ", tag)   // Name tag :  json:"name"
    keyValue, _ := tag.Lookup("json")
    fmt.Println("key:json,value:", keyValue)  // key:json, value: name
}
```

