---
title: C++
date: 2020-11-21 11:22:12
categories:
- study
---

## Helloword

```  
#include<iostream>
using namespace std;	//名字空间
int main()
{
cout<<"helo word"<<endl;
cout<<"helo word\n";
return 0;
}
```

### 输入输出  
cin、cout是istream类的对象

### 重载  

### 复杂的数据类型  

### 类

```
class Test
{

};
```

定义构造器  
构造器的名字必须与它所在的类的名字一样  
构造器没有返回值  
```
class Car{
public:
	Car(void);	//名称与类名一致
};

Car::Car(void)
{
xxx
}


调用：

class Car{
public:
	Car(void);	//名称与类名一致
	void runing();
};

Car::Car(void)
{
xxx
}

void runing(){
xxx
}

int main (){
Car mycar;
mycar.runing();
return 0;
```

数组：

```
Car mycar[10];
调用：
mycar[x].runing;	//x为给定数组元素的下标
```


副本构造器

```
MyClass（const MyClass &rhs);
```

析构器：与构造器一样，只是前边多了一个波浪符`~`

```
class Car
{
Car(void);
~Car();
}

Car::Car()
{
xxx
}

Car::~Car(){
xxx
}

```
this指针:让构造器知道哪个是参数哪个是属性

```
如果代码中不存在二义性，则不必使用this指针
this->fish=fish;
```

### 继承：
基类即父类
子类即从基类派生出来的类

```
class SubCar(子类):public Car(基类) {...};
子类也可以调用基类的成员
```
- 继承里的含参数构造器

```
class Animal
{
Animal(std::string Name);
};

class Pig:public Animal
{
Pig(std::string Name);
};

//构造器
Animal::Animal(std::string Name)
{
name=Name;
}

Pig::Pig(std::string Name) : Animal(Name)
{
xxx
}
```
### 访问控制
保护类里的方法和属性的手段

>public：任何代码
protected：这个类本身和它的子类
private：只有这个类本身

### 覆盖方法
在类里重新声明这个方法

### 重载

可以定义多个同名的函数，只要它们输入的参数不同


- 运算符的重载

重载不能改变运算符的操作个数，优先级，结核性

```
函数类型 operator 运算符（形参）
{
运算符的重载操作
}

如：
int operator +(int a,int b)
{
return (a-b);
}
```
>以下五个运算符不允许重载：
`.`;`*`;`::`;`sizeof`;`?:`;



### 友元关系

友元关系是类中的一种特殊关系，允许友元类访问对方pubulic、protected、private中的方法和属性

只要在类的声明里的地方加上`friend class **类名字***`

### 静态属性和静态方法

只要在它的声明前加上`static`即可

### 虚方法

只需要在其原型前加上`virtual`即可


```
每一个new操作都要对应一个delete操作
int *p=new int;
*p=10;
std::cout<<*p;
delete p;
```

### 抽象方法

在原型的末尾加上`=0`

### 多继承

```
class TeachingStudent:public Student,public Teacher
{
xxxx
};
```

### 虚继承


```
class Teacher:virtual public Person
{
xxx
}
```

### 动态内存

new语句申请内存，delete语句释放内存,再将指针指向NULL

```
int *i=new int;
delete i;
i=NULL;
```
- 动态数组

```
int *=new int[10];
x[1]=1;
....
delete [] x;	//删除动态数组
```



### 强制类型转换

静态：

```

Company *company=new Company("APPLE","iPhone");
TechCompany *tecCompany=(TecCompany*)company
delete company;
或

不能两个都删除
company=NULL;
tecCompany=NULL;
```

动态：

```
const_cast<MyClass*>(value)	//用来改变value的常量性
dynamic_cast<MyClass*>(value)	//用来把一中类型的对象指针安全地强制转换为另一种类型的对象指针，如果value的类型不是一个MyClass类或其子类的指针，则返回NULL
reinterpret_cast<T>(value)	//在不进行任何实质性的转换的情况下，把一种类型的指针解释为另一种类型的指针或者把一种整数解释为另一种整数
static_cast<T>(value)		//用来进行强制类型转换而不做任何运行时的检查

例如：
Company *company=new Company("APPLE",iPhone");
TechCompany *techcompany=dynamic_cast<TechCompany *>(company);
```

### 命名空间

```
namespace myNamespace
{
....
}


使用命名空间
* using namespace myNamespace

* myNamespace::xxxx

* using std::cout;
  cout<<",,,,";
```

### 链接

当使用编译器建立程序时：
1.执行预处理器指令  
2.把.cpp文件编译成.o文件  
3.把.o文件链接成一个可执行文件  

### 泛型编程


函数模板
```
template <class T>	//字母T表示接下来的函数里代表的一种不确定的数据类型
void swap(T &a,T &b)
{
T i=0;
....
}
```

类模板
```
template<class T>
class MyClass
{
MyClass();
void swap(T &a,T &b);
};

//构造器的实现
Myclass<T>::MyClass()
{
.....
}
```


### 内联函数  

```
inline int add (int x,int y,int z)
{
return x+Y+z;

}
```

