#  C语言第十二天课程笔记

每一天的笔记包含如下内容:

1. 当天授课内容安排
2. 课堂重点内容笔记
3. 课后思考题



## 1. 内容安排

**第一节课:**   `按字符读写文件`
**第二节课:**   `判断文件结束(EOF、feof函数)、实现文件拷贝`
**第三节课:**   `行文件读写（fputs、fgets、strtok、strchr）`
**第四节课:**   `块文件读写(fread、fwrite)`
**第五节课:**   `随机文件读写(fseek、fread、rewind、fwrite)`
**第六节课:**   `案例-登陆读写账号和密码`



数据展示(客户端) 业务逻辑(功能实现)  数据持久化(数据存储)

1. printf scanf 实现 cmd 命令行客户端的交互，数据展示。QT第三方C++库，GUI库。
2. C语言的流程控制、函数语法，实现业务逻辑。 C++面向语言泛型。系统编程(Linux系统)
3. 文件操作: 数据持久化。数据库。

行业结合。



## 2. 字符文件读写

1. 打开和关闭文件

   1. FILE是系统使用typedef定义出来的有关文件信息的一种结构体类型，结构中含有文件名、文件状态和文件当前位置等信息。

      声明FILE结构体类型的信息包含在头文件“stdio.h”中，一般设置一个指向FILE类型变量的指针变量，然后通过它来引用这些FILE类型变量。通过文件指针就可对它所指的文件进行各种操作。 

   2. fopen

      ```c
      #include <stdio.h>
      FILE* fopen(const char * filename, const char * mode);
      功能：打开文件
      参数：
      	filename：需要打开的文件名，根据需要加上路径
      	mode：打开文件的模式设置
      返回值：
      	成功：文件指针
      	失败：NULL
      ```

   3. fclose

      ```c
      #include <stdio.h>
      int fclose(FILE * stream);
      功能：关闭先前fopen()打开的文件。此动作让缓冲区的数据写入文件中，并释放系统所提供的文件资源。
      参数：
      	stream：文件指针
      返回值：
      	成功：0
      	失败：-1
      ```

      

2. 文件打开模式

   | **打开模式** | **含义**                                                     |
   | ------------ | ------------------------------------------------------------ |
   | r或rb        | 以只读方式打开一个文本文件（不创建文件，若文件不存在则报错） |
   | w或wb        | 以写方式打开文件(如果文件存在则清空文件，文件不存在则创建一个文件) |
   | a或ab        | 以追加方式打开文件，在末尾添加内容，若文件不存在则创建文件   |
   | r+或rb+      | 以可读、可写的方式打开文件(不创建新文件)                     |
   | r+或rb+      | 以可读、可写的方式打开文件(不创建新文件)                     |
   | w+或wb+      | 以可读、可写的方式打开文件(如果文件存在则清空文件，文件不存在则创建一个文件) |
   | a+或ab+      | 以添加方式打开文件，打开文件并在末尾更改文件,若文件不存在则创建文件 |

3. 文件关闭

   1. 打开的文件会占用内存资源，如果总是打开不关闭，会消耗很多内存

   2. 一个进程同时打开的文件数是有限制的，超过最大同时打开文件数，再次调用fopen打开文件会失败

   3. 如果没有明确的调用fclose关闭打开的文件，那么程序在退出的时候，操作系统会统一关闭。

4. 文件字符读写

   1. fputc

      ```c
      #include <stdio.h>
      int fputc(int ch, FILE * stream);
      功能：将ch转换为unsigned char后写入stream指定的文件中
      参数：
      	ch：需要写入文件的字符
      	stream：文件指针
      返回值：
      	成功：成功写入文件的字符
      	失败：返回-1
      ```

   2. fgetc

      ```c
      #include <stdio.h>
      int fgetc(FILE * stream);
      功能：从stream指定的文件中读取一个字符
      参数：
      	stream：文件指针
      返回值：
      	成功：返回读取到的字符
      	失败：-1
      ```

