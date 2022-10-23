#  C语言第十一天课程笔记

每一天的笔记包含如下内容:

1. 当天授课内容安排
2. 课堂重点内容笔记
3. 课后思考题



## 1. 内容安排

**第一节课:**   `内存操作回顾、结构体训练`
**第二节课:**   `结构体嵌套指针、结构体嵌套结构体、结构体嵌套自身类型指针`
**第三节课:**   `结构体作为函数参数、结构体数组作为函数参数、main 函数参数`
**第四节课:**   `联合体 union、typedef 用法`
**第五节课:**   `enum枚举、逗号运算符`
**第六节课:**   `文件理解`



内存分区：代码区、全局静态区、堆区、栈区

​	我们关注数据分区，主要原因:   我们得了解变量的声明周期，能够知道哪些数据可以用，哪些数据不能用。

​	分区: **管理方式**、**大小**、**效率**。

 	1. 希望一个变量只在函数内存活，应该把变量定义在栈区。
      	1. 存放哪些变量:  函数形参、函数的局部变量
      	2. 自动申请、自动释放。
      	3. 内存大小固定的，容量有限。大数据不要放在栈上。如果栈满了，Stack Overflow, 栈溢出。
      	4. 效率高。
	2. 希望一个变量在整个程序运行期间都存在，并且不需要手动管理。变量应该定义全局静态区。
    	1. 存放哪些变量: 全局变量、静态变量、字符串常量(只读).
    	2. 自动分配空间、自动释放空间。
    	3. 固定大小。
    	4. 效率高。
	3. 希望手动控制变量的生命周期。应该把变量放在堆区。
    	1. 通过 malloc 函数分配的空间在堆上。
    	2. 手动申请(malloc)、手动释放(free)。如果 malloc 了，没有 free, 会出现内存泄露问题。底层内存函数。
    	3. 比较大的。
    	4. 效率低。优化程序效率的话，减少 malloc free 的次数。

内存操作: 

1. 内存初始化: memset。
2. 内存拷贝: memcpy(效率高、不能处理内存重叠的拷贝)、memmove(效率较低、能够处理内存重叠拷贝)。
3. 内存比较: memcmp 比较是否相等.
4. memcpy strcpy 区别:
   1. strcpy(char *dst, char *src)、 memcpy(void *dst, void *src, size_t size)
   2. strcpy 处理字符串的 \0, memcpy 不处理。
   3. 字符串拷贝就用 strcpy。



## 2. 结构体训练

1. 定义结构体 student，包括 name、age、score。 实现输入 name、age、score，并打印 name、age、score。

   ```c
   struct Student
   {
   	// 名字
   	char name[64];
   	// 年龄
   	int age;
   	// 分数
   	int score;
   };
   
   void test01()
   {
   	struct Student student = { "司马小花", 18, 99 };
   	// 如果 student 是一个普通变量，使用点访问
   	// 如果 student 是一个指针类型的变量，使用->访问
   	printf("Name: %s, Age: %d, Score: %d\n", student.name, student.age, student.score);
   
   	printf("请输入姓名:");
   	scanf("%s", student.name);
   	printf("请输入年龄:");
   	// 点的优先级为最高， &优先级是8级
   	scanf("%d", &student.age);
   	printf("请输入分数:");
   	scanf("%d", &student.score);
   
   	printf("Name: %s, Age: %d, Score: %d\n", student.name, student.age, student.score);
   }
   ```

2. 定义结构体 account，每个 account 包含账号、身份证号码，姓名，地址，金额。 实现输入各个值，保存到结构体变量中并输出。



## 3. 结构体嵌套使用注意

1. 结构体嵌套指针

   ```c
   struct Person
   {
   	char *name;  // 结构体内部嵌套指针变量
   	int age;
   	int salary;
   };
   
   void test01()
   {
   	// name 指针指向字符串常量
   	struct Person p1 = { "Obama", 20, 9999 };
   	// 尝试向 name 指向的内存空间中拷贝字符串数据
   	//  写入位置 0x01216B30 时发生访问冲突。
   	// 原因: 1. 写入的空间不是自己申请的。 2. 空间是自己，但是空间是只读的。
   	strcpy(p1.name, "Trump");
   }
   
   void test02()
   {
   	// name 指向的空间是字符串常量区，所以禁止修改
   	// name 指向自己申请空间
   	struct Person person = { NULL, 20, 9999 };
   	// 重新申请一块堆上的内存，name 指向它
   	person.name = malloc(sizeof(char) * 64);
   	if (NULL == person.name)
   	{
   		return;
   	}
   	// 将 obama 数据拷贝到 name 指向的堆空间中
   	strcpy(person.name, "obama");
   	// 打印 Person
   	printf("Name:%s Age:%d Salary:%d\n", person.name, person.age, person.salary);
   	// 释放 name 指向的堆内存
   	if (person.name != NULL)
   	{
   		free(person.name);
   		person.name = NULL;
   	}
   }
   ```

   <img src="assets/07_结构体嵌套指针.jpg" />

