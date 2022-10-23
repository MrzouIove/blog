#  C语言第十天课程笔记

每一天的笔记包含如下内容:

1. 当天授课内容安排
2. 课堂重点内容笔记
3. 课后思考题



## 1. 内容安排

**第一节课:**   `作用域、变量分类(静态变量、非静态变量，静态函数、非静态函数)`
**第二节课:**   `内存分区(代码区、数据区-堆区、全局静态区、字符串常量区、栈区)`
**第三节课:**   `内存操作(malloc、free、memset、memcpy、memcmp、memmove)`
**第四节课:**   `结构体语法(结构体定义、结构体成员访问、结构体变量定义)`
**第五节课:**   `结构体使用注意、结构体作为函数参数`
**第六节课:**   `typedef、enum`



## 2. 作用域

作用域: 主要探讨标识符（函数名、变量名），这些名字在哪些范围内可以使用。变量名的可见范围.

函数只有文件作用域.

C作用域:  

 1. 文件作用域. 例如： 有些变量在 a.c 定义的，在 b.c 就不能访问.

 2. 函数作用域. 例如: a函数内定义了一个变量 `int number = 10`,  b函数中就不能使用 a 函数定义的 number 变量.

 3. 代码块作用域. 例如：

    ```c
    if(条件)
    {
      int a= 10;
    }
    
    if(条件)
    {
      printf("a = %d\n", a);
    }
    ```



## 3. 变量分类

1. 非静态变量: 全局变量 局部变量
   1. 非静态全局变量
      1. 作用域：整个项目中都可以访问
      2. 内存管理: main 函数执行之前被创建，main 函数执行结束，内存回收.
   2. 非静态的局部变量
      1. 作用域: 该变量只在函数内可见
      2. 内存管理: 局部变量内存函数调用时，分配内存，函数调用结束，回收内存.
2. 静态变量：静态全局变量，静态局部变量
   1. 静态全局变量
      1. 作用域: 文件作用域. 只能在当前 .c 文件内访问，在其他 .c 文件中不可访问，不可使用.
      2. 内存管理: main 函数执行之前创建，main函数执行结束之后回收.
   2. 静态局部变量
      1. 作用域: 静态局部变量也是一个局部变量，作用域是当前函数内部.
      2. 内存管理: main 函数执行之前创建，main函数执行结束之后回收.

**非静态变量:**

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

// 非静态变量: 全局变量 局部变量

// 非静态全局变量
// 作用域：整个项目中都可以访问
// 内存管理: main 函数执行之前被创建，main 函数执行结束，内存回收.
int g_number = 0;


void test01()
{
	// 非静态的局部变量
	// 作用域: 该变量只在函数内可见
	// 内存管理: 局部变量内存函数调用时，分配内存，函数调用结束，回收内存.
	int number = 0;
}
```



**静态变量:**

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

// 静态变量：静态全局变量，静态局部变量

// s_number 静态全局变量
// 作用域: 文件作用域. 只能在当前 .c 文件内访问，在其他 .c 文件中不可访问，不可使用.
// 内存管理: main 函数执行之前创建，main函数执行结束之后回收.
static int s_number = 100;


void test01()
{
	// 静态局部变量
	// 作用域: 静态局部变量也是一个局部变量，作用域是当前函数内部.
	// 内存管理: main 函数执行之前创建，main函数执行结束之后回收.
	static int a = 100;
}

// 静态局部变量不会被销毁
void test02()
{
	static int a = 10;
	++a;
	printf("a = %d\n", a);
}

// 1. 允许
// 2. 原因: a 变量是静态变量，整个程序运行期间一直存在.
int* get_number_pointer()
{
	static int a = 100;
	return &a;
}

// 全局变量、静态变量都是程序执行之前创建，程序执行之后销毁，整个程序运行期间，内存一直存在.

int main()
{
	test02();
	test02();
	test02();
	test02();


	int *p = get_number_pointer();


	system("pause");
	return EXIT_SUCCESS;
}
```



## 4 函数分类

1. 非静态函数

   ```c
   void func()
   {
     printf("hello world");
   }
   ```

   非静态函数也叫做全局函数，可以在整个项目任何的 .c 文件中访问.

2. 静态函数

   ```c
   static void func()
   {
     printf("hello world");
   }
   ```

   静态函数只能在当前文件内访问.

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

// 访问在其他 .c 文件中定义的函数, 有两步:
// 1. 先声明该函数， 告诉编译器这个函数在其他文件中定义.
// 2. 调用函数.

// 如果 func 函数在其他文件中定义为静态函数，则在当前文件中不能使用.
void func();  // 函数声明