5. 示例代码：

   1. 文件打开与关闭

   ```c
   
   // 1. 文件打开关闭
   void test01()
   {
   	// r 以只读方式打开一个文本文件（不创建文件，若文件不存在则报错）
   	// 下面写法不推荐
   	// fopen("C:\\Users\\cpp\\Documents\\codes\\day12\\day12\\demo1.txt", "r");
   	// 大部分系统都支持下面的路径写法（绝对路径）
   	// fopen("C:/Users/cpp/Documents/codes/day12/day12/demo1.txt", "r");
   	// 相对路径
   	FILE *fp = fopen("./demo1.txt", "r");
   	// 返回的 FILE 指针，指向了一块内存，这块内存就存储了当前打开文件的信息
   	// FILE 指针指向的空间是由系统帮我们申请的
   	// 文件打开失败，返回 NULL
   	if (NULL == fp)
   	{
   		printf("文件打开失败!\n");
   		return;
   	}
   
   
   	// 关闭文件
   	// 如果打开文件没有关闭，当程序结束的时候，文件也被关闭。
   	// 文件使用完毕，要及时释放。
   	// 1. fp 指向的内存就会被释放.
   	// 2. 刷新缓冲区，会将缓冲区中的文件内容写入到磁盘中
   	// 3. 对于程序，可打开的文件数量有上限的。
   	fclose(fp);
   	fp = NULL;
   }
   ```

   

## 3. 文件结束

1. EOF

   ```c
   在C语言中，EOF表示文件结束符(end of file)。在while循环中以EOF作为文件结束标志，这种以EOF作为文件结束标志的文件，必须是文本文件。在文本文件中，数据都是以字符的ASCII代码值的形式存放。我们知道，ASCII代码值的范围是0~127，不可能出现-1，因此可以用EOF作为文件结束标志。
   
   #define EOF     (-1)
   ```

2. feof 函数

   ```c
   当把数据以二进制形式存放到文件中时，就会有-1值的出现，因此不能采用EOF作为二进制文件的结束标志。为解决这一个问题，ANSI C提供一个feof函数，用来判断文件是否结束。feof函数既可用以判断二进制文件又可用以判断文本文件。
   ```

   函数声明:

   ```c
   #include <stdio.h>
   int feof(FILE * stream);
   功能：检测是否读取到了文件结尾。判断的是最后一次“读操作的内容”，不是当前位置内容。
   参数：
   	stream：文件指针
   返回值：
   	非0值：已经到文件结尾
   	0：没有到文件结尾
   ```

案例代码:

```c
#if 1
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

// 1. 字符文件读写  fputc fgetc
void test01()
{
	// 1. 文件打开
	FILE *fp = fopen("./demo2.txt", "r");
	if (NULL == fp)
	{
		printf("文件打开失败!\n");
		return;
	}


	// 2. 读取文件内容
#if 0
	char c = fgetc(fp);
	printf("c = %c\n", c);

	c = fgetc(fp);
	printf("c = %c\n", c);

	c = fgetc(fp);
	printf("c = %c\n", c);

	// 文本文件的结尾符号就是 -1
	c = fgetc(fp);
	printf("c = %d\n", c);
#endif

	char ch = 0;
	// end of file
	while ((ch = fgetc(fp)) != EOF)
	{
		printf("%c", ch);
	}


	// 文件关闭
	fclose(fp);
	fp = NULL;

}

void test02()
{
	FILE *fp = fopen("./demo2.txt", "r");
	if (NULL == fp)
	{
		printf("文件打开失败!\n");
		return;
	}


	// 2. 读取文件内容
	char contents[128] = { 0 };
	int index = 0;
	while ((contents[index] = fgetc(fp)) != EOF)
	{
		++index;
	}
	
	// 3. 
	printf("contents = %s\n", contents);


	// 文件关闭
	fclose(fp);
	fp = NULL;
}

void test03()
{
	// w 以写方式打开文件(如果文件存在则清空文件，文件不存在则创建一个文件)
	FILE *fp = fopen("./demo3.txt", "w");
	if (NULL == fp)
	{
		printf("文件打开失败!\n");
		return;
	}


	char *s = "hello world";
	// 写入文件内容 fputc
	for (int i = 0; i < strlen(s); ++i)
	{
		fputc(s[i], fp);
	}

	// 关闭文件
	fclose(fp);
	fp = NULL;

	printf("文件写入成功!\n");
}


// 2. EOF和feof函数
// feof 适用于文本文件、二进制文件(先读再判断是否结束)
// EOF 适用于文本文件
void testp4()
{
	FILE *fp = fopen("./demo2.txt", "r");
	if (NULL == fp)
	{
		printf("文件打开失败!\n");
		return;
	}


	// 2. 读取文件内容
	char ch = 0;
	// "abc[-1]"
	while (1)
	{
		ch = fgetc(fp);
		// 如果 feof 函数返回 true, 表示文件结束
		if(feof(fp))
		{
			break;
		}

		printf("%c %d\n", ch, ch);
	}


	// 文件关闭
	fclose(fp);
	fp = NULL;
}


int main()
{
	testp4();

	system("pause");
	return EXIT_SUCCESS;
}
#endif
```



