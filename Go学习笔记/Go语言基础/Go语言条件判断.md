# Go语言条件判断

## IF条件判断

```go
package main
import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 10
 
   /* 使用 if 语句判断布尔表达式 */
   if a < 20 {
       /* 如果条件为 true 则执行以下语句 */
       fmt.Printf("a 小于 20\n" )
   }
   fmt.Printf("a 的值为 : %d\n", a)
}
```

## IF -ElSE

```go
package main
import "fmt"
//poke v捅，戳，探出
func main() {
	var flog bool =  true
	if flog {
		fmt.Println("ture")
	}else {
		fmt.Println("false")
	}
}
```

## Swith-Case

```go
package main
import "fmt"
//pendulum  -- n 钟摆
func main() {
	var grade string  = "A"
	var sorce int32 =90

	// 判断分数对应的等级 int32 直接比较
	switch sorce {
	case 90 : grade = "A"
	case 80 : grade = "B"
	case 70 : grade = "C"
	case 60 : grade = "D"
	default : grade = "E"
}
	// string 需要用于string比较
	switch  {
	case grade == "A":
		fmt.Printf("A:成绩好\n")
	case grade == "B":
		fmt.Printf("B:成绩较好\n")
	case grade == "C":
		fmt.Printf("C:成绩良")
	case grade == "D":
		fmt.Printf("D：成绩一般")
	case grade == "E":
		fmt.Printf("E:成绩不及格")	
	}
}
```

