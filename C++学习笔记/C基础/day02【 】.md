# C语言第二天课程笔记

每一天的笔记包含如下内容:

1. 当天授课内容安排
2. 课堂重点内容笔记
3. 课后思考题



## 1. 内容安排

**第一节课:**   `变量的命名规则`、`变量的命名规范`、`变量名练习`、`变量的类型`
**第二节课:**   `整型变量的输入和输出(1)`
**第三节课:**   `整型变量的输入和输出(2)`
**第四节课:**   `进制转换(手动转换、进制输出、进制赋值、程序转换)`
**第五节课:**   `字符类型(定义大小存储、相关操作、练习、转义字符)`
**第六节课:**   `浮点数类型`



## 2. 课堂笔记

课堂主要内容、重要内容。



#### 2.1 变量命名规则

回顾: 变量的作用就是临时存储程序运行过程中需要的数据。

变量：变量名、变量地址、变量值，在程序中可以通过变量名、地址来操作变量(读写变量)。

```c
int a = 10;  // int类型、a变量名、10值
```



**变量名命名规则:**

1. 标识符不能是关键字
2. 标识符只能由字母、数字、下划线组成
3. 第一个字符必须为字母或下划线
4. 标识符中字母区分大小写

**变量名定义要求:** 变量名一定要见名知意。 

**变量名规范:**

1. 小驼峰法: 变量由多个单词构成则首个单词小写，后面单词首字母大写. 如果一个单词则小写。userNameAge

2. 大驼峰法: 变量由多个单词构成则所有单词首个字母大写。如果一个单词，则首字母大写。UserNameAge

3. 小写+下划线:  单词之间使用下划线相连。 C语言中该命名方法居多。user_name_age 

  在C语言，用小写+下划线最多。  

**练习:**

```c
fromNo12
from#12
my_Boolean
my-Boolean
Obj2
2ndObj
myInt
test1
Mike2jack
My_tExt
_test
test!32
haha(da)tt
int
jack_rose
jack&rose
GUI
G.U.I
```



#### 2.2 变量类型

1. 指定变量开辟多大的内存空间来存储数据。 int a = 10;
2. 指定数据能否进行运算，以及运算规则是什么。

**类型分类**:

1. 基本数据类型， 编译器内置的数据类型 int，同一个系统同一个编译器大小是一样的。
   1. 整型（short、int、long、long long）
   2. 浮点型
   3. 字符型

2. 自定义数据类型，大小和定义有关系
3. 指针类型。基本类型的指针、自定义类型的指针。



每一种类型的：空间大小、取值范围、该类型输出、类型的输入、有符号类型和无符号类型

只有整数类型才有符号和无符号之分，小数不分。

```c
signed int a = 100;  // 有符号的变量，既可以存储正数、也可以存储负数
unsigned int b = 100;  // 有符号的最小值 0
```



#### 2.3 short 类型变量的输入和输出

```c
#if 1
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>

// C语言常量
#define MAX_VALUE 1000000

// 1. short 类型的输入和输出
void test01()
{
	// 1.1 无符号、有符号 short 类型的输入和输出
	short s = 100;
	printf("有符号 short 类型 s 变量的值是: %hd\n", s);
	unsigned short us = 100;
	printf("无符号的 short 类型 us 变量的值是: %hu\n", us);


	// 1.2 short 类型占用的内存大小 sizeof
	int short_size = sizeof(short);  // 先执行等号右侧， 将结果赋值给等号左侧的变量
	printf("short 类型占用的内存大小是: %d\n", short_size);
	printf("short 类型占用的内存大小是: %d\n", sizeof(short));
	printf("unsigned short 类型占用的内存大小是: %d\n", sizeof(unsigned short));

	// 1.3 short 类型和 unsigned short 类型的取值范围
	printf("short 类型最小值是: %hd, 最大值是: %hd\n", SHRT_MIN, SHRT_MAX);
	printf("unsiged short 类型最小值: 0, 最大值是: %hu\n", USHRT_MAX);

	// 1.4 short 类型输入操作  scanf
#if 0
	short shrt_number = 0;
	printf("请输入一个 short 类型的数字:");
	// %hd 表示按照 short 类型输入， &shrt_number 表示键盘输入数据之后，数据存储到内存中哪个位置
	scanf("%hd", &shrt_number);  // 阻塞等待
	printf("您输入的 short 类型的值是: %hd\n", shrt_number);
#endif

	unsigned short ushrt_number = 0;
	printf("请输入一个 unsigned short 类型的数字:");
	scanf("%hu", &ushrt_number);
	printf("您输入的 unsigned short 类型的值是: %hu\n", ushrt_number);
}


// 2. int 类型的输入和输出
// 3. long 类型的输入和输出
// 4. long long 类型的输入和输出

int main(int argc, char *argv[])
{

	test01();

	system("pause");
	return 0;
}
#endif
```



