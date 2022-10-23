# GO语言数组

## 数组的定义

### 方式一：

```go
var variable_name [SIZE] variable_type{} //定长
variable_name:= [...] variable_type{}  //可变字长
```

```go
package main
import "fmt"
func main() {
	var numberClub [5] int;
	numberClub[0]=1;
	numberClub[3]=10086;
	for i:=0;i<5;i++{
		fmt.Println(numberClub[i])
	}
}
```

### 方式二：

```go
ints :=[...] int {1,2,3,4,5}  //定义一个可变字长的数组

intstrings :=[...] string {"ad","cd","ef","gh"}  //定义一个可变字长的数组
```

```java
// 定义一个数组 2
numberClub02 := []int{1,2,3,4,5,6} ;
var socre int = 99
    for i:=5;i>0;i--{
        fmt.Printf("%d ", i)
            numberClub02[i]=socre
            socre--
    }
fmt.Println("")
    for i := 0; i <= 5; i++ {
        fmt.Println(numberClub02[i])
    }
```




