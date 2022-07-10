> 在静态类型语言（C++/Java/Golang等）中规定在创建一个变量或者常量时，必须要指定出相应的数据类型，否则无法给变量分配内存

## 整型

* 有符号整型：`int8`、`int16`、`int32`、`int64`、`int`
* 无符号整型：`uint8`、`uint16`、`uint32`、`uint64`、`uint`

![11011651636282_.pic](https://cdn.jsdelivr.net/gh/zorroe/img-host@main/img/2022-07-10-16-00-28-bfebcdb12db8025069f86bb786e6ebf8-e6c9d24ely1h1w8do4sznj20td0cugn5-6df282.jpg)

> * 除非对整型的大小有特定的需求，否则你通常应该使用`int`表示整型宽度，在32位系统下是32位，在64位系统下是64位
> * 对于 `int8` ， `int16` 等这些类型后面有一个数值的类型来说，它们能表示的数值个数是固定的。所以，在有的时候：例如在二进制传输、读写文件的结构描述(为了保持文件的结构不会受到不同编译目标平台字节长度的影响)等情况下，使用更加精确的 `int32` 和 `int64` 是更好的。

## 浮点型

> 浮点型表示存储的数据是实数，如3.145。

|  类型   | 字节数 |     说明      |
| :-----: | :----: | :-----------: |
| float32 |   4    | 32 位的浮点型 |
| float64 |   8    | 64 位的浮点型 |

> 浮点数能表示的数值很大，但是会有精度上的损失
>
> * `float32`大约能表示小数点后6位
> * `float64`大约能表示小数点后15位

## 字符

> 字符串中的每一个元素叫做**字符**，定义字符使用**<u>单引号</u>**

| 类 型 | 字 节 数 |                            说 明                             |
| :---: | :------: | :----------------------------------------------------------: |
| byte  |    1     | 表示`UTF-8`字符串的单个字节的值，表示的是`ASCII`码表中的一个字符,`uint8`的别名类型 |
| rune  |    4     |           表示单个`unicode`字符，`int32`的别名类型           |

* byte类型只能表示$2^8$个值，若响表示其他值，比如中文，就需要使用`rune`类型

## 字符串

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

## 布尔类型

> `true`或者`false`
>
> 在Go中，true不等于1，false不等于0

## 复数类型

>复数型用于表示数学中的复数，如 1+2j、1-2j、-1-2j 等。在 Go 语言中提供了两种精度的复数类型：`complex64` 和`complex128`，分别对应`float32`和`float64`两种浮点数精度

|    类 型     | 字 节 数 |                        说 明                        |
| :----------: | :------: | :-------------------------------------------------: |
| `complex64`  |    8     | 64 位的复数型，由`float32`类型的实部和虚部联合表示  |
| `complex128` |    16    | 128 位的复数型，由`float64`类型的实部和虚部联合表示 |

## `fmt`格式输出

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