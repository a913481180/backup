---
title: 数据结构
date: 2020-11-11 20:33:33
categories: 
- study
---

# 数据结构
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
- 单链表整表创建的算法思路：
1. 声明一个结点p和计数器变量n；
2. 初始化一空链表L；
3. 让L的头结点的指针指向NULL；
4. 循环实现后继结点的赋值和插入；


```
#include<stdio.h>
#include<stdlib.h>
	
typedef int DataType;

typedef struct LNode{
	DataType data;		//数据域
	struct LNode *next;	//指针域，指向下一个节点
}LNode,*LinkList;

//创建///
//声明函数
LinkList CreateList(int n);	//创建链表
void print(LinkList h);		//打印链表

//创建链表
LinkList CreateList(int n){
	//定义指针L指向该链表即头指针，结点q为第一个结点，结点p为下一个结点
	LinkList L,p,q;
	//为头指针申请空间
	L=(LNode*)malloc(sizeof(LNode));
	if(!L){return 0;}
	//头指针指向NULL即空链表
	L->next =NULL;
	//第一个结点指向头指针
	q=L;
	//循环创建链表结点
	for(int i=0;i<n;i++){
		//为下一个结点申请空间
		p=(LinkList)malloc(sizeof(LNode));
		printf("输入第%d个元素的值：",i);
		//存入数据
		scanf("%d",&(p->data));
		//下一个结点的指针指向NULL，即尾部
		p->next=NULL;
		//第一个结点的指针指向下一个结点,即上一个结点与该结点相连
		q->next=p;
		//移动第一个结点到下一个结点
		q=p;
	}
	//返回链表头部
	return L;
}

//打印链表
void print (LinkList h){
	//创建一个指针指向链表的第一个元素结点；
	LinkList p=h->next;
	//当不是尾部时
	while(p!=NULL){
	//打印数据
	printf("%d ",p->data);
	//移动到下一个元素
	p=p->next;
	}
}
int main (){
	//创建一个头指针并指向NULL；即空链表·
	LinkList head=NULL;
	//输入结点个数n
	int n;
	printf("请输入链表长度：");
	scanf("%d",&n);
	//创建链表
	head=CreateList(n);
	//打印链表
	printf("刚刚建立的各个链表元素的值为:\n");
	print(head);
	printf("\n\n");
	//按下任意键结束
	getchar();
	getchar();
	return 0;
}

```

- 单链表整表的删除算法思路：
1. 声明结点p和q；
2. 将第一个接地赋值给p，下一个结点赋值给q；
3. 循环执行释放p和将q赋值给p的操作；free(p)会同时把数据域和指针域同时删除
```
//h为头指针
void delect(LinkList h){
LinkList p,q;
p=h->next;	//p指向第一个节点
while(p!=NULL){
q=p->next;	//q指向第二个节点
free(p);	//删除第一个节点
p=q;		//p等于第二个节点
}
h->next==NULL;	//头节点指向空
}
```

- 获取第i个数据的算法思路：
1. 声明一个结点p指向链表第一个结点，初始化j从1开始；
2. 当j\<i时,就遍历链表，让p的指针向后移动，不断指向下一个结点，j+1；
3. 若到链表末尾p为空，则说明第i个元素不存在；
4. 若查找成功则返回结点p的数据；

- 单链表第i个数据插入结点的算法思路：
1. 将数据元素e赋值给s-\>data即s-\>data=e;
2. s-next=p-\>next;
3. p-next=s;

- 单链表的删除
1. p-\>next=p-\>next-\>next;


头指针不为空，头指针是链表的必要元素，头指针是链表指向第一个节点的指针。头结点在第一个元素的节点前，其数据一般无意义，但也可用来存放链表的长度。空链表头指针直接指向null
### 循环链表

尾指针指向头指针

### 双向链表

结构：
```
typedef struct DualNode{
int data;
struct DualNode *prior; 	//前驱节点
struct DualNode *next;		//后继节点
}DualNode,*DuLinkList;
```
插入操作
```
//s为插入节点，p为后一个节点；
s->next=p;
s->prior=p->prior;
p->prior-next=s;
p->prior=s;
```
删除操作：
```
//删除p节点
p->prior->next=p->next;
p->next->prior=p->prior;
free(p);
```
### 静态链表
用数组描述的链表称为静态链表
```
#define maxsize 1000
typedef struct{
int  data;//数据
int cut;//游标
}component,staticlinklist[maxsize];	//结构体数组
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
```