#### 2.4 int 类型变量的输入和输出

```c
#if 1
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>

// C语言常量
#define MAX_VALUE 1000000

// 2. int 类型的输入和输出(整型)
void test02()  // 函数  名字叫做 test02, 执行函数要调用
{

	// 2.1 int 类型和 unsigned int 类型输入和输出
	int i = 100;
	printf("i = %d\n", i);
	unsigned int ui = 666;
	printf("ui = %u\n", ui);


	// 2.2 int 类型占用的内存大小(4字节)
	printf("int 类型占用的内存大小: %d\n", sizeof(int));
	printf("unsigned int 类型占用的内存大小: %d\n", sizeof(unsigned int));


	// 2.3 有符号、无符号类型取值范围
	printf("int 类型的最小值:%d, 最大值:%d\n", INT_MIN, INT_MAX);
	printf("unsigned int 类型最小值: 0, 最大值: %u\n", UINT_MAX);


	// 2.4 int类型 变量输入
	int inumber = 0;
	printf("请输入 int 类型的值:");
	scanf("%d", &inumber);
	printf("您输入的 int 类型的值是: %d\n", inumber);

	unsigned int uinumber = 0;
	printf("请输入 unsigned int 类型的值:");
	scanf("%u", &uinumber);
	printf("您输入的 unsigned int 类型的值是: %u\n", uinumber);

}

// short < int(long) << long long

int main(int argc, char *argv[])
{

	test02();

	system("pause");
	return 0;
}
#endif
```



#### 2.5 long 类型变量的输入和输出

```c
#if 1
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>

// C语言常量
#define MAX_VALUE 1000000

// 3. long 类型的输入和输出（长整型）
void test03()
{
	
	// 1. long 类型和 unsigned long 类型输出
	long l = 888;
	printf("l = %ld\n", l);
	unsigned long ul = 888;
	printf("ul = %lu\n", ul);

	// 2. long 类型占用的内存大小
	// long 的大小不能比 int 小
	// gcc 编译器 long 类型占8个字节
	printf("long 的内存大小是:%d\n", sizeof(long));

	// 3. long 取值范围
	printf("long 类型的最小值:%ld, 最大值:%ld\n", LONG_MIN, LONG_MAX);
	printf("unsigned long 类型最小值: 0, 最大值:%lu\n", ULONG_MAX);

	// 4. long 输入
	long lnumber = 0;
	scanf("%ld", &lnumber);
	printf("lnumber = %ld\n", lnumber);

	unsigned long ulnumber = 0;
	scanf("%lu", &ulnumber);
	printf("ulnumber = %lu\n", ulnumber);
}

// short < int(long) << long long

int main(int argc, char *argv[])
{

	test03();

	system("pause");
	return 0;
}
#endif
```



#### 2.6 2long long 类型变量的输入和输出

```c
#if 1
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>

// C语言常量
#define MAX_VALUE 1000000

// 4. long long 类型的输入和输出
void test04()
{
	
	// 1. long long 类型输出
	// 有些编译器不支持 long long 类型
	long long ll = 100;
	printf("ll = %lld\n", ll);

	unsigned long long ull = 100;
	printf("ull = %llu\n", ull);

	// 2. long long 内存大小
	printf("long long 类型的内存大小: %d\n", sizeof(long long));

	// 3. 取值范围
	printf("long long 类型的最小值:%lld, 最大值:%lld\n", LLONG_MIN, LLONG_MAX);
	printf("unsigned long long 类型的最小值: 0, 最大值: %llu\n", ULLONG_MAX);

	// 4. long long 输入
	long long llnumber = 0;
	scanf("%lld", &llnumber);
	printf("llnumber = %lld\n", llnumber);


	unsigned long long ullnumber = 0;
	scanf("%llu", &ullnumber);
	printf("ullnumber = %llu\n", ullnumber);
}

// short < int(long) << long long

int main(int argc, char *argv[])
{

	test04();

	system("pause");
	return 0;
}
#endif
```

#### 2.7 字符类型

