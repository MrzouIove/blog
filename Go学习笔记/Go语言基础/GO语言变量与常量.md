# GO语言变量与常量

## 变量的类型

1. **布尔型**

   > 布尔型的值只可以是常量 true 或者 false。一个简单的例子：var b bool = true。

2. **数字类型**

   | **1** | **uint8 无符号 8 位整型 (0 到 255)**                         |
   | :---: | ------------------------------------------------------------ |
   | **2** | **uint16 无符号 16 位整型 (0 到 65535)**                     |
   | **3** | **uint32 无符号 32 位整型 (0 到 4294967295)**                |
   | **4** | **uint64 无符号 64 位整型 (0 到 18446744073709551615)**      |
   | **5** | **int8 有符号 8 位整型 (-128 到 127)**                       |
   | **6** | **int16 有符号 16 位整型 (-32768 到 32767)**                 |
   | **7** | **int32 有符号 32 位整型 (-2147483648 到 2147483647)**       |
   | **8** | **int64 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807)** |

   ### 浮点型

   | 序号 | 类型和描述                        |
   | :--- | :-------------------------------- |
   | 1    | **float32** IEEE-754 32位浮点型数 |
   | 2    | **float64** IEEE-754 64位浮点型数 |
   | 3    | **complex64** 32 位实数和虚数     |
   | 4    | **complex128** 64 位实数和虚数    |

   ------

   ## 其他数字类型

   以下列出了其他更多的数字类型：

   | 序号 | 类型和描述                               |
   | :--- | :--------------------------------------- |
   | 1    | **byte** 类似 uint8                      |
   | 2    | **rune** 类似 int32                      |
   | 3    | **uint** 32 或 64 位                     |
   | 4    | **int** 与 uint 一样大小                 |
   | 5    | **uintptr** 无符号整型，用于存放一个指针 |

3. **字符串类型**

   **字符串类型:**
   字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。Go 语言的字符串的字节使用 UTF-8 编码标识 Unicode 文本

4. **派生类型**

   **派生类型:**
   包括：

   - (a) 指针类型（Pointer）
   - (b) 数组类型
   - (c) 结构化类型(struct)
   - (d) Channel 类型
   - (e) 函数类型
   - (f) 切片类型
   - (g) 接口类型（interface）
   - (h) Map 类型
   
   ```go
   func main() {
   	/**
   		go 1.9版本对于数字类型，无需定义int及float32、float64 系统会自动识别。
   	*/
   	const Legth  = 100 
   	const width  =0.500
   	const aren float64 = Legth * width;
   }
   ```

## Go 语言的占位符

```go
func main() {
	/**
		go 1.9版本对于数字类型，无需定义int及float32、float64，系统会自动识别。
	*/
	const Legth  = 100 
	const width  =0.500
	const aren float64 = Legth * width;

	fmt.Printf("%T\n",Legth )	
	fmt.Printf("%T\n",width )
	fmt.Printf("%T\n",aren )  // # 打印类型
	fmt.Printf("%d\n", aren)  // # 十进制值的表示
	fmt.Printf("%q\n",aren )  // # 字符字面值
	fmt.Printf("%b\n",aren )  // # 二进制数值表示
	fmt.Printf("%v\n",aren )  // # 将值表示出来
	fmt.Printf("%e\n",aren )  // # 将值科学计数法表示出来
	fmt.Printf("%o\n",aren )  // # 将值八进制表示出来
	fmt.Printf("%x\n",aren )  // # 将值十六进制表示出来（小写）
	fmt.Printf("%X\n",aren )  // # 将值十六表示出来 （大写）
	fmt.Printf("%f\n",aren )  // # 将值小数表示出来
}

```

## Go语言中的空

> 在Go语言中，布尔类型的零值（初始值）为 false，数值类型的零值为 0，字符串类型的零值为空字符串 "" ，而指针、切片、映射、通道、函数和接口的零值则是 **nil**

 ## 变量的命名规则 var

>Go 语言变量名由字母、数字、下划线组成，其中首个字符不能为数字。
>
>声明变量的一般形式是使用 var 关键字

```go
var identifier type
```

```go
package main
import "fmt"
func main() {
    var a string = "Runoob"
    fmt.Println(a)

    var b, c int8 = 1, 2
    fmt.Println(b, c)
}
```

## 常量 const

 ### 常量的命名规则

> 常量是一个简单值的标识符，在程序运行时，不会被修改的量。
>
> 常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。

```go
func main() {
	// 单个常量的创建和赋值
	const name string = "李四"
	// 多个常量的命名
	const (
	Legth  = 100 
	width  =0.500
	aren float64 = Legth * width
	)
	// 多重常量的创建和多赋值
	const a,b,c = 10,0.5,"zhangsan"

	fmt.Printf("%T\n",a )	
	fmt.Printf("%T\n",b )
	fmt.Printf("%T\n",c )  // # 打印类型
	fmt.Printf("%T\n", name)
	fmt.Printf("%v\n", name)
	fmt.Printf("%q\n", name)

}
```

##  **iota**

> iota，特殊常量，可以认为是一个可以被编译器修改的常量。
>
> iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每新增一行常量声明将使 iota 计数一次(iota 可理解为 const 语句块中的行索引)。
>
> iota 可以被用作枚举值： 

```go
package main
import "fmt"

func main() {
	const(
		a = iota
		b = iota
		c = iota
		d = 2000
		e = iota
		f = 3000
	)
	fmt.Printf("%v\n", a)
	fmt.Printf("%v\n", b)
	fmt.Printf("%v\n", c)
	fmt.Printf("%v\n", d)
	fmt.Printf("%v\n", e)
	fmt.Printf("%v\n", f)
}
```

