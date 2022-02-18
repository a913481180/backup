---
title: PHP
date: 2021-03-01 20:22:22
categories:
- web
---

# PHP

（全称：PHP：Hypertext Preprocessor，即"PHP：超文本预处理器"）是一种通用开源脚本语言。PHP 代码在服务器上执行，结果以纯 HTML 形式返回给浏览器

## PHP 语法

- PHP 脚本可以放在文档中的任何位置。
- PHP 脚本以` <?php` 开始，以 `?>` 结束
- 通过 PHP，有两种在浏览器输出文本的基础指令：`echo` 和 `print`。echo - 可以输出一个或多个字符串.print - 只允许输出一个字符串，返回值总为 1.可以使用括号，也可以不使用括号
- 可用`#`注释单行语句

## PHP 变量规则

- 变量以` $` 符号开始，后面跟着变量的名称
- 变量名必须以字母或者下划线字符开始
- 变量名只能包含字母数字字符以及下划线（`A-z`、`0-9` 和` _` ）
- 变量名不能包含空格
- 变量名是区分大小写的（`$y `和 `$Y` 是两个不同的变量）
- PHP 没有声明变量的命令
- PHP 是一门弱类型语言,根据变量的值，自动把变量转换为正确的数据类型。

## PHP 变量作用域


- global


>在所有函数外部定义的变量，拥有全局作用域。除了函数外，全局变量可以被脚本中的任何部分访问，要在一个函数中访问一个全局变量，需要使用 global 关键字进行声明。

在 PHP 函数内部声明的变量是局部变量，仅能在函数内部访问：global 关键字用于函数内访问全局变量。

实例
```
<?php
$x=5;
$y=10;
function Test()
{
    global $x,$y;
    echo $x;
    $y=$x+$y;
}
myTest();
?>
```
PHP 将所有全局变量存储在一个名为 $GLOBALS[index] 的数组中。 index 保存变量的名称。这个数组可以在函数内部访问，也可以直接用来更新全局变量。


实例
```
<?php
$x=5;
$y=10;
function myTest()
{
    $GLOBALS['y']=$GLOBALS['x']+$GLOBALS['y'];
    echo $y;
} 
 
myTest();
?>
```
- static

当一个函数完成时，它的所有变量通常都会被删除。然而，有时候您希望某个局部变量不要被删除。
要做到这一点，请在您第一次声明变量时使用 static 关键字：然后，每次调用该函数时，该变量将会保留着函数前一次被调用时的值。
注释：该变量仍然是函数的局部变量。

```
<?php
function myTest()
{
    static $x=0;
    echo $x;
    $x++;
    echo PHP_EOL;    // 换行符
}
 
myTest();
myTest();
myTest();
?>
```

## PHP EOF(heredoc) 使用说明

PHP EOF(heredoc)是一种在命令行shell（如sh、csh、ksh、bash、PowerShell和zsh）和程序语言（像Perl、PHP、Python和Ruby）里定义一个字符串的方法。
1. 必须后接分号，否则编译通不过。
2. EOF 可以用任意其它字符代替，只需保证结束标识与开始标识一致。以` <<<EOF `开始标记开始，以 EOF 结束标记结束，结束标记必须顶头写，不能有缩进和空格，且在结束标记末尾要有分号 。只要保证开始标记和结束标记不在正文中出现即可
3. 位于开始标记和结束标记之间的变量可以被正常解析，但是函数则不可以。在 heredoc 中，变量不需要用连接符 . 或 , 来拼接，如下：
4. 开始标识可以不带引号。EOF 中是会解析 html 格式内容的，并且在双引号内的内容也有转义效果。
5. 当内容需要内嵌引号（单引号或双引号）时，不需要加转义符，本身对单双引号转义，加不加引号转义字符都有效。

实例
```
<?php
$name="jjj"
$a= <<<EOF
$name
<br/><h1>我的第一个标题</h1>
\$name
\t
jjjjj
EOF;
echo $a;
?>
```

## 数据类型

字符串, 整型(整数是一个没有小数的数字。可以是正数或负数,十进制， 十六进制（ 以 0x 为前缀）或八进制（前缀为 0）。),浮点型(浮点数是带小数部分的数字，或是指数形式。),布尔型,数组,对象,NULL 值.

