---
title: JavaScript语法
date: 2021-05-22 21:23:11
categories:
- web 
---

## JavaScript

运行在客户端的脚本语言；不需要编译，运行过程中由js引擎逐行解释并执行,也可以用于后端node.js技术
浏览器引擎：
渲染引擎：内核
js引擎：js解释器

- 面向对象编程

类：一类具有相同特征事物的抽象概念;
对象：具体的某一个个体，具有唯一性的实例；

编程思想：

   1. 分析有哪些实体
   2. 设计实体的属性和功能
   3. 实体间的相互作用

- 书写位置
  1. 行内式：`<input type="botton" onclick="alert('hello!!') />`Html推荐双引号，js推荐单引号

>注释：外部脚本不能包含 `<script>` 标签。  
>优势：分离了 HTML 和代码,使 HTML 和 JavaScript 更易于阅读和维护,已缓存的 JavaScript 文件可加速页面加载  
>可通过完整的 URL 或相对于当前网页的路径引用外部脚本：`<script src="www.xxx.com/.."><script>`

  2. 外部链接：`<script src="xxx.js"></script>`,标签中间不能写代码，会忽略
  3. 内联式：`<script>alert('hello')</script>`

>js脚本可被放置与 HTML 页面的 `<body>` 或`<head>` 部分中，或兼而有之。  
>提示：把脚本置于`<body>` 元素的底部，可改善显示速度，因为脚本编译会拖慢显示。  
>注释：旧的 JavaScript 例子也许会使用 type 属性：`<script type="text/javascript">`。  
>注释：type 属性不是必需的。JavaScript 是 HTML 中的默认脚本语言。

### 输入输出语句

- alert(msg):浏览器弹窗提示
- console.log(msg):控制台打印信息
- innerHTML:写入HTML元素

>innerHTML 和 outerHTML 的区别：1）innerHTML:从对象的起始位置到终止位置的全部内容, 不包括HTML标签。2）outerHTML:除了包含innerHTML的全部内容外, 还包含对象标签本身。
>如需访问 HTML 元素，JavaScript 可使用 document.getElementById(id) 方法。如：`document.getElementById("demo").innerHTML = 5 + 6;`

- document.write()：写入HTML输出

>会解析标签
>注意：在 HTML 文档完全加载后使用 document.write() 将会删除所有的标签 ;

- prompt(info):浏览器弹出，用户输入

### 堆栈

#### 栈

 由编译器自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。
 基本数据类型变量保存在栈内存中，因为基本数据类型占用空间小、大小固定，通过值来访问，属于被频繁使用的数据。
>闭包中的基本数据类型变量是保存在堆内存里的，当函数执行完弹出调用栈后，返回一个内部函数的一个引用，这时候函数的变量就会转移到堆上，因此内部函数依然能访问到上一层函数的变量。

#### 堆

一般由程序员分配释放， 若程序员不释放，程序结束时可能由OS回收 。注意它与数据结构中的堆是两回事，分配方式倒是类似于链表
引用数据类型存储在堆内存中，引用数据类型占据空间大、大小不固定，如果存储在栈中，将影响程序的运行性能。
引用数据类型会在栈中存储一个指针，这个指针指向堆内存空间中该实体的起始地址。
当解释器寻找引用值时，会先检索其在栈中的地址，取得地址后，从堆中获得实体。

### 变量

变量是弱类型的,重复声明 JavaScript  变量，将不会丢它的值。

|情况|说明|结果|
|-|-|-|
|var age;console.log(age)|只声明不负值|undefined|
|console.log(age)|不声明，不负值|报错|
|age=10;console.log(age)|不声明，只赋值|10(如果您为尚未声明的变量赋值，此变量会自动成为全局变量。)|

可以在字符串添加转义字符来使用引号：

如果变量在函数内没有声明（没有使用 var 关键字），该变量为全局变量。

#### 命名规范

- 区分大小写
- 不能数字开头
- 可使用的符号作开头：下划线`_`，美元符`$`

### 数据类型

动态数据类型。同一变量可做不同数据类型使用

基本数据类型：`number` `boolean` `string` `undefined` `null`,`BigInt`,`Symbol`
复杂(引用)数据类型:`object`

#### 查看数据类型

- typeof

typeof 运算符不是变量。它属于运算符。运算符（比如 + - * /）没有数据类型。
但是，typeof 始终会返回字符串（包含运算数的类型）。

`console.log(typeof age);`
`console.log(typeof(age));`

- instanceof运算符
instanceof用于判断某个对象是否被另一个函数构造。
>只能判断复杂类型
instanceof，用于检测某个对象的原型链是否包含某个构造函数的 prototype 属性。
instanceof 适用于检测对象，它是基于原型链运作的。
instanceof 除了适用于任何 object 的类型检查之外，也可以用来检测内置对象，比如：Array、RegExp、Object、Function
instanceof 对基本数据类型检测不起作用，主要是因为基本数据类型没有原型链。

- constructor 属性
>每个原型对象上都有个constructor属性,指向该原型对象的构造函数
constructor 属性返回所有 JavaScript 变量的构造器函数。
`undefined`,`null`没有constructor

实例

```js
"Bill".constructor                 // 返回 "function String()  { [native code] }"
(3.14).constructor                 // 返回 "function Number()  { [native code] }"
false.constructor                  // 返回 "function Boolean() { [native code] }"
[1,2,3,4].constructor              // 返回 "function Array()   { [native code] }"
{name:'Bill', age:62}.constructor  // 返回" function Object()  { [native code] }"
new Date().constructor             // 返回 "function Date()    { [native code] }"
function () {}.constructor         // 返回 "function Function(){ [native code] }"
```

#### 布尔类型

`var flag=true;`
true参加数学加法时当做1，false为0；

true:`-15` `"false"`
false:`0` `-0` `""` `undefined` `null`  `false`  `NaN`

#### `undefined`与`null`

undefined 表示尚未初始化的变量的值,这个变量从根本上就没有定义，而 null 表示该变量有意缺少对象指向,这个值虽然定义了，但它并未指向任何内存中的对象

```js
null == undefined  // true
null === undefined  // false
!!null === !!undefined  // true

JSON.stringify({a: undefined})  // '{}'
JSON.stringify({b: null})  // '{b: null}'
JSON.stringify({a: undefined, b: null})  // '{b: null}'

let obj1 = { a: undefined }
let obj2 = {}
console.log(obj1.a)  // undefined
console.log(obj2.a)  // undefined
```

`undefined`与数字相加后结果为`NaN`;而`null`会隐式转换为0
`undefined`其类型也将是`undefined`。而`null`的类型为对象。
>ypeof null 输出为 'object' 其实是一个底层的错误，但直到现阶段都无法被修复。
>原因是，在 JavaScript 初始版本中，值以 32位 存储。前 3位 表示数据类型的标记，其余位则是值。
>对于所有的对象，它的前 3位 都以 000 作为类型标记位。在 JavaScript 早期版本中， null 被认为是一个特殊的值，用来对应 C 中的 空指针 。但 JavaScript 中没有 C 中的指针，所以 null 意味着什么都没有或者 void 并以 全0(32个) 表示。
>因此每当 JavaScript 读取 null 时，它前端的 3位 将它视为 对象类型 ，这也是为什么 typeof null 返回 'object' 的原因。
任何变量均可通过设置值等于 `undefined` （`void 0`） 或`null`进行清空。

#### 数字型

绝不要用前导零写数字（比如 07）。
`Infinity`:无穷大；
`-Infinity`:无穷小；
除以 0（零）也会生成 Infinity

```js
var x =  2 / 0;          // x 将是 Infinity
var y = -2 / 0;          // y 将是 -Infinity
```

##### `NaN`:非数字
>
>NaN 是数，typeof NaN 返回 number
尝试用一个非数字字符串进行除法会得到 NaN
可使用全局 JavaScript 函数 `isNaN()` 来确定某个值是否是数：

JavaScript 数值始终是 `64 位`的浮点数,其中 0 到 51 存储数字（片段），52 到 62 存储指数，63 位存储符号

##### 精度

整数（不使用指数或科学计数法）会被精确到 `15 位`。
小数的最大数是 `17 位`，但是浮点的算数并不总是 100% 精准

##### 数字字符串

JavaScript 字符串可以拥有数字内容：

在所有数字运算中，JavaScript 会尝试将字符串转换为数字：

```js
var x = "100";
var y = "10";
var z = x / y;       // z 将是 10
var z = x * y;       // z 将是 1000
var z = x - y;       // z 将是 90
//警告！！
//JavaScript 的加法和级联（concatenation）都使用 + 运算符。
//如果您对一个数和一个字符串相加，结果也是字符串级联：
var z = x + y;       // z 不会是 110（而是 10010）
```

##### 数值属性

|属性 |描述|
|-|-|
|MAX_VALUE |返回 JavaScript 中可能的最大数。|
|MIN_VALUE |返回 JavaScript 中可能的最小数。|
|NEGATIVE_INFINITY |表示负的无穷大（溢出返回）。|
|NaN |表示非数字值（"Not-a-Number"）。|
|POSITIVE_INFINITY |表示无穷大（溢出返回）。|

>数字属性不可用于变量
>数字属性属于名为 number 的 JavaScript 数字对象包装器。
>这些属性只能作为 Number.MAX_VALUE 访问。
>使用 myNumber.MAX_VALUE，其中 myNumber 是变量、表达式或值，将返回 undefined：

实例

```js
var x = 6;
var y = x.MAX_VALUE;    // y 成为 undefined
//只能如下使用
var x = Number.MIN_VALUE; //返回 JavaScript 中可能的最小数字。
```

#### 字符串

使用单引号或双引号，推荐单引号。在js中字符串既是基本数据类型，又是复合数据类型.