## 4. 实现文件拷贝

实现思路:

1. 输入待拷贝的文件路径.
2. 输入拷贝的目的地路径.
3. 打开两个文件:
   1. 待拷贝文件: 以读的方式打开
   2. 目的地文件: 以写的方式打开
4. 文件拷贝
   1. 从待拷贝文件中读取一个字符
   2. 将该字符写入到目的地文件
5. 关闭两个文件。

```c
void test()
{
	// 1. 获得文件路径
	char file_src[128] = { 0 };
	char file_dst[128] = { 0 };

	printf("请输入待拷贝文件路径:");
	scanf("%s", file_src);

	printf("请输入目的地文件路径:");
	scanf("%s", file_dst);


	// 2. 打开文件
	FILE *fp_read = fopen(file_src, "r");
	if (NULL == fp_read)
	{
		printf("文件: %s 打开失败!\n", file_src);
		return;
	}

	FILE * fp_write = fopen(file_dst, "w");
	if (NULL == fp_write)
	{
		// 关闭前面打开的文件
		fclose(fp_read);
		fp_read = NULL;

		printf("文件: %s 打开失败!\n", file_dst);
		return;
	}

	// 3. 拷贝实现
	char ch = 0;
	// 从待拷贝文件中读取内容
	while ((ch = fgetc(fp_read)) != EOF)
	{
		// 将读取的内容写入到目的地文件中
		fputc(ch, fp_write);
	}


	// 4. 关闭文件
	fclose(fp_read);
	fp_read = NULL;

	fclose(fp_write);
	fp_write = NULL;


	printf("%s 拷贝到 %s 成功!\n", file_src, file_dst);

}
```



## 5. 行文件读写

1. fputs

   ```c
   #include <stdio.h>
   int fputs(const char * str, FILE * stream);
   功能：将str所指定的字符串写入到stream指定的文件中，字符串结束符 '\0'  不写入文件。 
   参数：
   	str：字符串
   	stream：文件指针
   返回值：
   	成功：0
   	失败：-1
   ```

2. fgets

   ```c
   #include <stdio.h>
   char * fgets(char * str, int size, FILE * stream);
   功能：从stream指定的文件内读入字符，保存到str所指定的内存空间，直到出现换行字符、读到文件结尾或是已读了size - 1个字符为止，最后会自动加上字符 '\0' 作为字符串结束。
   参数：
   	str：字符串
   	size：指定最大读取字符串的长度（size - 1）
   	stream：文件指针
   返回值：
   	成功：成功读取的字符串
   	读到文件尾或出错： NULL
   ```

