# C语言第一天课程笔记

每一天的笔记包含如下内容:

1. 当天授课内容安排
2. 课堂重点内容笔记
3. 课后思考题



## 1. 内容安排

第一节课: 课程概述、计算机硬件/软件
第二节课: **操作系统的作用**、编程语言与编译器的作用、编程语言发展历程、C语言标准/优缺点
第三节课: vs使用(创建项目、设置字体、大小)、**第一个C语言程序**
第四节课:  #include 指令作用、main函数、代码块、print函数
第五节课: 注释(注释的编写、如何写注释)、断点调试(逐步、逐过程、跳出)
第六节课: C程序编译过程



## 2. 课堂笔记

课堂主要内容、重要内容。



#### 2.1 计算机硬件

CPU： 计算和控制。计算包含逻辑运算、数值运算等。控制外部硬件设备。CPU执行程序。执行速度非常快。

内存：电信号存储数据，读写速度快，缺点是断电数据丢失。

硬盘：磁信号存储数据，读写速度慢，优点可以持久存储数据。

硬盘中的数据都需要加载到内存中，CPU只从内存中读写数据，当内存中的数据想要长久你保存，必须要写入到磁盘。（文件读写）



#### 2.2 计算机软件

操作系统： 对下进行硬件管理、对上给用户提供操作方式，图形界面普通用户）、终端命令(开发人员)、系统调用(开发人员)



#### 2.3 编程语言和编译器

编程语言：告诉CPU要做什么事情，控制计算机硬件。编程语言就是文字和符号的组合。我们学编程语言最主要学习这些符号和文字的组合规则。

编译器：编译器是一个程序，负责将人能够读的代码转换成计算机能够识别的代码(0001010101010)。



#### 2.4 编程语言发展

```c
00001010101
01010101010
00000001101
```



汇编语言： 移植性比较差。编码比较复杂。维护性比较差。

#### 2.5 C语言标准

ANSI/IOS C语言标准就是一本对C语言说明的书。

微软 、Borland 、Intel、开源组织等进行C语言开发(开发编译器)，既然是说明书，厂商可以选择性放弃某些规则，优化规则，各个编译器之间会有一些差异。

C标准: C89 C99 C11,如果用到了某个语法，编译器报错，该编译器不支持。

写程序一定要知道：编译器  标准。gcc对标准支持最好。 Linux 用 gcc  

微软：vc++  宝蓝：bcc 、intel c++



#### 2.6 C语言学习理由

1. 速度快，效率高
2. 啥都能干

缺点：代码量多一些。



#### 2.7 上午自习任务

1. 代码片段管理器添加到你的vs里， 按 `#1 + tab` 能够添加代码。
2. C语言第一个程序写3遍
3. 笔记过一遍，重要概念看一遍。
4. 预习下午内容.



#### 2.8 main 函数作用

main函数是程序的入口点，操作系统运行程序的时候从main函数开始执行。

一个程序只能有一个main函数。

main 函数两种写法:

```c
int main()
{
	return 0;
}
```

```c
void main()
{
	
}
```

项目开发中写第一种。



#### 2.9 #include 包含

使用工具箱(包含头文件)，在每一个被导入的工具箱中都定义了一系列相关的、已经写好的功能函数。如果想使用:

1. 导入
2. 使用

工具箱分为两种：C语言自带的(`#include` <>, 告诉编译器去系统目录下找 )、自定义(`#include "", `告诉编译器在当前目录下找)



#### 2.10 注释

注释：就是在程序中添加一些解释说明文字。被注释的代码、文本，编译器不会编译执行。

C语言中添加注释有三种方式:

第一种

```c
// 第一行注释文本
// 第二行注释文本
```

尽量不要在代码后侧去写。

第二种

```c
/*
		注释内容
		注释内容
		注释内容
*/

/* 注释内容 */
```

第三种

```c
#if 0
	所有之间代码都会被注释
#endif
```

大部分公司都有编码规范，注释规范，按照公司要求编写注释。

如果公司没有规范，三种可以选择，尽量不要混用注释。尽量保持注释风格统一。

VS里有注释的快捷键：注释-ctrl+k, c  解除注释 ctrl+k,u



**什么情况下写注释:**

1. 可以写代码的实现思路
2. 代码比较有技巧，增加注释解释说明
3. 文件注释
4. .....



#### 2.11 调试

在代码上前面单击，可以在改行增加断点。当程序执行到断点时，程序就会停止。

逐语句：碰到代码是一个功能，会进入到功能内部继续按行执行。

逐过程：碰到代码是一个功能，不会进入到功能内部执行。

跳出：跳出功能内部，回到外部。



增加断点快捷键： F9



####  2.12 编译四个步骤



<img src="assets/01_编译过程.jpg" />



#### 2.13 变量的作用及其命名规则

程序用来处理数据。待处理的数据要能够保存，用变量来保存数据。

变量其实就是一块内存空间，这块内存空间中存储的值可以修改的。

变量: 变量名字  变量的地址  变量的空间大小。

```c
int a = 10;  // 分配空间，把10存储到a这个空间中。  printf("%p", &a);
int b = 20;
int c = a + b;
```

程序结束之后，a b c三个变量的值是否还存在？数据就不存在了。

变量用来存储程序运行过程中所需要的临时数据。













































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















