##### 声明

   1. new运算符声明(对象）

      ```js
      var str=new String("100")
      ```

   2. 省略new

      ```js
      var str=String("jjj");
      ```

   3. 字符串常量声明

      ```js
      var str="jjj";
      ```

 嵌套：`var str='啥"嘎"嘎和'; var str="航空'港行'供货商";`也可以加转义符`\`
 字符串长度：`var str='画口红管工行';  alert(str.length);`
>注：中文UTF-8（三个字符代表一个汉字），gbk（两个字符表示一个汉字）但计数时都当成一个字来计数。

字符串拼接：`字符串+任何类型=拼接后的字符串`

#### js 判断字符串中是否包含某个字符串

- `indexOf()` 与`lastIndexOf`
   `indexOf()` 方法可返回某个指定的字符串值在字符串中首次出现的位置。（从0开始，未找到返回-1）
   `lastIndexOf()` 方法返回指定文本在字符串中最后一次出现的索引,其为从后向前进行检索
   两种方法都接受作为检索起始位置的第二个参数。如`str.indexOf("China", 18);`

      ```js
      var str = "123";
      console.log(str.indexOf("3",0) != -1 );  // 未找到文本将返回-1
      ```
- `include(str,position)`
   是否可以在另一个字符串中找到一个字符串，并根据情况返回 true 或 false
  方法接受作为检索起始位置的第二个参数。如`str.include("China", 18);`

      ```js
      var str = "123";
      console.log(str.indexOf("3",0) != -1 );  // 未找到文本将返回-1
      ```

- `search()`
search() 方法用于检索字符串中指定的子字符串，或与正则表达式相匹配的子字符串，首次出现的位置。如果没有找到任何匹配的子串，则返回 -1。

>传入的不是正则会隐式转换为正则

      ```js
      var str = "123";
      console.log(str.search("3") != -1 );  //并返回匹配位置的下标
      ```

- `match()`

用于检索字符串中指定的子字符串，或正则表达式相匹配的子串数组,否则返回null

      ```js
      var str = "123";
      var reg = RegExp(/3/);
      if(str.match(reg)){
          // 包含        
      }
      ```

- `test()`
`test()` 方法用于检索字符串中匹配的正则是否存在。若有则返回 true 否则 false。

      ```js
      var str = "123";
      var reg = RegExp(/3/);
      console.log(reg.test(str)); // true
      ```

- `exec()`
exec() 方法用于检索字符串中的正则表达式的匹配。返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。

      ```js
      var str = "123";
      var reg = RegExp(/3/);
      if(reg.exec(str)){
          // 包含        
      }
      ```

#### 提取部分字符串

- `slice(start, end)`
提取`[start,end)`这部分字符串，返回新的字符串
如果某个参数为负，则从字符串的结尾开始计数。
如果省略第二个参数，则该 `slice()` 将删除前面的部分，保留后面的部分。
提示：负值位置不适用 Internet Explorer 8 及其更早版本。

```js
var str = "Apple, Banana, Mango";
var res = str.slice(7,13);
```

- `substring(start, end)`

`substring()` 类似于 `slice()`。提取`[start,end)`这部分字符串，返回新的字符串
不同之处在于 `substring()`无法接受负的索引，后一个值大于前一个时会调换参数。
如果省略第二个参数，则该`substring()` 将删除前面的部分，保留后面的部分。

- `substr(start, length)`(废弃)
不同之处在于第二个参数规定被提取部分的长度。
如果首个参数为负，则从字符串的结尾计算位置。第二个参数不能为负，因为它定义的是长度。
如果省略第二个参数，则该 substring() 将删除前面的部分，保留后面的部分。

#### 替换文本

- `replace()`

`str.replace(oldstr,newstr);`该方法用另一个值替换在字符串中指定的值,会返回替换成功的新字符串：

```js
str = "Please visit Microsoft!";
var n = str.replace("Microsoft", "W3School");
```

默认地，`replace()` 只替换首个匹配：如需替换所有匹配，请使用正则表达式的 `g` 标志（用于全局搜索)
`replace()` 对大小写敏感。如需执行大小写不敏感的替换，请使用正则表达式 `i`（大小写不敏感）

```js
str = "Please visit Microsoft!";
var n = str.replace(/MICROSOFT/gi, "W3School");
```

#### 转换为大写和小写

通过 `toUpperCase()` 把字符串转换为大写：
通过 `toLowerCase()` 把字符串转换为小写：

```js
var text1 = "Hello World!";       // 字符串
var text2 = text1.toUpperCase();  // text2 是被转换为大写的 text1
```

#### concat() 连接两个或多个字符串

```js
var text1 = "Hello";
var text2 = "World";
text3 = text1.concat(text2);
text4 = text1.concat("jjjjjj",text2,"jj");
```

#### 删除字符串两端的空白符

`trim()` 方法删除字符串两端的空白符
`trimEnd()` 方法删除字符串末尾的空白符
`trimStart()` 方法删除字符串开头的空白符

```js
var str = "       Hello World!        ";
alert(str.trim());
//Internet Explorer 8 或更低版本不支持 trim() 方法。
//如需支持 IE 8，您可搭配正则表达式使用 replace() 方法代替：
var str = "       Hello World!        ";
alert(str.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, ''));
```

#### 提取字符串中的一个字符

这是两个提取字符串字符的安全方法：

- charAt(position)
charAt() 方法返回字符串中指定下标（位置）的字符串：

```js
var str = "HELLO WORLD";
str.charAt(0);            // 返回 H
```

- charCodeAt(position)

charCodeAt() 方法返回字符串中指定索引的字符 unicode 编码：

```js
var str = "HELLO WORLD";
str.charCodeAt(0);         // 返回 72
```

- String.fromCharCode(码值1，码值2...）
用于将ASCII码转换为对应字符,返回值为组合的字符串

```js
var str=String.fromCharCode(99,93,83);
```

- 属性访问（Property Access）

使用属性访问有点不太靠谱：

 不适用 Internet Explorer 7 或更早的版本
 它让字符串看起来像是数组（其实并不是）
 如果找不到字符，[ ] 返回 undefined，而 charAt() 返回空字符串。
 字符串是只读的，无法像数组一样通过索引下标去修改单个字符，除非整个修改。str[0] = "A" 不会产生错误（但也不会工作！）

>提示：如果您希望按照数组的方式处理字符串，可以先把它转换为数组。

```js
var str = "HELLO WORLD";
str[0];                   // 返回 H
```

- 把字符串转换为数组

可以通过 split(分割符/正则,限制返回的数量)分割字符串，返回分割的子串组成的数组。

```js
var txt = "a,b,c,d,e";   // 字符串
txt.split(",");          // 用逗号分隔
txt.split(" ");          // 用空格分隔
txt.split("|");          // 用竖线分隔
//如果省略分隔符，被返回的数组将是整个字符串即["string"]。
//若没有匹配，被返回的数组也是整个字符串即["string"]
//如果分隔符是 ""，被返回的数组将是间隔单个字符的数组
```

##### 正则表达式

正则表达式是构成搜索模式（search pattern）的字符序列。
语法：`/pattern/modifiers;`如：`var patt = /hello/i;`

###### 声明

1. new声明

参数：第一个为字符串，第二个为修饰符(修饰符无顺序）

```js
var box1=new RegExp("hello","ig");
var box1= RegExp("hello","ig");
```

2. 省略new声明

```js
var box1=/hello/gi;
```

正则表达式修饰符
修饰符可用于大小写不敏感的更全局的搜素：

|修饰符 |描述|
|-|-|
|i |执行对大小写不敏感的匹配。|
|g |执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。|
|m |执行多行匹配。|

>字符串中，遇到换行，重新开始计算行首；

正则表达式模式
括号用于查找一定范围的字符串：

|表达式 |描述|
|-|-|
|`[]` |查找方括号之间的任何符合的字符。|
|`[^]`|匹配除括号内的字符|
|`[0-9]` |查找任何从 0 至 9 的数字。|
|`(x|y)` |查找由 `|` 分隔的任何选项。|

元字符（Metacharacter）是拥有特殊含义的字符：

|元字符 |描述|
|-|-|
|.|匹配单个任意字符|
|\\d |查找数字。|
|\\D|匹配非数字|
|\\s |查找空白符、空格、制表符、换行符。|
|\\S|匹配非空白字符|
|\\b |匹配空格字符。|
|\\n |查找换行符。|
|\\r |查找回车符。|
|\\t |查找制表符。|
|\\0 |查找null符。|

锚字符
|||
|-|-|
|^|行首匹配|
|$|行尾匹配|

重复字符：(n为任意的单个字符）

|量词 |描述|
|-|-|
|n? |匹配任何包含零个或一个 n 的字符。|
|n+ |匹配任何包含至少一个 n 的字符。|
|n* |匹配任何包含零个或多个 n 的字符。|
|n{m,n}|匹配至少m个，至多n个|
|n{m}|必须匹配m个|
|(abd)+|小括号中的部分代表一个字符串处理|

#### 数据类型转换

##### 数字转为字符串

|||
|-|-|
|toString()|`var num=1;alert(num.toString());`|
|String()强制转换|`var num=1; alert(String(num));`|
|`+`拼接，只要有一边是字符串，另一半也会转为字符串|`var num=1; alert(num+"");`|

- 能够使用 toString() 方法把数输出为十六进制、八进制或二进制。

```js
var myNumber = 128;
myNumber.toString(16);     // 返回 80
myNumber.toString(8);      // 返回 200
myNumber.toString(2);      // 返回 10000000
```

- toExponential() 返回字符串值，它包含已被四舍五入并使用指数计数法的数字。

```js
var x = 9.656;
x.toExponential(2);     // 返回 9.66e+0
x.toExponential(4);     // 返回 9.6560e+0
x.toExponential(6);     // 返回 9.656000e+0
```

- toFixed() 返回字符串值，它包含了指定位数小数的数字：

```js
var x = 9.656;
x.toFixed(0);           // 返回 10
x.toFixed(2);           // 返回 9.66
x.toFixed(4);           // 返回 9.6560
x.toFixed(6);           // 返回 9.656000
```

- toPrecision() 返回字符串值，它包含了指定长度的数字：

```js
var x = 9.656;
x.toPrecision();        // 返回 9.656
x.toPrecision(2);       // 返回 9.7
x.toPrecision(4);       // 返回 9.656
x.toPrecision(6);       // 返回 9.65600
```

>在 JavaScript 中，数字可以是原始值（typeof = number）或对象（typeof = object）。
>在 JavaScript 内部使用 valueOf() 方法可将 Number 对象转换为原始值。
>所有 JavaScript 数据类型都有 valueOf() 和 toString() 方法。

##### 字符串转数字

|||
|-|-|
|`parseInt(string)`保留整数,去掉单位;只返回首个数字;如果无法转换为数值，则返回 NaN|`parseInt('11');`|
|`parseFloat(string)`只返回首个数字,包括小数部分|`parseFloat('222');`|
|`Number()`强制转换|`Number('22');`|
|js隐式转换（减，乘，除)|`'11'-0`|

>Number() 还可以把日期转换为数字：`Number(new Date("2019-04-15"));    // 返回 1506729600000`方法返回 1970 年 1 月 1 日至今的毫秒数。

`==`符号会把字符串的数据类型转换为数字型再比较。
`===`两侧的值和数据类型一样才相等。

### 运算符

#### 一元操作符

- 自增和自减操作符`--`,`++`
`++i,i++`单独使用没有区别

```js
//先加再用
let i=1
console.log(++i + 2)//4
//先用再加
let a=1
console.log(a++ + 2)//3

let b=1
console.log(b++ + 3 ++b)//7
```

- 一元加减操作符