```c
#if 1
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>
#include <ctype.h>

// 1. 字符语法(字符变量的定义、输入和输出、字符的内存大小、字符的存储原理、字符取值范围)
void test01()
{

	// 1.1 字符变量的定义、输出
	char c1 = 'a';
	char c2 = '1';  // 区分数字1和字符1
	printf("c1 = %c\n", c1);
	printf("c2 = %c\n", c2);


	// 1.2 字符占用内存大小(1)
	printf("char 类型内存大小: %d\n", sizeof(char));


	// 1.3 字符存储, 字符在存储的时候实际存储的是一个数字。 ascii 码表
	printf("c1 = %d\n", c1);
	char c3 = 68;
	printf("c3 = %c\n", c3);

	// 1.4 char 类型取值范围
	printf("char 的最小值:%d, 最大值:%d\n", CHAR_MIN, CHAR_MAX);
	printf("unsigned char 的最小值: 0, 最大值:%d\n", UCHAR_MAX);

	// 1.5 char 输入
	char c4 = 0;
	scanf("%c", &c4);
	printf("c4 = %c\n", c4);

}


// 2. 字符简单操作
void test02()
{
	char c1 = 'a';
	char c2 = 'B';

	// 如果需要使用字符处理函数(功能)， 需要导入头文件 ctype.h
	// 2.1 把字符转换为大写
	char c3 = toupper(c1);  // 将c1值转换成大写并返回，c1 仍然小写的
	printf("c3 = %c, c1 = %c\n", c3, c1);

	// 2.2 把字符转换为小写
	char c4 = tolower(c2);
	printf("c4 = %c, c2 = %c\n", c4, c2);

	// 2.3 把字符转换为 ascii 码值
	int ascii_val = toascii(c1);
	printf("ascii_val = %d\n", ascii_val);
}


// 3. 练习: 从键盘输入任意一个字母，将其转换为大写并输出
void test03()
{
	printf("请输入任意一个字母:");
	char c = 0;
	scanf("%c", &c);

	char upper_char = toupper(c);
	printf("您输入的字母是: %c\n", upper_char);
}


// 4. 转义字符
// \a \b \n \t \\ \" \022 \x22 %%
// 字符本身具有一定的意义，加上反斜杠之后就变成另外的意思。转义
void test04()
{
	// 1. 警报
	// printf("\a");

	// 2. \b 退格，删除键
	// printf("abcd\b");

	// 3. \n 换行
	// char c = '\n';
	// printf("abc\n");

	// 4. \t 制表符 tab键
	// printf("Name\tAge\n");
	// printf("Trump\t56\n");

	// 5. "\\" 输出一个"\"
	// printf("\\t");

	// 6. \" 输出一个双引号
	// printf("\"");

	// 7. %%  %d %c %hd %u 占位符
	// printf("99.9%%");

	// 8. \0dd
	// printf("\022");  // 21
	// printf("\x22");

}


int main(int argc, char *argv[])
{
	test04();


	system("pause");
}
#endif
```

#### 2.8 进制转换

十进制： 逢十进一， 0-9

二进制：逢二进一， 0-1

八进制：逢八进一，0-7

十六进制：逢十六进一，0-F

十进制转二进制

小数转二进制



```c
#if 1
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// 1. 进制的赋值、输出
void test01()
{
	// 程序不支持二进制赋值
	// 程序不支持二进制输出

	int a = 12;  // 十进制赋值  12
	int b = 012;  // 八进制    10   oct
	int c = 0x12; // 十六进制   18   hex

	printf("a = %d\n", a);
	printf("b = %d\n", b);
	printf("c = %d\n", c);

	printf("a = 0%o\n", a);
	printf("a = 0x%x\n", a);
}

void test02()
{
	int number = 38;

	// char bin[128] 申请指定大小的内存空间
	//  = { 0 } 表示将 128字节空间全部清空
	char bin[128] = { 0 };

	// 第一个值表示要进行进制转换的值
	// 第二个值表示进制转换完毕之后，将结果存储到哪个内存中
	// 第三个值表示要进行二进制转换、八进制、十六进制、32进制.... 2^n次方
	_ltoa(number, bin, 2);  // "0101010101"
	printf("%s\n", bin);


	char oct[128] = { 0 };
	_ltoa(number, oct, 8);
	printf("%s\n", oct);


	char hex[256] = { 0 };
	_itoa(number, hex, 16);
	printf("%s\n", hex);
}


int main(int argc, char *argv[])
{
	test02();

	system("pause");
}
#endif
```



#### 2.9 浮点类型

float 精度：6-7， 6位准确，7位不保证准确

double精度: 15-16,15位准确，16不保证准确

float 和 double 取值范围很大，但是精度不高，当输入一个浮点数时，编译器会将其精确到某一个近似值上。