int main()
{

	func();


	system("pause");
	return EXIT_SUCCESS;
}
```



## 5. 内存分区

编译器会将程序所使用的的内存。代码区、数据区。代码区放在只读区。

为什么代码放在只读区？为什么数据放在可读可写的区域呢？

代码一旦编写完毕，运行过程中不允许任意修改，为了保护代码，在设计的时候，代码就放在只读的内存区域。放代码的区域，叫做代码区。

**数据区:**

栈区：自动申请，自动释放。

1. 函数的参数
2. 函数内部定义局部变量
3. 可读、可写

全局/静态区:  在程序执行之前分配内存，程序结束之后回收内存。

1. 全局变量
2. 静态全局变量
3. 静态局部变量
4. 可读、可写

字符串常量区: 程序运行之前创建，程序运行之后销毁.

1. 双引号括起来的字符串会放在字符串常量区.
2. 只读，不能修改.

堆区: 手动管理内存申请、手动释放内存。

	1. 根据需要申请任意大小的内存。
 	2. 根据需要选择在合适的时间释放内存。



内存4区： 最主要目的是让我清楚，变量的生命周期。

栈区: 函数开始存储，函数结束释放。

全局静态区: 整个程序运行期间都会存在.

堆区: 手动申请，手动释放，如果申请了没有释放，会造成内存问题：内存泄露。



```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

// 1. 申请和释放堆内存，需要两个底层函数  malloc  free
void test01()
{
	// 栈变量
	int a = 10;
	// 全局静态变量
	static int b = 20;
	// 向操作系统要了4字节内存，函数返回这个4个字节内存的首地址
	// p 指向的内存 1. 手动释放 2. 程序结束之后，统一回收
	int* p =  (int *)malloc(4);
	*p = 30;  // 堆内存赋值

	printf("*p = %d\n", *p);

	// free 函数用于释放堆内存
	// 当 free 调用之后，内存就不能再使用了。
	// free(p);
}


// 2. 给double类型分配堆空间
void test02()
{
	// 在堆上分配了8字节内存，用于存储 double 类型数据
	double *d = (double *)malloc(sizeof(double));
	// 如何写内存
	*d = 3.14;
	// 如何读内存
	printf("*d = %lf\n", *d);
	// 释放内存
	free(d);
}

long* create_long()
{
	// 动态申请一块long大小的内存
	long *l = malloc(sizeof(long));

	// 返回
	return l;
}

void test03()
{
	// 问题: ll 能不能使用？ 程序有有什么问题？
	long *ll = NULL;
	ll = create_long();

	free(ll);
}

// 给 long 分配内存，并且赋值为 666, 打印输出

int main()
{

	test02();

	system("pause");
	return EXIT_SUCCESS;
}
```



## 内存操作

代码区：不关心.

全局区：项目中所有的文件共享。变量和函数。

​		extern 类型 变量名;

​		返回值类型 变量名(参数...);

静态区:  只要函数、变量加上 static 关键字，这些变量和函数只能在当前文件内访问。

字符串常量区: 双引号括起来的字符串。不能修改。

**栈区:**  

 1. 系统自动管理，不能使用 free 去释放栈区内存。

 2. 栈区比较小。如果大数据不要放在栈上。当程序运行的时候，栈区大小是固定的。

     	1. 栈空间占用如果超过最大上限，会出现 Stack Overflow 栈溢出。

 3. 栈区内存由系统管理，它的内存申请、释放效率非常高的。

    ```c
    int arr1[100000000];
    	int arr2[100000000];
    	int arr3[100000000];
    	int arr4[100000000];
    ```

**堆区:**

1. 堆区的内存由开发人员自己申请，自己释放。如果忘记 free, 会出现内存泄露。
2. 堆区的内存比较大。真正开发环境下，大量的数据需要放在堆区存储。
3. 有些数据，我们需要控制它的生命周期，将数据存储在堆上。
4. 堆区相对于栈区，内存管理效率就很低。在项目中，进行优化。内存池。
   1. 程序一运行，一次性 malloc 一大块内存。
   2. 当程序需要内存时，找到自己的内存池，使用。
   3. 用完之后再放到内存池中。
   4. 减少 malloc free 的次数。



## 7. 内存操作

操作内存时，不用再区分堆、栈等区别。定义变量的时候，需要区分。

memset ： 初始化内存

memcpy:  内存拷贝, 不能出现内存重叠。将内存中的字节拷贝到另外一个内存。处理字符串拷贝，建议还是用 strcpy

memmove: 内存移动.  处理内存重叠现象。效率比 memcpy 低。

memcmp: 内存比较。 strcmp  从开始位置到\0比较。

四个函数都是 mem 开头，\#include <memory.h> .



**memset例子：**

```c
// 1. memset 数组初始化方式、int arr[10] = { 0 }、for 循环、memset
void test01()
{
	// 数组初始化方式一
	// int arr[10] = {0};

	// 数组初始化方式二
	//int arr[10];
	//for (int i = 0; i < 10; ++i)
	//{
	//	arr[i] = 0;
	//}

	int arr[10];
	// 第一个参数，是初始化内存的首地址, 类型 void *
	// 第二个参数，将内存初始化成什么值，类型 int
	// 第三个参数，从首地址开始多少个字节，设置为 0
	memset(arr, 0, sizeof(arr));


	for (int i = 0; i < 10; ++i)
	{
		printf("%d ", arr[i]);
	}
	printf("\n");
}

