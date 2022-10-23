

# Go 语言Map(集合)

Map 是一种无序的键值对的集合。Map 最重要的一点是通过 key 来快速检索数据，key 类似于索引，指向数据的值。

Map 是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，Map 是无序的，我们无法决定它的返回顺序，这是因为 Map 是使用 hash 表来实现的。

## 定义 Map

可以使用内建函数 make 也可以使用 map 关键字来定义 Map:

```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
```

如果不初始化 map，那么就会创建一个 nil map。nil map 不能用来存放键值对

## 实例

```go
package main
import "fmt"
func main() {
	/* 方式一：声明一个map对象 */
	var  HashMap map[string] string 
	HashMap =make(map[string]string)
	HashMap["姓名"] = "张三"
	HashMap["年龄"] = "18"
	HashMap["地址"] = "北京"
	HashMap["户籍"] = "北京东城"
	/* map是无序的 */
	for key, value := range HashMap {
		fmt.Printf("%v:%v ", key,value)
	}
	fmt.Println("")
}
```

## Map的切片

> *如果确定是真实的,则存在,否则不存在*

```go
vaule , ok := map["参数"]
```

```go
/* 方式一：声明一个map对象 */
	var  HashMaps map[string] string 
	HashMaps =make(map[string]string)
	HashMaps["姓名"] = "张三"
	HashMaps["年龄"] = "18"
	HashMaps["地址"] = "北京"
	HashMaps["户籍"] = "北京东城"
	printMap(HashMaps)
	value ,ok := HashMaps["爱好"]
	if value == ""{
		fmt.Printf("value为空\n")
	}
	fmt.Printf("value:%v %T %b\n", value,value,value)
	fmt.Printf("ok:%v\n", ok)
```

## delete() 函数

> delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key。实例如下：

```go
/**
* 删除map中的值
**/
func DeleteMap[T comparable](entity map[T]T,param T)(bool){
	value,ok := entity[param]
	if ok {
		delete(entity,param)
		fmt.Printf("%v已经被删除\n",value)
		return true
	}
	fmt.Println("不存在参数")
	fmt.Printf("\n******************\n")
	return false
}
```