3. 案例

   ```c
   #define _CRT_SECURE_NO_WARNINGS
   #include<stdio.h>
   #include<string.h>
   #include<stdlib.h>
   
   // 1. 把内容写入到文件中
   void test01()
   {
   	// 1. 打开文件
   	FILE *fp = fopen("./demo6.txt", "w");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// 2. 写文件
   #if 0
   	char *s = "hello world";
   	// 从 s 字符串第一个字符开始到 \0 之前的内容写入到文件中
   	fputs(s, fp);
   #endif
   
   	// 字符串指针数组
   	char *name[] = { "Obama\n", "Trump\n", "Cliton\n", "Bush\n", "Polly\n", "Smith\n" };
   	for (int i = 0; i < sizeof(name)/ sizeof(name[0]); ++i)
   	{
   		fputs(name[i], fp);
   	}
   
   
   	// 3. 关闭文件
   	fclose(fp);
   	fp = NULL;
   }
   
   
   // 2. 从文件中读取出来
   void test02()
   {
   	// 1. 打开文件
   	FILE *fp = fopen("./demo6.txt", "r");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// 2. 读文件
   	while(1)
   	{
   		// 申请空间，用于存储文件内容
   		char line[128] = { 0 };
   		// 从文件中一次读取一行
   		fgets(line, 128, fp);
   		// 判断文件是否结束
   		if (feof(fp))
   		{
   			break;
   		}
   		// 打印文件内容
   		printf("%s", line);
   	
   	}
   
   	// 3. 关闭文件
   	fclose(fp);
   	fp = NULL;
   }
   
   
   // 统计文件 demo6.txt 文件中有多少行
   int file_count()
   {
   	// 1. 打开文件
   	FILE *fp = fopen("./demo6.txt", "r");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// 2. 读文件
   	int cnt = 0;  // 记录文件行数
   
   	while (1)
   	{
   		// 申请空间，用于存储文件内容
   		char line[128] = { 0 };
   		// 从文件中一次读取一行
   		fgets(line, 128, fp);
   		// 判断文件是否结束
   		if (feof(fp))
   		{
   			break;
   		}
   		// 累加文件行数
   		++cnt;
   
   	}
   
   	// 3. 关闭文件
   	fclose(fp);
   	fp = NULL;
   
   	// 返回文件行数
   	return cnt;
   }
   
   
   void test03()
   {
   	// 1. 获取文件行数
   	int line_cnt = file_count();
   	printf("line_cnt = %d\n", line_cnt);
   
   	// 2. 根据文件行数开辟字符串指针数组
   	char **lines = malloc(sizeof(char*) * line_cnt);
   
   	// 3. 再次从头按行读取文件内容，根据内容长度开辟空间，并存储内容
   	FILE *fp = fopen("./demo6.txt", "r");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// 读取文件内容
   	int idx = 0;
   	while (1)
   	{
   		char content[128] = { 0 };
   		// 读取每一行数据
   		fgets(content, 128, fp);
   		if (feof(fp))
   		{
   			break;
   		}
   		// 计算当前字符串的长度
   		int len = strlen(content) + 1;
   		// 给字符串指针开辟空间
   		lines[idx] = malloc(len);
   		// 将 content 中的数据拷贝到新开辟的堆空间中
   		strcpy(lines[idx], content);
   		// 累加下标
   		++idx;
   	}
   
   	fclose(fp);
   	fp = NULL;
   
   	// 4. 打印文件内容
   	for (int i = 0; i < line_cnt; ++i)
   	{
   		printf("%s", lines[i]);
   	}
   
   
   	// 5. 释放堆内存空间
   	for (int i = 0; i < line_cnt; ++i)
   	{
   		if (lines[i] != NULL)
   		{
   			free(lines[i]);
   			lines[i] = NULL;
   		}
   	}
   
   	free(lines);
   	lines = NULL;
   }
   ```



案例:

```c
// 1. strtok
void test01()
{
	char s[] = "aa#bb#cc";

	// 1. 第一次传入第一个参数 char* 字符串
	char *p = strtok(s, "#");
	printf("p = %s\n", p);

	if (s[2] == '\0')
	{
		printf("第一个#号被替换成了\\0\n");
	}

	// 2. 第二个以后就不需要传入，第一个参数写 NULL
	p = strtok(NULL, "#");
	printf("p = %s\n", p);

	if (s[5] == '\0')
	{
		printf("第一个#号被替换成了\\0\n");
	}

	p = strtok(NULL, "#");
	printf("p = %s\n", p);

	p = strtok(NULL, "#");
	printf("p = %s\n", p);

	// 注意: strtok 所分割的字符串必须可写
}

// 2. strchr strstr 
// strchr 在字符串查找指定字符，返回第一个匹配的字符地址. 查找失败返回 NULL
// strstr 在字符串中查找子串，返回子串首字符的地址，查找失败返回NULL
void test02()
{
	char *s = "abcdef";

	// 如果找到，返回该字符的地址
	// 如果找不到，返回空指针
	char *pos = strchr(s, 'g');
	if (pos != NULL)
	{
		printf("pos = %c\n", *pos);
	}
	else
	{
		printf("找不到!\n");
	}
}

void test03()
{
	char *s = "abcdefg";

	char *pos = strstr(s, "def");

	// 保存子串的内存
	char buf[32] = { 0 };

	if (pos != NULL)
	{
		printf("pos = %s\n", pos);
		// 从字符串中将查找到的子串，扣出来
		strncpy(buf, pos, 3);
	}
	else
	{
		printf("查找失败!\n");
	}

	printf("buf = %s\n", buf);

}


void test04()
{
	FILE *fp = fopen("./demo7.txt", "r");
	if (NULL == fp)
	{
		return;
	}


	// 1. 按行读内容 10+20
	while (1)
	{
		char line[64] = { 0 };
		fgets(line, 64, fp);
		// 判断是否文件结束
		if (feof(fp))
		{
			break;
		}
		// 2. 查找+号的位置
		char *p = strchr(line, '+');

		// 3. +号左侧的内容抠出来，就是左操作数
		char left[32] = { 0 };
		// 在数组中，两个指针相减可以获得他们之间相差元素的个数
		strncpy(left, line, p - line);
		
		// 4. +号右面的内容抠出来，就是右操作数
		char right[32] = { 0 };
		// 拷贝完的字符串，最后一个字符是\n
		// 需要将最有一个字符 \n 换成 \0
		strcpy(right, p + 1);
		// 123\n\0
		right[strlen(right) - 1] = '\0';

		// 5. 使用 atoi 函数，将字符串转换成数字
		int l = atoi(left);
		int r = atoi(right);

		// 6. 计算并输出结果
		printf("%d+%d=%d\n", l, r, l + r);
	}


	fclose(fp);
	fp = NULL;
}
```



## 6. 格式化文件读写

printf sprintf fprintf

1. fprintf

   ```c
   #include <stdio.h>
   int fprintf(FILE * stream, const char * format, ...);
   功能：根据参数format字符串来转换并格式化数据，然后将结果输出到stream指定的文件中，指定出现字符串结束符 '\0'  为止。
   参数：
   	stream：已经打开的文件
   	format：字符串格式，用法和printf()一样
   返回值：
   	成功：实际写入文件的字符个数
   	失败：-1
   ```

2. fscanf

   ```c
   #include <stdio.h>
   int fscanf(FILE * stream, const char * format, ...);
   功能：从输入流(stream)中读入数据，存储到 argument 中，遇到空格和换行时结束。
   参数：
   	stream：已经打开的文件
   	format：字符串格式，用法和scanf()一样
   返回值：
   	成功：参数数目，成功转换的值的个数
   	失败： - 1
   ```