```js
let a = -1;
let b = "hello";
let c = BigInt;
console.log(+a)  // -1
console.log(+b)  // NaN
console.log(+c)  // NaN

let a = -1;
let b = 2;
console.log(-a)  // 1
console.log(-b)  // -2
```

#### 逻辑运算符

- 逻辑非

如果操作数是对象，则返回 false；
如果操作数是空字符串，则返回 true；
如果操作数是非空字符串，则返回 false；
如果操作数是数值0，则返回 true；
如果操作数是非0数值（包括 Infinity)，则返回 false；
如果操作数是 null，则返回 true；
如果操作数是 NaN，则返回 true；
如果操作数是 undefined， 则返回 true.

逻辑非操作符也可以用于将任何值转化为布尔值，同时使用两个!，相当于调用了Boolean()方法：

```js
!!"blue" // true
!!0;     // false
!!NaN    // false
!!""     // false
!!12345  // true
```

- 逻辑与
`表达式1&&表达式2`如果第一个表达式为真，则返回表达式2，否则返回表达式1

>`0` `''` `null` `undefind` `NaN`为假

- 逻辑或
`表达式1||表达式2`如果表达式1为真则返回表达式1，否则返回表达式2

#### 比较操作符

- 等于（==）不等于（!=）

首先会判断两者类型是否相同，相同的话就比较两者的大小；
类型不相同的话，就会进行类型转换；
会先判断是否在对比 null 和 undefined，是的话就会返回 true
判断两者类型是否为 string 和 number，是的话就会根据ASCII将字符串转换为 number
判断两者类型是否为 string 和 string，是的话就会根据ASCII将字符串转换为 number,从左到右比较，第一位相等就比较下一位，由此类推
判断其中一方是否为 boolean，是的话就会把 boolean 转为 number 再进行判断
判断其中一方是否为 object 且另一方为 string、number 或者 symbol，是的话就会把 object 转为原始类型再进行判断

- 全等（===）不全等（!==）
只有当两个操作数的数据类型和值都相等时，才会返回true。它并不会进行数据类型的转化。​

```js
//NaN不等于任何值，包括它本身 
NaN==NaN //false
NaN===NaN //false
null==undefined //true
null===undefined //false
1=='1' //true
1==='1' //false

//浏览器自己隐式创建的包装对象和我们显式创建的包装对象不严格相等，我们举个例子说明下：
var name =  "神奇的程序员";
var info = new String("神奇的程序员");
console.log(name == info);    // true
console.log(name === info);   // false

- 浮点数相等比较
```js

1 / 3 === (1 - 2 / 3); // false
// 浮点数在运算过程中会产生误差，因为计算机无法精确表示无限循环小数。要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值
Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true

```

#### 关系操作符

小于（<）
大于（>）
小于等于（<=）
大于等于（>=）

如果这两个操作数都是数值，则执行数值比较；
如果两个操作数都是字符串，则比较两个字符串对应的字符编码值；
如果一个操作数是数值，则将另一个操作数转换为一个数值，然后执行数值比较；
如果一个操作数是对象，则调用这个对象的valueOf()方法，并用得到的结果根据前面的规则执行比较；
如果一个操作数是布尔值，则先将其转换为数值，然后再执行比较。

#### 空值合并操作符（`??`）

只有 ?? 左边为 null 或 undefined时才返回右边的值：

```js
const dogName = false; 
const name =  dogName ?? 'default';  // name = false;
```

#### 可选链操作符（`?.`）

在引用为null 或 undefined 的情况下不会引起错误，该表达式短路返回值是 undefined。与函数调用一起使用时，如果给定的函数不存在，则返回 undefined。

```js
const name = system?.user?.addr?.province?.name || 'default';
a?.[x]
// 等同于
a == null ? undefined : a[x]

a?.b()
// 等同于
a == null ? undefined : a.b()

a?.()
// 等同于
a == null ? undefined : a()

```

#### `void`操作符

`void` 是一元运算符，它可以出现在任意类型的操作数之前执行操作数，会忽略操作数的返回值，返回一个 undefined。void 常用于 HTML 脚本中执行 JavaScript 表达式，但不需要返回表达式的计算结果。比如对于链接标签，我们并不想让它发生跳转，就可以设置`href="javascript:void(0)`
由于 `void` 运算符的优先级比较高，高于普通运算符的优先级，所以在使用时应该使用小括号明确 `void` 运算符操作的操作数，避免引发错误。

```js
let a = b = c = 2;  // 定义并初始化变量的值
d = void (a -= (b *= (c += 5)));  // 执行void运算符，并把返回值赋予变量d
console.log(a);  // -12
console.log(b);  // 14
console.log(c);  // 7
console.log(d);  // undefined
```

#### 位运算符

1）原码

原码就是一个数的二进制数。例如：10的原码为0000 1010

2）反码

正数的反码与原码相同，如：10    反码为 0000 1010
负数的反码为除符号位，按位取反，即0变1，1变0。

3）补码

正数的补码与原码相同，如：10    补码为 0000 1010
负数的补码是原码除符号位外的所有位取反即0变1，1变0，然后加1，也就是反码加1。

位运算符处理 32 位数。

该运算中的任何数值运算数都会被转换为 32 位的数。结果会被转换回 JavaScript 数。

|运算符| 描述| 例子| 等同于| 结果| 十进制|
|-|-|-|-|-|-|
|`&` |与 |5 & 1 |0101 & 0001 |0001 |1|
|`|` |或 |5 \| 1 |0101 \| 0001 |0101 |5|
|`~` |非 |~ 5 |~0101 |1010 |10|
|`^` |异或 |5 ^ 1 |0101 ^ 0001 |0100 |4
|`<<` |各二进制位全部左移若干位,高位丢弃，低位补0 |5 << 1 |0101 << 1 |1010 |10|
|`>>` |各二进制位全部右移若干位，正数左补0，负数左补1，右边丢弃 |5 >> 1 |0101 >> 1 |0010 |2|
|`>>>` |零填充右位移 |5 >>> 1 |0101 >>> 1 |0010 |2|

>无符号右移操作符（>>>）会将数值的32位全部右移。对于正数，有符号右移操作符和无符号右移操作符的结果是一样的。对于负数的操作，两者就会有较大的差异。
>无符号右移操作符将负数的二进制表示当成正数的二进制表示来处理。所以，对负数进行无符号右移操作之后就会变的特别大。

>上例使用 4 位无符号的例子。但是 JavaScript 使用 32 位有符号数。

>因此，在 JavaScript 中，\~ 5 不会返回 10，而是返回 -6。

>\~00000000000000000000000000000101 将返回 11111111111111111111111111111010。

幂运算符为`**`而不是`^`,也可以用`Math.pow(x,y)`实现

#### 加减乘除操作符

- 加

```js
1 + 2             // 3
"1" + "2"         // "12"
"1" + 2           // "12"
1 + {}            // "1[object Object]"
true + false      // 1  布尔值会先转为数字，再进行运算
1 + null          // 1 null会转化为0，再进行计算
1 + undefined     // NaN undefined转化为数字是NaN

```

如果有一个操作数是NaN，则结果是NaN；
如果是Infinity加Infinity，则结果是Infinity；
如果是-Infinity加-Infinity，则结果是-Infinity；
如果是Infinity加-Infinity，则结果是NaN；
如果是+0加+0，则结果是+0；
如果是-0加-0，则结果是-0；
如果是+0加-0，则结果是+0。

- 减
减法操作和加法操作符类似， 但是减法操作符只能用于数值的计算，不能用于字符串的拼接

```js
3 - 1      // 2
3 - true   // 2
3 - ""     // 3
3 - null   // 3
NaN - 1    // NaN
```

如果是Infinity减Infinity，则结果是NaN；
如果是-Infinity减-Infinity，则结果是NaN；
如果是Infinity减-Infinity，则结果是Infinity；
如果是-Infinity减Infinity，则结果是-Infinity；
如果是+0减+0，则结果是+0；
如果是-0减+0，则结果是-0；
如果是-0减-0，则结果是+0。

- 乘
如果有一个操作数是NaN，则结果是NaN；
如果Infinity与0相乘，则结果是NaN；
如果Infinity与非0数值相乘，则结果是Infinity或-Infinity，取决于有符号操作数的符号；
如果Infinity与Infinity相乘，则结果是Infinity。

- 除法`/`
如果有一个操作数是NaN，则结果是NaN；
如果0除以0，则结果是NaN；
如果Infinity除以Infinity，则结果是Infinity。
如果是非零的有限数被零除，则结果是Infinity或-Infinity，取决于有符号操作数的符号；
如果是Infinity被任何非零数值除，则结果是Infinity或-Infinity，取决于有符号操作数的符号。

- 取余`%`
如果被除数是无穷大值而除数是有限大的数值，则结果是NaN；
如果被除数是有限大的数值而除数是零，则结果是NaN；
如果是Infinity被Infinity除，则结果是NaN；
如果被除数是有限大的数值而除数是无穷大的数值，则结果是被除数；
如果被除数是零，则结果是零。

- 幂运算`**`

`Math.pow(2, 10);    // 1024`

#### 运算符号优先级

|||
|-|-|
|小括号|`()`|
|一元运算符|`++`,`--`,`!`|
|算数运算符|先`*`,`/`,`%`,`**`,后`+`,`-`,|
|关系运算符|`>`,`>=`,`<=`,`<`,|
|相等运算符|`==`,`===`,`!==`,`!=`,|
|逻辑运算符|先`&&`后`||`|
|赋值|`=`|
|逗号|`,`|

### 创建数组

数组是对象
请不要最后一个元素之后写逗号（比如 "BMW",）可能存在跨浏览器兼容性问题。
JavaScript 方法 toString() 把数组转换为数组值（逗号分隔）的字符串。

```js
//利用new创建数组
var a=new Array(); //注意大写
var a=new Array(2); //只有一个数字时，为直接创建长度为2的空数组
var cars = new Array("Saab", "Volvo", "BMW"); //也可以直接赋值
```

```js
//数组字面量方式创建
var a=[];
var a=['xxx',77,true];
```

```js
//获取数组元素：数组名[索引号]
console.log(a[0]);
//二位数组,只对字符串有用
console.log(a[0][0]); //结果为第二个x
console.log(a[2][0]); //结果为undefined
```

```js
//数组长度：数组名.length
console.log(a.length); //新增数组元素
//修改数组索引追加数组元素
//不能直接给数组名赋值，否则会覆盖以前的数据
```

```js
//变量里存放的是数组的地址
var arr=["jjj","222",true,222];
var arr1=arr
console.log(arr1);
arr1.push("end");
console.log("push:",arr1);
console.log(arr); //arr也会一起变化
```

### 数组遍历

