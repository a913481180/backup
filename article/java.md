---
title: Java
date: 2022-12-12  12:34:21
categories:
  - java
---

# Java

## 编写 Java 程序时，应注意以下几点

- 大小写敏感：Java 是大小写敏感的，这就意味着标识符 Hello 与 hello 是不同的。
- 类名：对于所有的类来说，类名的首字母应该大写。如果类名由若干单词组成，那么每个单词的首字母应该大写，例如 MyFirstJavaClass 。
- 方法名：所有的方法名都应该以小写字母开头。如果方法名含有若干单词，则后面的每个单词首字母大写。
- 源文件名：源文件名必须和类名相同。当保存文件的时候，你应该使用类名作为文件名保存（切记 Java 是大小写敏感的），文件名的后缀为 .java。（如果文件名和类名不相同则会导致编译错误）。

  `MyClass.java`

  ```java
  public class MyClass {
    public static void main(String[] args) {
      System.out.println("Hello World");
    }
  }
  ```

  ```bash
  C:\Users\Your Name>javac MyClass.java
  # 这将编译您的代码。如果代码中没有错误，命令提示符将引导您进入下一行。现在输入 "java MyClass" 以运行该文件
  C:\Users\Your Name>java MyClass
  ```

- 主方法入口：所有的 Java 程序由 public static void main(String[] args) 方法开始执行。
- 每个 Java 程序都有一个必须与文件名匹配的 class 类名，并且每个程序都必须包含 main()方法

## Java 标识符

Java 所有的组成部分都需要名字。类名、变量名以及方法名都被称为标识符。

关于 Java 标识符，有以下几点需要注意：

- 所有的标识符都应该以字母（A-Z 或者 a-z）,美元符（$）、或者下划线（\_）开始
- 首字符之后可以是字母（A-Z 或者 a-z）,美元符（$）、下划线（\_）或数字的任何字符组合
- 关键字不能用作标识符
- 标识符是大小写敏感的
- 合法标识符举例：age、$salary、\_value、\_\_1_value
- 非法标识符举例：123abc、-salary

## Java 修饰符

像其他语言一样，Java 可以使用修饰符来修饰类中方法和属性。主要有两类修饰符：

- 访问控制修饰符 : default, public , protected, private
- 非访问控制修饰符 : final, abstract, static, synchronized

## Java 变量

Java 中主要有如下几种类型的变量

- 局部变量
- 类变量（静态变量）
- 成员变量（非静态变量）

要创建变量，必须指定类型并为其赋值：

```java
String name = "John";
//或
String name;
name="xxx";
```

Java 数据类型

数据类型分为两组:
基本数据类型 - 包括 byte, short, int, long, float, double, boolean 和 char
|数据类型| 大小| 描述|
|:-|:-|:-|
|byte| 1 byte |存储从 -128 到 127 的整数|
|short| 2 bytes| 存储从 -32,768 到 32,767 的整数|
|int| 4 bytes |存储从 -2,147,483,648 到 2,147,483,647 的整数|
|long| 8 bytes| 存储从 -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807 的整数|
|float| 4 bytes| 存储小数。 足以存储 6 到 7 个十进制数字|
|double| 8 bytes| 存储小数。 足以存储 15 位十进制数字, float 的精度只有 6 位或 7 位十进制数字，而 double 变量的精度约为 15 位|
|boolean| 1 bit |存储 true 或 false 值|
|char| 2 bytes |存储单个字符/字母或 ASCII 值|

非基本数据类型 - 例如 String, Arrays 和 Classes (您将在后面的章节中了解更多)

原始类型在 Java 中是预定义的（已经定义的）。 非原始类型由程序员创建，不是由 Java 定义的（String 除外）。
非原始类型可以用来调用方法来执行某些操作，而原始类型不能。
非原始数据类型称为引用类型，因为它们引用对象。
原始类型始终有一个值，而非原始类型可以为 null。
原始类型以小写字母开头，而非原始类型以大写字母开头。
原始类型的大小取决于数据类型，而非原始类型的大小都相同。

在 Java 中，有两种类型的数据转换：

隐式类型转换 (自动) - 从小类型到大类型，不需要强制转换符
byte -> short -> char -> int -> long -> float -> double

```java
 int myInt = 9;
 double myDouble = myInt; // 自动转换: int 到 double

```

强制类型转换 (手动) - 从大类型到小类型，需要强制转换符实现强制转换
double -> float -> long -> int -> char -> short -> byte

