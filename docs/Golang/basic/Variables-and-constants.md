## 变量

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

## 常量

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

## 关键字

> Go语言中有25个关键字

|    break     |     default     |    func    |  interface  |   select   |
| :----------: | :-------------: | :--------: | :---------: | :--------: |
|   **case**   |    **defer**    |   **go**   |   **map**   | **struct** |
|   **chan**   |    **else**     |  **goto**  | **package** | **switch** |
|  **const**   | **fallthrough** |   **if**   |  **range**  |  **type**  |
| **continue** |     **for**     | **import** | **return**  |  **var**   |