2. 结构体嵌套结构体： 结构体允许嵌套其他类型的结构体变量.	

3. ```c
   struct DrangonWeapon
   {
   	// 武器攻击力
   	int attack;
   	// 暴击率
   	int crit_rate;
   	// 暴击伤害
   	int crit_damage;
   };
   
   
   struct Hero
   {
   	// 英雄名字
   	char name[64];
   	// 攻击力
   	int damage;
   	// 防御力
   	int defense;
   	// 血量
   	int hp;
   	// 魔法
   	int mp;
   	// 武器
   	struct DrangonWeapon weapon;
   };
   
   #if 0
   struct Hero
   {
   	// 英雄名字
   	char name[64];
   	// 攻击力
   	int damage;
   	// 防御力
   	int defense;
   	// 血量
   	int hp;
   	// 魔法
   	int mp;
   	// 武器(匿名结构体)
   	struct
   	{
   		// 武器攻击力
   		int attack;
   		// 暴击率
   		int crit_rate;
   		// 暴击伤害
   		int crit_damage;
   	}weapon;  // 定义类型同时定义变量
   };
   #endif
   ```

   

4. 结构体嵌套自身类型指针

   ​	结构体允许嵌套本身类型的指针变量。

   ​	结构体不允许嵌套本身类型的结构体变量，原因： 无法确定结构体，不能分配内存。

   ```c
   // 结构体不允许嵌套本身类型的结构体变量
   #if 0
   struct MyStruct
   {
   	int a;
   	double b;
   	struct MyStruct c;  // 编译器不知道 MyStruct 大小
   	char d;
   };
   #endif
   
   struct MyStruct2
   {
   	int a;
   	double b;
   	struct MyStruct2 *c;   // 无论什么类型的指针，只占4个字节
   	char d;
   };
   
   ```

   **思考:**

   ```c
   struct Person
   {
   	char *name;
   	int age;
   	int salary;
   }p1 = {NULL, 10, 9999}, *p2 = NULL;
   ```

   1. 第一个问题： p2 是什么类型的？结构类型指针。4个字节。

   2. 第二个问题：()圆括号 [] 方括号 {}花括号。 花括号在初始化的时候使用。

   3. 花括号使用的场景:

      1. 数组定义初始化： `int arr[] = {10, 20, 30}`

      2. 结构体变量定义初始化: `struct Person p = {NULL, 10, 9999}`

      3. 注意:

         ```c
         struct Person p;
         // 不能使用。只能初始化时候使用
         // p 重新赋值操作，此时不能使用花括号
         p = {}
         ```

         


## 4. 结构体作为函数参数

1. 结构体变量作为函数参数

   ```c
   struct Person
   {
   	char name[64];
   	int age;
   };
   
   // 结构体值传递
   void print_person(struct Person p)
   {
   	printf("Name:%s Age:%d\n", p.name, p.age);
   }
   
   // 地址(指针)传递
   void print_person_by_pointer(const struct Person *p)
   {
   	printf("Name:%s Age:%d\n", p->name, p->age);
   }
   
   void test01()
   {
   	// 对于变量: 值 地址
   	struct Person person = { "Obama", 33 };
   	// person 和 p 没有任何关系， p 的修改不会导致 person 改变
   	// 将结构体 68 字节数据拷贝给 p 变量
   	print_person(person);
   
   	// 传递效率不高
   	// 函数无法修改外部变量
   	print_person_by_pointer(&person);
   }
   
   
   void test02()
   {
   	// 创建结构体指针
   	struct Person *p_person = NULL;
   	// 创建结构体变量
   	struct Person person = { "Obama", 33 };
   	// 1. 要知道变量是指针还是普通变量
   	// 2. 变量的内存在哪里？
   	// 3. 如果是指针变量，我们得知道指向的是哪里？指向类型？
   
   	// p_person 栈区， person 栈区
   	// 指针可以指向 栈变量、堆变量、字符串常量
   	p_person = &person;
   	// 修改指针指向
   	p_person = malloc(sizeof(struct Person));
   	if (p_person != NULL)
   	{
   		free(p_person);
   		p_person = NULL;
   	}
   }
   
   void create_person(struct Person **p)
   {
   	struct Person *person = malloc(sizeof(struct Person));
   	strcpy(person->name, "Obama");
   	person->age = 100;
   
   	// 通过指针间接赋值的特性，将函数结果返回
   	*p = person;
   }
   
   void test03()
   {
   	// 定义指针变量
   	struct Person *p_person = NULL;
   	// 编写函数，在函数内部为 p_person 分配空间
   	create_person(&p_person);
   	// 打印输出变量值
   	printf("Name:%s Age:%d\n", p_person->name, p_person->age);
   	// 释放堆内存
   	if (p_person != NULL)
   	{
   		free(p_person);
   		p_person = NULL;
   	}
   }
   ```

   <img src="assets/08_结构体作为函数参数.jpg" />

