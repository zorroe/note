所谓流程控制就是指“程序怎么执行”或者说“程序执行的顺序”。程序整体上确实是从上往下执行，但又不单纯是从上往下。
流程控制可分为三类：

1. 顺序执行。这个非常简单，就是先执行第一行再执行第二行……这样依次从上往下执行。
2. 选择执行。有些代码可以跳过不执行，有选择地执行某些代码。
3. 循环执行。有些代码会反复执行。

## if

```go
if 条件1 {
  逻辑代码1
} else if  条件2 {
  逻辑代码2
} else if 条件 ... {
  逻辑代码 ...
} else {
  逻辑代码 else
}
```

* `if`语句还有另一种写法，包含一个`statement`可选语句部分。该语句在条件判断之前运行

  ```go
  if statement; condition {
  
  }
  ```

  例如

  ```go
  if score := 88; score >= 60 {
      fmt.Println("成绩及格")
  }
  ```

  

## switch

```go
switch 表达式 {
    case 表达式值1:
        业务逻辑代码1
    case 表达式值2:
        业务逻辑代码2
    case 表达式值3:
        业务逻辑代码3
    case 表达式值 ...:
        业务逻辑代码 ...
    default:
        业务逻辑代码
}
```

在执行完`case`后，会退出循环，若都不符合，会执行`default`内语句。

* 一个case多个条件

  ```go
  month := 5
  switch month {
  case 1, 3, 5, 7, 8, 10, 12:
      fmt.Println("该月份有 31 天")
  case 4, 6, 9, 11:
      fmt.Println("该月份有 30 天")
  case 2:
      fmt.Println("该月份闰年为 29 天，非闰年为 28 天")
  default:
      fmt.Println("输入有误！")
  }
  ```

* `switch`语句还有另一种写法，包含一个`statement`可选语句部分。该语句在条件判断之前运行

  ```go
  switch statement; expression {
  
  }
  ```

  更改上面的代码

  ```go
  switch month := 5; month {
  case 1, 3, 5, 7, 8, 10, 12:
      fmt.Println("该月份有 31 天")
  case 4, 6, 9, 11:
      fmt.Println("该月份有 30 天")
  case 2:
      fmt.Println("该月份闰年为 29 天，非闰年为 28 天")
  default:
      fmt.Println("输入有误！")
  }
  ```

* `switch`后面可以接函数，只需要保证函数的返回值与case内值类型保持一致

  ```go
  switch func(month) {
  case 1, 3, 5, 7, 8, 10, 12:
      fmt.Println("该月份有 31 天")
  case 4, 6, 9, 11:
      fmt.Println("该月份有 30 天")
  case 2:
      fmt.Println("该月份闰年为 29 天，非闰年为 28 天")
  default:
      fmt.Println("输入有误！")
  }
  ```

* `switch`后面的表达式可以省略

  ```go
  score := 80
  switch {
  case score > 0 && score < 60:
      fmt.Println("不及格")
      fallthrough
  case score >= 60 && score < 75:
      fmt.Println("及格")
  case score >= 75 && score < 85:
      fmt.Println("良好")
  case score >= 85 && score <= 100:
      fmt.PrintLn("优秀")
  default:
      fmt.Println("输入有误！")
  }
  ```

* `fallthrough`语句：别的语言在`case`中需要使用`break`跳出，Go语言中在满足一个`case`之后会自动跳出，相反，Go中可以在Case块中使用`fallthrough`，将控制权转移到下一个`case`块。`fallthrough`必须是`case`块的最后一个语句。

  ```go
  score := 80
  switch {
  case score > 0 && score < 60:
      fmt.Println("不及格")
      fallthrough
  case score >= 60 && score < 75:
      fmt.Println("及格")
  case score >= 75 && score < 85:
      fmt.Println("良好")
  }
  ```

## for

```go
// for 接三个表达式
for initialisation; condition; post {
   code
}

// for 接一个条件表达式
for condition {
   code
}

// for 接一个 range 表达式
for range_expression {
   code
}

// for 不接表达式
for {
   code
}
```

例如

```go
// 接一个表达式
num := 0
for num < 5 {
   fmt.Println(num)
   num += 1
}

// 接三个表达式
for num := 0; num < 5; num++ {
   fmt.Println(num)
}

// 使用range
nums := []int{1, 2, 3, 4, 5}
for idx, val := range nums {
   fmt.Printf("%d:%d\n", idx, val)
}

// 不接表达式，无限循环，可使用break跳出
for {
   fmt.Println("Jack")
   break
}

// continue跳出当前循环
for num := 1; num <= 10; num++ {
   if num % 2 == 0 {
      continue
   }
   fmt.Println(num)
}
```

## defer

含有`defer`语句的函数，会在函数返回前，调用另一个函数。就是`defer`语句后面跟着的函数会延迟到当前函数执行完再执行

```go
func PrintHello() {
   fmt.Println("Hello")
}
func PrintBye() {
   fmt.Println("Bye")
}

func main() {
   defer PrintBye()
   defer PrintHello()
   fmt.Println("Main Hello")
}

// 执行结果
// Main Hello
// Hello
// Bye  
```