```java
 double myDouble = 9.78;
 int myInt = (int) myDouble; // 手动转换：double 到 int
```

## Java 关键字

下面列出了 Java 关键字。这些保留字不能用于常量、变量、和任何标识符的名称。

- 访问控制

  |  关键字   |   说明   |
  | :-------: | :------: |
  |  private  |  私有的  |
  | protected | 受保护的 |
  |  public   |  公共的  |
  |  default  |   默认   |

- 类、方法和变量修饰符

  |    关键字    |              说明              |
  | :----------: | :----------------------------: |
  |   abstract   |            声明抽象            |
  |    class     |               类               |
  |   extends    |           扩充,继承            |
  |    final     |       最终值,不可改变的        |
  |  implements  |          实现（接口）          |
  |  interface   |              接口              |
  |    native    | 本地，原生方法（非 Java 实现） |
  |     new      |            新,创建             |
  |    static    |              静态              |
  |   strictfp   |           严格,精准            |
  | synchronized |           线程,同步            |
  |  transient   |              短暂              |
  |   volatile   |              易失              |

- 程序控制语句
  |关键字|说明|
  |:------:|:------:|
  |break|跳出循环|
  |case |定义一个值以供 switch 选择|
  |continue| 继续|
  |do| 运行|
  |else| 否则|
  |for| 循环|
  |if| 如果|
  |instanceof| 实例|
  |return| 返回|
  |switch| 根据值选择执行|
  |while| 循环|

- 错误处理

  | 关键字  |          说明          |
  | :-----: | :--------------------: |
  | assert  |   断言表达式是否为真   |
  |  catch  |        捕捉异常        |
  | finally |    有没有异常都执行    |
  |  throw  |    抛出一个异常对象    |
  | throws  | 声明一个异常可能被抛出 |
  |   try   |        捕获异常        |

- 包相关
  |关键字|说明|
  |:------:|:------:|
  |import |引入|
  |package |包|

- 基本类型
  |关键字|说明|
  |:------:|:------:|
  |boolean |布尔型|
  |byte |字节型|
  |char |字符型|
  |double |双精度浮点|
  |float |单精度浮点|
  |int |整型|
  |long |长整型|
  |short |短整型|
- 变量引用
  |关键字|说明|
  |:------:|:------:|
  |super| 父类,超类|
  |this| 本类|
  |void| 无返回值|

- 保留关键字
  |关键字|说明|
  |:------:|:------:|
  |goto |是关键字，但不能使用|
  |const |是关键字，但不能使用|

注意：Java 的 `null` 不是关键字，类似于 `true` 和 `false`，它是一个字面常量，不允许作为标识符使用。

## Java 方法

方法必须在类中声明。它是用方法的名称定义的，后跟括号()

```java
public class MyClass {
  static int myMethod(int x, int y) {
    return x + y;
  }

  public static void main(String[] args) {
    int z = myMethod(5, 3);
    System.out.println(z);
  }
}

```

使用方法重载，多个方法可以具有相同的名称和不同的参数,只要参数的数量或类型不同，多个方法就可以具有相同的名称。

```java
static int plusMethod(int x, int y) {
  return x + y;
}

static double plusMethod(double x, double y) {
  return x + y;
}

public static void main(String[] args) {
  int myNum1 = plusMethod(8, 5);
  double myNum2 = plusMethod(4.3, 6.26);
  System.out.println("int: " + myNum1);
  System.out.println("double: " + myNum2);
}
```

## Java 对象和类

- 对象：对象是类的一个实例（对象不是找个女朋友），有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。
- 类：类是一个模板，它描述一类对象的行为和状态。

```java
public class Dog {
    String breed;
    int size;
    String colour;
    int age;

    void eat() {
    }

    void run() {
    }

    void sleep(){
    }

    void name(){
    }


}
```

一个类可以包含以下类型变量：

- 局部变量：在方法、构造方法或者语句块中定义的变量被称为局部变量。变量声明和初始化都是在方法中，方法结束后，变量就会自动销毁。
- 成员变量：成员变量是定义在类中，方法体之外的变量。这种变量在创建对象的时候实例化。成员变量可以被类中方法、构造方法和特定类的语句块访问。
- 类变量：类变量也声明在类中，方法体之外，但必须声明为 static 类型。
  一个类可以拥有多个方法

### 构造方法