2. 结构体数组作为函数

   ```c
   struct Person
   {
   	char name[64];
   	int age;
   };
   
   
   void print_person_array(struct Person ps[], int len)
   {
   	for (int i = 0; i < len; ++i)
   	{
   		printf("Name:%s Age:%d\n", ps[i].name, ps[i].age);
   	}
   }
   
   void test()
   {
   	// 定义结构体数组
   	// 对于基本类型，作为函数参数，默认值传递
   	// 对于数组类型, 作为函数参数，数组名指向首元素的指针
   	struct Person persons[] = {
   		{ "Obama",  45 },
   		{ "Trump",  70 },
   		{ "Polly",  65 },
   	};
   
   	print_person_array(persons, 3);
   }
   ```

   

## 5. main 函数参数作用

1. argc 参数作用

   1. 参数的个数，名字可以修改的。

2. argv 参数作用

   1. char* 类型数组
   2. 第一个参数：可执行文件的名字
   3. 第二个参数之后才是真正要传递给 main 函数的外部数据。
   4. 通过命令行传递的参数，都会保存在该数组中。

   ```c
   	// main 函数的参数，也是由外部传递进来的
   	// 当你执行 exe 程序的时候，传递进来的。 
   	// 命令行启动程序的时候，可以传递参数
   	// argc 传递参数的个数
   	// argv 里面的指针，分别指向了每一个参数
   
   	printf("参数的个数是:%d\n", argc);
   	for (int i = 0; i < argc; ++i)
   	{
   		printf("%s\n", argv[i]);  // argv[i] 是什么类型? char*类型， char* 类型用于指向字符串
   	}
   ```

3. main 的参数是否能够省略，取决于编译。如果省略，在有些编译就会报错，编译失败。建议的方式：写上main的参数。

4. 通过命令传递的参数都是 char* 类型的字符串，无论输入的是什么。



## 6. 联合体 Union 特性与使用

普通数据定义时，就需要按照一种方式去使用内存。而 Union 允许程序员使用多种方式使用内存数据。

1. union 特性

   1. union 的数据成员共享内存.
      1. 每个成员的地址相同。
      2. 对一个成员赋值会影响其他成员。
   2. union 类型大小取决于最大成员的大小.

2. union 使用

   union 可以使用初始化列表默认给第一个成员初始化.

   

   

## 7. typedef 作用与用法

给类型取别名，今后通过别名来定义变量。

1. 给普通变量定义别名

   ```c
   typedef int my_int_type;
   typedef double my_double_type;
   
   void test01()
   {
   	int a = 10;
   	my_int_type b = 10;
   	my_double_type c = 3.14;
   }
   ```

   

2. 给结构体变量定义别名

   ```c
   #if 0
   struct Person
   {
   	char name[64];
   	int age;
   };
   
   typedef struct Person MyPerson;
   #else
   
   typedef struct Person
   {
   	char name[64];
   	int age;
   }MyPerson;
   
   #endif
   
   void test02()
   {
   	MyPerson person = { "Obama", 56 };
   }
   ```

3. 使用场景: 定义跨平台的数据类型.

   ```c
   // 需求: 定义当前平台支持的最大整型  long long
   
   typedef long LLLONG;
   
   void test03()
   {
   	LLLONG a = 10;
   	LLLONG b = 20;
   }
   ```

   

## 8. 枚举 enum 作用与使用



1. enum 使用语法

   ```c
   void test01()
   {
   	// 定义枚举类型， enum Week 类型的名字
   	enum Week {Mon, Tue, Wed, Thu, Fri, Sat, Sun};
   	// enum Week { Mon = 100, Tue = 200, Wed = 500, Thu = 800, Fri = 900, Sat = 1000, Sun = 1200 };
   	// enum Week { Mon, Tue, Wed = 100, Thu, Fri, Sat, Sun };
   
   	// 错误:表达式必须为整型常量表达式, 枚举值不能是小数、字符串
   	// enum Week { Mon = "hello", Tue, Wed = 100, Thu, Fri, Sat, Sun };
   	// enum Week { Mon = 3.14, Tue, Wed = 100, Thu, Fri, Sat, Sun };
   
   	// 使用枚举类型定义变量
   	enum Week my_week = Mon;
   	my_week = Sun;
   
   	// 打印枚举值
   	printf("my_week = %d\n", my_week);
   
   }
   ```

   