### 栈
是一个先进后出的线性表，要求只在表尾进行删除和插入操作；对与栈来说，表尾称为栈的栈顶top,表头称为栈底bottom.
结构:
```
typedef struct{
int *bottom;	//指向栈底的指针；
int *top;
int stacksize;	//指当前可使用的最大总量。
}sqStack;
```
创建一个栈：
```
#define STACK_INIT_SIZE 100
initStack(sqStack *s){
s->bottom=(int*)malloc(STACK_INIT_SIZE*sizeof(int));
if(!s->base){exit(0);}
s->top=s->base;		//开始栈顶就是栈底
s->stacksize=STACK_INIT_SIZE;
}
//初始化
void initstack(sqStack *s){
s->bottom=(int*)malloc(STACK_INIT_SIZE*sizeof(int));
if(!s-bottom){exit(0);}
s->top=s->bottom;
s->stacksize=STACK_INIT_SIZE;
}
```
入栈操作：
```
#define STACKINCREMENT 10
push(sqStack *s,int e);
{
//栈满追加空间
if (s->top - s->bottom==s->stacksize) {
s->bottom=(int*)realloc(s->bottom,(s->stacksize+STACKINCREMENT)*sizeof(int));
if(!s->bottom){exit(0);}
s->top=s->bottom+s->stacksize;			//设置栈顶
s->stacksize=s->stacksize+STACKINCREMENT;	//重新设置栈的最大容量
}
*(s->top)=e;
s->top++;
}
```

出栈操作：
```
pop(sqStack *s, int *e){
if(s->top==s->base){rerutn;}	//栈空退出；
*e=*--(s->top);
}
```
清空栈：
```
clearsqstack(sqStack *s){
s->top=s->bottom;
}
```
销毁一个栈：
```
destroystack(sqStack *s){
int i,len;
len=s->stacksize;
for(i=0;i<len;i++){
free(s->bottom);
s->bottom++;
}
s->bottom=s->top=NULL;
s->stacksize=0;
}
```
计算栈容量：
```
int stacklen(sqStack *s){
return(s.top-s.bottom);
}
```
栈的存储结构：
顺序存储
链式存储：栈顶为单链表的头部
```
typedef struct stackNode{
ElemType data;		//存放栈的数据
struct stackNode *next;
}stackNode,*LinkStackPtr;
typedef strucct LinkStack{
LinkStackPtr top;	//top指针
int count;		//计数器
}
//栈链的进栈
//s为新节点，top为栈顶指针；
Status push(LinkStack *s,ElemType e){
LinkStackPtr p=(LinkStackPtr)malloc(sizeof(LinkStackPtr));
p->data=e;
p->next=s->top;
s->cont++;
return OK;
}
//链表的出栈操作
Status pop(LinkStack *s,ElemType *e){
LintStack p;
if(StackEmpty(*s)){return ERROR;}	//判断是否为空栈
*e=s->top->data;
p=s->top;
s->top=s->top->next;
free(p);
s->count--;
return OK;
}
```
### 队列
队列只允许在一端进行插入操作，在另一端进行删除操作的线性表（先进先出）

队列的链式存储结构
```
typedef struct QNode{
ElemType data;
struct QNode *next;
}QNode,*QueuePrt;
type struct{
QueuePrt front,rear;	//队头、尾指针；
}LinkQueue;

```
创建队列：在内存中创建一个头结点，将队列的头、尾指针都指向这个生成的头结点，此时为空队列；
```
initQueue(LinkQueue *q)
{q->front=q->rear=(QueuePtr)malloc(sizeof(QNode));
if(!q->front){exit(0);}
q->front->next=NULL;
}
```
入队操作

