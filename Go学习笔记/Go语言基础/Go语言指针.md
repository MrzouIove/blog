# Go语言指针

> 基本数据类型：变量存的是值，也叫值类型
>
> Go 语言的取地址符是 &，放到一个变量前使用就会返回相应变量的内存地址。

## 取值符的操作(&)

```go
package main
import "fmt"
/* 指针的使用 */
func main() {
	var a int = 10
	fmt.Printf("a of address:%v\n", &a)
	fmt.Printf("a of address:%d\n", &a)
	fmt.Printf("a of address:%b\n", &a)
	fmt.Printf("a of address:%o\n", &a)
	fmt.Printf("a of address:%x\n", &a)
}
```

## 指针的基本用法

```go
package main
import "fmt"
func main() {
	var a int = 9
	var	point * int   //定义一个变量
	point = &a        //将指针指向a的地址变量
	fmt.Printf("point指向的地址：%v\n", point)
	fmt.Printf("point指向的地址的在值：%v\n", * point)
	fmt.Printf("a:的值：%d\n", a)

}
```

## Go 空指针

> 当一个指针被定义后没有分配到任何变量时，它的值为 nil。
>
> nil 指针也称为空指针。
>
> nil在概念上和其它语言的null、None、nil、NULL一样，都指代零值或空值。
>
> 一个指针变量通常缩写为 ptr。

```go
package main
import "fmt"
func main() {
	var pointNull * int;  //定义一个空指针
	fmt.Printf("pointNull的值:%v\n", pointNull)
	fmt.Printf("pointNull的值:%d\n", pointNull)

}
```

## Go指针数组

```go
package main
import "fmt" 

func main() {
	var arrayPoint = [] int{1,2,3,4,5,8,7}  //定义一个数组

	var StringArrays = [] string{"zhangsan","lisi","liuliu","qiqi"}  //定义一个字符数组

	var pointInt *[] int = &arrayPoint;  //将整形指针指向第一个数组

	var pointString *[] string = &StringArrays //将字符指针指向第一个字符数组

	fmt.Printf("第一个数组的首地址的值：%v\n", *pointInt)	
	fmt.Printf("第一个数组的首地址：%v\n", &pointInt)


	fmt.Printf("第一个字符的首地址的值：%v\n", *pointString)
	fmt.Printf("第一个字符的首地址：%v\n", &pointString)

	ForEach(*pointInt)
	ForEach(*pointString)


}

/* 遍历的泛型函数 */
func ForEach[T comparable] (Array[] T)(){
	for key, value := range Array {
		fmt.Printf("第%d个值:%v\n", key,value)
	}
}

```

## Go 语言指针作为函数参数

```go
package main
import "fmt"

/* 将指针作为函数的参数 */
func main() {
	a:=1
	b:=2
	fmt.Printf("a:%v,b:%v\n", a,b)
	Swith(&a,&b)
	fmt.Printf("a:%v,b:%v\n", a,b)
}
/** 引用传递的实现 **/
func Swith[T comparable ](a * T,b * T)()  {
	var temp T;
	temp = *a
	*a = *b
	*b = temp
}
```

![image-20221004171511035](E:\Typora\data\img\image-20221004171511035.png)

## Go语言中双重指针

> 定义一个双重指针

```go
package main
import "fmt"
/* 双重指针 */
func main() {
	var DoublePoint **int    //定义一个双重指针

	var SingerPoint *int

	var Value int = 500

	SingerPoint = &Value   //将value赋值给指针

	DoublePoint = &SingerPoint  //将单重指针指向双重指针

	fmt.Printf("Value的地址：%v\n", &Value)
	fmt.Printf("Value的值：%v\n", Value)
	fmt.Printf("************************************\n")
	fmt.Printf("SingerPoint的地址：%v\n", &SingerPoint)
	fmt.Printf("SingerPoint的地址：%v\n", SingerPoint)
	fmt.Printf("SingerPoint的值：%v\n", *SingerPoint)
	fmt.Printf("************************************\n")
	fmt.Printf("DoublePoint 的地址：%v\n", &DoublePoint )
	fmt.Printf("DoublePoint 的单重指针值：%v\n", *DoublePoint )
	fmt.Printf("DoublePoint 的值：%v\n", **DoublePoint )
}
```

