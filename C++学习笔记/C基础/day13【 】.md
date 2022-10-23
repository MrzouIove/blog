#  C语言第十二天课程笔记

每一天的笔记包含如下内容:

1. 当天授课内容安排
2. 课堂重点内容笔记
3. 课后思考题



## 1. 内容安排

**第一节课:**   `堆区指针使用注意、goto语句`
**第二节课:**   `电话本(数据结构定义、添加、显示)`
**第三节课:**   `电话本(删除、修改、数据存储)`
**第四节课:**   `函数封装、分文件编写`
**第五节课:**   `课程总回顾`
**第六节课:**   `课程总回顾`



## 2. 堆区指针使用注意

1. 不能 free 指向非堆的内存.
2. 堆指针在释放之前禁止重新赋值.
3. 堆内存释放之后仍然使用堆指针.
4. 堆指针地址修改之后无法再 free.
5. malloc(0) 时，不建议使用该空间

```c
// 1. 不能 free 指向非堆的内存[常犯].
int g_a = 100;
void test01()
{
	// 1. 栈变量
	int a = 10;
	int *p = &a;
	// free(p);

	// 2. 字符串常量
	char *s = "hello world";
	// free(s);

	// 3. 全局变量
	int *gp = &g_a;
	free(gp);
}

// 2. 堆指针在释放之前禁止重新赋值【常犯】.
void test02()
{
	int *p = (int *)malloc(sizeof(int));
	*p = 100;

	p = (int *)malloc(sizeof(int));
}

// 3. 堆内存释放之后仍然使用堆指针[常犯].
void test03()
{
	int *p = (int *)malloc(sizeof(int));
	*p = 100;

	free(p);
	// 内存回收，使用权已经不归我们了，
	// 虽然没有报错，原则不能再使用
	// 既然已经分析出错误，至于结果是什么并不重要
	*p = 200;
	printf("*p = %d\n", *p);
}

// 4. 堆指针地址修改之后无法再 free[常犯].
void test04()
{
	int *p = (int *)malloc(sizeof(int) * 10);

	int *p_temp = p;
	p_temp = p_temp + 1;

	// p = p + 1;
	// free 的时候一定要拿到空间的首地址
	// free 的参数是无类型指针
	// 我们有没有告诉 free 我们的堆空间多大？
	// 编译器在首地址附近会存储堆空间大小
	// 我们虽然申请了40字节，但实际上，申请的内存大于40，可能是44，多出来的4个字节存储堆空间大小.
	free(p);

}

// 5. malloc(0) 时，不建议使用该空间
void test05()
{
	int len = 0;

	int *p = malloc(len);
	if (NULL == p)
	{
		return;
	}

	// *p = 100;  // 没有报错
	// memcpy(p, "hello", 3);

	printf("p = %p\n", p);

	free(p);
}
```



## 3. 电话本数据结构

1. 数据展示：字符界面
2. 业务功能：C语言语法、数据类型
3. 数据存储:  文件



一个项目数据是核心. 数据结构. 电话本：

1. 联系人信息（姓名、电话）.
2. 联系人的数组.  联系人当前的数量.

分析的思路转换为具体代码实现:

```c
#define MAX_CONTACTS 100

// 联系人
struct Contact
{
  char name[32];
  char tele[64];
};


struct Book
{
  // 存储联系人信息
  struct Contact contacts[MAX_CONTACTS];
  // 记录当前电话本中存储了多少个联系人
	int size;
};
```

## 4. 分析业务流程

1. 显示操作菜单
2. 获得用户输入的操作
3. 根据用户输入做相应的操作

```c
#if 1
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

#define MAX_CONTACTS 100

// 操作常量
enum { ADD = 1, REMOVE, EDIT, SEARCH, EXIT };

// 联系人
struct Contact
{
	char name[32];
	char tele[64];
};

struct Book
{
	// 存储联系人信息
	struct Contact contacts[MAX_CONTACTS];
	// 记录当前电话本中存储了多少个联系人
	int size;
};

void test()
{
	// 创建保存电话本信息的变量
	struct Book book;
	memset(&book, 0, sizeof(struct Book));

	/*************电话本开始工作****************/

	while (1)
	{
		// 1. 显示操作菜单
		printf("--------------------------------\n");
		printf("电话本系统v1.0\n");
		printf("--------------------------------\n");
		printf("1. 添加联系人\n");
		printf("2. 删除联系人\n");
		printf("3. 修改联系人\n");
		printf("4. 查找联系人\n");
		printf("5. 退出电话本\n");
		printf("--------------------------------\n");

		// 2. 获得用户输入的操作
		printf("请输入您的操作:");
		int flag = -1;
		// scanf： 返回0表示读取失败，输入了非整数字符. 非0: 正常
		int ret = scanf("%d", &flag);
		if (!ret)
		{
			// 清空缓冲区
			char buf[128] = { 0 };
			fgets(buf, 128, stdin);

			printf("请不要输入非法字符, 有效字符为 1-5.\n");
			continue;
		}

		// 3. 根据用户输入做相应的操作
		switch (flag)
		{
		case ADD:
			// 添加联系人
			system("cls");  // 清屏
			printf("添加联系人!\n");
			break;
		case REMOVE:
			// 删除联系人
			system("cls");  // 清屏
			printf("删除联系人!\n");
			break;
		case EDIT:
			// 修改联系人
			system("cls");  // 清屏
			printf("修改联系人!\n");
			break;
		case SEARCH:
			// 查找联系人
			system("cls");  // 清屏
			printf("查找联系人!\n");
			break;
		case EXIT:
			// 退出系统
			system("cls");  // 清屏
			printf("欢迎再次使用《电话本v1.0》!\n");
			system("pause");  // 等待
			exit(0);
			break;

		default:
			printf("您输入的操作暂不支持, 请期待下一个v1.1版本!\n");
			break;
		}

	}

}

int main()
{
	test();

	system("pause");
	return EXIT_SUCCESS;
}
#endif
```