Java 中的构造函数是一种用于初始化对象的特殊方法。在创建类的对象时调用构造函数。它可用于设置对象属性的初始值
在创建一个对象的时候，至少要调用一个构造方法。构造方法的名称必须与类同名，并且不能有返回类型(如 void),一个类可以有多个构造方法。
每个类都有构造方法。如果没有显式地为类定义构造方法，Java 编译器将会为该类提供一个默认构造方法。但是，您无法设置对象属性的初始值。
下面是一个构造方法示例：

```java
public class Puppy{
    public Puppy(){
    }

    public Puppy(String name){
        // 这个构造器仅有一个参数：name
    }
}
```

### 创建对象

对象是根据类创建的。在 Java 中，使用关键字 new 来创建一个新的对象。创建对象需要以下三步：

声明：声明一个对象，包括对象名称和对象类型。
实例化：使用关键字 new 来创建一个对象。
初始化：使用 new 创建对象时，会调用构造方法初始化对象。
下面是一个创建对象的例子：

```java
public class Puppy{
  int age;
  public Puppy(String name){
      //这个构造器仅有一个参数：name
      System.out.println("小狗的名字是 : " + name );
   }

  static void eat(){}
  public static void main(String[] args){
      // 下面的语句将创建一个Puppy对象
      Puppy myPuppy = new Puppy( "tommy" );
      myPuppy.age=4
      myPuppy.eat();
   }
}
```

### 修饰符

#### 方法修饰符

static 静态方法，这意味着可以在不创建类的对象的情况下访问该方法，而 public 只能由对象访问

```java
public class MyClass {
  // Static 方法
  static void myStaticMethod() {
    System.out.println("Static methods can be called without creating objects");
  }

  // Public 方法
  public void myPublicMethod() {
    System.out.println("Public methods must be called by creating objects");
  }

  // Main 方法
  public static void main(String[] args) {
    myStaticMethod(); // 调用静态方法
    // myPublicMethod(); 这会编译一个错误

    MyClass myObj = new MyClass(); // 创建一个 MyClass 的对象
    myObj.myPublicMethod(); // 调用对象的公共方法
    myObj.myStaticMethod(); // 调用对象的静态方法
  }
}
```

#### 修饰符分为两组

- 访问修饰符 - 控制访问级别

对于 classes，可以使用 public 或 default:

| 修饰符  | 描述                                                     |
| ------- | -------------------------------------------------------- |
| public  | 该类可由任何其他类访问                                   |
| default | 该类只能由同一包中的类访问。在不指定修改器时使用此选项。 |

对于属性、方法和构造函数，可以使用以下选项之一：

| 修饰符    | 描述                                                     |
| --------- | -------------------------------------------------------- |
| public    | 所有类都可以访问该代码                                   |
| private   | 代码只能在声明的类中访问                                 |
| default   | 该类只能由同一包中的类访问。在不指定修改器时使用此选项。 |
| protected | 代码可以在相同的包和子类中访问。                         |

- 非访问修饰符 - 不控制访问级别，但提供其他功能

对于类，可以使用 final 或 abstract:

| 修饰符   | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| final    | 该类不能被其他类继承                                         |
| abstract | 该类不能用于创建对象（要访问抽象类，它必须从另一个类继承。） |

对于属性和方法，可以使用以下选项之一：

| 修饰符       | 描述                                                                                                                |
| ------------ | ------------------------------------------------------------------------------------------------------------------- |
| final        | 无法覆盖/修改属性和方法                                                                                             |
| static       | 属性和方法属于类，而不是对象                                                                                        |
| abstract     | 只能在抽象类中使用，并且只能在方法上使用。该方法没有主体，例如抽象 abstract void run();。主体由子类（继承自）提供。 |
| transient    | 序列化包含属性和方法的对象时，将跳过属性和方法                                                                      |
| synchronized | 方法一次只能由一个线程访问                                                                                          |
| volatile     | 属性值不是本地缓存的线程，总是从"主内存"中读取                                                                      |

## Java 包与 API

软件包分为两类：

### 内置包（来自 Java API 的包

该库分为包和类。这意味着您可以导入单个类（及其方法和属性），也可以导入包含属于指定包的所有类的整个包。

要使用库中的类或包，需要使用 import 关键字：

语法

```java
import package.name.Class;   // 导入 single 类
import package.name.*;   // 导入整个包
```

导入类
如果需找到要使用的类，例如用于获取用户输入的 Scanner 类，请编写以下代码：

```java
import java.util.Scanner;
```

### 用户定义的包（创建自己的包）