## PHP 常量

(常量名不需要加 $ 修饰符,默认是全局变量)。

`bool define ( string $name , mixed $value , bool $case_insensitive)`
case_insensitive ：可选参数，如果设置为 TRUE，该常量则大小写不敏感。默认是大小写敏感的。
```
<?php

define ("PI",3.14);
echo PI;
?>
```

## 并置运算符

在 PHP 中，只有一个字符串运算符。并置运算符 `.` 用于把两个字符串值连接起来。

实例

```
<?php
$txt1="Hello world!";
$txt2="What a nice day!";
echo $txt1 . " " . $txt2;
?>
```
- strlen() 函数
strlen() 函数返回字符串的长度（字节数）。
`echo strlen("Hello world!");`

- strpos() 函数

strpos() 函数用于在字符串内查找一个字符或一段指定的文本。如果在字符串中找到匹配，该函数会返回第一个匹配的字符位置。如果未找到匹配，则返回 FALSE。第一个字符的位置是 0;
`strpos("Hello world!","world");`


## 运算符

- PHP7+ 版本新增整除运算符 intdiv()该函数返回值为第一个参数除于第二个参数的值并取整（向下取整）
`intdiv(10, 3)`

- 在 PHP7+ 版本多了一个 NULL 合并运算符 ??

```
// 如果 $_GET['user'] 不存在返回 'nobody'，否则返回 $_GET['user'] 的值
$username = $_GET['user'] ?? 'nobody';
```
- PHP7+ 支持组合比较符（combined comparison operator）也称之为太空船操作符，符号为 <=>。
组合比较运算符可以轻松实现两个变量的比较，当然不仅限于数值类数据的比较。相等返回0，大于返回1，小于返回-1；

语法格式如下：

`$c = $a <=> $b;`

## 数组

数组是一个能在单个变量中存储多个值的特殊变量。

在 PHP 中，array() 函数用于创建数组：
`$cars=array("Volvo","BMW","Toyota");`

获取数组的长度 - count() 函数

```
<?php
$cars=array("Volvo","BMW","Toyota");
echo count($cars);
?>
```

- PHP 关联数组

关联数组是使用您分配给数组的指定的键的数组。

这里有两种创建关联数组的方法：
实例
```
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
echo "Peter is " . $age['Peter'] . " years old.";
?>
```
or
```
$age['Peter']="35";
$age['Ben']="37";
$age['Joe']="43";
```
- 遍历关联数组
遍历并打印关联数组中的所有值，您可以使用 foreach 循环，如下所示：
foreach 循环用于遍历数组。

语法
```
foreach ($array as $value)
{
    要执行代码;
}
```
每进行一次循环，当前数组元素的值就会被赋值给 $value 变量（数组指针会逐一地移动），在进行下一次循环时，您将看到数组中的下一个值。
```
foreach ($array as $key => $value)
{
    要执行代码;
}
```
每一次循环，当前数组元素的键与值就都会被赋值给 $key 和 $value 变量（数字指针会逐一地移动），在进行下一次循环时，你将看到数组中的下一个键与值。

```
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
 
foreach($age as $x=>$x_value)
{
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
}
?>
```

## PHP - 数组排序函数
在本章中，我们将一一介绍下列 PHP 数组排序函数：

sort() - 对数组进行升序排列
rsort() - 对数组进行降序排列
asort() - 根据关联数组的值，对数组进行升序排列
ksort() - 根据关联数组的键，对数组进行升序排列
arsort() - 根据关联数组的值，对数组进行降序排列
krsort() - 根据关联数组的键，对数组进行降序排列

## PHP 超级全局变量
PHP中预定义了几个超级全局变量（superglobals） ，这意味着它们在一个脚本的全部作用域中都可用。 你不需要特别说明，就可以在函数及类中使用。

PHP 超级全局变量列表:

- $GLOBALS
$GLOBALS 是PHP的一个超级全局变量组，在一个PHP脚本的全部作用域中都可以访问。

- $_SERVER
$_SERVER 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目由 Web 服务器创建。不能保证每个服务器都提供全部项目；
下表列出了所有 $_SERVER 变量中的重要元素:

