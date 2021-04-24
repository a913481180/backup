---
title: JavaScript
date: 2020-09-22 21:22:11
catergories:
- study
---

### JavaScript
运行在客户端的脚本语言；不需要编译，运行过程中由js引擎逐行解释并执行,也可以用于后端node.js技术
浏览器引擎：
渲染引擎：内核
js引擎：js解释器

- 书写位置
  1. 行内式：`<input type="botton" onclick="alert('hello!!') />`Html推荐双引号，js推荐单引号
  2. 外部链接：`<script src="xxx.js"></script>`
  3. 内联式：`<script>alert('hello')</script>`

#### 输入输出语句
- alert(msg):浏览器弹窗提示
- console.log(msg):控制台打印信息
- prompt(info):浏览器弹出，用户输入

#### 变量

|情况|说明|结果|
|-|-|-|
|varage;console.log(age)|只声明不负值|undefined|
|console.log(age)|不声明，不负值|报错|
|age=10;console.log(age)|不声明，只赋值|10|

#### 命名规范

- 区分大小写
- 不能数字开头
- 可使用的符号作开头：下划线`_`，美元符`$`

#### 变量数据类型
动态数据类型。同一变量可做不同数据类型使用
类型种类：`number``boolean``string``undefined``null`
- 数字型
Infinity:无穷大；
-Infinity:无穷小；
NaN:非数字

- 字符串
使用单引号或双引号，推荐单引号
嵌套：`var str='啥"嘎"嘎和'; var str="航空'港行'供货商";`也可以加转义符`\`
字符串长度：`var str='画口红管工行';  alert(str.length);`
字符串拼接：`字符串+任何类型=拼接后的字符串`

- 布尔类型
`var flag=true;`
true参加数学加法时当做1，false为0；

- undefined
与数字相加后结果为NaN;

- typeof
`console.log(typeof age);`
查看数据类型

#### 数据类型转换

- 转为字符串

|||
|-|-|
|toString()|`var num=1;alert(num.toString());`|
|String()强制转换|`var num=1; alert(String(num));`|
|拼接|`var num=1; alert(num+"");`|

- 转数字

|||
|-|-|
|parseInt(string)保留整数,去掉单位|`parseInt('11');`|
|parseFloat(string)|`parseFloat('222');`|
|Number()强制转换|`Number('22');`|
|js隐式转换（减，乘，除)|`'11'-0`|

`==`符号会把字符串的数据类型转换为数字型再比较。
`===`两侧的值和数据类型一样才相等。

### 逻辑运算符

- 逻辑与
`表达式1&&表达式2`如果第一个表达式为真，则返回表达式2，否则返回表达式1

>`0` `''``null``undefind``NaN`为假

- 逻辑或
`表达式1||表达式2`如果表达式1为真则返回表达式1，否则返回表达式2


### 结构

```
//switch
var num=3;
switch(num){
case 2:
console.log(num);
break;
case 3:
console.log(num);
break;
default:
}
```

continue用于跳出本次循环，继续下一次循环；
break用于结束循环；

### 创建数组

```
//利用new创建数组
var a=new Array();
//数组字面量方式创建
var a=[];
var a=['xxx',77,true];
//获取数组元素：数组名[索引号]
console.log(a[0]);
//数组长度：数组名.length
console.log(a.length);
//新增数组元素
//修改数组索引追加数组元素
//不能直接给数组名赋值，否则会覆盖以前的数据
```

### 函数
- 声明
```
//函数关键字声明
function a(){
console.log('xxxxxxxxx');
}
//函数表达式声明
var b=function(){};
//function 函数名（形参1，形参2...）{//形参即形式上的参数}
function b(n){
console.log(n);
}
b('xxx');
//实参与形参个数一致时，正常输出
//实参小于形参时，多余的形参可看作不用声明的变量，结果为undefined
//实参多于形参时，只取到形参的个数
//#######################################
//函数的返回值
function c(){
return 返回的内容；
}
var n=c();
//return 之后的代码不会执行；
//return只返回一个值，且只返回最后一个值。
//return可返回数组，字符串，
//如果没有return则返回undefind
//###########
function fn(){
console.log(arguments);
}
fn(1,2,2,34,9);
//arguments里储存了所有传过来的实参，它是一个伪数组，具有length属性，可用索引的方式遍历，但不具有数组的push，pop等方法
```

### 作用域
```
//js中没有块级作用域，如{} if{} for{}，只有全局作用域和局部作用域
//作用域链：内部函数访问外部函数变量，采用的是链式查找方式来取值的，其采取就近原则
var num=10;
function fn (){
var num=20;
function fun(){
console.log(num)}
}

//js引擎运行分两步
//预解析:js引擎会把js里的所有var， function提升到当前作用域的最前面，但不提升赋值操作
//代码执行：顺序从上往下执行
fun();
var fun=function(){xxx};//会报错
//-------------
fun();
function fun(){xxx}//正常运行
```
