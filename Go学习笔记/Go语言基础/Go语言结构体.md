# Go语言结构体

## Go语言结构体的基本语法

```go
type Book struct {
	BOOK_NAME string
	ANTHOR string
	SUBJECT string
	BOOK_ID int64
}
```

## 结构体作为参数

```go
func PrintBooK(BookEntity  Book ) (){

  fmt.Printf("%v\n", BookEntity.BOOK_NAME)

  fmt.Printf("%v\n", BookEntity.ANTHOR)

  fmt.Printf("%v\n", BookEntity.SUBJECT)

  fmt.Printf("%v\n", BookEntity.BOOK_ID)

}
```

## 结构体的使用

```go
package main
import "fmt"
func main() {
	var person Person //定义一个person的变量

	person.name = "张三"
	person.age = 18
	person.height = 173
	person.weight = 53
	person.hobby = "学习"
	fmt.Printf("person.name=%v\n", person.name)
	fmt.Printf("person.age=%v\n", person.age)
	fmt.Printf("person.height=%v\n", person.height)
	fmt.Printf("person.weight=%v\n", person.weight)
	fmt.Printf("person.hobby=%v\n", person.hobby)

}
type Person struct {

	name string  //姓名

	age int64    //年龄
	
	height int64 //身高

	weight int64 //体重

	hobby string //爱好

}
```

## 结构体方法

```go
//type关键字用于定义类型；Student结构体拥有两个属性/字段
type Student struct {
    Name  string
    Score int
}
```

```go
结构体方法，方法中可以使用结构体变量；
func (s Student) Study() {
    s.Score += 10
}
```

## 使用结构体创造类

```go
package main
import "fmt"
// 创建一个学生类
type Student struct {
	id uint
	name string
	age int
	socre uint
}
//学生类的构造函数
func NewStudent(sid uint,s_name string,s_age int,s_score uint) *Student {
	return &Student{id:sid,name:s_name,age:s_age,socre:s_score}
}
//学生类的成员函数Set需要传入指针
func (s *Student) SetStudentName(name string){
	s.name = name
}
//Get不需要传入指针
func (s Student) GetStudentName() string{
	return s.name
}
func main() {
	var stu * Student
	stu = NewStudent(1,"张三",18,18)
	fmt.Printf("stu:%v\n", *stu)
	stu.SetStudentName("李四")
	fmt.Printf("stu:%v age:%v\n", *stu,stu.GetStudentName())

}
```

