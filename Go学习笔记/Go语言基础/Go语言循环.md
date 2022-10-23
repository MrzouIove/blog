# Go语言循环

## for循环

### 方式一

```go
for init; condition; post { }
```

```go
package main
import "fmt"
func main() {
	for i := 0; i < 15; i++ {
		fmt.Printf("i = %d ", i)
		if i>10{
			break  //循环结束语句
		}
	}
}
```

### 方式二：

```go
for key, value := range oldMap {
    newMap[key] = value
}
```

```go
package main
import "fmt"
func main() {

	//可变长度的字符组
	stringDemo :=[] string {"zhansan","lisi","WANG"}
	for key, value := range stringDemo {
		fmt.Printf("key:%v,value:%v\n", key,value)
	}
}
```

### 方式三：

> 无限循环

```go
package main
import "fmt"
func main() {
	var sum int = 0
	var i int = 1
	//无限循环
	for {
		if i == 101 {
			break
		}
		sum  = i + sum
		i++
	
	}
	fmt.Println("")
	fmt.Println(sum)
}
```

### 方式四：

> 带有条件的循环

```go
package main
import "fmt"
func main() {
	var i int = 10
	//无限循环
	for i<20 {
		if i == 19 {
			break
		}
		i++
	}
}
```

## Break跳出循环

```go
package main
import "fmt"
func main() {
	var i int = 10
	//无限循环
	for i<20 {
		if i == 19 {
			break
		}
		i++
	}
}
```

## continue跳出本次循环

```go
package main
import "fmt"
func main() {
	var i int = 10
	//无限循环
	for i<20 {
		if i == 19 {
			continue
		}
		i++
	}
}
```



## goto语句跳出循环

> goto语句的使用

```go
goto label;
..
.
label: statement;
```

```go
package main
import "fmt"
func main() {
	var a int = 10
	LOOP:for a<20 {
		if(a == 15){
			a++;
			goto LOOP //跳回循环点
		}
		a++
		fmt.Printf("a的值%d\n", a)
	}
}
```