```
InsertQueue(LinkQueue *q,ElemType e){
QueuePtr p;
p=(QueuePtr)malloc(sizeof(QNode));
if(p==NULL){eixt(0);}
p->data=e;
p->next=NULL;
q->rear->next=p;
q->rear=p;
}
```
出队列
```
DeleteQueue(LinkQueue *q,ElemType *e){
Queueptr p;
if(q->front==q->rear){return;}	//判断队列是否为空；
p=q->front->next;
*e=p->data;
q->front->next=p->next;
if(q->rear==p){q->rear=q->front;}
free(p);
}
```
销毁一个队列
```
DestroyQueue(LinkQueue *q){
while(q->front){
q->rear=q->front-next;
free(q->front);
q->front=q->rear;
}
}
```
### 串

有零个或多个字符组成的有限序列`S='a1a2a3a4a5....'`

- 串的长度：串中字符的个数
- 子串：串中任意个连续的字符串组成的子序列；
- 串中的位置：该字节在串中的序号，子串在主串中的位置以子串的第一个字符在主串中的位置来表示
- 空格串：有一个或多个空格组成的串
- 空串

### 递归

方法或函数调用自身的方式称为递归调用，调用称为递，返回称为归。

- 递归的优缺点？

1.优点：代码的表达力很强，写起来简洁。
2.缺点：空间复杂度高、有堆栈溢出风险、存在重复计算、过多的函数调用会耗时较多等问题。

- 一个问题只要同时满足以下3个条件，就可以用递归来解决：
1.问题的解可以分解为几个子问题的解。何为子问题？就是数据规模更小的问题。
2.问题与子问题，除了数据规模不同，求解思路完全一样
3.存在递归终止条件

- 解决方案

1.警惕堆栈溢出：可以声明一个全局变量来控制递归的深度，从而避免堆栈溢出。
2.警惕重复计算：通过某种数据结构来保存已经求解过的值，从而避免重复计算。

 
## 树

在一颗非空树中，有且只有一个根节点，结点拥有的子树树称为度；度为0 的结点称为叶子或终端结点；不为0 的结点称为分支结点或非终端结点；树的度是树内结点度的最大值；同一个双亲的孩子互称为兄弟；从根到该结点所经过分支上的结点称为祖先，从该结点到下面所有结点称为子孙；树中结点的最大层次称为树的深度；

从左到右是有次序的称为该树的有序树，否则称为无序树；
森林是m棵互不相交的树的集合；

###  二叉树

每个结点至多只有两棵子树，子树有左右之分，次序不能颠倒

在二叉树的第i层上至多有2^(i-1)个结点

深度为k的二叉树至多有2^k-1个结点

终端结点数为n0，度为2的结点数为n，则n0=n+1;

- 满二叉树

深度为k且结点数为2^k-1的二叉树

- 完全二叉树

每个结点都与满二叉树中的编号一一对应。

具有n个结点的完全二叉树的深度为[log(2)n]+1;

若2i>n，则结点i无左孩子，否则其左孩子是结点2i；

若2i+1>n，则结点i无右孩子，否则其右孩子为结点2i+1;

若i>1，则其双亲为结点[i/2]

- 二叉树的存储结构

   - 顺序存储

	将完全二叉树上编号为i的结点元素存储在一维数组中

   - 链式存储

	链表中的结点至少包含三个域：数据域、左右结点指针域；还可以增加一个指向其双亲的指针域；

- 遍历二叉树

   - 先序遍历

	根-左-右

   - 中序遍历

	左-根-右

   - 后续遍历

	左-右-根

- 二叉链表

```
#include<stdio.h>
typedef char ElemType;
typedef struct BiTNode
{
ElemType data;
struct BiTNode *lchild, *rchild;
}BidTNode,*BiTree;

//创建一颗二叉树，按照前序遍历的方式输入数据
CreateBidTree(BiTree *T)
{
char c;
scanf("%c",&c);
if(' '==c)	//空格表示结点无子树
{
*T=NULL;
}
else
{
*T=(BiTNode *)malloc(sizeof(BiTNode));
(*T)->data=c;
CreateBiTree(&(*T)->lchild);
CreateBiTree(&(*T)->rchild);
}
}
visit(){
//访问结点时的操作
}
//遍历二叉树
PreOrderTraverse(BidTree T,int level)
{
if(T)
{
visit(T->data,level);
PreOrderTraverse(T->lchild,level+1);
PreOrderTraverse(T->rchild,level+1);
}
}
int main()
{
int level=1;
BiTree T=NULl;
CreateBiTree(&T);
PreOrderTraverse(T,level);
return 0;
}
```