|元素/代码|	描述|
|-|-|
|$_SERVER['PHP_SELF']|	当前执行脚本的文件名，与 document root 有关。例如，在地址为 http://example.com/test.php/foo.bar 的脚本中使用 $_SERVER['PHP_SELF'] 将得到 /test.php/foo.bar。__FILE__ 常量包含当前(例如包含)文件的完整路径和文件名。 从 PHP 4.3.0 版本开始，如果 PHP 以命令行模式运行，这个变量将包含脚本名。之前的版本该变量不可用。|
|$_SERVER['GATEWAY_INTERFACE']|	服务器使用的 CGI 规范的版本；例如，"CGI/1.1"。|
|$_SERVER['SERVER_ADDR']|	当前运行脚本所在的服务器的 IP 地址。|
|$_SERVER['SERVER_NAME']|	当前运行脚本所在的服务器的主机名。如果脚本运行于虚拟主机中，该名称是由那个虚拟主机所设置的值决定。(如: www.runoob.com)|
|$_SERVER['SERVER_SOFTWARE']|	服务器标识字符串，在响应请求时的头信息中给出。 (如：Apache/2.2.24)|
|$_SERVER['SERVER_PROTOCOL']|	请求页面时通信协议的名称和版本。例如，"HTTP/1.0"。|
|$_SERVER['REQUEST_METHOD']|	访问页面使用的请求方法；例如，"GET", "HEAD"，"POST"，"PUT"。|
|$_SERVER['REQUEST_TIME']|	请求开始时的时间戳。从 PHP 5.1.0 起可用。 (如：1377687496)|
|$_SERVER['QUERY_STRING']|	query string（查询字符串），如果有的话，通过它进行页面访问。|
|$_SERVER['HTTP_ACCEPT']|	当前请求头中 Accept: 项的内容，如果存在的话。|
|$_SERVER['HTTP_ACCEPT_CHARSET']|	当前请求头中 Accept-Charset: 项的内容，如果存在的话。例如："iso-8859-1,*,utf-8"。|
|$_SERVER['HTTP_HOST']|	当前请求头中 Host: 项的内容，如果存在的话。|
|$_SERVER['HTTP_REFERER']|	引导用户代理到当前页的前一页的地址（如果存在）。由 user agent 设置决定。并不是所有的用户代理都会设置该项，有的还提供了修改 HTTP_REFERER 的功能。简言之，该值并不可信。)|
|$_SERVER['HTTPS']|	如果脚本是通过 HTTPS 协议被访问，则被设为一个非空的值。|
|$_SERVER['REMOTE_ADDR']|	浏览当前页面的用户的 IP 地址。|
|$_SERVER['REMOTE_HOST']|	浏览当前页面的用户的主机名。DNS 反向解析不依赖于用户的 REMOTE_ADDR。|
|$_SERVER['REMOTE_PORT']|	用户机器上连接到 Web 服务器所使用的端口号。|
|$_SERVER['SCRIPT_FILENAME']|	当前执行脚本的绝对路径。|
|$_SERVER['SERVER_ADMIN']|	该值指明了 Apache 服务器配置文件中的 SERVER_ADMIN 参数。如果脚本运行在一个虚拟主机上，则该值是那个虚拟主机的值。(如：someone@runoob.com)|
|$_SERVER['SERVER_PORT']|	Web 服务器使用的端口。默认值为 "80"。如果使用 SSL 安全连接，则这个值为用户设置的 HTTP 端口。|
|$_SERVER['SERVER_SIGNATURE']|	包含了服务器版本和虚拟主机名的字符串。|
|$_SERVER['PATH_TRANSLATED']|	当前脚本所在文件系统（非文档根目录）的基本路径。这是在服务器进行虚拟到真实路径的映像后的结果。|
|$_SERVER['SCRIPT_NAME']|	包含当前脚本的路径。这在页面需要指向自己时非常有用。__FILE__ 常量包含当前脚本(例如包含文件)的完整路径和文件名。|
|$_SERVER['SCRIPT_URI']|	URI 用来指定要访问的页面。例如 "/index.html"。|

- $_REQUEST

$_REQUEST 用于收集HTML表单提交的数据。

