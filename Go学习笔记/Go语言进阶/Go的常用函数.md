# Go语言的常用函数

## 导包

```go
import "strconv"   //操作字符的包
import "strings"   //操作字符串的包
import "time"	   //导入时间相关的函数
```

## 字符串函数

### len()统计字符串长度

> golang编码统一是utf8编码  --汉字占三个字节

```
func len (string) (int){...}
```

```go
package main
import "fmt"
func main() {
	str:="hello背" //8  
	fmt.Println("str=",len(str))
}
```

### strconv.Atoi 将字符串转为整数

```go
// 将字符串转化为整数
func IntString(str string)(){
	value, err:= strconv.Atoi(str)
	if err!=nil{
		fmt.Printf("转换失败:%v\n", err)
	}else {
		fmt.Printf("转换成功的值:%v\n", value)
		fmt.Printf("转换成功的类型:%T\n", value)
	}
}
```

### 整数转字符串strconv.Itoa()

> strconv.Itoa()

```go
// 将整数转为字符串

func IntToString(number int)(){

  str := strconv.Itoa(number)

  fmt.Printf("%v  %t\n", str,str)

}
```



### r = []rune (str) 中文的变历

> 对含有中文的字符串遍历

```
str1 = []rune(str2)
```

```go
func ChineseEachChar(str string)(){
	r := []rune(str)
	fmt.Printf("\n*****************\n")
	for i:=1;i<len(r);i++{
		fmt.Printf("字符：%c\n", r[i])
	}
}
```

> 对不含有中文的字符串遍历可用

```go
	str1 :="hello"
	for i:=0;i<len(str1);i++{
		fmt.Printf("字符%c\n", str1[i])
	}
```

### 将字符串转为 []byte  

```go
var bytes = [] byte("hello")
```

### 将byte转为string

```go
 str := strings([]byte{1,2,3,4})
```

### 查询子串是否在指定的子串

```go
strings.Contains("seafood","food") // true
```

### 统计一个子串在另一个子串中存在多少个

```go
// 统计一个字符串中含有多少个子串
func CountNumber(str1 string ,str2 string)(int){
	return strings.Count(str1,str2)
}
```

### 字符串的比较（区分大小写与不区分大小写）

> == 区分大小写
>
> string.EqualFold(string,string) :不区分大小写

```go
b:=string.EqualFold(string01,string02) 
```

### 返回子串第一次出现的位置

```go
strings.index(str1,str2)

strings.index("as_dd","_dd")   //2
```

### 返回最后一次出现的位置

```go
strings.Lastindex(str1,str2)

strings.Lastindex("as_dd","_dd") 
```

### 將子串替換

```go
strings.Replace("go go hello","go","go語言",n) //n代表你想替換几个
```

### 将字符串进行切割

```go
strings.split("hello,world,ok",",")
```

### 字符串的首字母大小写

```go
string.ToLower("Go")  //小写
string.ToUpper("go")  //大写
```

## 日期时间函数

### 获取年月日时分秒

```go
import (
    "fmt"
    "time"
)
func main() {
    now := time.Now() //获取当前时间
    fmt.Printf("current time:%v\n", now)
    year := now.Year()     //年
    month := now.Month()   //月
    day := now.Day()       //日
    hour := now.Hour()     //小时
    minute := now.Minute() //分钟
    second := now.Second() //秒
    fmt.Printf("%d-%02d-%02d %02d:%02d:%02d\n", year, month, day, hour, minute, second)
}
```

### 获取时间戳

```go
package main
import (
    "fmt"
    "time"
)
func main() {
    now := time.Now()            //获取当前时间
    timestamp1 := now.Unix()     //时间戳
    timestamp2 := now.UnixNano() //纳秒时间戳
    fmt.Printf("现在的时间戳：%v\n", timestamp1)
    fmt.Printf("现在的纳秒时间戳：%v\n", timestamp2)
}
```

### 获取当前是星期几

> time 包中的 Weekday 函数能够返回某个时间点所对应是一周中的周几，示例代码如下

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    //时间戳
    t := time.Now()
    fmt.Println(t.Weekday().String())
}
```

### 时间格式化

> 时间类型有一个自带的 Format 方法进行格式化，需要注意的是Go语言中格式化时间模板不是常见的`Y-m-d H:M:S `而是使用Go语言的诞生时间 2006 年 1 月 2 号 15 点 04 分 05 秒。
>
> 提示：如果想将时间格式化为 12 小时格式，需指定 PM。

```go
package main
import (   
    "fmt"  
    "time"
)
func main() { 
    now := time.Now()  
    // 格式化的模板为Go的出生时间2006年1月2号15点04分 Mon Jan   
    // 24小时制   
    fmt.Println(now.Format("2006-01-02 15:04:05.000 Mon Jan"))    
    // 12小时制    
    fmt.Println(now.Format("2006-01-02 03:04:05.000 PM Mon Jan"))   
    fmt.Println(now.Format("2006/01/02 15:04"))  
    fmt.Println(now.Format("15:04 2006/01/02")) 
    fmt.Println(now.Format("2006/01/02"))
}
```