- for循环
- for ..in 快速遍历（没有判断,速度更快）
- forEach
- for....of

```js
var array=[10,11,12,13,14];

for(var i in array){
document.write(i,array[i]);
}

array.forEach(function(item,n,array){
document.write(n,item);
});

for (var item of array){
//item为当前遍历到的元素
}
```

### 数组的操作方法

10种：push,pop,shift,unshift,splice,slice,concat,join,sort,reverse

- JS数组添加元素

   1. 在末尾追加push()；

示例代码：

```js
//向arr数组的末尾追加元素，
arr.push('lemon', 'cucumber');
alert(arr);
//push()方法会返回新数组的长度
```

也可以使用 length 属性向数组添加新元素：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits[fruits.length] = "Lemon";     // 向 fruits 添加一个新元素 (Lemon)
//添加具有高索引的元素会在数组中创建未定义的undefined
fruits[10] = "Lemon";
```

   2. 在第一位添加unshift()；

示例代码：

```js
//向arr数组的第一位添加元素，
arr.unshift('peach', 'pear'); //返回新数组的长度。
alert(arr);
```

   3. 在指定位置添加splice();

示例代码：

```js
///从数组中添加/删除项目，然后返回被删除的项目,如果有的话。
arr.splice(3,0,'元素1','元素2'); 
alert(arr);
//arrayObject.splice(index,howmany,item1,.....,itemX)
//index 必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
//howmany 必需。要删除的项目数量。如果设置为 0，则不会删除项目。
```

- pop() 方法从数组中删除最后一个元素：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.pop();              // 从 fruits 删除最后一个元素（"Mango"）
```

- pop() 方法返回被删除的值：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var x = fruits.pop();      // x 的值是 "Mango"
```

- shift() 方法会删除首个数组元素，

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.shift();            // 从 fruits 删除第一个元素 "Banana"
//shift() 方法返回被删除的字符串即 返回 "Banana"
```

- 删除元素
既然 JavaScript 数组属于对象，其中的元素就可以使用 JavaScript delete 运算符来删除：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
delete fruits[0];           // 把 fruits 中的首个元素改为 undefined
//用 delete 会在数组留下未定义的空洞。请使用 pop() 或 shift() 取而代之。
//原型中声明的属性和对象自带的属性无法被删除；
//通过var声明的变量和通过function声明的函数拥有dontdelete特性，是不能被删除。
```

使用 splice() 来删除元素

```js
//通过聪明的参数设定，您能够使用 splice() 在数组中不留“空洞”的情况下移除元素：
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(0, 1);        // 删除 fruits 中的第一个元素
```

- 拼接数组
splice() 方法可用于向数组添加新项：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2, 0, "Lemon", "Kiwi");
//第一个参数（2）定义了应添加新元素的位置（拼接）。
//第二个参数（0）定义应删除多少元素。
//其余参数（“Lemon”，“Kiwi”）定义要添加的新元素。
//splice() 方法返回一个包含已删除项的数组：
```

- concat() 方法通过合并现有数组来创建一个新数组（浅拷贝）：

```js
var myGirls = ["Cecilie", "Lone"];
var myBoys = ["Emil", "Tobias", "Linus"];
var myChildren = myGirls.concat(myBoys,"jjjj",11);   // 连接 myGirls 和 myBoys,"jjj"等
//concat() 方法不会更改现有数组。它总是返回一个新数组。
//concat() 方法可以使用任意数量的数组参数：
```

- join() 方法

会将数组内的所有元素结合为一个字符串。当然您还可以规定分隔符(默认以逗号分割)：`array.join(",")`

- 裁剪数组
slice(start,end) 方法用数组的某个片段切出新数组。原数组不会发生改变

本例从数组元素 1 （"Orange"）开始切出一段数组：

```js
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1);  //默认到结尾
```

- sort() 方法以字母顺序对数组进行排序：

该函数很适合字符串（"Apple" 会排在 "Banana" 之前）。

不过，如果数字按照字符串来排序，则 "25" 大于 "100"，因为 "2" 大于 "1"。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();            // 对 fruits 中的元素进行排序
```

sort() 方法在对数值排序时会产生不正确的结果。

我们通过一个比值函数来修正此问题：

```js
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return a - b}); 
//比值函数
//比较函数的目的是定义另一种排序顺序。
//比较函数应该返回一个负，零或正值，这取决于参数：
//function(a, b){return a-b}
//当 sort() 函数比较两个值时，会将值发送到比较函数，并根据所返回的值（负、零或正值）对这些值进行排序。
```

- 对数组使用 Math.max()
您可以使用 Math.max.array 来查找数组中的最高值;或使用 Math.min.apply 来查找数组中的最低值

- 反转数组
reverse() 方法反转数组中的元素。

您可以使用它以降序对数组进行排序：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();            // 对 fruits 中的元素进行排序
fruits.reverse();         // 反转元素顺序
```

小程序查看对象中是否包含某个属性-----hasOwnProperty

### 函数

- 声明
在函数中，声明的参数是局部变量。只能在函数内访问。局部变量在函数开始时创建，在函数完成时被删除。

```js
//函数关键字声明
function a(){//具名函数
console.log('xxxxxxxxx');
}
```

```js
//函数表达式声明
var b=function(){//匿名函数
};
//function 函数名（形参1，形参2...）{//形参即形式上的参数}
function b(n){
console.log(n);
}
b('xxx');
//实参与形参个数一致时，正常输出
//实参小于形参时，多余的形参可看作不用声明的变量，结果为undefined
//实参多于形参时，只取到形参的个数

```

```js
//函数的返回值
function c(){
return 返回的内容；
}
var n=c();
//return 之后的代码不会执行；
//return只返回一个值，且只返回最后一个值。
//return可返回数组，字符串，
//如果没有return则返回undefind
```

```js
//###########
function fn(){
console.log(arguments);
}
fn(1,2,2,34,9);
//arguments里储存了所有传过来的实参，它是一个伪数组，具有length属性，可用索引的方式遍历，但不具有数组的push，pop等方法
```

```js
window.onload=function(){
写在这的代码会在整个页面加载完后再运行
}
```

```js
//简化写法
function $(id){
return document.getElenmentById(id);
}
var text=$("button");

```

- 递归

函数自己调用自己，一般情况下有参数，有return

   1. 首先找到临界值，即无需计算获得的值；
   2. 找到这一次和上一次的关系
   3. 假设当前函数已经可以使用，调用自身计算上一次。

### 作用域

- JavaScript 变量的有效期

JavaScript 变量的有效期始于其被创建时。
局部变量会在函数完成时被删除，其又分为函数作用域和块作用域。
全局变量会在您关闭页面是被删除。

函数参数也是函数内的局部变量。
在 HTML 中，全局作用域是 window。所有全局变量均属于 window 对象。script标签和js文件的最外层都是全局作用域
实例

```js

var carName = "porsche";
// 此处的代码能够使用 window.carName

```

- 变量提升

>let/const不会变量提升
在 JavaScript 中，可以在使用变量之后对其进行声明。

```js
//作用域链：内部函数访问外部函数变量，采用的是链式查找方式来取值的，其采取就近原则
//优先在当前函数作用域中查找，若不存在，则逐级查找父级作用域直到全局作用域
var num=10;
function fn (){
var num=20;
function fun(){
console.log(num) //结果为20
}
return fun();
}

```

```js

//js引擎运行分两步
//预解析:js引擎会把js里的所有var， function提升到当前作用域的最前面，但不提升赋值操作
//代码执行：顺序从上往下执行
fun();
var fun=function(){xxx};//会报错
//-------------
fun();
function fun(){xxx}//正常运行

```

```js
//预编译:创建AO对象;找已声明的变量,值都是undefined;实参形参值相等;找函数声明，会覆盖形参的值
function fn(a,c){
 console.log(c); //2
 console.log(a); //function a()
 function a(){ }

 var a=12;
 console.log(a);  //12

 console.log(b);  //undefined
 var b=function(){}
 console.log(b);  //function b()

}
fn(1,2);

```

JavaScript 初始化不会被提升
JavaScript 只提升声明，而非初始化。

```js
var x = 5; // 初始化 x
elem = document.getElementById("demo"); // 查找元素
elem.innerHTML = x + " " + y;           // 显示 x 和 y

var y = 7; // 初始化 y

```

## ES6 中的一些新特性

- JavaScript let
- JavaScript const
- 幂 (**)
- 默认参数值
- Array.find()
在数组中查找符合条件的元素，只要找到一个符合条件的元素，就终止遍历，返回值为找到的元素。

```js
var array=[1,2,3,45,5];
var res=arr.find(function(item,idex,array){
//查找条件
return item>5;
})
console.log(res);

```

- Array.findIndex()

```js

array.findIndex(item=>item>20);

```

- Array.copyWithin()

>es6
第一个参数：从哪个下标开始
第二第三个参数：开始与结束的范围`[start,end)`
把范围里的元素从第一个参数开始覆盖。

```js

var arr=[2,3,4,1,4,5,1,6,7,8,9,1,3];
arr.copyWithin(2,5,7);

```

- Object.assign();合并对象(es6)

将所有传入的对象都合并到第一个对象里。浅拷贝，

- let 和 const 关键字(es6)
用 let 或 const 声明的变量和常量不会被提升！遇到大括号就形成作用域。

>全局（在函数之外）声明的变量拥有全局作用域。
>局部（函数内）声明的变量拥有函数作用域。

通过 var 关键词声明的变量没有块作用域。将变量或者形参所在函数的大括号作为作用域处理。
在块 {} 内声明的变量可以从块之外进行访问。

实例

```js

{
  var x = 10;
}
// 此处可以使用 x

```

在 ES2015 之前，JavaScript 是没有块作用域的。

可以使用 let 关键词声明拥有块作用域的变量。

在块 {} 内声明的变量无法从块外访问：

实例

```js

{
  let x = 10;
}
// 此处不可以使用 x

```

- 重新声明变量

使用 var 关键字重新声明变量会带来问题。

在块中重新声明变量也将重新声明块外的变量：

实例

```js

var x = 10;
// 此处 x 为 10
{
  var x = 6;
  // 此处 x 为 6
}
// 此处 x 为 6

```

使用 let 关键字重新声明变量可以解决这个问题。

在块中重新声明变量不会重新声明块外的变量：

实例

```js

var x = 10;
// 此处 x 为 10
{
  let x = 6;
  // 此处 x 为 6
}
// 此处 x 为 10

```

>Internet Explorer 11 或更早的版本不完全支持 let 关键词。

- var 与let 区别