以下实例显示了一个输入字段（input）及提交按钮(submit)的表单(form)。 当用户通过点击 "Submit" 按钮提交表单数据时, 表单数据将发送至<form>标签中 action 属性中指定的脚本文件。 在这个实例中，我们指定文件来处理表单数据。如果你希望其他的PHP文件来处理该数据，你可以修改该指定的脚本文件名。 然后，我们可以使用超级全局变量 $_REQUEST 来收集表单中的 input 字段数据:
```
<html>
<body>
<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
Name: <input type="text" name="fname">
<input type="submit">
</form>
<?php 
$name = $_REQUEST['fname']; 
echo $name; 
?>
</body>
</html>
```
- $_POST

$_POST 被广泛应用于收集表单数据，在HTML form标签的指定该属性："method="post"。

以下实例显示了一个输入字段（input）及提交按钮(submit)的表单(form)。 当用户通过点击 "Submit" 按钮提交表单数据时, 表单数据将发送至<form>标签中 action 属性中指定的脚本文件。
```
<?php 
$name = $_POST['fname']; 
echo $name; 
?>
```
- $_GET
 $_GET 同样被广泛应用于收集表单数据，在HTML form标签的指定该属性："method="get"。
$_GET 也可以收集URL中发送的数据。
`<a href="test_get.php?subject=PHP&web=runoob.com">Test $GET</a>`
当用户点击链接 "Test $GET", 参数 "subject" 和 "web" 将发送至"test_get.php",你可以在 "test_get.php" 文件中使用 $_GET 变量来获取这些数据。

以下实例显示了 "test_get.php" 文件的代码:
```
<?php 
echo "Study " . $_GET['subject'] . " @ " . $_GET['web'];
?>
```
- $_FILES
- $_ENV
- $_COOKIE
- $_SESSION

## 函数

语法
```
<?php
function functionName()
{
    // 要执行的代码
}
?>
```

## 魔术常量

PHP 向它运行的任何脚本提供了大量的预定义常量。
不过很多常量都是由不同的扩展库定义的，只有在加载了这些扩展库时才会出现，或者动态加载后，或者在编译时已经包括进去了。
有八个魔术常量它们的值随着它们在代码中的位置改变而改变。

- \__LINE__
文件中的当前行号。

- \__FILE__
文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。自 PHP 4.0.2 起，\__FILE__ 总是包含一个绝对路径（如果是符号连接，则是解析后的绝对路径），

- \__DIR__
文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录。

- \__FUNCTION__
函数名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该函数被定义时的名字

- \__CLASS__
类的名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该类被定义时的名字（区分大小写）。

- \__TRAIT__
Trait 的名字（PHP 5.4.0 新加）。自 PHP 5.4.0 起，PHP 实现了代码复用的一个方法，称为 traits。

- \__METHOD__
类的方法名（PHP 5.0.0 新加）。返回该方法被定义时的名字（区分大小写）。

- \__NAMESPACE__
当前命名空间的名称（区分大小写）。此常量是在编译时定义的（PHP 5.3.0 新增）。

PHP 命名空间可以解决以下两类问题：

1. 用户编写的代码与PHP内部的类/函数/常量或第三方类/函数/常量之间的名字冲突。
2. 为很长的标识符名称(通常是为了缓解第一类问题而定义的)创建一个别名（或简短）的名称，提高源代码的可读性。

定义命名空间
默认情况下，所有常量、类和函数名都放在全局空间下，就和PHP支持命名空间之前一样。

命名空间通过关键字namespace 来声明。如果一个文件中包含命名空间，它必须在其它所有代码之前声明命名空间。语法格式如下；
```
<?php  
// 定义代码在 'MyProject' 命名空间中  
namespace MyProject;  
 
// ... 代码 ...  

```
也可以在同一个文件中定义不同的命名空间代码，如：

```
<?php  
namespace MyProject;

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }

namespace AnotherProject;

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
?>  
```
不建议使用这种语法在单个文件中定义多个命名空间。建议使用下面的大括号形式的语法。
```
<?php
namespace MyProject {
    const CONNECT_OK = 1;
    class Connection { /* ... */ }
    function connect() { /* ... */  }
}

namespace AnotherProject {
    const CONNECT_OK = 1;
    class Connection { /* ... */ }
    function connect() { /* ... */  }
}
?>
```

