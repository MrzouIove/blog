#  C语言第四天课程笔记

每一天的笔记包含如下内容:

1. 当天授课内容安排
2. 课堂重点内容笔记
3. 课后思考题



## 1. 内容安排

**第一节课:**   `数据强化训练`
**第二节课:**   `if分支(语法、条件编写、if分支训练)`
**第三节课:**   `if..else 分支、if...else if...else if...else 分支、if嵌套`
**第四节课:**   `三目运算符(条件运算符)`
**第五节课:**   `switch 语句`
**第六节课:**   `分支强化训练和总结结转到下一天`



## 2. 课堂笔记

课堂主要内容、重要内容。

#### 2.1 数据强化训练

1. 对下面的各种数据使用合适的数据类型?

   a. 保存北京的人口  unsigned long long、unsigned int、unsigned long

   b. 保存员工的工资 double、float

   c. 保存当前文档中出现次数最多的字母  char

   d. 保存当前文档中某个字母的出现次数  short int long

2. 说出下列值的类型?

   a. '\t'  char

   b. 1088 int short long

   c. 88.66  float、double

   d. 0xAA  int、long、long long

   e. 3.14f float

   f. 2.0e+30 double

3. 请说出下列程序中有哪些错误?

   ```c
   include <stdio.h>
   
   main()
   (
     float g; h;
     float tax, rate;
     g = e12;
     tax = rate * g;
   )
   ```

   1. 第一行： `include` -> `#include`

   2. 第三行: `main` ->`int main(void)`、`int main(int argc, char *argv[])`

   3. 第四行、第九航: `()`  -> `{}` 

   4. 第五行: `float g; h;` -> `float g, h;`

   5. 第七行: `e12` -> e前面必须要有数,例如 `3e12`

   6. 第八航: `rate` 没有初始化。要不编译器报错、要不计算错误。

       

1. 指出表中各个常量的数据类型，以及在 printf 输出中的占位符:

   | 常量    | 说明符      | 类型                  |
   | ------- | ----------- | --------------------- |
   | 12      | %hd         | short                 |
   | 0x3     | %d、%x、%o  | int                   |
   | 'B'     | %c          | char                  |
   | 2.34e07 | %lf         | double                |
   | '\040'  | %c          | char                  |
   | 7.0     | %lf         | double                |
   | 6L      | %ld         | long                  |
   | 6.0f    | %f          | float                 |
   | 012     | %hd、%hu    | short、unsigned short |
   | 's'     | %c          | char                  |
   | 100000  | %u          | unsigned int          |
   | '\n'    | %c          | char                  |
   | 20.0f   | %f          | float                 |
   | 0x44    | %hd、%x、%o | short                 |
   | 2.7e-20 | %lf         | double                |

5. 假设 ch 为 char 类型变量，使用转义字符、十进制、八进制、十六进制等方式将其赋值为换行符。

   ```c
   void test01()
   {
   	char c1 = '\n';  // 回车\r 换行\n
   	char c2 = 10;
   	char c3 = 012;
   	char c4 = 0xa;
   
   	printf("1%c2%c3%c4%c", c1, c2, c3, c4);
   }
   ```

   

3. 说出下列转义字符的含义:

   a. \\r

   b. \\\

   c. \\*

   d. \\t

   e. %% 

7. 编写程序，要求输入一个 ASCII 码值(如：66), 输出相应字符。

   ```c
   void test02()
   {
   	// 1. 输入的一个值
   	int ch_num = 0;
   	printf("请输入一个字符的ascii码值:");
   	scanf("%d", &ch_num);
   
   	// 2. 打印该字符
   	printf("ch = %c", ch_num);
   }
   ```

   

8. 编写程序，发出警报声，并打印下列文字：

   ```c
   警报声已经响起，请自觉遵守规则.
   ```

   ```c
   void test03()
   {
   	printf("\a");
   	printf("警报声已经响起，请自觉遵守规则.\n");
   }
   ```

   

9. 编写程序，输入一个浮点数，并分别以小数形式和指数形式打印，输出如同下面格式:

   ```c
   您输入的是: 21.290000，指数形式为: 2.129000e001。
   ```

   ```c
   void test04()
   {
   	float f = 0.0f;
   	printf("请输入一个浮点数:");
   	scanf("%f", &f);
   
   	printf("f = %f\n", f);
   	printf("f = %e\n", f);
   }
   ```

   

10. 一年约有 3.156 x 10<sup>7</sup> 秒, 编写程序，要求输入年龄，然后显示该年龄有多少秒?

   ```c
   void  test05()
   {
   	unsigned int age = 0;
   	printf("请输入年龄:");
   	scanf("%u", &age);
   
   	unsigned int sec_one_year = 3.156e+7;
   	unsigned int total_sec = sec_one_year * age;
   	printf("您的年龄%u, 总共有:%u秒!\n", age, total_sec);
   }
   ```

   

11. 一英寸等于 2.54 厘米, 编写程序，输入您的身高，然后显示身高值等于多少厘米？

    ```c
    void test06()
    {
    	float cm = 2.54f;
    
    	printf("请输入您的身高(单位:英寸):");
    	float height = 0;
    	scanf("%f", &height);
    
    	float total = height *cm;
    	printf("您的身高是:%f cm\n", total);
    }
    ```

    

9. 下面那几个是 C 的关键字：

   a. main  不是，函数名

   b. int  是关键字

   c. function  不是关键字

   d. char  是关键字

   e. =  不是，只是赋值运算符

10. 编写程序，创建一个名为 toes 的整数变量，并设置初始值为 10，计算两个 toes 的和、toes 的平方值，并输出这三个值。

   ```c
   void test07()
   {

   	int toes = 10;
   	int toes_sum = toes * 2;
   
   	// 1. (double)toes 将 int 类型的变量强制转换成 double 类型，传递给 pow 函数
   	// 2. (int)pow((double)toes, 2.0); ->  将 pow 函数执行结果(double类型)强制转换成int类型
   	// int toes_square = (int)pow((double)toes, 2.0);
   	double toes_square = pow((double)toes, 2.0);
   	printf("%d %d %lf\n", toes, toes_sum, toes_square);
   }
   ```
   
   

#### 2.2 if 和 if...else



#### 2.3 if...else if... else if... else 和 if 嵌套



#### 2.4 三目运算符



#### 2.5 switch 语句






