通过 var 关键词定义的全局变量属于 window 对象
通过 let 关键词定义的全局变量不属于 window 对象
允许在程序的任何位置使用 var 重新声明 JavaScript 变量
在相同的作用域，或在相同的块中，通过 let 重新声明一个 var 变量是不允许的
在相同的作用域，或在相同的块中，通过 let 重新声明一个 let 变量是不允许的
在相同的作用域，或在相同的块中，通过 var 重新声明一个 let 变量是不允许的
在不同的作用域或块中，通过 let 重新声明变量是允许的
通过 var 声明的变量会提升到顶端
通过 let 定义的变量不会被提升到顶端

- const常量

通过 const 定义的变量与 let 变量类似，但不能重新赋值;
const 变量必须在声明时赋值,
const 变量不能在声明之前使用,不能重复声明

- 但常量对象是可以更改

实例

```js

// 您可以创建 const 对象：
const car = {type:"porsche", model:"911", color:"Black"};

// 您可以更改属性：
car.color = "White";

// 您可以添加属性：
car.owner = "Bill";

```

但是您无法重新为常量对象赋值：

实例

```js

const car = {type:"porsche", model:"911", color:"Black"};
car = {type:"Volvo", model:"XC60", color:"White"};    // ERROR

```

同理常量数组可以更改；但是您无法重新为常量数组赋值
在同一作用域或块中，不允许将已有的 var 或 let 变量重新声明或重新赋值给 const;
在同一作用域或块中，为已有的 const 变量重新声明声明或赋值是不允许的;
在另外的作用域或块中重新声明 const 是允许的：

实例

```js

const x = 2;       // 允许
{
  const x = 3;   // 允许
}
{
  const x = 4;   // 允许
}

```

>Internet Explorer 10 或更早版本不支持 const 关键词。

- 声明严格模式

通过在脚本或函数的开头添加 "use strict"; 来声明严格模式。(浏览器低版本不兼容)
在脚本开头进行声明，拥有全局作用域（脚本中的所有代码均以严格模式来执行）：
在函数中声明严格模式，拥有局部作用域（只有函数中的代码以严格模式执行）

实例

```js

"use strict";
x = 3.14;       // 这会引发错误，因为 x 尚未声明

```

- 严格模式中不允许的事项：
在不声明变量的情况下使用变量，是不允许的;
在不声明对象的情况下使用对象也是不允许的：
删除变量（或对象）是不允许的`delete:x;`
删除函数是不允许的,重复参数名是不允许的,八进制数值文本是不允许的,转义字符是不允许的写入只读属性是不允许的,写入只能获取的属性是不允许的,删除不可删除的属性是不允许的,字符串 "eval" 不可用作变量,字符串 "arguments" 不可用作变量,with 语句是不允许的,处于安全考虑，不允许 eval() 在其被调用的作用域中创建变量,在类似 f() 的函数调用中，this 的值是全局对象。在严格模式中，现在它成为了 undefined。

### 对象

```js

//利用对象字面量创建对象{}
var obj={}; //创建了一个空的对象
var obj={
username:'张山',
age: 18,
sex:'男',
hi:function(){
console.log('hello!!!!!!!!!!!!!!');
}
}

```

```js

//#################
//利用new Object 创建对象
var obj1= new Object();//创建一个空对象,可省略new
obj1.username='张山';
obj1.age= 18;
obj1.sex='男';
obj1.hi=function(){
console.log('hello!!!!!!!!!!!!!!');
}

```

```js

//里面的属性或方法，采用键值对的形式->  属性名： 属性值
//多个属性或方法中间用逗号隔开
//方法冒号后面跟的是一个匿名函数
//##################################
//调用对象的属性，采用 对象名.属性名
console.log(obj1.uname);
//或者 对象名['属性名']
console.log(obj1['age']);
//调用对象的方法-> 对象名.方法名
obj1.hi();
//如果您不使用 () 访问 fullName 方法，则将返回函数的具体内容

```

```js

//##############################
//遍历对象
//for(变量 in 对象){}
for (var i in obj){
console.log(k);  
console.log(obj[i]);
}

```

```js

//################################
//利用构造函数创建对象
//function 构造函数名(){
//this.属性名=值;
//this.方法名=function(){}
//}
// new 构造函数名();
function Star(username,age,sex){
this.name=username;
this.age=age;
this.sex=sex;
}
new Star('刘德华',58,'男');

```

- 箭头函数（Arrow Function）
箭头函数允许使用简短的语法来编写函数表达式。

您不需要 function 关键字、return 关键字以及花括号。

>注意:箭头函数不能用new，如果返回值是一个对象，一定要加（）；箭头函数没有自己的this，它内部的this是作用域链的上一层的this；

实例

```js

var x = function(x, y) {
   return x *y;
}
//多个参数，有返回值
var  x = (x, y) => x* y;
//或
var x=(x,Y)=>{
reruen x*y;
}
--------------------

function add(x){
return x+10;
}
//一个参数，有返回值
var add=x=>x+10;
add(5);
//或
var add=x=>{
return x+10;
}
--------------------

function show(){
alert("hello");
}
//无参数，无返回值
var show=()=>{
alert("hello");
}
--------------------

function xxx(num){
alert(num);
}
//有一个参数，无返回值
var xxx=num=>{
alert(num);
}
----------------

function show(){
return {name:'xxx'};
}
//返回对象
var show=()=>({..});

```

箭头功能没有自己的 this。它们不适合定义对象方法。
箭头功能未被提升。它们必须在使用前进行定义。

使用 const 比使用 var 更安全，因为函数表达式始终是常量值。
如果函数是单个语句，则只能省略 return 关键字和花括号。因此，保留它们可能是一个好习惯：

实例

```js

const x = (x, y) => { return x * y };

```

#### 内置对象(如同内置函数）如:Math、 Date、 Array、 String、

Math对象

```js

Math.PI  //圆周率
Math.abs() //求绝对值
Math.max() //求最大
Math.min() //求最小值
Math.pow(x,y)  //x的y次幂
Math.round() //四舍五入
Math.random() //获取随机数
Math.ceil() // 向上取整,有小数就整数部分加1
Math.floor() // 向下取整,丢弃小数部分

```

- 获取当前时间

```js

//时间对象是静态的
//必须实例化
var nowdate=new Date();
console.log(nowdate);

```

```js

//############
//Date()构造函数的参数
//自定义日期格式
var nowdate=new Date("2020-2-22");

```

```js

//日期格式化
//getFUllYear() //年
//getMonth()  //月
//getDate() //天
//getDay() //星期（周日0到周六6）
//getHours()
//getMinutes()
//getSeconds()
var date= new Date();
console.log(date.getHours());

```

```js

//封装一个获取时间的方法
function gettime(){
var time = new Date();
var h=time.getHours();
h= h<10 ? "0"+h : h;  //小于10前面添加0
var m=time.getMinutes();
var s=getSeconds();
return h +':'+ m +':'+ s;
}
console.log(gettime());

```

```js

//######################
//获取Date总毫秒数，距离1970.1.1的总毫秒数
var date= new Date();
console.log(date.valueOf());
//或
console.log(date.getTime());
//或
var date1= +new Date();  //常用
console.log(date1);
//或
console.log(Date.now()); //H5新增

```

- js 时分秒转化为秒

```js

var time = '00:02:10';
          var hour = time.split[':'](0);
          var min = time.split[':'](1);
          var sec = time.split[':'](2);

          s = Number(hour*3600) + Number(min*60) + Number(sec);
          console.log(s);//130

```

- 设置日期对象的日期值

|方法| 描述|
|-|-|
|setDate()| 以数值（1-31）设置日|
|setFullYear()| 设置年（可选月和日）|
|setHours()| 设置小时（0-23）|
|setMilliseconds()| 设置毫秒（0-999）|
|setMinutes()| 设置分（0-59）|
|setMonth()| 设置月（0-11）|
|setSeconds()| 设置秒（0-59）|
|setTime()| 设置时间（从 1970 年 1 月 1 日至今的毫秒数）|

setFullYear() 方法设置日期对象的年份。这个例子设置为 2020 年：

实例

```html

<script>
var d = new Date();
d.setFullYear(2020);
document.getElementById("demo").innerHTML = d;
</script>

```

- 比较日期

日期可以很容易地进行比较。

下面的例子把今日与 2049 年 1 月 16 日进行比较：

实例

```js

var today, someday, text;
today = new Date();
someday = new Date();
someday.setFullYear(2049, 0, 16);
if (someday > today) {
  text = "今天在 2049 年 1 月 16 日之前";
} else {
  text = "今天在 2049 年 1 月 16 日之后";
}
document.getElementById("demo").innerHTML = text;

```

### 定时器

- setInterval("函数"，毫秒数);
每隔对应的毫秒执行一次传入的函数;
返回值为：系统给启动定时器分配的编号；

- clearInterval(定时器编号);
取消定时器

### 延时器

    只执行一次

- 创建延时器
    `window.setTimeout(函数类型，延迟时间)`
    返回值timeoutID是一个正整数，表示定时器的编号需要注意的是setTimeout()和setInterval()共用一个编号池，技术上，clearTimeout()和 clearInterval() 可以互换。但是，为了避免混淆，不要混用取消定时函数。

```js

    var timer1=window.setTimeout(function (){
                    console.log("你好啊！！！");
                },3000);

```

- 清除延时器

`window.clearTimeout(延时器名称)`

```js

    function func(){
                    //清除延时器
                    window.clearTimeout(timer1);
                }

```

### JS this 关键词指的是它所属的对象

它拥有不同的值，具体取决于它的使用位置：

- 在方法中，this 指的是所有者对象。

在对象方法中，this 指的是此方法的“拥有者”。
在本页最上面的例子中，this 指的是 person 对象。
person 对象是 fullName 方法的拥有者。

```js

var person = {
  firstName: "Bill",
  lastName : "Gates",
  id       : 678,
fullName : function() {
  return this.firstName + " " + this.lastName;
}

```

- 单独的情况下，this 指的是全局对象。

在单独使用时，拥有者是全局对象，因此 this 指的是全局对象。
在浏览器窗口中，全局对象是 [object Window]：

实例

```js

var x = this;

```

在严格模式中，如果单独使用，那么 this 指的是全局对象 [object Window]

- 在函数中，this 指的是全局对象。

在 JavaScript 函数中，函数的拥有者默认绑定 this。
因此，在函数中，this 指的是全局对象 [object Window]。

实例

```js

function myFunction() {
  return this;
}

```

箭头函数没有自己的this，其中的this指的是window全局对象

在函数中使用时，在严格模式下，this 是未定义的（undefined）

- 在事件中，this 指的是接收事件的元素。

在 HTML 事件处理程序中，this 指的是接收此事件的 HTML 元素：

实例

```js

<button onclick="this.style.display='none'">
  点击来删除我！
</button>
```

- 像 call() 和 apply() 这样的方法可以将 this 引用到任何对象。

  - call