将全局的非命名空间中的代码与命名空间中的代码组合在一起，只能使用大括号形式的语法。全局代码必须用一个不带名称的 namespace 语句加上大括号括起来，例如：

```
<?php
namespace MyProject {

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
}

namespace { // 全局代码
session_start();
$a = MyProject\connect();
echo MyProject\Connection::start();
}
?>
```

在声明命名空间之前唯一合法的代码是用于定义源文件编码方式的 declare 语句。所有非 PHP 代码包括空白符都不能出现在命名空间的声明之前。

```
<?php
declare(encoding='UTF-8'); //定义多个命名空间和不包含在命名空间中的代码
namespace MyProject {

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
}

namespace { // 全局代码
session_start();
$a = MyProject\connect();
echo MyProject\Connection::start();
}
?>
```

## 子命名空间
与目录和文件的关系很像，PHP 命名空间也允许指定层次化的命名空间的名称。因此，命名空间的名字可以使用分层次的方式定义：

```
<?php
namespace MyProject\Sub\Level;  //声明分层次的单个命名空间

const CONNECT_OK = 1;
class Connection { /* ... */ }
function Connect() { /* ... */  }

?>
```

命名空间使用
PHP 命名空间中的类名可以通过三种方式引用：

- 非限定名称，或不包含前缀的类名称，例如 $a=new foo(); 或 foo::staticmethod();。如果当前命名空间是 currentnamespace，foo 将被解析为 currentnamespace\foo。如果使用 foo 的代码是全局的，不包含在任何命名空间中的代码，则 foo 会被解析为foo。 警告：如果命名空间中的函数或常量未定义，则该非限定的函数名称或常量名称会被解析为全局函数名称或常量名称。

```
<?php
namespace Foo\Bar\subnamespace; 

const FOO = 1;
function foo() {}
class foo
{
    static function staticmethod() {}
}
?>
```

- 限定名称,或包含前缀的名称，例如 $a = new subnamespace\foo(); 或 subnamespace\foo::staticmethod();。如果当前的命名空间是 currentnamespace，则 foo 会被解析为 currentnamespace\subnamespace\foo。如果使用 foo 的代码是全局的，不包含在任何命名空间中的代码，foo 会被解析为subnamespace\foo。

```
<?php
namespace Foo\Bar;
include 'file1.php';

const FOO = 2;
function foo() {}
class foo
{
    static function staticmethod() {}
}

/* 非限定名称 */
foo(); // 解析为函数 Foo\Bar\foo
foo::staticmethod(); // 解析为类 Foo\Bar\foo ，方法为 staticmethod
echo FOO; // 解析为常量 Foo\Bar\FOO

/* 限定名称 */
subnamespace\foo(); // 解析为函数 Foo\Bar\subnamespace\foo
subnamespace\foo::staticmethod(); // 解析为类 Foo\Bar\subnamespace\foo,
                                  // 以及类的方法 staticmethod
echo subnamespace\FOO; // 解析为常量 Foo\Bar\subnamespace\FOO
                                  
/* 完全限定名称 */
\Foo\Bar\foo(); // 解析为函数 Foo\Bar\foo
\Foo\Bar\foo::staticmethod(); // 解析为类 Foo\Bar\foo, 以及类的方法 staticmethod
echo \Foo\Bar\FOO; // 解析为常量 Foo\Bar\FOO
?>
```
注意访问任意全局类、函数或常量，都可以使用完全限定名称，例如 \strlen() 或 \Exception 或 \INI_ALL。

在命名空间内部访问全局类、函数和常量：
```
<?php
namespace Foo;

function strlen() {}
const INI_ALL = 3;
class Exception {}

$a = \strlen('hi'); // 调用全局函数strlen
$b = \INI_ALL; // 访问全局常量 INI_ALL
$c = new \Exception('error'); // 实例化全局类 Exception
?>
```
- 完全限定名称，或包含了全局前缀操作符的名称，例如， $a = new \currentnamespace\foo(); 或 \currentnamespace\foo::staticmethod();。在这种情况下，foo 总是被解析为代码中的文字名(literal name)currentnamespace\foo。