3. 案例代码

   ```c
   void test01()
   {
   	FILE *fp = fopen("./demo8.txt", "w");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   
   	fprintf(fp, "%d+%d\n", 10, 20);
   	fprintf(fp, "%d+%d\n", 20, 30);
   	fprintf(fp, "%d+%d\n", 30, 40);
   	fprintf(fp, "%d+%d\n", 40, 50);
   	fprintf(fp, "%d+%d\n", 50, 60);
   
   
   
   	fclose(fp);
   	fp = NULL;
   }
   
   void test02()
   {
   	FILE *fp = fopen("./demo8.txt", "r");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// fscanf 碰到换行符或者空格就会终止匹配
   	int a = 0;
   	int b = 0;
   
   	while (1)
   	{
   		fscanf(fp, "%d+%d", &a, &b);
   		if (feof(fp))
   		{
   			break;
   		}
   		
   		printf("%d+%d=%d\n", a, b, a + b);
   	}
   
   	fclose(fp);
   }
   ```

   





## 7. 块文件读写

1. fwrite

   ```c
   #include <stdio.h>
   size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
   功能：以数据块的方式给文件写入内容
   参数：
   	ptr：准备写入文件数据的地址
   	size： size_t 为 unsigned int类型，此参数指定写入文件内容的块数据大小
   	nmemb：写入文件的块数，写入文件数据总大小为：size * nmemb
   	stream：已经打开的文件指针
   返回值：
   	成功：实际成功写入文件数据的块数目，此值和nmemb相等
   	失败：0
   ```

2. fread

   ```c
   #include <stdio.h>
   size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
   功能：以数据块的方式从文件中读取内容
   参数：
   	ptr：存放读取出来数据的内存空间
   	size： size_t 为 unsigned int类型，此参数指定读取文件内容的块数据大小
   	nmemb：读取文件的块数，读取文件数据总大小为：size * nmemb
   	stream：已经打开的文件指针
   返回值：
   	成功：实际成功读取到内容的块数，如果此值比nmemb小，但大于0，说明读到文件的结尾。
   	失败：0
   ```

3. 案例：结构体数据读写文件

   **单个结构体变量读写:**

   ```c
   struct Person
   {
   	char name[32];
   	int age;
   };
   
   void test01()
   {
   	// 创建了结构体变量
   	struct Person person = { "Obama", 50 };
   	// 数据写入到文件中，都是二进制
   	// 变量在内存中都是以二进制的方式存储
   	// 将变量在内存中的字节，一个一个的写到文件中
   
   	FILE *fp = fopen("./demo9.txt", "wb");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// fputc fputs fprintf 一般都用于文本文件读写
   	// fread fwrite 一般用于二进制文件读写
   	fwrite(&person, sizeof(struct Person), 1, fp);
   
   
   	fclose(fp);
   	fp = NULL;
   }
   
   void test02()
   {
   	FILE *fp = fopen("./demo9.txt", "rb");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   
   	// 申请Person大小的空间
   	struct Person p = {0};
   	printf("Name:%s Age:%d\n", p.name, p.age);
   	fread(&p, sizeof(struct Person), 1, fp);
   	printf("Name:%s Age:%d\n", p.name, p.age);
   
   	fclose(fp);
   	fp = NULL;
   }
   ```

   **结构体数组读写:**

   ```c
   struct Person
   {
   	char name[32];
   	int age;
   };
   
   void test03()
   {
   	// 创建Person数组，并且将数组存储到文件中
   	struct Person ps[] = {
   		{"Obama", 99},
   		{"Trump", 89},
   		{"Clinton", 100}
   	};
   
   	FILE *fp = fopen("./demo10.txt", "wb");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// 将数组写入到文件中
   	fwrite(ps, sizeof(struct Person), 3, fp);
   
   
   	fclose(fp);
   	fp = NULL;
   }
   
   void test04()
   {
   	FILE *fp = fopen("./demo10.txt", "rb");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// 从文件中读取结构体数组
   	struct Person ps[3];
   	fread(ps, sizeof(struct Person), 3, fp);
   	// 打印内容
   	for (int i =0; i < 3; ++i)
   	{
   		printf("Name:%s Age:%d\n", ps[i].name, ps[i].age);
   	}
   
   	fclose(fp);
   	fp = NULL;
   }
   ```

   