格式：函数名.call();
参数：第一个参数：传入该函数this指向的对象，传入什么强制指向什么。
      第二参数开始：将原函数的参数往后顺延一位。
      会立即调用函数,返回值就是函数的返回值

  - apply
格式：函数名.apply()
参数：第一个参数：传入该函数this指向的对象，传入什么强制指向什么。
      第二个参数：数组 其放入我们所有原来的参数。
      会立即调用函数,返回值就是函数的返回值

  - bind预设this指向
参数：第一个参数：传入该函数this指向的对象，传入什么强制指向什么。
      第二参数开始：将原函数的参数往后顺延一位。
      不会调用函数，返回值是改变this指向后的新函数

```js
function show(x,y){
console.log(this);
console.log(x);
console.log(y);
}
var res=show.bind("bind");
console.log(res);
```

call() 和 apply() 方法是预定义的 JavaScript 方法。
它们都可以用于将另一个对象作为参数调用对象方法。
在下面的例子中，当使用 person2 作为参数调用 person1.fullName 时，this 将引用 person2，即使它是 person1 的方法：

```js
var person1 = {
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
}
var person2 = {
  firstName:"Bill",
  lastName: "Gates",
}
person1.fullName.call(person2);  // 会返回 "Bill Gates"
```

## js错误

- try 语句使您能够测试代码块中的错误。

- catch 语句允许您处理错误。

- throw 语句允许您创建自定义错误。

- finally 使您能够执行代码，在 try 和 catch 之后，无论结果如何。

```js
try {
     供测试的代码块
 throw new Error("直接允许");
}
 catch(err) {
console.log(err);
     处理错误的代码块
//如果try中的代码无异常则不执行catch中的代码
} 
finally {
     无论 try / catch 结果如何都执行的代码块
}
//----------------------------------
function myFunction() {
    var message, x;
    message = document.getElementById("message");
    message.innerHTML = "";
    x = document.getElementById("demo").value;
    try { 
        if(x == "") throw "是空的";
        if(isNaN(x)) throw "不是数字";
         x = Number(x);
        if(x >  10) throw "太大";
        if(x <  5) throw "太小";
    }
    catch(err) {
        message.innerHTML = "错误：" + err + ".";
    }
    finally {
        document.getElementById("demo").value = "";
    }
}

```

### JavaScript 闭包

- 闭包特点：
   1. 函数嵌套函数
   2. 内部函数使用外部函数的形参和变量
   3. 被引用的形参和变量不会被内存回收

- 好处：
   1. 变量能常驻内存当中
   2. 避免全局变量污染
   3. 可以声明私有成员

JavaScript 变量属于本地或全局作用域。

全局变量能够通过闭包实现局部（私有）。
闭包指的是有权访问父作用域的函数，即使在父函数关闭之后

>拥有相同名称的全局变量和局部变量是不同的变量。修改一个，不会改变其他。不通过关键词 var 创建的变量总是全局的，即使它们在函数中创建。

```js
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();

add();
add();
add();

// 计数器目前是 3 
/*
第一次调用add()，相当于执行了function(){return (++counter)}一次；第二次调用add()，相当于又执行了function(){return (++counter)}一次；又由于function(){return (++counter)}是闭包，引用了其父函数的变量counter，所以在函数调用完毕counter依然存在，没有清零。
    而且只在第一次给add赋值时，将counter设置为0。以后每次调用add()，counter都自增一次，没有重置。也就是前文说的，父函数只执行一次，每次调用都只执行了子函数。
    这是为什么呢？因为只有执行下面这个函数，counter才重置。而这个函数只在第一次给add赋值时执行过一次。以后每次调用add()，都相当于调用function(){return(++counter)}。
    function(){
       var counter = 0;
       return function(){
          return (++counter);
       }
    }
*/
```

```js
var moduleA=(function(mod){
var conut=10; //私有变量
function showA(){ //私有函数
count+=20;
console.log(count);
}
function showB(){
count*=10;
console.log(count);
}
mod.outA=showA;
mod.outB=showB;
//对外暴露
return mod;
})(moduleA||{});

var moduleA=(function(mod){
function showC(){
console.log("jjjj");
}
mod.outC=showC;
return mod;
})(moduleA||{});

mouleA.outA();
mouleA.outB();
mouleA.outC();
```

- 立即执行函数写法

```js
var test=(function(){
var c=0;
var m1=function(){xxxx};
var m2=function(){xxxx};
return{
m1:m1,
m2:m2
}
})();
```

#### 模块化规范

- CommonJs规范(nodejs)

- AMD规范：
  - 声明：

```js
define(function(){
//代码
return{
outA:showA,
outB:showB
}
})
```

引用：（异步执行）

```js
require("moduleA.js",function(moduleA){
moduleA.putA();
moduleA.putB();
})
```

- ECMA6

moduleB.js

```js
//导出
export={
   outA:showA
   outB:showB
}

export const age='xx'
export const name='xx'
export function sayName(){}



```

```html
<script type="module">
//导入
import {age,sayName,name} from "./moduleA.js"

sayName()
</script>
```

moduleA.js

```js
//默认导出
let obj={name:'xx',}
//只能使用一次
export default obj


```

```html
<script type="module">
//导入
import moduleA from "./moduleB.js"
//或
import * as a from "./moduleB.js"
moduleA.outA();
moduleA.outB();

</script>
```

- JavaScript 性能

   1. 减少循环中的活动
   2. 减少 DOM 访问
与其他 JavaScript 相比，访问 HTML DOM 非常缓慢
   3. 缩减 DOM 规模
请尽量保持 HTML DOM 中较少的元素数量。
   4. 避免不必要的变量
   5. 延迟 JavaScript 加载
请把脚本放在页面底部，使浏览器首先加载页面。
   6. 避免使用 with
请避免使用 with 关键词。它对速度有负面影响。它也将混淆 JavaScript 作用域。

- JavaScript JSON

JSON 是存储和传输数据的格式。
JSON 格式仅仅是文本，它能够轻松地在服务器浏览器之间传输，并用作任何编程语言的数据格式。
JSON 语法衍生于 JavaScript 对象标记法语法:

- 数据在名称/值对中
- 数据由逗号分隔
- 花括号容纳对象
- 方括号容纳数组

JSON 的值不可以是以下数据类型之一：函数;日期;undefined.

把 JSON 文本转换为 JavaScript 对象
首先，创建包含 JSON 语法的 JavaScript 字符串：

```js
var text = '{ "employees" : [' +
'{ "firstName":"Bill" , "lastName":"Gates" },' +
'{ "firstName":"Steve" , "lastName":"Jobs" },' +
'{ "firstName":"Alan" , "lastName":"Turing" } ]}';
```

然后，使用 JavaScript 的内建函数 JSON.parse() 来把这个字符串转换为 JavaScript 对象：

```js
var obj = JSON.parse(text);
```

最后，请在您的页面中使用这个新的 JavaScript 对象：

实例

```html
<p id="demo">\</p>
<script>
document.getElementById("demo").innerHTML =
obj.employees[1].firstName + " " + obj.employees[1].lastName;
</> 
```

JSON 与 XML 的差异在于：

- JSON 不使用标签
- JSON 更短
- JSON 的读写速度更快
- JSON 可使用数组

最大的不同在于：

XML 必须使用 XML 解析器进行解析。而 JSON 可通过标准的 JavaScript 函数进行解析。

- JavaScript 表单验证
HTML 表单验证能够通过 JavaScript 来完成

JavaScript 实例

```js
function validateForm() {
    var x = document.forms["myForm"]["fname"].value;
    if (x == "") {
        alert("必须填写姓名");
        return false;
    }
}
```

该函数能够在表单提交时被调用：

HTML 表单实例

```js
<form name="myForm" action="action\_page\_post.php" onsubmit="return validateForm()" method="post">
姓名：<input type="text" name="fname">
<input type="submit" value="Submit">
</form>
```

## HTML 事件

HTML 事件可以是浏览器或用户做的某些事情。

写法：

1. 内联模式
2. 脚本模式/外联模式

```html
<button onclick="btnClick()"> 内联模式</button>

<button id="btn">外联模式</button>

<script>
var oBtn=document.getElementById("btn");
0Btn.onclick=function(){
console.log("click button");
}
</script>
```

绑定事件格式：`元素节点.on+事件类型=匿名函数`；

>系统会在事件绑定完成时，生成一个事件对象。触发事件时，系统会自动去调用事件绑定的函数，将事件对象当做第一个参数传入。

```js
function show(){
console.log(argument.length);
}

window.onload=function(){
var oBtn=document.getElement("btn1");
//oBtn.onclick=show;

/*
button属性：左键0；中键1；右键2；

鼠标位置：(原点位置不同）
clientX    clientY  //可视窗口的左上角
pageX    pageY   //整个页面的左上角（滚动也包括）
screenX    screenY  //电脑屏幕的左上角
*/

oBtn.onclick=function(e){
//事件对象获取的方式，固定写法
var ev=e||window.event;
console.log(ev);
//console.log(ev.button);
//console.log(ev.clientX);
}
}
```

循环添加

```js
var Oblock=document.querySelectorAll('div');
   //console.log(Oblock);
   for (var i=0 ;i< Oblock.length;i++){
    Oblock[i].index=i;
    Oblock[i].onmousedown=function(e){
    console.log(this.index);
    }
   }
```

- window事件

  - load 当页面加载完后会触发
  - unload 当页面解析的时候触发（如刷新界面、关闭当前界面）IE浏览器兼容
  - scroll 页面滚动
  - resize 窗口大小发生变化的时候触发

- 表单事件

  - blur 失去焦点
  - focus 获取焦点
  - select  在输入框内选中文本时触发
  - change 修改输入框内容并失去焦点时触发

  - sumit 只对sumit按钮有效
  - reset 只对reset按钮有效

下面是一些常见的 HTML 事件：

|事件 |描述|
|-|-|
|onchange |HTML 元素已被改变|
|onclick |用户点击了 HTML 元素|
|ondblclick |  双击|
|onmouseover |用户把鼠标移动到 HTML 元素上(经过子节点会重复触发)|
|onmouseout |用户把鼠标移开 HTML 元素(经过子节点会重复触发)|
|onmouseenter |用户把鼠标移动到 HTML 元素上(经过子节点不会重复触发,IE8不兼容)|
|onmouseleave |用户把鼠标移开 HTML 元素(经过子节点不会重复触发,IE8不兼容)|
|onmousedown |鼠标任一按钮被按下|
|onmousemove |鼠标被移动,会不停触发|
|onmouseup |鼠标按键被松开|
|onkeypress |用户按下键盘按键(只支持字符建，shift等没用)|
|onkeydown |用户按下键盘按键|
|onkeyup |用户抬起键盘按键|
|onload |浏览器已经完成页面加载

