# Go 语言切片(Slice)

> Go 语言切片是对数组的抽象。
>
> Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go 中提供了一种灵活，功能强悍的内置类型切片("动态数组")，与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。

## 例子：

```go
package main
import "fmt"
func main() {
	var data DataTest;
	data.Slice =  [] int64 {1,2,3,11,5,6}
	number := len(data.Slice)  //len函数返回的int类型
	fmt.Printf("number:%v", number)
	fmt.Printf("data:%v", data.Slice)
	
}
type DataTest struct {
	 Slice [] int64  //定义一个切片
	 Id int64
} 
```

## 切片

### 切片初始化

```go
s :=[] int {1,2,3 } 
```

> 直接初始化切片，**[ ]** 表示是切片类型，**{1,2,3}** 初始化值依次是 **1,2,3**，其 **cap=len=3**。

```go
s := arr[:] 
```

> 初始化切片 **s**，是数组 arr 的引用。

```go
s := arr[startIndex:endIndex] 
```

> 将 arr 中从下标 startIndex 到 endIndex-1 下的元素创建为一个新的切片。

```go
s := arr[startIndex:] 
```

> 默认 endIndex 时将表示一直到arr的最后一个元素。

```go
s := arr[:endIndex] 
```

> 默认 startIndex 时将表示从 arr 的第一个元素开始。

```go
s1 := s[startIndex:endIndex] 
```

### 通过切片 s 初始化切片 s1。

```go
s :=make([]int,len,cap) 
```

```go
package main
import "fmt"
func main() {
	var data DataTest;
	data.Slice =  [] int64 {1,2,3,11,5,6}
	number := len(data.Slice)  //len函数返回的int类型
	s01 := data.Slice[0:]
	s02 := data.Slice[0:2]
	s03 := data.Slice[2:4]
	fmt.Printf("number:%v\n", number)
	fmt.Printf("data:%v\n", data.Slice)
	fmt.Printf("s01:%v\n", s01)
	fmt.Printf("s02:%v\n", s02)
	fmt.Printf("s03:%v\n", s03)
}
type DataTest struct {
	 Slice [] int64  //定义一个切片
	 Id int64
} 
```

