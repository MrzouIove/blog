# Go语言中的函数

## 函数的用法

```go
func function_name( [parameter list] ) [return_types] {
   函数体
}
```

```go
package main
import "fmt"

func main() {
	var x int = sumTwoNumber(3,4)
	fmt.Printf("%d", x)
}
/**函数名  （参数，参数） 返回类型 **/
func sumTwoNumber(a, b int) (int)  {
	/* 定义局部变量 */
	var result int;
	result = a + b
	return result
}
```

## 无参数无返回值的函数

```go
//无参数无返回值的函数
func NoAll()()  {
	fmt.Printf("我是一个什么也没有的函数\n")
}
```

## 无参数有返回值的函数

```go
func NoParam() (string) {
	return "我是一个无参数但是有返回值的函数"
}
```

## 有参数有返回值的函数

```go
// 求和
func sumTwoNumber(a, b int) (int)  {
	/* 定义局部变量 */
	var result int;
	result = a + b
	return result
}
```

## 多返回值的函数

```go

/**多个返回值**/
func swapStrings(oldString01,oldString02 string) (string,string) {
	return oldString02 ,oldString01
}
```

## 返回值的命名

> 在go语言中可以对返回值的参数进行命名

```go
package main
import "fmt"
func main() {
	var a string
	var b bool
	a,b = Inc()
	fmt.Printf("%v:%v\n", a,b)
	
}
func Inc()(value string,ok bool){
	m :=make(map[string]string)
	m["address"]="地址"
	value,ok = m["address"]
	return
}
```



## Go函数的闭包

> Go 语言支持匿名函数，可作为闭包。匿名函数是一个"内联"语句或表达式。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。

```go
func main() {
	var param int =0
	FunctionA:=GetFunction()
	param =FunctionA()
	fmt.Printf("%d\n", param)
}

/**函数的闭包**/
func GetFunction() (func () (int) ) { //可以简写 func GetFunction() func () (int) {...}
	var i int = 0
	return func()(int){
		var result int = i
		result++
		return  result
	}
}
```

![image-20221003171608247](E:\Typora\data\img\image-20221003171608247.png)

## Go语言泛型函数

```go
func AddFloat(a, b float64) float64

func Add[T any](a, b T) T
```

### comparable 包

> Go 内置提供了一个 comparable 约束，表示可比较的。参考下面代码：

​		

```go
/* 遍历的泛型函数 */
func ForEach[T comparable] (Array[] T)(){
	for key, value := range Array {
		fmt.Printf("第%d个值:%v\n", key,value)
	}
}
```



## Go语言的标准输入输出

```go
package main
import (
	"fmt"
)
func main() {
	var str string
	fmt.Scanln(&str)
	fmt.Printf("INPUT :%s\n", str);
}
```