// 将int类型设置为0值
void test02()
{
	int a = 412312;
	memset(&a, 0, sizeof(int));
	printf("a = %d\n", a);

}

void test03()
{
	// malloc 返回的指针类型，就是指向首元素类型的指针
	char *s = malloc(sizeof(char) * 32);
	// 将内存初始化成0
	memset(s, 0, 32);
	printf("s = %s\n", s);
}
```



**内存拷贝:**

```c
// memcpy 不能有内存重叠，如果有内存重叠，不保证一定成功。
// memmove 内存实现交换两个变量的值
void test04()
{
	// 内存拷贝
	int a = 10;
	int b = 20;

	// 第一个参数： 目标空间的首地址
	// 第二个参数:  源空间的首地址
	// 第三个参数: 从源空间拷贝多少字节的数据到目标空间
	printf("a = %d, b = %d\n", a, b);
	memcpy(&a, &b, 4);
	printf("a = %d, b = %d\n", a, b);
}

void test05()
{
	int arr[] = { 1, 2, 3, 4, 5 };
	for (int i = 0; i < 5; ++i)
	{
		printf("%d ", arr[i]);
	}

	printf("\n");
	// 第一个参数： 目标空间的首地址
	// 第二个参数:  源空间的首地址
	// 第三个参数: 从源空间拷贝多少字节的数据到目标空间
	memmove(arr, arr + 1, sizeof(int) * 4);
	for (int i = 0; i < 5; ++i)
	{
		printf("%d ", arr[i]);
	}
}

// 使用 memcpy 交换两个变量的值
void test06()
{
	int a = 10;
	int b = 20;
	printf("a = %d, b = %d\n", a, b);

	int temp = 0;

	// 将 a 的数据拷贝到 temp 中
	memcpy(&temp, &a, 4);
	// 将 b 的数据拷贝到 a 中
	memcpy(&a, &b, 4);
	// 将 temp 的数据拷贝到 b 中
	memcpy(&b, &temp, 4);

	printf("a = %d, b = %d\n", a, b);
}
```



**内存比较:**

```c
// memcmp 内存实现比较两个数组是否相等
// 主要用于判断是否相等, 逐个字节比较
void test07()
{
	int a = 10;
	int b = 20;

	// 第一个参数，参与比较的数据的首地址
	// 第二个参数, 参与比较数据的首地址
	// 第三个参数，从首地址开始比较的字节数
	if (memcmp(&a, &b, 4) == 0)
	{
		printf("相等!\n");
	}
	else
	{
		printf("不相等!\n");
	}

}
```



## 8. 结构体



**结构体定义语法:**

先定义类型，再使用类型定义变量.

```c
// 类型定义
struct Person
{
	int age;
	double salary;
	char name[64];
};

// 3. 结构体是一个类型， 类型都拿来定义变量
void test01()
{
	struct Person  p;  // 使用 Person 类型定义出变量叫做 p
	p.age = 30;
	p.salary = 9999.99;
	strcpy(p.name, "Obama");

	printf("Name:%s Age:%d Salary%lf\n", p.name, p.age, p.salary);
}
```

定义类型之后，顺便定义全局变量：

```c
// 2. 员工类型 emp, 员工编号 员工姓名 员工工资 员工电话
struct Emp
{
	int emp_no;  // 员工编号
	char emp_name[64];  // 员工姓名
	double emp_salary;  // 员工工资
	char emp_tele[128]; // 员工电话
}my_emp = {10001, "司马狗剩", 6789.98, "1234567890"};  // 定义类型之后，马上定义全局变量 my_emp

void test03()
{
	my_emp.emp_no = 10001;
	printf("%d\n", my_emp.emp_no);
}

```





















