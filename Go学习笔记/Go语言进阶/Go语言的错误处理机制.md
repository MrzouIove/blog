# Go 错误处理

## 基本语法

### error的定义

> Go 语言通过内置的错误接口提供了非常简单的错误处理机制。
>
> error类型是一个接口类型，这是它的定义：

```go
type error interface {
    Error() string
}
```

> 我们可以在编码中通过实现 error 接口类型来生成错误信息。
>
> 函数通常在最后的返回值中返回错误信息。使用errors.New 可返回一个错误信息：

### 返回错误信息

```go
func Sqrt(f float64) (float64, error) {
    if f < 0 {
        return 0, errors.New("math: square root of negative number")
    }
    // 实现
}
```

## 例子

> 以下代码出错

```go
package main
import (
  "fmt"
)
func main() {
  num1:=10
  num2:=0
  res:=num1/num2
  fmt.Printf("res:%v\n", res)

}
```

![image-20221007160439630](E:\Typora\data\img\image-20221007160439630.png)

```go
package main
import (
	"fmt"
)
func main() {
	test()
}
func test(){
	//使用defer+recover处理异常
	defer func (){
		err:=recover() //recover 是一个内置函数
		if err !=nil{
			fmt.Printf("err=", err)
		}
	}()
	num1:=10
	num2:=0
	res:=num1/num2
	fmt.Printf("res:%v\n", res)
}
```



## Go语言中处理异常

### defer

> defer是golang提供的关键字，在函数或者方法执行完成，返回之前调用。
> 每次defer都会将defer函数压入栈中，调用函数或者方法结束时，从栈中取出执行，所以多个defer的执行顺序是先入后出。

### panic (抛出异常)







### recover（捕获异常）

> recover 是一个内置函数