- 线索二叉树

将定义好的二叉树结构进行扩容：

||||||
|-|-|-|-|-|
|lchild|ltag|data|rtag|rchild|

>ltag=0时,lchild指向该结点的左孩子，为1时指向该结点的前驱。rtag=0，rchild指向该结点的右孩子，为1时指向该结点的后继。

``` 
#include<stdio.h>
#include<stdlib.h>
typedef char ElemType;
//线索存储标志位
//Link(0):表示指向左右孩子的指针
//Thread(1):表示指向前驱后继的线索
typedef enum{Link,Thread} PointerTag;
typedef struct BiThrNode
{
char data;
struct BiThrNode *lchild, *rchild;
PointerNag ltag;
PointerNag rTag;
}BiThrNode,*BiThrTree;

//全局变量，始终指向刚刚访问过的结点
BiThrTree pre;

//创建一棵二叉树，按照前序遍历的方式输入数据
CreateBiThrTree(BiThrTree *T)
{
char c;
scanf("%c",&c);
if(' '==c){
*T=NULL;
}
else{
*T=(BiThrNode *)malloc(sizeof(BiThrNode));
(*T)->data=c;
(*T)->ltag=Link;
(*T)->rtag=Link;

CreateBiThrTree(&(*T)->lchild);
CreateBiThrTree(&(*T)->rchild);
}
}

//中序遍历线索
InThreading(BiThrTree T)
{
if(T)
{
InThreading(T->lchild);	//递归左孩子线索化
if(!T->lchild)	//如果该节点没有左孩子，则设置ltag为thread，并把lchild指向刚刚访问过的结点
{
T->ltag=Thread;
T->lchild=pre;
}
if(!pre->rchild)
{
pre->rtag=Thread;
Pre->rchild=T;
}

Pre=T;
InThreading(T->rchild);		//递归右孩子线索化
}
}

InOrderThreading(BiThrTree *p,BiThrTree T)
{
*p=(BiThrTree)malloc(sizeof(BiThrNode));
(*p)->ltag=Link;
(*P)->rtag=Thread;
(*p)->rchild=*p;
if(!T){
(*P)->lchild=*p;
}
else
{
(*P)->lchild=T;
Pre=*p;
InThreading(T);
Pre->rchild=*p;
Pre->rtag=Thread;
(*P)->rchild=Pre;
}
}
int main(){
BiThrTree P,T=NULL;
CreateBiThrTree(&T);
InOrderThreading(&P,T);
return 0;
}
```

- 普通树转换为二叉树
   1. 所有兄弟结点之间加一条连线
   2. 对树中的每一个结点，只保留它与第一孩子结点的连线，删除它与其它孩子结点间的连线

- 森林转换为二叉树
   1. 把每棵树转化为二叉树
   2. 第一颗二叉树不动，从第二棵二叉树开始，依次把后一棵二叉树的根结点作为前一棵二叉树的根结点的右孩子，用线连起来

>树、森林的前序遍历和二叉树的前序遍历结果相同，后序遍历与二叉树的中序遍历结果相同。
## 图

图（Graph）是由顶点的有穷非空集合和顶点之间边的集合组成，通常表示为：G（V，E）其中，G表示一个图，V是图G中顶点的集合，E是图G中边的集合。

- 图按照边的有无方向分为无向图和有向图。无向图由顶点和边组成，有向图由顶点和弧构成。弧有弧尾和弧头之分，带箭头一端为弧头。

- 图按照边或弧的多少分稀疏图和稠密图。如果图中的任意两个顶点之间都存在边叫做完全图，有向的叫有向完全图。若无重复的边或顶点到自身的边则叫简单图。

- 图中顶点之间有邻接点、依附的概念。无向图顶点的边数叫做度。有向图顶点分为入度和出度。

- 图上的边或弧带有权则称为网。

