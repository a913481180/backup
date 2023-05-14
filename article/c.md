---
title: C
date: 2019-01-02 11:33:44
categories:
- study
---
---
***

# C语言

## 一.数据类型

### 常量

- 符号常量  

（预处理命令以#开头)
宏定义：`#define 标识符 常量 //行末不需要加";"`  

```c
#define p1 char* //只是简单替换
typedef char* p2; //对类型说明符重新命名
p1 i,j; //等价于char *i,j;
p2 i,j; //等价于char *i,*j;  
```

>符号常量的标识符通常用大写，变量标识符用小写  
>符号常量与变量不同，其值在作用域内不能改变和赋值  

带参宏定义：`#define M(y) y*y+1 //宏名与形参之间不能有空格，如：#defind MAX (a,b) a+b`  
调用： `k=M(30);`

条件编译：  

```c
#ifdef 标示符   //如果宏定义了标示符则执行。。。。
程序段1
#else
程序段2
#endif
----------------
#ifndef 标示符 //如果没有定义标示符则执行.....
程序段1
#else
程序段2
#endif
------------------------------
#if 常量表达式
程序段1
#else
程序段2
#endif
```

- 整型常量  
 （程序中是根据前缀来区别各种进制数的）

1. 八进制整型常数：必须0开头  
如：015（十进制13）、0177777(十进制65535）  
2. 十六进制：前缀为0X或0x
如：0X2A（十进制42）、OXFFFF（十进制65535）  
3. 十进制： 无前缀  
**在十六位字长的机器上(short)，整型长度为16位，所以数的范围是有限的。十进制无符号整型常数的范围为0-65535（2^16）；有符号整型常数为-32768~+32767**

- 实型常量  
标准C允许浮点数使用“f或F”作后缀，如：32f等价于32.

1. 十进制数：必须有小数点  
如：`0.0`、`300.`、`25.0`、`-2.30`

2. 指数形式：十进制数+阶码“e、E”组成  
如：2.1E5=2.1*(10^5)  

>**不合法的实数：**  
无小数点或阶码标志：32  
阶码标志前无数字：E6  
负号位置不对：3.-E3  
无阶码：2.3E  

实数数据在内存中的存放形式：  
(一般占4个字节)  
`符号(1位)+小数部分(7位)+指数部分(24位)`

- 字符常量

只能用单引号括起来的一个字符，不能参与运算  
转义字符“\”  
>\n:回车换行  
\t:横向调到下一制表符  
\b:退格
\r:回车  
\f:换页  

ASCII表：  
>大写字母:65-90  
小写字母：97-122

- 字符串常量  
（双引号括起来）  
不能把字符串常量赋给字符变量：可以char a='a'而不能char a="a";  
字符串常量所占内存为字符数加1，增加一个字节用于存放字符串结尾标志“\o”

### 变量

- 整型常量  

>内存中以2进制储存  
>数值以补码表示的：  

- 正数的补码与原码相同
- 负数的补码：取反再加1  

**整型变量分类**  
整型变量所占字节与系统和编译器有关  

>1. 基本型： int 4字节  
 unsigned int(无符号) 4字节  

2. 长整型： long 8字节
3. 短整型： short 2 字节  

- 实型变量

单精度：float  4字节  
双精度： double 8字节
长双精度： long double  

>double b;  
>b=1.0/3\*3=1.000000
b=1/3\*3=0.000000  
b=3/2=1.000000  

- 数据类型转换  
  1. 自动转换  
转换按数据长度增加的方向进行  

  2. 强制类型转换：

```c
(float)a  //把a转换为实型  
(int)(x+Y) /把x+y的结果转换为整型
```

### 算术运算  

>1. 除法运算符“/”：双目运算具有左结合性，若参与运算量均为整型则结果为整型，舍去小数；若有一个为实型，则结果为双精度实型。  
>2. 求模运算符“%”:求余数  
>3. 自增、自减运算符：i++先参与运算再减1  
>4. 赋值运算符：` a+=5为a=a+5、x*=y+3为x=a*(y+3) ` `x=y=z: x=(y=z)`
>5. 逗号运算符：将多个表达式链接起来，并把最后一个作为整个逗号表达式的值,如`y=(e+4),(s+3),(d+s)等价于y=d+s`
>6. 逻辑运算符：&&、||、！结果为真或假
>7. 关系运算符： < == <= != 结果为真或假，优先级被逻辑运算高  
>8. 条件运算符："？"和" ："
`表达式1 ? 表达式2 ： 表达式3  //若表达式1的值为真，则以表达式2的值作为条件表达式的值。若表达式1为假则执行表达式3`

## 语句

错误语句：  
if((x=y+2;)>0)z=j;应为if((x=y+2)>0)z=j;  

- putchar(字符输出函数）  

```c
putchar('a');  //输出字母a  
putchar(a); //输出变量a的值  
putchar('\n'); //回车  
```

- getchar(键盘输入函数）  

```c
char c;  
c=getchar();
```

- printf(格式输出函数）  

```c
printf("格式控制字符串",输出列表);  
%d--十进制整型输出  
%u--十进制无符号整数
%ld--十进制长整型
%f--小数形式
%c--字符型  
%s-- 字符串
%e,E--指数形式
%o--八进制无符号
%x,X--十六进制无符号
```

- scanf(格式化输入函数)

```c
scanf("格式控制字符",地址表列);  //地址是由地址运算符“&”后缀变量名组成的
&a表示变量a的地址
输入多个数据时可用空格、tab、回车作间隔，c编译时碰到这些即认为数据结束
若格式控制串中无非格式字符，则所有输入有效
scanf("%c%c%c",&a,&b,&c);
若在格式控制串中加入空格作为间隔，则输入数据间可加入空格
scanf("%c %c %c",&a,&b,&c);

scanf("%5.2f",&a);错误语法，scanf无精度控制
```

- if语句  

```c
if(**){xxxxx}
else if (**) {xxxxxxxxx}
else if (**) {xxxxxxxxx}
else{xxxxxxxxx}
```

- switch语句

```c
switch(a){
case 常量表达式： 语句;
 语句;
 语句;
break;
case 常量表达式： 语句;
break;
default: 语句;
}  
```

- goto 语句

```c
goto 标识符;  
--------------
loop: if(**)
{  
xxxx   
xxx
goto loop;
}   
```

- do-while语句

```c
do   
//先循环再判断
while(**);  
```

- for语句

```c
for(表达式1;表达式2;表达式3){

//表达式1，2，3都可以省略，但“:”不能缺少
//省略表达式2会进入死循环

}   
```

### 数组

- 一位数组  
例如： `int a[10];`  
c语言不允许对数组的大小作动态定义  
数组的下表从0开始。
  - 引用：数组名[下标]，如：a[5]=a[1++]  
  - 赋值:`int a[10]={1,1,2,3};`
    `int a[]={1,2,3,4};`

- 二位数组  

如定义为3x4(3行4列）： `int a[3][4];` `int a[][4]={2,2,2,3,4,4..} //可以省略行`
初始化：`int a[3][4]={{2,3},{2.2.2.2},{3}};`或`int a[3][4]={1,2,3,4,5,6,7,8,9,...}`

### 函数

mian中声明函数:`void xxxxx();`  

(如果被调用的函数在主函数之前则可以不用定义）  
定义函数：  

```c
无参函数：void xxxx(){ .....  };  
有参函数：int xxxx(int x,int y)){ .....}  
```

不同函数间传递数据：
1.形式参数与实际参数  
2.return 语句返回计算值  
3.全局变量  

递归调用：直接或间接地调用本身，递归必须有一个退出条件

数组函数作函数参数：  
1.数组元素作函数实参：`xxxx(a[3]);`  
2.数组名作函数参数:实参和形参应一致,数组名作参数传递时传递的是地址

```c
main(){
void test(int b[10]); //可不用定义数组大小
int a[10]={0};
test(a);
return 0;
}
void test(int b[]){
xxx
}
```

全局变量：函数外部定义的，有效范围从定义变量开始，到本源文件结束。使用全局变量在程序运行过程中都占用着储存单元。
局部变量：一个函数内部定义的变量，只在本函数内有效.在复合语句中定义的变量只在复合语句中有效，如for语句,if语句。  

- static声明局部变量：  
函数调用结束后局部变量不消失保留原值，在程序运行的期间占用储存单元不释放。  
- register变量：将局部变量放到cpu的寄存器中，调用时而不用从内存中  取
- extern声明外部变量：即全局变量,在另一个文件中声明外部变量即可调用`extern A;` ,不让其他文件引用可在定义全局变量时加`static` 。定义内部函数不给外部引用也可以加`static`  

### 指针  

指针是个特殊的变量，其存放着地址：

```c
int *P;
p=&i; 
printf("%d\n",*p);

*:取值操作符
&：取址操作符
结合性自右向左：&*p=&i *&i=i
```

定义指针变量必须指定基类型，指针变量只能存放地址。  
引用数组元素：

```c
int a[10];
itn *p;
p=&a[0];
p=a; //数组名为数组第一个元素的地址
printf("%d\n",*(p+1));
```

二维数组：  

```c
int a[3][4];
int *(p)[4];
p=a;
a[1]+2=a[1][2] 
*(*(a+1)+2)=a[1][2]
```

字符串：  

```c
char *a="what the fuck";
// 等价于
char *a;
a="what the fuck";

// 可用下标引用指针变量所指的字符串
printf("%c\n",a[1]);
// ---------
char str[20]={what are you doning";
// 不等价于
char srt[20];
srt[]="what are you doing";或
srt="what are you doing ";
只能scanf("%s",str);
```

函数：  

```c
int max(int ,int);
int a,c,d;
int (*p)();
p=max;
a=(*p)(c,d);
```

指针数组：  

```c
int a[3]={1.2.3.4};
int *name[3]={&a[0],&a[2],&a[3],&a[1];
printf("%d ",*name[1]);
```

总结：  

```c
int a[n];
int *p[n]; //指针数组,包含n个指针元素
int (*p)[n]; //p为指向n个元素的一位数组的指针变量
int f();
int *p(); //p为带回一个指针的函数，该指针只想整形数据
int (*p)(); //p为指向函数的指针，该函数返回一个整型值1
const int *p  //常量指针不可被修改
void *P  //可转换为其他类型
```

## 结构体与共用体

- 定义一个结构体：  

```c
struct 结构名(也是可选标记名)

{
成语列表
};
 
//直接创建结构体变量时可省略结构体名，但这样只能用一次
struct {
char title[20];
int num;
}book;

//或者用typedef重新命名时，可省略结构体名
typedef struct{
char title[20];
int num;
}book;

book book1,book2,book3;


//定义结构体变量：
struct student student1,student2;
// 或着
struct student
{
// 成员列表
} student1,student2;

// -------------------
// 结构体变量的引用：  
// 结构体变量名.成员名  
// 如：student1.num=100
// 结构体之间可以复制：student1=student2;
```

- 结构体初始化：
(与数组初始化类似，用逗号分隔，但使用的花括号括起来)

```c
struct student
{
// 成员列表
} student1,student2={002,"mame","m"};

//以下可以不按顺序初始化
struct student student1={
.num=002,
.name="xiaoming",
.gender="man"
};

///注意以下是不可行的，只有在定义变量的时候直接初始化，之后就不能全体赋值了，只能单个赋值，即student1.name="name";
struct student
{
    // 成员列表
    }student1;
student1={002,"name","man"};
```

- 结构体数组：  

```c
 struct person
{
char name[20];
char phone[20];
};

struct person person1[3]={
{"name",26,"0"},
{"name",28,"1"},
{"name",20,"0"},
};

gets(person1[0].name);
....
```

- 结构体指针：  

指向该结构体的指针：`struct 结构名 *结构指针变量名`

访问形式：  `(*结构指针变量).成员名`或`结构指针变量->成员名`

- 结构体函数：

```c
//结构体
struct person {
xxx
}person1;
//函数
void sss(struct person person1)//person1为形参
{
return person1.num1+person1.num2;
}
//调用
person1.num1=...;
person1.num2=...;
sss(person1);
```

动态储存分配：

`malloc`向系统申请分配指定字节的内存空间

```c
void *malloc (unsigned int size);
void *calloc (unsigned int size);
void free(*P);
```

### 共用体  

定义：  

```c
union 共用体名
{
成员列表
}变量列表;

结构体变量所占内存长度为各成员所占内存之和。共用体所占内存长度等于最长成员所占的内存长度。共用体中的成员，每一瞬时只能存放一种，存放后会覆盖其他成员的值  
```

### 枚举变量  

```c
enum weekday{1,2,3,4,5,6,7};或
enum weekday a,b,v; 或
enum weekday{1,2,3,4,5,6,7}a,b;
使用：
a=1;
b=3;
```

## 文件操作  

文件型指针

```c
FILE *p;
p=fopen(文件名，使用方式); //文件的打开(fopen函数).出错时返回NULL
fclose(文件指针） //成功返回0，失败返回EOF(-1)
FILE f[3]; //文件型数组
```

- 文件的读写：  
字符读写：`fgetc` `futc`  

```c
fputc(字符,文件指针); //成功返回写入的字符，错误返回EOF
ch=fgetc(fp); //以只读或读写形式打开文件
feof(fp); //判断文件是否真的结束，若结束返回1，反之返回0
```

字符串读写：`fgets` `fputs`  

```c
fgets(str,n,fp); //从文件中读取n-1个字符送入字符串中，返回首地址
fputs("xxxx",fp); //把字符串输入到文件中，成功返回0失败返回  
fgets(buffer,len,stdin); //scanf("%s",buffer);不能接受空格.stdin为键盘输入文件
```

数据块的读写：`freed` `fwrite`  

```c
fread(buffer,size,count,fp); //size:要读写的字节数，count:要进行读写多少个size字节的数据线
```

格式化读写：`fscanf` `fprintf`  

```c
fprint(fp,"%d,%f",i,t);
fscanf(fp,"%d,%f",&i,&t);  
```

## 位运算  

- 位运算符  

>运算量只能是整型或字符型，不能为实型即浮点型

按位与：`&` 真真才为真 (不进位)  
按位或：`|` 一真则为真
按位异或：`^` 相同为假，不同为真(与0异或，保留原值.与1异或翻转)
取反：`~`
左移：`<<`高位左移溢出后舍弃,未溢出时左移一位相当与乘于2,  
右移：`>>`未溢出时，右移n位相当与除于2^n  

---

- malloc||realloc

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
 
int main()
{
   char *str;
 
   /* 最初的内存分配 */
   str = (char *) malloc(15);
   strcpy(str, "runoob");
   printf("String = %s,  Address = %u\n", str, str);
 
   /* 重新分配内存 */
   str = (char *) realloc(str, 25);
   strcat(str, ".com");
   printf("String = %s,  Address = %u\n", str, str);
 
   free(str);
 
   return(0);
}
```

- 字符串拼接

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char *firstName = "Theo";
    char *lastName = "Tsao";
    char *name = (char *) malloc(strlen(firstName) + strlen(lastName));
    strcpy(name, firstName); // 把firstName复制到name中
    strcat(name, lastName); // 把lastName追加到name中
    printf("%s\n", name);
    return 0;
}
//、、、、、、、、、、、、、、、

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char *firstName = "Theo";
    char *lastName = "Tsao";
    char *name = (char *) malloc(strlen(firstName) + strlen(lastName));
    // 发送格式化输出到name指定的字符串
    sprintf(name, "%s%s", firstName, lastName); 
    printf("%s\n", name);
    return 0;
}
```

-- 数字与字符串互转

```c
int String2Int(char *str)//字符串转数字 
{
    char flag = '+';//指示结果是否带符号 
    long res = 0;
    
    if(*str=='-')//字符串带负号 
    {
        ++str;//指向下一个字符 
        flag = '-';//将标志设为负号 
    } 
    //逐个字符转换，并累加到结果res 
    while(*str>=48 && *str<57)//如果是数字才进行转换，数字0~9的ASCII码：48~57 
    {
        res = 10*res+  *str++-48;//字符'0'的ASCII码为48,48-48=0刚好转化为数字0 
    } 
 
    if(flag == '-')//处理是负数的情况
    {
        res = -res;
    }
 
    return (int)res;
}
char *Int2String(int num, char *str) // 10进制
{
    int i = 0;   //指示填充str
    if (num < 0) //如果num为负数，将num变正
    {
        num = -num;
        str[i++] = '-';
    }
    //转换
    do
    {
        str[i++] = num % 10 + 48; //取num最低位 字符0~9的ASCII码是48~57；简单来说数字0+48=48，ASCII码对应字符'0'
        num /= 10;                //去掉最低位
    } while (num);                // num不为0继续循环

    str[i] = '\0';

    //确定开始调整的位置
    int j = 0;
    if (str[0] == '-') //如果有负号，负号不用调整
    {
        j = 1; //从第二位开始调整
        ++i;   //由于有负号，所以交换的对称轴也要后移1位
    }
    //对称交换
    for (; j < i / 2; j++)
    {
        //对称交换两端的值 其实就是省下中间变量交换a+b的值：a=a+b;b=a-b;a=a-b;
        str[j] = str[j] + str[i - 1 - j];
        str[i - 1 - j] = str[j] - str[i - 1 - j];
        str[j] = str[j] - str[i - 1 - j];
    }

    return str; //返回转换后的值
}
```

-- 管道

```c
/*********写管道进程*****************/

#include <stdio.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>
#include <unistd.h>
char *Int2String(int num, char *str); //函数声明
int String2Int(char *str);//函数声明 
int main()
{
    char c[10] = {"0,pipe1"};
    char *line = &c[0];
    char buf[128];
    int fd;
    int r, w;
    mknod("mypipe", S_IFIFO, 0);
    fd = open("mypipe", O_RDWR | O_NONBLOCK);
    int n = 0;
    while (n < 10)
    {
        Int2String(n, c);
        n++;
        r = read(fd, buf, 128);
        printf("pipe1_r=%d,\n", r);
        if (r > 0)
        {
            printf("pip1收到内容：%s\n", buf);
            memset(buf, 0, 128);
        }
        else
        {
            w = write(fd, line, strlen(line));
            if (w < 0)
            {
                printf("w=%d,没有写入\n", w);
            }
            printf("%d,pipe1写入内容：%s\n", w, line);
        }
        sleep(1);
    }
    close(fd);
}
```

```c

/*********读管道进程*****************/

#include <stdio.h>
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>

int main()
{
    char buf[128];
    char *line = "pipe2";
    int fd = open("mypipe", O_RDWR | O_NONBLOCK);
    int r, w, n = 0;
    while (n < 10)
    {
        n++;
        r = read(fd, buf, 128);
        printf("pipe2_r=%d\n", r);
        if (r > 0)
        {
            printf("@@pipe2收到内容：%s\n", buf);
            memset(buf, 0, 128);
        }
        else
        {
            w = write(fd, line, strlen(line));
            printf("@@pipe2,w=%d,内容：%s\n", w, line);
        }
        sleep(2);
    }
    close(fd);
}

```
