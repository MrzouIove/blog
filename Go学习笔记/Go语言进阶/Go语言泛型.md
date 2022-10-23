# Go语言的泛型

```go
func AddFloat(a, b float64) float64

func Add[T any](a, b T) T
```

## comparable 包

> Go 内置提供了一个 comparable 约束，表示可比较的。参考下面代码：		

```go
/* 遍历的泛型函数 */
func ForEach[T comparable] (Array [] T)(){
	for key, value := range Array {
		fmt.Printf("第%d个值:%v\n", key,value)
	}
}
```

```go
func Max[T any](input []T "T any") (max T) { 
    for _, v := range input { 
        if v > max { 
            max = v 
        } 
    } 
    return 
}
```