键盘事件对象属性：

- shiftKey //按下shift键为true，默认为false
- altKey
- ctrlKey
- metaKey  //按下win键
- keyCode 键码  //不区分大小写，只返回大写ASCII码，支持shift、ctrl等非字符键；只在keydown下支持,可用 which兼容

```js
window.onload=function(){
window.onkeydown=function(ev){
var e=ev||window.event;
var w=e.which||e.keyCode;
console.log(w);
}
}
```

- charCode  键码  //区分大小写，只支持字符键，只在keypress下支持,可用 which兼容

```js
window.onload=function(){
window.onkeypress=function(ev){
var e=ev||window.event;
var w=e.which||e.keyCode;
console.log(w);
}
}
```

- target
目标对象/触发对象

IE8以下不兼容，可使用window.event.srcElement

```js
window.onload=function(){
var oUl=document.getElementById("ul1");

window.onkeypress=function(ev){
var e=ev||window.event;
var target=e.target||window.event.srcElement;
console.log(this.tagName);
console.log(target.innerHTML);
}
}
```

- 事件冒泡：由里向外逐级触发

>事件冒泡：同样的事件会在该元素的所有祖先元素中依次被触发(从里到外)
>事件捕获：从根元素开始去执行对应事件(从外到里)

事件对象的属性和方法:
`cancelBubble=true;`
`stopPropagation();`(捕获阶段也有效)

```js
window.onload=function(){
var oDiv=document.getElementById("div1");
for(var i=0;i<aDivs.length;i++){
window.onkeypress=function(ev){
var e=ev||window.event;
console.log(this.tagName);
e.cancelBubble=true;
//e.stopProgagation();
}
}
}
```

>mouseover和mouseout会有冒泡效果
>mouseenter和mouseleave没有冒泡效果
>

#### ES6 新特性

##### 函数默认值

```js
//es5
function test(a,b){
   a=a||10;
   b=b||10;
   return a+b
}
//es6
function test(a=10,b=10){
   return a+b
}
```

##### 函数参数

```js
//es5
function test(args){
   console.log(arguments);
}

//es6
function test(a,...args){
   console.log(args)
}
test(1,2,3,4,5)
```

##### 箭头函数

- 没有自己的this
- 内部没有arguments
- 不能使用new 关键词实例化对象
- function 是对象，箭头函数不是对象，是表达式,

```js
let test=()=>{}
```

##### 扩展运算符号

```js
let arr=[1,2,3]
let obj={
   name:'xxx',
   age:10
}
console.log(...arr)
console.log(...obj)
```

##### 解构赋值

```js
var [x,y,z]=[1,"dd",false];
//相当与
var x=1,y="dd",z=fase;

//还可以
const [x,[a,b],y]=[10,[10,"dd"],"a"];

//交换两数方便
const [x,y]=[1,2];
[x,y]=[y,x];

//函数可一次性返回多个数据
function xxx(){
   return ["jj",3,true];
}
const [a,b,c]=xxx();


//不用按循序赋值
const {name,age,sex:x,...res}={
   sex:"男",
   name:"xiaomi",
   age:11,
   school:'xx'
}
console.log(name,age,x)
```

- 反引号

  - 可用反引号代替单双引号括起字符串。其中的换行、代码缩进都会保留。
  - 拼接字符串时可用`${变量/表达式/函数}`代替`+''+`

##### Array.includes()

```js
let a=[1,2,3,4]
a.includes(5)//false
```

##### Array.from()

将伪数组（没有真数组身上的方法）转换为真数组

```js
var aLid=document.getElementById("li1");
let aLis=Array.from(aLis);
aLis.push("test");

//第二个参数

let aLis=Array.from(aLis,item=>item);

//可使用扩展运算符，转为真数组
let list=[...aLid]
```

##### Array.of()

将一组的值，转换为数组

```js
Array.of(3,11,'xx',{name:'xx'},[1,2])
```

##### Symbol

>原始数据类型，表示独一无二的值(内存地址不同)
>用于定义对象的私有变量

```js
const name=Symbol('xx')
const name2=Symbol('xx')
console.log(name===name2)//false


let obj={}
obj[name]=name
//必须使用中括号
//无法遍历,不可枚举
console.log(obj[name])

//可通过以下获取key
let s=Object.getOwnPropertySysmbols(obj)
console.log(s)
let m=Reflect.ownKeys(obj)
console.log(m)
```

##### 迭代器Iterator
>
>新的遍历机制

```js
let arr=['1','2','3']
//创建迭代器
const ite=arr[Symbol.iterator]()
console.log(ite.next())//{value:'1',done:false}
console.log(ite.next())//{value:'2',done:false}
console.log(ite.next())//{value:'3',done:false}
console.log(ite.next())//{value:undefined,done:true}
```

##### 生成器generator
>
>通过yield关键字，将函数挂起,暂停执行
>只能在函数内部使用yield表达式，将函数挂起

```js
function* test(){
   console.log('one')
   yield 1;
   console.log('two')
   yield 2;
   console.log('end')
   return a+b
}
let fn=test()
console.log(fn.next())//one \n {value:'1',done:false}
console.log(fn.next())//two \n {value:'2',done:false}
console.log(fn.next())//end \n {value:undefined,done:true}
```

```js
 //赋值
function* test(x){
   console.log('one',x)
  let a= yield 1;
   console.log('two',a)
   yield 2;
   console.log('end',b)
   return a+b
}
let fn=test(999)
console.log(fn.next())//one 999 \n {value:'1',done:false}
console.log(fn.next(10))//two 10 \n {value:'2',done:false}
console.log(fn.next(20))//end 20 \n {value:30,done:true}
```

>用于给不具备interator接口的对象提供遍历操作
>用于解决回调地域

```js
//for...of.. foreach 不能遍历对象
//for...of..只能遍历带有iterator迭代器接口的,for...of底层利用iterator迭代器实现，遍历时会调用自身的next方法，让yield向下执行
//使用生成器，遍历对象
function* aobjectEntries(obj){
   const keys=Object.keys(obj)
   for(let i of keys){
      yield [i,obj[i]]
   }
}

const obj={name:'xx',age:10};
obj[Symbol.iterator]=objectEntries;

for(let [key,value] of objectEntries(obj))
{
   console.log(key,value)

}
```

##### 新数据类型：Set集合(无重复值的有序列表)

```js
let imgs= new Set();
//添加元素
imgs.add(100);
imgs.add(100);
imgs.add("ddd");
imgs.delete(100)
imgs.has(100)//false
imgs.add(new String("ddd"));
imgs.forEach((val,key)=>{
   //键值是相等的
   console.log(val)
   console.log(key)
})
console.log(imgs,imgs.size);

//set中的对象引用无法被释放
let set3=new Set()
let obj={name:'xx'}
set3.add(obj)
obj=null

//使用WeakSet代替
let set3=new WeakSet()
set3.add(obj)
obj=null
//它不能传入非对象类型的参数
//不能遍历
//没有size属性

```

遍历集合`for....of`

```js
for(let item of imgs.keys()){ //遍历键
console.log(item);
}
for(let item of imgs.values()){  //只遍历值
console.log(item);
}
for(let item of imgs.entries()){    //遍历键与值
console.log(item);
}
```

数组与集合互转

```js
//数组变集合
var set=new Set([3,2,4,5,6,1,3,4]);
console.log(set);
//集合变数组，将数据结构展开为数组
var arr = [...set];
console.log(arr);
```

##### Map映射(键值对的有序列表，键值可以是任意类型)

```js
let map=new Map();
//添加数据
map.set("Tom","fishman");
map.set("Jack","woker");
map.set("Sim","teacher");
map.set("Jack","bussnessman");
map.delete('Sim')
map.has('Sim')
console.log(map);
//取值
console.log(map.get("Tom"));
//map遍历：for   of
for(let [key,value] of map){
console.log(key,value);
}
```

>遍历：数组[for循环、for...in、foreach、for...of],对象[for...in]，set[for...of],map[for...pf];

##### 构造函数

这种通过new调用函数，叫做构造函数(首字母一般大写)，其可构造对象，

   1. 当前函数中的this指向新创建的对象
   2. 自动完成原料和出厂操作

```js
function createPerson(name,sex){
   var obj=new Object(); //原料
   //加工
   obj.name=name;
   obj.sex=sex;
   obj.showName=function(){
   console.log(this.name);
   }
   obj.showSex=functin(){
   console.log(this.sex);
   }
return obj; //出厂
}
var p1=createPerson("Tom","man");
p1.showName();
p1.showSex();
```
- 首字母大写
- 内部return返回值无效,无需写
- 构造函数中的this指向新的对象
```js
function Pig(name){
   this.name=name
}
const p=new Pig('xxx')//实例化
```

- 静态成员（静态属性，静态方法）
>只能通过构造函数访问，
>静态方法中的this指向构造函数
```js
function Person(name,age){}
//静态属性
Person.eyes=2
//静态方法
Person.walk=function(){}
```

##### prototype 原型对象

每一个函数上，都一个原型对象prototype

```js
function show(){
}
console.log(show.prototype);
```

用在构造函数上，我们可以给构造函数的原型prototype，添加方法

   1. 如果我们将方法添加到构造函数的原型prototype对象上
   2. 构造函数构造出来的对象共享原型上所有的方法

构造函数构造出来的对象，有一个属性_proto_,指向构造除这个对象的构造函数的原型。

instanceof关键字，判断某一个对象是否是这个构造函数构造出来的。

```js
var arr1=[3,32,4,1,45];
var arr2=[4,3,2,1];
Array.prototype.sum=function(){
var res=0;
for(var i=0;i<this.length;i++){
res+=this[i];
}
return res;
}
console.log(arr1.sum());
console.log(arr2.sum());
console.log(arr1.sum==arr2.sum);
```

在子一级构造函数重写方法，只会在子一级生效，并不会影响父一级构造函数的方法。

继承和多态：
继承侧重是从父一级构造函数，继承到属性和方法；
多态侧重的是子一级自己重写和新增的属性和方法;

#### ECMA6 class语法

```js
//es5
function Person(name,sex,age){
   this.name=name;
   this.sex=sex;
   this.age=age;
}
Person.prototype.sayName=function(){
   return this.name
}

let p1=new Person('xiaomi','man',10)
console.log(p1)
```

```js
//es6
class preson{
//class属性添加。
//实例化的时候立即调用
constructor(name,sex,age){
   this.name=xxx;
   this.sex=sex;
   this.age=age;
   showself(){
      console.log(`${this.name},${this.age}`);
   }
}

var p1=new person("Tony","man",33);
p1.showself();

//extends继承
class worker extends person{
   constructor(name,sex,age,job){
   //继承到父一级的属性
   super(name,sex,age);//要写在最前面
   this.job=job;
   //子类自己的方法
   sayName(){
      console.log(this.name)
   }
   //重写父类方法
   showself(){
      console.log('xx')
      super.showself()
   }
}

```

