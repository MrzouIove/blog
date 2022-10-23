# ES6的学习

## 变量的定义

>  变量
>
> 变量：一小块存储数据的内存空间
>
> 1. Java语言是强类型语言，而JavaScript是弱类型语言。
> 2. 强类型：在开辟变量存储空间时，定义了空间将来存储的数据的数据类型。只能存储固定类型的数据
> 3. 弱类型：在开辟变量存储空间时，不定义空间将来的存储数据类型，可以存放任意类型的数据。
>
> 语法：
>
> ```js
> var 变量名 = 初始化值;
> 
> let 变量名 = 初始化值;
> 
> ```
>
> 

### let与var定义变量的作用范围

```js
<script>
    
    // es6 定义变量
    {
         var a = 1
         let b = 2 
    }
    console.log(a)
    console.log(b)

</script>
```

![image-20221009232744542](E:\Typora\data\img\image-20221009232744542.png)

### let不能多次定义相同变量

```js
let a = 3
let b = 4  //出错

var a = 4
var b = 6  //可以

```

### const 常量定义后无法改变

```js
const a  = 10
a =8   //出错
```

### const常量定义需要初始化

```js
const a 
```

### typeof

> typeof运算符：获取变量的类型。
>
> 注：null运算后得到的是object

```js
typeof "John"                // 返回 string
typeof 3.14                  // 返回 number
typeof false                 // 返回 boolean
typeof [1,2,3,4]             // 返回 object
typeof {name:'John', age:34} // 返回 object
```

## 对象

> 对象也是一个变量，但对象可以包含多个值（多个变量），每个值以 **name:value** 对呈现。

定义 JavaScript 对象可以跨越多行，空格跟换行不是必须的：

### 实例

```js
var person = {
  firstName:"John",
  lastName:"Doe",
  age:50,
  eyeColor:"blue"
};
```



```js
var car = {name:"Fiat",
           model:500, 
           color:"white"};

let person = {name:"张三",
           	 age:10,
             class:"grade1 class1"};
```

### es6获取对象值

```js
//传统
let name = user.name
let age =user.age

//es6
let name ="zhangsan"
let ae = 10
const p2 = {name,age}
let {name,age}=user
console.log(name+"***"+age)
```

## 模板字符串（es6）

###  模板字符串的例子

![image-20221009234855050](E:\Typora\data\img\image-20221009234855050.png)

### 输出模板


> 
>
> 传统的 JavaScript 语言，输出模板通常是这样写的（下面使用了 jQuery 的方法）。

```js
$('#result').append(  'There are <b>' + basket.count + '</b> '
                    +  'items in your basket, ' +  
                    '<em>' + basket.onSale + 
                    '</em> are on sale!');
```

> 上面这种写法相当繁琐不方便，ES6 引入了模板字符串解决这个问题。

```js
$('#result').append(`  There are <b>${basket.count}</b> items   in your basket, <em>${basket.onSale}</em>  are on sale!`);
```

> `模板字符串`（template string）是增强版的字符串，用反引号 ' ' 标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

```js
// 普通字符串
`In JavaScript '\n' is a line-feed.`
// 多行字符串
`In JavaScript this is not legal.`console.log(`string text line 1string text line 2`);
// 字符串中嵌入变量
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`  
```

```js
<script>
    // 1.使用模板字符串
    // 模板字符串保留其中的空格和换行
    let str1= `hello 模板字符串
        world
        test
    `
    console.log(str1)

    //2. 在模板字符串中获取変量
    let name = "张三"
    let age = 18
    let str2= `hello ${name} ,你的年龄是${age+1}吗？`
    console.log(str2)
   
	// 3. 在模板字符串中调用方法
	function f1(){
        return "hello world "
    }
	let str3 = `demo,${f1()}`
    console.log(str3)
    
</script>
```

![image-20221009235115447](E:\Typora\data\img\image-20221009235115447.png)

## 箭头函数

```js
<script>
//传统函数
var f1 = function(a){
    return a
}
console.log(f1(5))

// 箭头函数
var fi = m => m;
console.log(fi(8))

</script>
```