## 8. 随机文件读写

1. fseek

   ```c
   #include <stdio.h>
   int fseek(FILE *stream, long offset, int whence);
   功能：移动文件流（文件光标）的读写位置。
   参数：
   	stream：已经打开的文件指针
   	offset：根据whence来移动的位移数（偏移量），可以是正数，也可以负数，如果正数，则相对于whence往右移动，如果是负数，则相对于whence往左移动。如果向前移动的字节数超过了文件开头则出错返回，如果向后移动的字节数超过了文件末尾，再次写入时将增大文件尺寸。
   	whence：其取值如下：
   		SEEK_SET：从文件开头移动offset个字节
   		SEEK_CUR：从当前位置移动offset个字节
   		SEEK_END：从文件末尾移动offset个字节
   返回值：
   	成功：0
   	失败：-1
   ```

   fseek(fp, -10, SEEK_END);

2. ftell

   ```c
   #include <stdio.h>
   long ftell(FILE *stream);
   功能：获取文件流（文件光标）的读写位置。
   参数：
   	stream：已经打开的文件指针
   返回值：
   	成功：当前文件流（文件光标）的读写位置
   	失败：-1
   ```

3. rewind

   ```c
   #include <stdio.h>
   void rewind(FILE *stream);
   功能：把文件流（文件光标）的读写位置移动到文件开头。
   参数：
   	stream：已经打开的文件指针
   返回值：
   	无返回值
   ```

4. 案例：随机读写结构体

   ```c
   struct Person
   {
   	char name[32];
   	int age;
   };
   
   void test01()
   {
   	// 创建Person数组，并且将数组存储到文件中
   	struct Person ps[] = {
   		{ "Obama", 99 },
   		{ "Trump", 89 },
   		{ "Clinton", 100 }
   	};
   
   	FILE *fp = fopen("./demo11.txt", "wb");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// 将数组写入到文件中
   	fwrite(ps, sizeof(struct Person), 3, fp);
   
   
   	fclose(fp);
   	fp = NULL;
   }
   
   
   void test02()
   {
   	FILE *fp = fopen("./demo11.txt", "rb");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// 1. 移动指针到第二个 Person 数据首地址
   	long pos = ftell(fp);
   	printf("当前文件指针位置:%ld\n", pos);
   
   	fseek(fp, sizeof(struct Person), SEEK_SET);
   	// 创建变量保存数据
   	struct Person person = {0};
   	fread(&person, sizeof(struct Person), 1, fp);
   	printf("Name:%s Age:%d\n", person.name, person.age);
   
   	// 将文件指针从指向 cliton 转回指向 trump
   	int offset = sizeof(struct Person);
   	fseek(fp, -offset, SEEK_CUR);
   	printf("Name:%s Age:%d\n", person.name, person.age);
   
   
   	// 2. 继续读取下一个数据
   	fread(&person, sizeof(struct Person), 1, fp);
   	printf("Name:%s Age:%d\n", person.name, person.age);
   
   	// 3. 可以将文件指针移动到开始位置
   	// fseek(fp, 0, SEEK_SET);
   	rewind(fp);
   	fread(&person, sizeof(struct Person), 1, fp);
   	printf("Name:%s Age:%d\n", person.name, person.age);
   
   
   	fclose(fp);
   	fp = NULL;
   }
   
   
   void test03()
   {
   	// 获得文件大小
   	FILE *fp = fopen("./demo11.txt", "rb");
   	if (NULL == fp)
   	{
   		return;
   	}
   
   	// 将文件指针移动到文件末尾
   	fseek(fp, 0, SEEK_END);
   	long file_size = ftell(fp);
   	printf("文件大小是:%ld %u\n", file_size, sizeof(struct Person));
   
   
   	fclose(fp);
   	fp = NULL;
   
   }
   ```

   

## 9. 文件读写案例-登陆功能实现

1. 文件存储用户名密码
2. 用户输入用户名和密码