- 图中顶点间存在路径，两顶点存在路径则说明是连通的，如果路径最终回到起始点则称为环，当中不重复的叫简单路径。若任意两顶点都是连通的，则图就是连通图，有向则称为强连通图。图中有子图，若子图极大连通则就是连通分量，有向的则称为强连通分量。

- 无向图中连通且n个顶点n-1条边称为生成树。有向图中一顶点入度为0其余顶点入度为1的叫有向树。一个有向图由若干棵有向树构成生成森林。

### 图的存储结构

- 邻接矩阵

用两个数组来表示图，一个一维数组里储存着顶点的信息，一个二维数组储存着图中的边或弧的信息。

>无向图的邻接矩阵是对称的

>有向图的邻接矩阵可能是不对称的，顶点的度=顶点的出度+顶点的入度；


```
#define MVNum 100	//最大顶点数
typedef char VerTexType;	//设顶点的数据类型为字符型
typedef int ArcType;		//假设边的权值类型为整型
typedef struct{
VerTexType Vexs[MVNum];		//顶点数组
ArcType arcs[MVNum][MVNum]	//邻接矩阵
}AMGraph;	

//在顶点数组中查找顶点
int LocateVex(AMGraph G,VertexType u){
//查找顶点u，返回下标
int i;
for (i=0;i<G.vexnum;++i){
if(U==G.vexs[i]){
return i;
}
}
reruen -1;
}
status CreateUDN(AMGraph &G){
//创建无向网G
cin>>G.vexnum>>G.arcnum;	//输入总顶点数，总边数
for(i=0;i<G.vexnum;++i)
cin>>G.vexs[i];		//依次输入点的信息
for(i=0;i<G.vexnum;++i)	//初始化邻接矩阵
for (j=0;j<G.vexnum;++j)
G.arcs[i][j]=MaxInt;	//边的权值均为最大值
fof(k=0;k<G.arcnum;++k){	//构造邻接矩阵
cin>>v1>>v2>>w;			//输入一条边所依附的顶点及边的权值
i=LocateVex(G,v1);
j=LocateVex(G,v2);	//确定v1和v2在G中的位置
G.arcs[i][j]=w;		//边<v1,v2>的权值置为w
G.arcs[j][i]=G.arcs[i][j];	//<v1,v2>的对称边<v2,v1>的权值为w
}
return 0;
}

```


- 邻接表


| |缺点|改进方法|
|-|-|-|
|有向图|求各结点的度困难|十字链表|
|无向图|每条边都要存储两遍|邻接多重表|


```
typedef struct VNode{
VerTexType data;	//顶点信息
ArcNode *firstarc;	//指向第一条依附该顶点的边的指针
}VNode，AdjList[MVNum];	//AdjList表示邻接表类型

status CreateUDG(ALGragh &G){	//创建无向图G
cin>>G.vexnum>>G.arcnum;	//输入图G的顶点，边个数
for(int i=0;i<G.vexnum;++i){
cin>>Gvertices[i].data;		//输入顶点值
G.vertices[i].firsarc=NULL;	//初始化表头结点的指针域
}
for(k=0;k<G.arcnum;++k){	//输入各边
cin>>v1>>v3;
i=LocateVex(G,v1);
j=LocateVex(G,v2);
p1=new ArcNode;		//生成一个新的边结点*p1
p1->adjvex=j;		//邻接点序号为j
p1->nextarc=G.vertices[i].firstarc;
G.vertices[i].firstarc=p1;;		//将新结点*p1插入顶点vi的边表头部
p2=new ArcNode;		//生成另一个对称的新的边结点*p2
p2->adjvex=i;		//邻接点序号为i
p2->nextarc=G.vertices[j].firstarc;
G.vertices[j].firstarc=p2;	//将新结点*p2插入顶点vj的边表头部
}
return OK;
}
```
## 遍历方法

- 深度优先遍历

也称深度优先搜索（DFS）

右手原则：在没有碰到重复顶点的情况下，分叉路口始终是向右手边走，每路过一个顶点就做一个记号。

- 广度优先遍历
又称广度优先搜索（BFS）

普利姆算法
克鲁斯卡尔算法