```js
function person(name,sex,age){
this.name=name;
this.sex=sex;
this.age=age;

}

person.prototype.showself=function(){
console.log(this.name,this.age);
}

function woker(name,sex,age,job){
//1.构造函数的伪装，继承父级的属性
Person.call(this,name,sex,age);
this.job=job;
}
//2.原型链  继承父一级的方法
//<1>通过for ...in 遍历继承
for (var funcName in person.prototype){
worker.prototype[funcName]=person.prototype[funcName];
}

//<2>Object.create()
worker.prototype=Object.create(person.prototype);
//<3>调用构造函数继承
worker.prototype=new person();

worker.prototype.showjob=function(){
console.log(this.job);
}
var w1=new worker("Tom","man",22,"driver");
```

##### promise
>
>回调函数：当一个函数当作参数传递给另一个函数的时候，这个函数就是回调函数

```js
let p=new Promise((resolve,reject)=>{
   //异步操作
})
p.then(res=>{},err=>{}).catch(err=>{})
//then()，第一个参数是resolve回调函数，第二个是reject回调函数，可选．
//catch(err=>{})等价于then(null,err=>{})

//resolve()方法,将现有的任何对象转化为promise对象
//reject()方法,将现有的任何对象转化为promise对象
let p1=Promise.resolve('123')
let p2=Promise.reject('123')
p1.then(val=>{
   //val==123
//等价于new Promise(resolve=>resolve('123'))
})



//all(),所有异步操作都成功才成功，只要有一个失败就都失败
let p3=new Promise((resolve,reject)=>{})
let p4=new Promise((resolve,reject)=>{})
let p5=Promise.all([p3,p4])
p5.then()

//race(),返回多个promise中最先执行完成的那个promise，无论成功还是失败
let p6=Promise.race([p3,p4])
p6.then()

//finally()，不管成功失败都会执行
//done()
p1.then(res=>{},err=>{}).finally(()=>{})

```

##### async 异步操作

```js
//async返回一个promise对象
//async 是generator的语法糖
async function test(){
   return await '123'
   //throw new Error('err')
}
test().then(val=>{
   //val=='123'
})
```

---

## 触发input-file的click事件：`document.querySelector('#file').click()`

## 清空input-file值

```js
//1.###
var test = document.getElementById('test');
test.value = '';  //test的value不能设为有字符的值，但是可以设置为空值

//2.###
var test = document.getElementById('test');
test.outerHTML = test.outerHTML; //重新初始化了test的html
```

## js时间戳转换成日期的方法：`(new Date(parseInt(n))).toLocaleDateString()`

## js执行机制（event loop 事件循环）
>
>javascript是一门单线程语言，在最新的HTML5中提出了Web-Worker，但javascript是单线程这一核心仍未改变。所以一切javascript版的"多线程"都是用单线程模拟出来的

1. 判断是同步任务还是异步任务
2. 同步的进入主线程，异步的进入Event Table并注册回调函数。
3. 当指定的事情完成时，Event Table会将这个回调函数移入Event Queue。
4. 主线程内的同步任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。
上述过程会不断重复，也就是常说的Event Loop(事件循环)。
那怎么知道主线程执行栈为空啊？js引擎存在monitoring process进程，会持续不断的检查主线程执行栈是否为空，一旦为空，就会去Event Queue那里检查是否有等待被调用的函数。

### setTimeout

setTimeout(fn,0)的含义是，指定某个任务在主线程最早可得的空闲时间执行，关于setTimeout要补充的是，即便主线程为空，0毫秒实际上也是达不到的。根据HTML的标准，最低是4毫秒。

### setInterval

对于执行顺序来说，setInterval会每隔指定的时间将注册的函数置入Event Queue，如果前面的任务耗时太久，那么同样需要等待。
唯一需要注意的一点是，对于setInterval(fn,ms)来说，我们已经知道不是每过ms秒会执行一次fn，而是每过ms秒，会有fn进入Event Queue。一旦setInterval的回调函数fn执行时间超过了延迟时间ms，那么就完全看不出来有时间间隔了。

### Promise与process.nextTick(callback)

>process.nextTick(callback)类似node.js版的"setTimeout"，在事件循环的下一次循环中调用 callback 回调函数。

我们进入正题，除了广义的同步任务和异步任务，我们对任务有更精细的定义：

macro-task(宏任务)：包括整体代码`script主代码块`，`setTimeout`，`setInterval`,`Ajax`、`DOM事件`。
micro-task(微任务)：`Promise`，`process.nextTick`,`async`、`await`。

JS的执行/运行机制，具体如下所示：
1、在执行栈中执行一个宏任务；
2、执行过程中遇到微任务，将微任务添加到微任务队列中；
3、当前宏任务执行完毕，立即执行微任务队列中的任务；
4、当前微任务队列中的任务执行完毕，检查渲染，GUI线程接管渲染；
5、渲染完毕后，js线程接管，开启下一次事件循环，执行下一次宏任务（事件队列中取）。

```js
setTimeout(function() {
    console.log('setTimeout');
})

new Promise(function(resolve) {
    console.log('promise');
    resolve()
}).then(function() {
    console.log('then');
})
console.log('console');

/**
 * setTimeout回调函数放入宏任务事件队列event queue
 * 立即执行new Promise,输出->promise ,将then()回调函数放入微任务事件队列
 * 执行console.log输出->console
 * 查看微任务
 * 执行then中的回调函数输出then
 * 第一轮事件循环结束
 * 从宏任务事件队列中取出任务
 * 执行setTimeout回调函数输出->setTimeout 
 * /

```

### 内存回收机制

栈内存中的基本数据类型，可以直接通过操作系统进行处理，而堆内存中的引用数据类型的值大小不确定，因此需要JS的引擎通过垃圾回收机制进行处理。

#### 引用计数

- 当变量进行声明并赋值后，值的引用数为1。
- 当同一个值被赋值给另一个变量时，引用数+1
- 当保存该值引用的变量被其它值覆盖时，引用数-1
- 当该值的引用数为0时，表示无法再访问该值了，此时就可以放心地将其清除并回收内存。

这种回收策略看起来很方便，但是当其进行循环引用时就会出现问题，会造成大量的内存不会被释放。当函数结束后，两个对象都不在作用域中，A 和 B 都会被当作非活动对象来清除掉,相比之下，引用计数则不会释放，也就会造成大量无用内存占用，这也是后来放弃引用计数，使用标记清除的原因之一。

#### 标记清除

- 垃圾收集器在运行时会给内存中的所有变量都加上一个标记
- 然后从各个根对象开始遍历，把还在被上下文变量引用的变量标记去掉标记
- 清理所有带有标牌机的变量，销毁并回收它们所占用的内存空间
- 最后垃圾回收程序做一次内存清理
使用标记清除策略的最重要的优点在于简单，无非是标记和不标记的差异。通过标记清除之后，剩余的对象内存位置是不变的，也会导致空闲内存空间是不连续的，这就造成出现内存碎片的问题。内存碎片多了后，如果要存储一个新的需要占据较大内存空间的对象，就会造成影响。对于通过标记清除产生的内存碎片，还是需要通过标记整理策略进行解决。

#### V8对于垃圾回收机制的优化

V8 的垃圾回收策略主要基于分代式垃圾回收机制，V8 中将堆内存分为新生代和老生代两区域，采用不同的垃圾回收器也就是不同的策略管理垃圾回收。
新生代的对象为存活时间较短的对象，简单来说就是新产生的对象，通常只支持 1～8M 的容量，而老生代的对象为存活事件较长或常驻内存的对象，简单来说就是经历过新生代垃圾回收后还存活下来的对象，容量通常比较大。V8 整个堆内存的大小就等于新生代加上老生代的内存，对于新老两块内存区域的垃圾回收，V8 采用了两个垃圾回收器来管控

##### 新生代内存回收

在64操作系统下分配为32MB，32位系统16M内存,因为新生代中的变量存活时间短，不太容易产生太大的内存压力，因此不够大也是能够理解。
对于新生代内存的回收，通常是通过Scavenge 的算法进行垃圾回收，就是将新生代内存进行一分为二，正在被使用的内存空间称为使用区，而限制状态的内存空间称为空闲区。

- 新加入的对象都会存放在使用区，当使用区快写满时就进行一次垃圾清理操作。
- 在开始进行垃圾回收时，新生代回收器会对使用区内的对象进行标记
- 标记完成后，需要对使用区内的活动对象拷贝到空闲区进行排序
- 而后进入垃圾清理阶段，将非活动对象占用的内存空间进行清理
- 最后对使用区和空闲区进行交换，使用区->空闲区，空闲区->使用区
新生代中的变量如果经过回收之后依然一直存在，那么会放入到老生代内存中，只要是已经经历过一次Scavenge算法回收的，就可以晋升为老生代内存的对象。
新生代存放的是新分配的小量内存，如果达到以下条件中的一个，将被分配至老生代
内存大小达到From space的25%
经历了From space <-> To space的一个轮回

##### 老生代内存回收

32位系统700M左右
64位系统1.4G左右
标记清除,标记整理进行老生代内存中的垃圾回收。
首先是标记阶段，从一组根元素开始，递归遍历这组根元素，遍历过程中能到达的元素称为活动对象，没有到达的元素就可以判断为非活动对象。清除阶段老生代垃圾回收器会直接将非活动对象，也就是数据清理掉。
同样的标记清除策略会产生内存碎片，因此还需要进行标记整理策略进行优化。

#### 内存泄漏

指在JS中已经分配内存地址的对象由于长时间未进行内存释放或无法清除

- 滥用闭包
-定时器或回调太多。与节点或数据相关联的计时器不再需要时，DOM节点对象可以清除，整个回调函数也不再需要。可是，计时器回调函数仍然没有被回收（计时器停止才会被回收）。当不需要setTimeout或setInterval时，定时器没有被清除，定时器的糊掉函数以及其内部依赖的变量都不能被回收，会造成内存泄漏。解决方法：在定时器完成工作时，需要手动清除定时器。
- 太多无效的DOM引用。DOM删除了，但是节点的引用还在，导致GC无法实现对其所占内存的回收。解决方法：给删除的DOM节点引用设置为null。
- 滥用全局变量,全局变量是根据定义无法被垃圾回收机制进行收集的，因此需要特别注意临时存储和处理大量信息的全局变量。如果必须使用全局变量来存储数据，请确保将其指定为null或在完成后重新分配它。解决方法：使用严格模式。