2. enum 使用注意

   ```c
   // 2. C语言的枚举有什么注意点
   void test02()
   {
   	enum Week { Mon, Tue, Wed, Thu, Fri, Sat, Sun };
   	enum Week my_week = Mon;
   	// 1. C语言允许给枚举变量赋值其他类型
   	my_week = 100;
   	// 2. 枚举值具有外层作用域
   	// int Mon = 200;
   	// 3. 不相同的两枚举类型之间是可以进行比较的，影响代码可读性。
   	enum Number { One, Two, Three };
   
   	if (my_week == One)
   	{
   		printf("相等!\n");
   	}
   	else
   	{
   		printf("不相等!\n");
   	}
   
   }
   ```

   





## 9. 逗号运算符语法与作用

逗号运算符：将多个表达式变成一个表达式。

1. 比原来的 for 循环更大点的for循环怎么写.

2. 能够看得懂别人写的逗号运算符的代码。

   

1. 逗号运算符语法和运算规则

    1.1 从左向右, 左边的表达式执行完毕之后，右侧的表达式才会被执行

   1.2 逗号运算符最后一个表达式的结果作为整个表达式的结果

   ```c
   void test01()
   {
   	// 常量表达式
   	int a = (10, 20, 30);
   	printf("a = %d\n", a);
   }
   
   void test02()
   {
   	int a = 10;
   	int b = 20;
   	int c = 30;
   
   	c = (a = b + 10, b = a + 10);
   	// a
   	// b
   	// c
   	printf("a = %d, b = %d, c = %d\n", a, b, c);
   }
   
   
   void test03()
   {
   	int a = 10;
   	int b = 20;
   	int c = 30;
   
   	c = (b + 10, b = a + 10);
   	// a 
   	// b 
   	// c
   	printf("a = %d, b = %d, c = %d\n", a, b, c);
   }
   
   void test04()
   {
   	int a = 10;
   	// int b = (a++, a++, a++, a++);
   	int b = (++a, ++a, ++a, ++a);
   	printf("a = %d, b = %d\n", a, b);
   }
   ```

2. ```c
   
   ```

   

3. 逗号运算符使用场景

   ```c
   void test05()
   {
   	for (int i = 0, j = 0; i < 10 || j < 50; ++i, j+=10 )
   	{
   		printf("i = %d, j = %d\n", i, j);
   	}
   }
   ```



## 10. 文件理解

1. 文件的作用

   文件就是数据来源，持久化保存数据。

2. 文件的打开路径

   绝对路径：从盘符开始记录的路径。

   相对路径：相对于当前目录的一个路径。

3. 文本文件和二进制文件区别

   打开文件分为两种方式: 文本模式、二进制模式打开

   文本模式:  换行符。

   Mac: \r 作为换行符

   Windows: \r\n 作为换行符

   Linux: \n 作为换行符

   程序中换行符: \n 作为换行符

   char s[] = "hello\n"; -> "hello\r";  Mac

   char s[] = "hello\n"; -> "hello\r\n";

   char s[] = "hello\n"; -> "hello\n";

   同理: hello\r -> hello\n;  "hello\r\n" -> hello\n   "hello\n" -> hello\n

   当使用文本模式打开一个文件的时，C语言会进行相应的换行符转换。保存文件、读取文件。

   二进制读写文件时不做任何转换。 如果数据是 "hello\n" -> "hello\n"

   假设 Windows 系统: "hello\r\n" -> "hello\r\n".

   

   思考:

   1. 如果是图片文件读写的话？用什么模式？二进制模式。 只要不是为了让你打开看的文件，都可以用二进制模式。
   2. 写了一篇论文，用文本模式。

   

   ​	

4. 文件缓冲区

   IO ： input（输入、读） output（输出、写），相对于程序而言的。

   标准IO：标准输入(scanf)-从键盘读， 标准输出(printf)-向屏幕写

   文件IO:   文件输入-从文件读，文件输出-向文件写

   网络IO:  网络输入-从网络获取数据 网络输出-向网络发送数据 

   

    标准IO 也是有缓冲区. 标准输入缓冲区、标准输出缓冲区。行缓冲区。碰到换行符刷新。

   文件缓冲区：块缓冲区，缓冲区满的时候，一次性写入数据。  关闭文件，强制刷新缓冲区。

   

   <img src="assets/09_缓冲区.jpg" />

   

   
