## 5. 添加联系人

1. 获得用户输入的联系人姓名、联系人电话
2. 将获得的数据存储 size 下标的位置
3. size 累加 1 个
4. 提示添加成功



```c
			/*1. 获得用户输入的联系人姓名、联系人电话*/
			printf("请输入联系人姓名:");
			char name[32] = { 0 };
			scanf("%s", name);
			printf("请输入联系人电话:");
			char tele[64] = { 0 };
			scanf("%s", tele);

			/*2. 将获得的数据存储 size 下标的位置*/
			strcpy(book.contacts[book.size].name, name);
			strcpy(book.contacts[book.size].tele, tele);

			/*3.  size 累加 1 个*/
			++book.size;

			/*4. 提示添加成功*/
			printf("联系人【%s】添加成功!\n", name);
```



## 6. 显示联系人

1. 遍历 book.contacts 数组
2. 遍历不需要遍历所有，只需要遍历 size 个即可.

```c
			system("cls");  // 清屏
			printf("--------------------------------\n");
			printf("姓名\t电话\n");
			printf("--------------------------------\n");
			for (int i = 0; i < book.size; ++i)
			{
				printf("%s\t%s\n", book.contacts[i].name, book.contacts[i].tele);
			}
			printf("--------------------------------\n");
```



## 7. 修改联系人

1. 输入要修改的联系人姓名
2. 通过姓名找到联系人的结构体

3. 输入修改之后的新的电话号码
4. 新的电话号码重新复制到对应联系人的 tele 字符串里

```c
		/*1. 输入要修改的联系人姓名*/
			printf("请输入要修改的联系人姓名:");
			char edit_name[32] = { 0 };
			scanf("%s", edit_name);

			/*2. 通过姓名找到联系人的结构体*/
			int flag = -1; // 假设联系人不存在
			for (int i = 0; i < book.size; ++i)
			{
				if (0 == strcmp(book.contacts[i].name, edit_name))
				{
					flag = 0;

					/*3. 输入修改之后的新的电话号码*/
					printf("您原来的电话号码是[%s], 您要修改为:", book.contacts[i].tele);
					char new_tele[64] = { 0 };
					scanf("%s", new_tele);

					/*4. 新的电话号码重新复制到对应联系人的 tele 字符串里*/
					strcpy(book.contacts[i].tele, new_tele);

					printf("联系人[%s]的电话号码修改成功!\n", book.contacts[i].name);
					break;
				}
			}

			// 判断联系人是否存在
			if (flag == -1)
			{
				printf("联系人[%s]信息不存在!\n", edit_name);
			}
```



## 8. 删除联系人

1. 输入要删除的联系人
2. 根据联系人姓名查找该联系人的信息
3. 要删除联系人.（将要删除元素后面的所有元素向前移动一个位置）

```c
		/*1. 输入要删除的联系人*/
			printf("请输入您要删除的联系人姓名:");
			char del_name[32] = { 0 };
			scanf("%s", del_name);

			/*2. 根据联系人姓名查找该联系人的信息*/
			int del_idx = -1;  // 记录要删除联系人的下标
			for (int i = 0; i < book.size; ++i)
			{
				if (strcmp(book.contacts[i].name, del_name) == 0)
				{
					del_idx = i;
					break;
				}
			}

			/*3. 要删除联系人.*/
			if (-1 == del_idx)
			{
				printf("您要删除的联系人[%s]不存在!\n", del_name);
			}
			else
			{
				for (int i = del_idx; i < book.size - 1; ++i)
				{
					// 将要删除元素后面的所有元素向前移动一个位置
					book.contacts[i] = book.contacts[i + 1];
				}
			}

			/*4. 更新联系人数目*/
			book.size--;
			
			printf("联系人[%s]已被删除!\n", del_name);
```



## 9. 查找联系人

1. 获得要查找联系人的姓名
2. 遍历数组，将联系人的电话打印出来

```c
	/*
			1. 获得要查找联系人的姓名
			2. 遍历数组，将联系人的电话打印出来
			*/

			printf("请输入要查找的联系人姓名:");
			char search_name[32] = { 0 };
			scanf("%s", search_name);

			int search_flag = -1;
			for (int i = 0; i < book.size; ++i)
			{
				if (strcmp(book.contacts[i].name, search_name) == 0)
				{
					printf("您查找的联系人[%s]的电话号码是:%s\n", search_name, book.contacts[i].tele);
					search_flag = 0;
					break;
				}
			}

			if (-1 == search_flag)
			{
				printf("您要查找的联系人[%s]不存在!\n", search_name);
			}
```



## 10. 退出电话本

1. 先要保存数据到文件: `contacts.data`
2. 退出系统

**系统启动时加载数据:**

```c
FILE *fp = fopen(file_path, "rb");
	if (NULL == fp)
	{
		printf("加载数据时, 文件[%s]打开失败!\n", file_path);
		return;
	}

	fread(&book, sizeof(struct Book), 1, fp);

	fclose(fp);
	fp = NULL;
```

**系统退出时存储数据:**

```c
			/*
			1. 先要保存数据到文件: `contacts.data`
			2. 退出系统
			*/

			FILE *fp = fopen(file_path, "wb");
			if (NULL == fp)
			{
				printf("文件[%s]打开失败!\n", file_path);
				return;
			}

			// 将联系人数据写入到文件中
			fwrite(&book, sizeof(struct Book), 1, fp);

			fclose(fp);
			fp = NULL;



			printf("欢迎再次使用《电话本v1.0》!\n");
			system("pause");  // 等待
			exit(0);
```



