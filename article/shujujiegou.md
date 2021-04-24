---
title: 数据结构
date: 2020-11-11 20:33:33
categories: 
- study
---


### 算法时间复杂度

T(n)=O(f(n))

1. 用常数1取代运行时间中的所有加法常数，如多条pintf语句只运行1次即O(1)
2. 在修改后的运行次数函数中，只保留最高项
3. 如果最高项不是1，则去除最高项的常数如3n^2即O(n^2)

所耗时间`O(1)<O(logn)<O(n)<O(nlogn)<O(n^2)<O(2^n)<O(n!)<O(n^n)`

### 线性表

由零个或多个数据元素组成的有限序列

[a1][a2]...[ai-1][ai][ai+1]....[an]
[ai-1]为[ai]的直接前驱元素
[ai+1]为[ai]的直接后继元素
每个元素有且只有一个前驱和后继元素，不存在一对多

抽象数据类型的形式定义

ADT=(D,S,P)

描述方法（伪码）：

ADT 抽象数据类型名{

数据对象：<数据对象的定义>

数据关系：<数据关系的定义>

基本操作：<基本操作的定义>

}ADT 抽象数据类型名


抽象数据类型的实现

```
#define TRUE 1
#define FALSE 0
#define OK 1
#define ERROR 0
#define INFEASIBLE -1
#define OVERFLOW -2
//Status是函数的类型，其值是函数结果的状态代码
typedef int Status;
Status 函数名{
//算法说明
语句序列;
}//函数名
```

```
ADT List{
	数据对象：D={ai|ai∈ElemSet,i=1,2,...,n,n≥0}
	数据关系：R1={<ai-1,ai>|ai-1,ai∈D,i=2,3,4,...,n}
	基本操作：
	InitList(&L)
		操作结果：构造一个空的线性表L。
	DestroyList(&L)
		初始条件：线性表L已存在。
		操作结果：销毁线性表L。
	ClearList(&L)
		初始条件：线性表L已存在。
		操作结果：将L重置为空表。
	ListEmpty (L)
		初始条件：线性表L已存在。
		操作结果：若L为空表，则返回TRUE，否则返回FALSE。
	ListLength(L)
		初始条件：线性表L已存在。
		操作结果：返回L中数据元素的个数。
	GetElem(L,i,&e)
		初始条件：线性表L已存在，l≤i≤ListLength(L)。
		操作结果：用e返回L中第i个数据元素的值。
	LocateElem(L,e,compare())
		初始条件：线性表L已存在，compare()是数据元素的判定函数。
		操作结果：用e返回L中第1个与e满足关系compare()的数据元素的位序。若这样的数据元素不存在，则返回值为0。
	PriorElem(L,cur_e,&pre_e)
		初始条件：线性表L已存在。
		操作结果：若cur_e是L的数据元素，且不是第一个，则用pre_e返回它的前驱，否则操作失败，pre_e无定义。
	NextElem(L,cur_e,&next_e)
		初始条件：线性表L已存在。
		操作结果：若cur_e是L的数据元素，且不是最后一个，则用next_e返回它的前驱，否则操作失败，next_e无定义。
	ListInsert(&L,i,e)
		初始条件：线性表L已存在，l≤i≤ListLength(L)+1。
		操作结果：在L中第i个位置之前插入新的数据元素e，L的长度加1.
	ListDelete(&L,i,&e)
		初始条件；线性表L已存在且非空，l≤i≤ListLength(L)。
		操作结果：删除L中第i个数据元素，并用e返回其值，L长度减1.
	ListTraverse(L,visit())
		初始条件：线性表L已存在。
		操作结果：依次对L的每个数据元素调用函数visit()。一旦visit()失败，则操作失败。
}ADT List
```

### 顺序存取
数组
### 链式存取
单链表整表创建的算法思路：
1.声明一个结点p和计数器变量i；
2.初始化一空链表L；
3.让L的头结点的指针指向NULL；
4.循环实现后继结点的赋值和插入；
```
#define ListSize 10
typedef int DataType;
typedef struct{
DataType data[ListSize];
int length;
}Seqlist;
Seqlist *L
```
单链表整表的删除算法思路：
1.声明结点p和q；
2.将第一个接地赋值给p，下一个结点赋值给q；
3.循环执行释放p和将q赋值给p的操作；free(p)会同时把数据域和指针域同时删除


获取第i个数据的算法思路：
1.声明一个结点p指向链表第一个结点，初始化j从1开始；
2.当j\<i时,就遍历链表，让p的指针向后移动，不断指向下一个结点，j+1；
3.若到链表末尾p为空，则说明第i个元素不存在；
4.若查找成功则返回结点p的数据；

单链表第i个数据插入结点的算法思路：
1.将数据元素e赋值给s-\>data即s-\>data=e;
2.s-next=p-\>next;
3.p-next=s;



单链表的删除
1.p-\>next=p-\>next-\>next;


头指针不为空，头指针是链表的必要元素，头指针是链表指向第一个节点的指针。头结点在第一个元素的节点前，其数据一般无意义，但也可用来存放链表的长度。空链表头指针直接指向null

### 静态链表
用数组描述的链表称为静态链表
```
#define maxsize 1000
typedef struct{
int  data;//数据
int cut;//游标
}component,staticlinklist[maxsize];
//静态链表的初始化，相当于初始化数组:
Status InitList(staticlinklist space){
int i;
for(i=0;i<maxsize-1;i++)
{space[i].cur=i+1;}
space[maxsize-1].cur=0;
return Ok;
}
//第一个与最后一个元素不存放数据,最后一个元素的游标为0。数组的第一个元素即下标为0的那个元素的cur游标就存放备用链表的第一个结点的下标；最后一个元素即maxsize-1的游标cur则存放第一个有数据的元素的下标，相当于单链表中的头结点作用。 
//静态链表中的插入和删除，只需修改游标
