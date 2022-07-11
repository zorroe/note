**包**用于组织Go源代码，提供更好的可重用性与可读性。Go语言有超过100个标准包，可以用`go list std | wc -l`命令查看标准包的具体数目。

## main包

`main`包包含一个`main()`函数，`main`函数是程序运行的入口

一个包下的所有`go`文件的`package packagename`应是相同的，且只有一个`main`函数

## 创建包

属于某一个包的源文件都应该置于一个单独命名的文件夹里，按照`Go`的惯例，应该用包名命名文件夹名。

## 导入包

```go
package main

import (
	_ "fmt" // //使用匿名导入的方法，只执行fmt包的init函数，否则只导入包，不运行函数会报错
	"github.com/zorroe/go-study/book"
)

func main() {
	book.PrintInfo()
}
```

也可以单行导入

```go
import "fmt"
import "github.com/zorroe/go-study/book"
```

## 使用别名

如果导入具有相同包名的不同包时，可以使用别名

```go
import (
    "crypto/rand"
    mrand "math/rand" // 将名称替换为 mrand 避免冲突
)
```

## 包的初始化

每个包都允许有一个或多个 `init` 函数， `init` 函数不应该有任何返回值类型和参数，在代码中也不能显式调用它，当这个包被导入时，就会执行这个包的 `init` 函数，做初始化任务， `init` 函数优先于 `main` 函数执行。该函数形式如下：

```go
func init() {
    
}
```

包的初始化顺序：首先初始化 **包级别(Package Level)** 的变量，紧接着调用 `init` 函数。包可以有多个 `init` 函数(在一个文件或分布于多个文件中)，它们按照编译器解析它们的顺序进行调用。如果一个包导入了另一个包，会先初始化被导入的包。尽管一个包可能会被导入多次，但是它只会被初始化一次。