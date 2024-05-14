---
title: JavaScript语法
date: 2021-05-22 21:23:11
categories:
  - web
---

## JavaScript

运行在客户端的脚本语言；不需要编译，运行过程中由 js 引擎逐行解释并执行,也可以用于后端 node.js 技术
浏览器引擎：
渲染引擎：内核
js 引擎：js 解释器

- 面向对象编程

类：一类具有相同特征事物的抽象概念;
对象：具体的某一个个体，具有唯一性的实例；

编程思想：

1.  分析有哪些实体
2.  设计实体的属性和功能
3.  实体间的相互作用

- 书写位置
  1. 行内式：`<input type="botton" onclick="alert('hello!!') />`Html 推荐双引号，js 推荐单引号

> 注释：外部脚本不能包含 `<script>` 标签。  
> 优势：分离了 HTML 和代码,使 HTML 和 JavaScript 更易于阅读和维护,已缓存的 JavaScript 文件可加速页面加载  
> 可通过完整的 URL 或相对于当前网页的路径引用外部脚本：`<script src="www.xxx.com/.."><script>`

2. 外部链接：`<script src="xxx.js"></script>`,标签中间不能写代码，会忽略
3. 内联式：`<script>alert('hello')</script>`

> js 脚本可被放置与 HTML 页面的 `<body>` 或`<head>` 部分中，或兼而有之。  
> 提示：把脚本置于`<body>` 元素的底部，可改善显示速度，因为脚本编译会拖慢显示。  
> 注释：旧的 JavaScript 例子也许会使用 type 属性：`<script type="text/javascript">`。  
> 注释：type 属性不是必需的。JavaScript 是 HTML 中的默认脚本语言。

### 堆栈

#### 栈

由编译器自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。
基本数据类型变量保存在栈内存中，因为基本数据类型占用空间小、大小固定，通过值来访问，属于被频繁使用的数据。

> 闭包中的基本数据类型变量是保存在堆内存里的，当函数执行完弹出调用栈后，返回一个内部函数的一个引用，这时候函数的变量就会转移到堆上，因此内部函数依然能访问到上一层函数的变量。

#### 堆

一般由程序员分配释放， 若程序员不释放，程序结束时可能由 OS 回收 。注意它与数据结构中的堆是两回事，分配方式倒是类似于链表
引用数据类型存储在堆内存中，引用数据类型占据空间大、大小不固定，如果存储在栈中，将影响程序的运行性能。
引用数据类型会在栈中存储一个指针，这个指针指向堆内存空间中该实体的起始地址。
当解释器寻找引用值时，会先检索其在栈中的地址，取得地址后，从堆中获得实体。

### 变量

变量是弱类型的,重复声明 JavaScript 变量，将不会丢它的值。

| 情况                     | 说明           | 结果                                                       |
| ------------------------ | -------------- | ---------------------------------------------------------- |
| var age;console.log(age) | 只声明不负值   | undefined                                                  |
| console.log(age)         | 不声明，不负值 | 报错                                                       |
| age=10;console.log(age)  | 不声明，只赋值 | 10(如果您为尚未声明的变量赋值，此变量会自动成为全局变量。) |

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

typeof 运算符不是变量。它属于运算符。运算符（比如 + - \* /）没有数据类型。
但是，typeof 始终会返回字符串（包含运算数的类型）。

`console.log(typeof age);`
`console.log(typeof(age));`

- instanceof 运算符
  instanceof 用于判断某个对象是否被另一个函数构造。

> 只能判断复杂类型
> instanceof，用于检测某个对象的原型链是否包含某个构造函数的 prototype 属性。
> instanceof 适用于检测对象，它是基于原型链运作的。
> instanceof 除了适用于任何 object 的类型检查之外，也可以用来检测内置对象，比如：Array、RegExp、Object、Function
> instanceof 对基本数据类型检测不起作用，主要是因为基本数据类型没有原型链。

- constructor 属性

> 每个原型对象上都有个 constructor 属性,指向该原型对象的构造函数
> constructor 属性返回所有 JavaScript 变量的构造器函数。
> `undefined`,`null`没有 constructor

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
true 参加数学加法时当做 1，false 为 0；

true:`-15` `"false"`
false:`0` `-0` `""` `undefined` `null` `false` `NaN`

#### `undefined`与`null`

undefined 表示尚未初始化的变量的值,这个变量从根本上就没有定义，而 null 表示该变量有意缺少对象指向,这个值虽然定义了，但它并未指向任何内存中的对象

```js
null == undefined; // true
null === undefined; // false
!!null === !!undefined; // true

JSON.stringify({ a: undefined }); // '{}'
JSON.stringify({ b: null }); // '{b: null}'
JSON.stringify({ a: undefined, b: null }); // '{b: null}'

let obj1 = { a: undefined };
let obj2 = {};
console.log(obj1.a); // undefined
console.log(obj2.a); // undefined
```

`undefined`与数字相加后结果为`NaN`;而`null`会隐式转换为 0
`undefined`其类型也将是`undefined`。而`null`的类型为对象。

> ypeof null 输出为 'object' 其实是一个底层的错误，但直到现阶段都无法被修复。
> 原因是，在 JavaScript 初始版本中，值以 32 位 存储。前 3 位 表示数据类型的标记，其余位则是值。
> 对于所有的对象，它的前 3 位 都以 000 作为类型标记位。在 JavaScript 早期版本中， null 被认为是一个特殊的值，用来对应 C 中的 空指针 。但 JavaScript 中没有 C 中的指针，所以 null 意味着什么都没有或者 void 并以 全 0(32 个) 表示。
> 因此每当 JavaScript 读取 null 时，它前端的 3 位 将它视为 对象类型 ，这也是为什么 typeof null 返回 'object' 的原因。
> 任何变量均可通过设置值等于 `undefined` （`void 0`） 或`null`进行清空。

#### 数字型

绝不要用前导零写数字（比如 07）。
`Infinity`:无穷大；
`-Infinity`:无穷小；
除以 0（零）也会生成 Infinity

```js
var x = 2 / 0; // x 将是 Infinity
var y = -2 / 0; // y 将是 -Infinity
```

##### `NaN`:非数字

> NaN 是数，typeof NaN 返回 number
> 尝试用一个非数字字符串进行除法会得到 NaN
> 可使用全局 JavaScript 函数 `isNaN()` 来确定某个值是否是数：

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
var z = x / y; // z 将是 10
var z = x * y; // z 将是 1000
var z = x - y; // z 将是 90
//警告！！
//JavaScript 的加法和级联（concatenation）都使用 + 运算符。
//如果您对一个数和一个字符串相加，结果也是字符串级联：
var z = x + y; // z 不会是 110（而是 10010）
```

##### 数值属性

| 属性              | 描述                             |
| ----------------- | -------------------------------- |
| MAX_VALUE         | 返回 JavaScript 中可能的最大数。 |
| MIN_VALUE         | 返回 JavaScript 中可能的最小数。 |
| NEGATIVE_INFINITY | 表示负的无穷大（溢出返回）。     |
| NaN               | 表示非数字值（"Not-a-Number"）。 |
| POSITIVE_INFINITY | 表示无穷大（溢出返回）。         |

> 数字属性不可用于变量
> 数字属性属于名为 number 的 JavaScript 数字对象包装器。
> 这些属性只能作为 Number.MAX_VALUE 访问。
> 使用 myNumber.MAX_VALUE，其中 myNumber 是变量、表达式或值，将返回 undefined：

实例

```js
var x = 6;
var y = x.MAX_VALUE; // y 成为 undefined
//只能如下使用
var x = Number.MIN_VALUE; //返回 JavaScript 中可能的最小数字。
```

#### 字符串

使用单引号或双引号，推荐单引号。在 js 中字符串既是基本数据类型，又是复合数据类型.

##### 声明

1.  new 运算符声明(对象）

    ```js
    var str = new String("100");
    ```

2.  省略 new

    ```js
    var str = String("jjj");
    ```

3.  字符串常量声明

    ```js
    var str = "jjj";
    ```

嵌套：`var str='啥"嘎"嘎和'; var str="航空'港行'供货商";`也可以加转义符`\`
字符串长度：`var str='画口红管工行';  alert(str.length);`

> 注：中文 UTF-8（三个字符代表一个汉字），gbk（两个字符表示一个汉字）但计数时都当成一个字来计数。

字符串拼接：`字符串+任何类型=拼接后的字符串`

#### js 判断字符串中是否包含某个字符串

- `indexOf()` 与`lastIndexOf`
  `indexOf()` 方法可返回某个指定的字符串值在字符串中首次出现的位置。（从 0 开始，未找到返回-1）
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

> 传入的不是正则会隐式转换为正则

      ```js
      var str = "123";
      console.log(str.search("3") != -1 );  //并返回匹配位置的下标
      ```

- `match()`

用于检索字符串中指定的子字符串，或正则表达式相匹配的子串数组,否则返回 null

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
var res = str.slice(7, 13);
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
var text1 = "Hello World!"; // 字符串
var text2 = text1.toUpperCase(); // text2 是被转换为大写的 text1
```

#### concat() 连接两个或多个字符串

```js
var text1 = "Hello";
var text2 = "World";
text3 = text1.concat(text2);
text4 = text1.concat("jjjjjj", text2, "jj");
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
alert(str.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, ""));
```

#### 提取字符串中的一个字符

这是两个提取字符串字符的安全方法：

- charAt(position)
  charAt() 方法返回字符串中指定下标（位置）的字符串：

```js
var str = "HELLO WORLD";
str.charAt(0); // 返回 H
```

- charCodeAt(position)

charCodeAt() 方法返回字符串中指定索引的字符 unicode 编码：

```js
var str = "HELLO WORLD";
str.charCodeAt(0); // 返回 72
```

- String.fromCharCode(码值 1，码值 2...）
  用于将 ASCII 码转换为对应字符,返回值为组合的字符串

```js
var str = String.fromCharCode(99, 93, 83);
```

- 属性访问（Property Access）

使用属性访问有点不太靠谱：

不适用 Internet Explorer 7 或更早的版本
它让字符串看起来像是数组（其实并不是）
如果找不到字符，[ ] 返回 undefined，而 charAt() 返回空字符串。
字符串是只读的，无法像数组一样通过索引下标去修改单个字符，除非整个修改。str[0] = "A" 不会产生错误（但也不会工作！）

> 提示：如果您希望按照数组的方式处理字符串，可以先把它转换为数组。

```js
var str = "HELLO WORLD";
str[0]; // 返回 H
```

- 把字符串转换为数组

可以通过 split(分割符/正则,限制返回的数量)分割字符串，返回分割的子串组成的数组。

```js
var txt = "a,b,c,d,e"; // 字符串
txt.split(","); // 用逗号分隔
txt.split(" "); // 用空格分隔
txt.split("|"); // 用竖线分隔
//如果省略分隔符，被返回的数组将是整个字符串即["string"]。
//若没有匹配，被返回的数组也是整个字符串即["string"]
//如果分隔符是 ""，被返回的数组将是间隔单个字符的数组
```

##### 正则表达式

正则表达式是构成搜索模式（search pattern）的字符序列。
语法：`/pattern/modifiers;`如：`var patt = /hello/i;`

###### 声明

1. new 声明

参数：第一个为字符串，第二个为修饰符(修饰符无顺序）

```js
var box1 = new RegExp("hello", "ig");
var box1 = RegExp("hello", "ig");
```

2. 省略 new 声明

```js
var box1 = /hello/gi;
```

正则表达式修饰符
修饰符可用于大小写不敏感的更全局的搜素：

| 修饰符 | 描述                                                     |
| ------ | -------------------------------------------------------- |
| i      | 执行对大小写不敏感的匹配。                               |
| g      | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
| m      | 执行多行匹配。                                           |

> 字符串中，遇到换行，重新开始计算行首；

正则表达式模式
括号用于查找一定范围的字符串：

| 表达式  | 描述                             |
| ------- | -------------------------------- | -------- | ------------------ |
| `[]`    | 查找方括号之间的任何符合的字符。 |
| `[^]`   | 匹配除括号内的字符               |
| `[0-9]` | 查找任何从 0 至 9 的数字。       |
| `(x     | y)`                              | 查找由 ` | ` 分隔的任何选项。 |

元字符（Metacharacter）是拥有特殊含义的字符：

| 元字符 | 描述                               |
| ------ | ---------------------------------- |
| .      | 匹配单个任意字符                   |
| \\d    | 查找数字。                         |
| \\D    | 匹配非数字                         |
| \\s    | 查找空白符、空格、制表符、换行符。 |
| \\S    | 匹配非空白字符                     |
| \\b    | 匹配空格字符。                     |
| \\n    | 查找换行符。                       |
| \\r    | 查找回车符。                       |
| \\t    | 查找制表符。                       |
| \\0    | 查找 null 符。                     |

锚字符
|||
|-|-|
|^|行首匹配|
|$|行尾匹配|

重复字符：(n 为任意的单个字符）

| 量词   | 描述                              |
| ------ | --------------------------------- |
| n?     | 匹配任何包含零个或一个 n 的字符。 |
| n+     | 匹配任何包含至少一个 n 的字符。   |
| n\*    | 匹配任何包含零个或多个 n 的字符。 |
| n{m,n} | 匹配至少 m 个，至多 n 个          |
| n{m}   | 必须匹配 m 个                     |
| (abd)+ | 小括号中的部分代表一个字符串处理  |

#### 数据类型转换

##### 数字转为字符串

|                                                   |                                    |
| ------------------------------------------------- | ---------------------------------- |
| toString()                                        | `var num=1;alert(num.toString());` |
| String()强制转换                                  | `var num=1; alert(String(num));`   |
| `+`拼接，只要有一边是字符串，另一半也会转为字符串 | `var num=1; alert(num+"");`        |

- 能够使用 toString() 方法把数输出为十六进制、八进制或二进制。

```js
var myNumber = 128;
myNumber.toString(16); // 返回 80
myNumber.toString(8); // 返回 200
myNumber.toString(2); // 返回 10000000
```

- toExponential() 返回字符串值，它包含已被四舍五入并使用指数计数法的数字。

```js
var x = 9.656;
x.toExponential(2); // 返回 9.66e+0
x.toExponential(4); // 返回 9.6560e+0
x.toExponential(6); // 返回 9.656000e+0
```

- toFixed() 返回字符串值，它包含了指定位数小数的数字：

```js
var x = 9.656;
x.toFixed(0); // 返回 10
x.toFixed(2); // 返回 9.66
x.toFixed(4); // 返回 9.6560
x.toFixed(6); // 返回 9.656000
```

- toPrecision() 返回字符串值，它包含了指定长度的数字：

```js
var x = 9.656;
x.toPrecision(); // 返回 9.656
x.toPrecision(2); // 返回 9.7
x.toPrecision(4); // 返回 9.656
x.toPrecision(6); // 返回 9.65600
```

> 在 JavaScript 中，数字可以是原始值（typeof = number）或对象（typeof = object）。
> 在 JavaScript 内部使用 valueOf() 方法可将 Number 对象转换为原始值。
> 所有 JavaScript 数据类型都有 valueOf() 和 toString() 方法。

##### 字符串转数字

|                                                                                   |                      |
| --------------------------------------------------------------------------------- | -------------------- |
| `parseInt(string)`保留整数,去掉单位;只返回首个数字;如果无法转换为数值，则返回 NaN | `parseInt('11');`    |
| `parseFloat(string)`只返回首个数字,包括小数部分                                   | `parseFloat('222');` |
| `Number()`强制转换                                                                | `Number('22');`      |
| js 隐式转换（减，乘，除)                                                          | `'11'-0`             |

> Number() 还可以把日期转换为数字：`Number(new Date("2019-04-15"));    // 返回 1506729600000`方法返回 1970 年 1 月 1 日至今的毫秒数。

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
console.log(+a); // -1
console.log(+b); // NaN
console.log(+c); // NaN

let a = -1;
let b = 2;
console.log(-a); // 1
console.log(-b); // -2
```

- `void`操作符

由于 `void` 运算符的优先级比较高，高于普通运算符的优先级，所以在使用时应该使用小括号明确 `void` 运算符操作的操作数，避免引发错误。

```js
let a = (b = c = 2); // 定义并初始化变量的值
d = void (a -= b *= c += 5); // 执行void运算符，并把返回值赋予变量d
console.log(a); // -12
console.log(b); // 14
console.log(c); // 7
console.log(d); // undefined
```

#### 加减乘除操作符

- 加

```js
1 + 2; // 3
"1" + "2"; // "12"
"1" + 2; // "12"
1 + {}; // "1[object Object]"
true + false; // 1  布尔值会先转为数字，再进行运算
1 + null; // 1 null会转化为0，再进行计算
1 + undefined; // NaN undefined转化为数字是NaN
```

如果有一个操作数是 NaN，则结果是 NaN；
如果是 Infinity 加 Infinity，则结果是 Infinity；
如果是-Infinity 加-Infinity，则结果是-Infinity；
如果是 Infinity 加-Infinity，则结果是 NaN；
如果是+0 加+0，则结果是+0；
如果是-0 加-0，则结果是-0；
如果是+0 加-0，则结果是+0。

- 减
  减法操作和加法操作符类似， 但是减法操作符只能用于数值的计算，不能用于字符串的拼接

```js
3 - 1; // 2
3 - true; // 2
3 - ""; // 3
3 - null; // 3
NaN - 1; // NaN
```

如果是 Infinity 减 Infinity，则结果是 NaN；
如果是-Infinity 减-Infinity，则结果是 NaN；
如果是 Infinity 减-Infinity，则结果是 Infinity；
如果是-Infinity 减 Infinity，则结果是-Infinity；
如果是+0 减+0，则结果是+0；
如果是-0 减+0，则结果是-0；
如果是-0 减-0，则结果是+0。

- 乘
  如果有一个操作数是 NaN，则结果是 NaN；
  如果 Infinity 与 0 相乘，则结果是 NaN；
  如果 Infinity 与非 0 数值相乘，则结果是 Infinity 或-Infinity，取决于有符号操作数的符号；
  如果 Infinity 与 Infinity 相乘，则结果是 Infinity。

- 除法`/`
  如果有一个操作数是 NaN，则结果是 NaN；
  如果 0 除以 0，则结果是 NaN；
  如果 Infinity 除以 Infinity，则结果是 Infinity。
  如果是非零的有限数被零除，则结果是 Infinity 或-Infinity，取决于有符号操作数的符号；
  如果是 Infinity 被任何非零数值除，则结果是 Infinity 或-Infinity，取决于有符号操作数的符号。

- 取余`%`
  如果被除数是无穷大值而除数是有限大的数值，则结果是 NaN；
  如果被除数是有限大的数值而除数是零，则结果是 NaN；
  如果是 Infinity 被 Infinity 除，则结果是 NaN；
  如果被除数是有限大的数值而除数是无穷大的数值，则结果是被除数；
  如果被除数是零，则结果是零。

- 幂运算`**`

`Math.pow(2, 10);    // 1024`

#### 关系操作符

小于（<）
大于（>）
小于等于（<=）
大于等于（>=）

如果这两个操作数都是数值，则执行数值比较；
如果两个操作数都是字符串，则比较两个字符串对应的字符编码值；
如果一个操作数是数值，则将另一个操作数转换为一个数值，然后执行数值比较；
如果一个操作数是对象，则调用这个对象的 valueOf()方法，并用得到的结果根据前面的规则执行比较；
如果一个操作数是布尔值，则先将其转换为数值，然后再执行比较。

#### 比较操作符

- 等于（`==`）不等于（`!=`）

首先会判断两者类型是否相同，相同的话就比较两者的大小；
类型不相同的话，就会进行类型转换；
判断两者类型是否为 string 和 number，是的话就会根据 ASCII 将字符串转换为 number
判断两者类型是否为 string 和 string，是的话就会根据 ASCII 将字符串转换为 number,从左到右比较，第一位相等就比较下一位，由此类推
判断其中一方是否为 boolean，是的话就会把 boolean 转为 number 再进行判断
判断其中一方是否为 object 且另一方为 string、number 或者 symbol，是的话就会把 object 转为原始类型再进行判断

- 全等（`===`）不全等（`!==`）
  只有当两个操作数的数据类型和值都相等时，才会返回 true。它并不会进行数据类型的转化。​

```js
//NaN不等于任何值，包括它本身
NaN == NaN; //false
NaN === NaN; //false
null == undefined; //true
null === undefined; //false
1 == "1"; //true
1 === "1"; //false

//浏览器自己隐式创建的包装对象和我们显式创建的包装对象不严格相等，我们举个例子说明下：
var name = "神奇的程序员";
var info = new String("神奇的程序员");
console.log(name == info); // true
console.log(name === info); // false
```

- 浮点数相等比较

```js
1 / 3 === 1 - 2 / 3; // false
// 浮点数在运算过程中会产生误差，因为计算机无法精确表示无限循环小数。要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值
Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true
```

#### 逻辑运算符

- 逻辑非

如果操作数是非 0 数值（包括 Infinity)，则返回 false；
如果操作数是 null，则返回 true；
如果操作数是 NaN，则返回 true；
如果操作数是 undefined， 则返回 true.

逻辑非操作符也可以用于将任何值转化为布尔值，同时使用两个!，相当于调用了 Boolean()方法：

```js
!!"blue"; // true
!!0; // false
!!NaN; // false
!!""; // false
!!12345; // true
```

- 逻辑与
  `表达式1&&表达式2`如果第一个表达式为真，则返回表达式 2，否则返回表达式 1

> `0` `''` `null` `undefind` `NaN`为假

- 逻辑或
  `表达式1||表达式2`如果表达式 1 为真则返回表达式 1，否则返回表达式 2

#### 空值合并操作符（`??`）

只有 ?? 左边为 null 或 undefined 时才返回右边的值：

```js
const dogName = false;
const name = dogName ?? "default"; // name = false;
```

#### 可选链操作符（`?.`）

在引用为 null 或 undefined 的情况下不会引起错误，该表达式短路返回值是 undefined。与函数调用一起使用时，如果给定的函数不存在，则返回 undefined。

```js
const name = system?.user?.addr?.province?.name || "default";
a?.[x];
// 等同于
a == null ? undefined : a[x];

a?.b();
// 等同于
a == null ? undefined : a.b();

a?.();
// 等同于
a == null ? undefined : a();
```

#### 位运算符

1）原码

原码就是一个数的二进制数。例如：10 的原码为 0000 1010

2）反码

正数的反码与原码相同，如：10 反码为 0000 1010
负数的反码为除符号位，按位取反，即 0 变 1，1 变 0。

3）补码

正数的补码与原码相同，如：10 补码为 0000 1010
负数的补码是原码除符号位外的所有位取反即 0 变 1，1 变 0，然后加 1，也就是反码加 1。

位运算符处理 32 位数。

该运算中的任何数值运算数都会被转换为 32 位的数。结果会被转换回 JavaScript 数。

| 运算符 | 描述                                                       | 例子    | 等同于       | 结果 | 十进制 |
| ------ | ---------------------------------------------------------- | ------- | ------------ | ---- | ------ |
| `&`    | 与                                                         | 5 & 1   | 0101 & 0001  | 0001 | 1      |
| \|     | 或                                                         | 5 \| 1  | 0101 \| 0001 | 0101 | 5      |
| `~`    | 非                                                         | ~ 5     | ~0101        | 1010 | 10     |
| `^`    | 异或                                                       | 5 ^ 1   | 0101 ^ 0001  | 0100 | 4      |
| `<<`   | 各二进制位全部左移若干位,高位丢弃，低位补 0                | 5 << 1  | 0101 << 1    | 1010 | 10     |
| `>>`   | 各二进制位全部右移若干位，正数左补 0，负数左补 1，右边丢弃 | 5 >> 1  | 0101 >> 1    | 0010 | 2      |
| `>>>`  | 零填充右位移                                               | 5 >>> 1 | 0101 >>> 1   | 0010 | 2      |

> 无符号右移操作符（>>>）会将数值的 32 位全部右移。对于正数，有符号右移操作符和无符号右移操作符的结果是一样的。对于负数的操作，两者就会有较大的差异。
> 无符号右移操作符将负数的二进制表示当成正数的二进制表示来处理。所以，对负数进行无符号右移操作之后就会变的特别大。

> 上例使用 4 位无符号的例子。但是 JavaScript 使用 32 位有符号数。

> 因此，在 JavaScript 中，\~ 5 不会返回 10，而是返回 -6。

> \~00000000000000000000000000000101 将返回 11111111111111111111111111111010。

幂运算符为`**`而不是`^`,也可以用`Math.pow(x,y)`实现

#### 运算符号优先级

|            |                               |
| ---------- | ----------------------------- | --- | --- |
| 小括号     | `()`                          |
| 一元运算符 | `++`,`--`,`!`                 |
| 算数运算符 | 先`*`,`/`,`%`,`**`,后`+`,`-`, |
| 关系运算符 | `>`,`>=`,`<=`,`<`,            |
| 相等运算符 | `==`,`===`,`!==`,`!=`,        |
| 逻辑运算符 | 先`&&`后`                     |     | `   |
| 赋值       | `=`                           |
| 逗号       | `,`                           |

### 数组

#### 创建数组

数组是对象

- 利用 new 创建数组

```js
var a = new Array(); //注意大写
var a = new Array(2); //只有一个数字时，为直接创建长度为2的空数组
var cars = new Array("Saab", "Volvo", "BMW"); //也可以直接赋值
```

- 数组字面量方式创建

```js
var a = [];
var a = ["xxx", 77, true];
```

- 获取数组元素：`数组名[索引号]`

```js
console.log(a[0]);
//二位数组,只对字符串有用
console.log(a[0][0]); //结果为第二个x
console.log(a[2][0]); //结果为undefined
```

- 数组长度：`数组名.length`

```js
console.log(a.length); //新增数组元素
//修改数组索引追加数组元素
//不能直接给数组名赋值，否则会覆盖以前的数据
```

- 变量里存放的是数组的地址

```js
var arr = ["jjj", "222", true, 222];
var arr1 = arr;
console.log(arr1);
arr1.push("end");
console.log("push:", arr1);
console.log(arr); //arr也会一起变化
```

#### 数组遍历

```js
const array = [10, 11, 12, 13, 14];

// for循环
for (let i = 0; i < array.length; i++) {
  console.log(array[i]);
}

// for ..in
//遍历数组时也会遍历非数字键名,所以不推荐 for..in 遍历数组
for (let i in array) {
  console.log(i, array[i]);
}

// forEach
//用来遍历数组中的每一项，不影响原数组，性能差
//不支持break和continue，return只会跳过本次循环，可用throw抛出异常配合try...catch来结束循环
array.forEach(function (item, index, array) {
  console.log(item, index, array);
  // 不能通过item改变数组原来的值
  //array参数是浅拷贝
});

// for....of
//语句支持return和 break
for (let item of array) {
  //item为当前遍历到的元素
}
// while 循环
let i = 0;
while (i < array.length) {
  console.log(array[i]);
  i++;
}
//map()
array.map(function (item) {
  return item;
});
```

#### 数组的操作方法

11 种：push,pop,shift,unshift,splice,slice,concat,join,sort,reverse,toString

- JS 数组添加元素

  1.  在末尾追加`push()`；

      ```js
      //向arr数组的末尾追加元素，
      arr.push("lemon", "cucumber");
      alert(arr);
      //push()方法会返回新数组的长度
      ```

      也可以使用 `length` 属性向数组添加新元素：

      ```js
      var fruits = ["Banana", "Orange", "Apple", "Mango"];
      fruits[fruits.length] = "Lemon"; // 向 fruits 添加一个新元素 (Lemon)
      //添加具有高索引的元素会在数组中创建未定义的undefined
      fruits[10] = "Lemon";
      ```

  2.  在第一位添加`unshift()`；

      ```js
      //向arr数组的第一位添加元素，
      arr.unshift("peach", "pear"); //返回新数组的长度。
      alert(arr);
      ```

  3.  在指定位置添加`splice()`;

      ```js
      ///从数组中添加/删除项目，然后返回被删除的项目,如果有的话。
      arr.splice(3, 0, "元素1", "元素2");
      alert(arr);
      //arrayObject.splice(index,howmany,item1,.....,itemX)
      //index 必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
      //howmany 必需。要删除的项目数量。如果设置为 0，则不会删除项目。
      ```

- 删除元素

  1.  `pop()` 方法从数组中删除最后一个元素：

      ```js
      var fruits = ["Banana", "Orange", "Apple", "Mango"];
      fruits.pop(); // 从 fruits 删除最后一个元素（"Mango"）

      var fruits2 = ["Banana", "Orange", "Apple", "Mango"];
      // pop() 方法返回被删除的值：
      var x = fruits2.pop(); // x 的值是 "Mango"
      ```

  2.  `shift()` 方法会删除首个数组元素，

      ```js
      var fruits = ["Banana", "Orange", "Apple", "Mango"];
      fruits.shift(); // 从 fruits 删除第一个元素 "Banana"
      //shift() 方法返回被删除的字符串即 返回 "Banana"
      ```

  3.  `delete` 运算符来删除：

      ```js
      var fruits = ["Banana", "Orange", "Apple", "Mango"];
      delete fruits[0]; // 把 fruits 中的首个元素改为 undefined
      //用 delete 会在数组留下未定义的空洞。请使用 pop() 或 shift() 取而代之。
      //原型中声明的属性和对象自带的属性无法被删除；
      //通过var声明的变量和通过function声明的函数拥有dontdelete特性，是不能被删除。
      ```

  4.  使用 `splice()` 来删除元素

      ```js
      //通过聪明的参数设定，您能够使用 splice() 在数组中不留“空洞”的情况下移除元素：
      var fruits = ["Banana", "Orange", "Apple", "Mango"];
      fruits.splice(0, 1); // 删除 fruits 中的第一个元素
      ```

- `concat()` 方法通过合并现有数组来创建一个新数组（浅拷贝,第一层有效）：

  ```js
  var myGirls = ["Cecilie", "Lone"];
  var myBoys = ["Emil", "Tobias", "Linus"];
  var myChildren = myGirls.concat(myBoys, "jjjj", 11); // 连接 myGirls 和 myBoys,"jjj"等
  //concat() 方法不会更改现有数组。它总是返回一个新数组。
  //concat() 方法可以使用任意数量的数组参数：
  ```

- `join()` 方法

会将数组内的所有元素结合为一个字符串。当然您还可以规定分隔符(默认以逗号分割)：`array.join(",")`

- `toString()`方法

把数组转换为数组值（逗号分隔）的字符串。

- `slice(start,end)`裁剪数组
  方法用数组的某个片段切出新数组。**原数组不会发生改变**

```js
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1); //默认到结尾
```

- `sort()` 方法以字母顺序对数组进行排序：

该函数很适合字符串（"Apple" 会排在 "Banana" 之前）。不过，如果数字按照字符串来排序，则 "25" 大于 "100"，因为 "2" 大于 "1"。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort(); // 对 fruits 中的元素进行排序
```

`sort()` 方法在对数值排序时会产生不正确的结果。我们通过一个比值函数来修正此问题：

```js
var points = [40, 100, 1, 5, 25, 10];
points.sort(function (a, b) {
  return a - b;
}); //升序，从小到大
//比值函数
//比较函数的目的是定义另一种排序顺序。
//比较函数应该返回一个负，零或正值，这取决于参数：
//function(a, b){return a-b}
//当 sort() 函数比较两个值时，会将值发送到比较函数，并根据所返回的值（负、零或正值）对这些值进行排序。
```

- `reverse()` 方法反转数组

您可以使用它以降序对数组进行排序：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort(); // 对 fruits 中的元素进行排序
fruits.reverse(); // 反转元素顺序
```

### 对象

- 利用对象字面量创建对象{}

```js
var obj = {}; //创建了一个空的对象
var obj = {
  username: "张山",
  age: 18,
  sex: "男",
  hi: function () {
    console.log("hello!!!!!!!!!!!!!!");
  },
};
```

- 利用 new Object 创建对象

```js
var obj1 = new Object(); //创建一个空对象,可省略new
obj1.username = "张山";
obj1.age = 18;
obj1.sex = "男";
obj1.hi = function () {
  console.log("hello!!!!!!!!!!!!!!");
};
```

- 利用构造函数创建对象

```js
//function 构造函数名(){
//this.属性名=值;
//this.方法名=function(){}
//}
// new 构造函数名();
function Star(username, age, sex) {
  this.name = username;
  this.age = age;
  this.sex = sex;
}
new Star("刘德华", 58, "男");
```

当通过 new 关键字调用时会创建一个空的对象实例，并将其作为 this 传递给构造函数。 new 操作内部主要执行了以下几个步骤：

1.创建一个新的空对象。

2.将新对象的 `__proto__` 指向构造函数的 propertyper。

3.该对象作为 this 参数传递给构造函数，从而成为构造函数的函数上下文，并执行构造函数中的语句。

4.新构造的对象作为 new 运算符的返回值，分以下两种情况讨论

如果构造函数返回一个对象，则该对象将作为整个表达式的值返回，而传入构造函数的 this 将被丢弃。
但是，如果构造函数返回的是非对象类型，则忽略返回值，返回新创建的对象。

- 访问属性

```js
//里面的属性或方法，采用键值对的形式->  属性名： 属性值
//多个属性或方法中间用逗号隔开
//方法冒号后面跟的是一个匿名函数
//调用对象的属性，采用 对象名.属性名
console.log(obj1.uname);
//或者 对象名['属性名']
console.log(obj1["age"]);
//调用对象的方法-> 对象名.方法名
obj1.hi();
//如果您不使用 () 访问 fullName 方法，则将返回函数的具体内容
```

- 遍历对象

```js
//for(变量 in 对象){}
for (let k in obj) {
  console.log(k, obj[k]);
}
```

- 删除属性
  `delete`只能删除对象自己的属性，不能删除其原型链上的属性
  全局作用域中的函数不能被 delete, 不论是使用关键字声明还是使用函数表达式的
  使用 delete 操作符并不会直接释放内存

```js
let a = { b: 1, c: 2 };
//成功返回true,如果删除对象不存在的属性，delete 无效，但是返回值仍然为 true
delete a.b;

//delete删除属性时只是解除了属性与对象的绑定，故当属性值为一个对象时，删除时会造成内存泄露 （其实还未删除）
//解决方法，用递归！！
// 将不需要的属性设置为 undefined

var person = {
  name: {
    firstname: "bob",
  },
};
var p = person.name;
delete person.name;
console.log(p.firstname); //bob
```

#### prototype 原型对象

JS 的每个函数在创建的时候，都会生成一个属性`prototype`，这个属性指向一个对象，这个对象就是此函数的`原型对象`。该原型对象中有个属性为`constructor`，指向该函数。这样原型对象和它的函数之间就产生了联系。

```js
function show() {}
console.log(show.prototype);
```

用在构造函数上，我们可以给构造函数的原型 prototype，添加方法。如果我们将方法添加到构造函数的原型 prototype 对象上，构造函数构造出来的对象共享原型上所有的方法

每个通过构造函数创建出来的实例对象，其本身有个属性`__proto__`，这个属性会指向该实例对象的`构造函数的原型对象`
当访问一个对象的某个属性时，会先在这个对象本身属性上查找，如果没有找到，则会通过它的`__proto__`隐式属性，找到它的`构造函数的原型对象`，如果还没有找到就会再在其构造函数的 prototype 的`__proto__`中查找，这样一层一层向上查找就会形成一个链式结构，我们称为原型链。原型链的尽头是 null

```js
// 构造函数
function Preson(name, age) {
  this.name = name;
  this.age = age;
}
// 所有实例共享的公共方法
Preson.prototype.say = function (word) {
  console.log(`${this.name}说：${word}`);
};

const p1 = new Preson("张三", 18); // 创建一个Person实例对象
const p2 = new Preson("李四", 20); // 新创建一个Proson实例对象
p1.say("hello world"); // 调用公共方法
p1.hasOwnProperty("say"); // false 说明不是定义在其本身上的
p1.__proto__.do = function () {
  console.log("往原型对象中添加方法");
};
p2.do(); // 打印出了-往原型对象中添加方法
```

instanceof 关键字，判断某一个对象是否是这个构造函数构造出来的。

```js
var arr1 = [3, 32, 4, 1, 45];
var arr2 = [4, 3, 2, 1];
Array.prototype.sum = function () {
  var res = 0;
  for (var i = 0; i < this.length; i++) {
    res += this[i];
  }
  return res;
};
console.log(arr1.sum());
console.log(arr2.sum());
console.log(arr1.sum == arr2.sum);
```

在子一级构造函数重写方法，只会在子一级生效，并不会影响父一级构造函数的方法。

继承和多态：
继承侧重是从父一级构造函数，继承到属性和方法；
多态侧重的是子一级自己重写和新增的属性和方法;

#### 内置对象（如同内置函数,如:Math、 Date、 Array、 String、）

Math 对象

```js
Math.PI; //圆周率
Math.abs(); //求绝对值
Math.max(); //求最大
Math.min(); //求最小值
Math.pow(x, y); //x的y次幂
Math.round(); //四舍五入
Math.random(); //获取随机数
Math.ceil(); // 向上取整,有小数就整数部分加1
Math.floor(); // 向下取整,丢弃小数部分
```

- Date 时间对象方法

```js
//时间对象是静态的
//必须实例化
var nowdate = new Date();
console.log(nowdate);

//Date()构造函数的参数
//自定义日期格式
var nowdate = new Date("2020-2-22");
```

- 时间对象方法

```js
//日期格式化
new Date().getFUllYear(); //年
new Date().getMonth(); //月(0-11)
new Date().getDate(); //天
new Date().getDay(); //星期（周日0到周六6）
new Date().getHours(); //时
new Date().getMinutes(); //分
new Date().getSeconds(); //秒
//获取Date总毫秒数，距离1970.1.1的总毫秒数
var date = new Date();
console.log(date.valueOf());
//或
console.log(date.getTime());
//或
var date1 = +new Date(); //常用
console.log(date1);
//或
console.log(Date.now()); //H5新增
```

- 设置日期对象的日期值

| 方法                  | 描述                                         |
| --------------------- | -------------------------------------------- |
| setDate()             | 以数值（1-31）设置日                         |
| setFullYear(年,月,日) | 设置年（可选月和日）                         |
| setHours()            | 设置小时（0-23）                             |
| setMilliseconds()     | 设置毫秒（0-999）                            |
| setMinutes()          | 设置分（0-59）                               |
| setMonth()            | 设置月（0-11）                               |
| setSeconds()          | 设置秒（0-59）                               |
| setTime()             | 设置时间（从 1970 年 1 月 1 日至今的毫秒数） |

- 比较日期

日期可以很容易地进行比较。

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
```

### JS this

它拥有不同的值，具体取决于它的使用位置：

- 作为函数直接被调用

如果一个函数没有作为方法、构造函数或者通过 apply 和 call 调用的话，我们就称之为作为 直接函数调用。 非严格模式下，this 指向全局对象 window（浏览器执行环境），但是在严格模式下，this 指向为 undefined。

```js
function test() {
  console.log(this);
}
test(); //window
```

- 作为方法，关联在对象上被调用或作为构造函数，实例化一个新对象时候被调用
  在对象方法中，this 指的是此方法的“拥有者”。

```js
var person = {
  firstName: "Bill",
  lastName: "Gates",
  id: 678,
  test: function() {
   console.log(this)
}
}
person.test()//person

function Person(name) {
   this.name = name
   test: function() {
   console.log(this)
}
}

let personins = new Person('xx')
persoins.test()//persoins
```

- 单独的情况下，this 指的是全局对象。

在单独使用时，拥有者是全局对象，因此 this 指的是全局对象。在浏览器窗口中，全局对象是 Window：

```js
var x = this;
```

在严格模式中，如果单独使用，那么 this 指的是全局对象 Window

- 在函数中，this 指的是全局对象。

在 JavaScript 函数中，函数的拥有者默认绑定 this。因此，在函数中，this 指的是全局对象 Window。

```js
function myFunction() {
  return this;
}
```

箭头函数没有自己的 this，其中的 this 指的是 window 全局对象,在函数中使用时，在严格模式下，this 是未定义的（undefined）

- 在事件中，this 指的是接收事件的元素。

在 HTML 事件处理程序中，this 指的是接收此事件的 HTML 元素：

```html
<button onclick="this.style.display='none'">点击来删除我！</button>
```

- 像 call() 和 apply() 这样的方法可以将 this 引用到任何对象。

  - call
    格式：函数名.call();
    参数：第一个参数：传入该函数 this 指向的对象，传入什么强制指向什么。
    第二参数开始：将原函数的参数往后顺延一位。
    会立即调用函数,返回值就是函数的返回值

  - apply
    格式：函数名.apply()
    参数：第一个参数：传入该函数 this 指向的对象，传入什么强制指向什么。
    第二个参数：数组 其放入我们所有原来的参数。
    会立即调用函数,返回值就是函数的返回值

  - bind 预设 this 指向
    参数：第一个参数：传入该函数 this 指向的对象，传入什么强制指向什么。
    第二参数开始：将原函数的参数往后顺延一位。
    不会调用函数，返回值是改变 this 指向后的新函数

```js
function show(x, y) {
  console.log(this);
  console.log(x);
  console.log(y);
}
var res = show.bind("bind");
console.log(res);
```

call() 和 apply() 方法是预定义的 JavaScript 方法。
它们都可以用于将另一个对象作为参数调用对象方法。
在下面的例子中，当使用 person2 作为参数调用 person1.fullName 时，this 将引用 person2，即使它是 person1 的方法：

```js
var person1 = {
  fullName: function () {
    return this.firstName + " " + this.lastName;
  },
};
var person2 = {
  firstName: "Bill",
  lastName: "Gates",
};
person1.fullName.call(person2); // 会返回 "Bill Gates"
```

### 函数

- 声明
  在函数中，声明的参数是局部变量。只能在函数内访问。局部变量在函数开始时创建，在函数完成时被删除。

```js
//函数关键字声明
function a() {
  //具名函数
  console.log("xxxxxxxxx");
}

//函数表达式声明
var b = function () {
  //匿名函数
};
```

- 参数

```js
//function 函数名（形参1，形参2...）{//形参即形式上的参数}
function b(n) {
  console.log(n);
}
b("xxx");
//实参与形参个数一致时，正常输出
//实参小于形参时，多余的形参可看作不用声明的变量，结果为undefined
//实参多于形参时，只取到形参的个数

function fn() {
  console.log(arguments);
}
fn(1, 2, 2, 34, 9);
//arguments里储存了所有传过来的实参，它是一个伪数组，具有length属性，可用索引的方式遍历，但不具有数组的push，pop等方法
```

- 函数的返回值

```js
function c(){
return 返回的内容；
}
var n=c();
//return 之后的代码不会执行；
//return只返回一个值，且只返回最后一个值。
//return可返回数组，字符串，
//如果没有return则返回undefind
```

- 递归

函数自己调用自己，一般情况下有参数，有 return

1.  首先找到临界值，即无需计算获得的值；
2.  找到这一次和上一次的关系
3.  假设当前函数已经可以使用，调用自身计算上一次。

#### 构造函数

这种通过 new 调用函数，叫做构造函数(首字母一般大写)，其可构造对象，

1.  当前函数中的 this 指向新创建的对象
2.  自动完成原料和出厂操作

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
- 内部 return 返回值无效,无需写
- 构造函数中的 this 指向新的对象

```js
function Pig(name) {
  this.name = name;
}
const p = new Pig("xxx"); //实例化
```

- 静态成员（静态属性，静态方法）

> 只能通过构造函数访问，
> 静态方法中的 this 指向构造函数

```js
function Person(name, age) {}
//静态属性
Person.eyes = 2;
//静态方法
Person.walk = function () {};
```

### 作用域

- JavaScript 变量的有效期

JavaScript 变量的有效期始于其被创建时。
局部变量会在函数完成时被删除，其又分为函数作用域和块作用域。
全局变量会在您关闭页面是被删除。

函数参数也是函数内的局部变量。
在 HTML 中，全局作用域是 window。所有全局变量均属于 window 对象。script 标签和 js 文件的最外层都是全局作用域
实例

```js
var carName = "porsche";
// 此处的代码能够使用 window.carName
```

- 变量提升

> let/const 不会变量提升
> 在 JavaScript 中，可以在使用变量之后对其进行声明。

```js
//作用域链：内部函数访问外部函数变量，采用的是链式查找方式来取值的，其采取就近原则
//优先在当前函数作用域中查找，若不存在，则逐级查找父级作用域直到全局作用域
var num = 10;
function fn() {
  var num = 20;
  function fun() {
    console.log(num); //结果为20
  }
  return fun();
}
```

```js
//js引擎运行分两步
//预解析:js引擎会把js里的所有var， function提升到当前作用域的最前面，但不提升赋值操作
//代码执行：顺序从上往下执行
fun();
var fun = function () {
  xxx;
}; //会报错
//-------------
fun();
function fun() {
  xxx;
} //正常运行
```

```js
//预编译:创建AO对象;找已声明的变量,值都是undefined;实参形参值相等;找函数声明，会覆盖形参的值
function fn(a, c) {
  console.log(c); //2
  console.log(a); //function a()
  function a() {}

  var a = 12;
  console.log(a); //12

  console.log(b); //undefined
  var b = function () {};
  console.log(b); //function b()
}
fn(1, 2);
```

JavaScript 初始化不会被提升
JavaScript 只提升声明，而非初始化。

```js
var x = 5; // 初始化 x
elem = document.getElementById("demo"); // 查找元素
elem.innerHTML = x + " " + y; // 显示 x 和 y

var y = 7; // 初始化 y
```

### 定时器

- setInterval("函数"，毫秒数);
  每隔对应的毫秒执行一次传入的函数;
  返回值为：系统给启动定时器分配的编号；

- clearInterval(定时器编号);
  取消定时器

### 延时器

- 创建延时器`window.setTimeout(函数，延迟时间)`

返回值 timeoutID 是一个正整数，表示定时器的编号需要注意的是 setTimeout()和 setInterval()共用一个编号池，技术上，clearTimeout()和 clearInterval() 可以互换。但是，为了避免混淆，不要混用取消定时函数。

```js
var timer1 = window.setTimeout(function () {
  onsole.log("你好啊！！！");
}, 3000);
```

- 清除延时器`window.clearTimeout(延时器名称)`

### JS 捕获错误

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
```

### JS 闭包

- 闭包特点：

  1.  函数嵌套函数
  2.  内部函数使用外部函数的形参和变量
  3.  被引用的形参和变量不会被内存回收

- 好处：
  1.  变量能常驻内存当中
  2.  避免全局变量污染
  3.  可以声明私有成员

JavaScript 变量属于本地或全局作用域。

全局变量能够通过闭包实现局部（私有）。
闭包指的是有权访问父作用域的函数，即使在父函数关闭之后

> 拥有相同名称的全局变量和局部变量是不同的变量。修改一个，不会改变其他。不通过关键词 var 创建的变量总是全局的，即使它们在函数中创建。

```js
var add = (function () {
  var counter = 0;
  return function () {
    return (counter += 1);
  };
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
var moduleA = (function (mod) {
  var conut = 10; //私有变量
  function showA() {
    //私有函数
    count += 20;
    console.log(count);
  }
  function showB() {
    count *= 10;
    console.log(count);
  }
  mod.outA = showA;
  mod.outB = showB;
  //对外暴露
  return mod;
})(moduleA || {});

var moduleA = (function (mod) {
  function showC() {
    console.log("jjjj");
  }
  mod.outC = showC;
  return mod;
})(moduleA || {});

mouleA.outA();
mouleA.outB();
mouleA.outC();
```

- 立即执行函数写法

```js
var test = (function () {
  var c = 0;
  var m1 = function () {
    xxxx;
  };
  var m2 = function () {
    xxxx;
  };
  return {
    m1: m1,
    m2: m2,
  };
})();
```

### ES6 新特性

#### let 和 const 关键字(es6)

用 let 或 const 声明的变量和常量不会被提升！遇到大括号就形成作用域。

> 全局（在函数之外）声明的变量拥有全局作用域。
> 局部（函数内）声明的变量拥有函数作用域。

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

```js
var x = 10;
// 此处 x 为 10
{
  let x = 6;
  // 此处 x 为 6
}
// 此处 x 为 10
```

> Internet Explorer 11 或更早的版本不完全支持 let 关键词。

- var 与 let 区别

通过 var 关键词定义的全局变量属于 window 对象
通过 let 关键词定义的全局变量不属于 window 对象
允许在程序的任何位置使用 var 重新声明 JavaScript 变量
在相同的作用域，或在相同的块中，通过 let 重新声明一个 var 变量是不允许的
在相同的作用域，或在相同的块中，通过 let 重新声明一个 let 变量是不允许的
在相同的作用域，或在相同的块中，通过 var 重新声明一个 let 变量是不允许的
在不同的作用域或块中，通过 let 重新声明变量是允许的
通过 var 声明的变量会提升到顶端
通过 let 定义的变量不会被提升到顶端

- const 常量

通过 const 定义的变量与 let 变量类似，但不能重新赋值;
const 变量必须在声明时赋值,
const 变量不能在声明之前使用,不能重复声明

- 但常量对象是可以更改

```js
// 您可以创建 const 对象：
const car = { type: "porsche", model: "911", color: "Black" };
// 您可以更改属性：
car.color = "White";
// 您可以添加属性：
car.owner = "Bill";
```

但是您无法重新为常量对象赋值：

```js
const car = { type: "porsche", model: "911", color: "Black" };
car = { type: "Volvo", model: "XC60", color: "White" }; // ERROR
```

同理常量数组可以更改；但是您无法重新为常量数组赋值
在同一作用域或块中，不允许将已有的 var 或 let 变量重新声明或重新赋值给 const;
在同一作用域或块中，为已有的 const 变量重新声明声明或赋值是不允许的;
在另外的作用域或块中重新声明 const 是允许的：

```js
const x = 2; // 允许
{
  const x = 3; // 允许
}
{
  const x = 4; // 允许
}
```

> Internet Explorer 10 或更早版本不支持 const 关键词。

- 声明严格模式

通过在脚本或函数的开头添加 "use strict"; 来声明严格模式。(浏览器低版本不兼容)
在脚本开头进行声明，拥有全局作用域（脚本中的所有代码均以严格模式来执行）：
在函数中声明严格模式，拥有局部作用域（只有函数中的代码以严格模式执行）

```js
"use strict";
x = 3.14; // 这会引发错误，因为 x 尚未声明
```

- 严格模式中不允许的事项：
  在不声明变量的情况下使用变量，是不允许的;
  在不声明对象的情况下使用对象也是不允许的：
  删除变量（或对象）是不允许的`delete:x;`
  删除函数是不允许的,重复参数名是不允许的,八进制数值文本是不允许的,转义字符是不允许的写入只读属性是不允许的,写入只能获取的属性是不允许的,删除不可删除的属性是不允许的,字符串 "eval" 不可用作变量,字符串 "arguments" 不可用作变量,with 语句是不允许的,处于安全考虑，不允许 eval() 在其被调用的作用域中创建变量,在类似 f() 的函数调用中，this 的值是全局对象。在严格模式中，现在它成为了 undefined。

#### 箭头函数（Arrow Function）

箭头函数允许使用简短的语法来编写函数表达式。不需要 function 关键字、return 关键字以及花括号。

> 注意:箭头函数没有自己的 this，它内部的 this 是作用域链的上一层的 this；
> 箭头功能未被提升。它们必须在使用前进行定义。
> 内部没有 arguments
> 不能使用 new 关键词实例化对象
> function 是对象，箭头函数不是对象，是表达式,

```js
//多个参数，有返回值
var  x = (x, y) => x* y;
//或
var x=(x,Y)=>{reruen x*y;}

//一个参数，有返回值
var add=x=>x+10;
//或
var add=x=>{return x+10;}

//无参数，无返回值
var show=()=>{alert("hello");}

//有一个参数，无返回值
var xxx=num=>{alert(num);}

//返回对象
var show=()=>({name:'xxx'})
```

#### 函数默认值

```js
//es5
function test(a, b) {
  a = a || 10;
  b = b || 10;
  return a + b;
}
//es6
function test(a = 10, b = 10) {
  return a + b;
}
```

#### 函数参数

```js
//es5
function test(args) {
  console.log(arguments);
}

//es6
function test(a, ...args) {
  console.log(args); //剩余参数
}
test(1, 2, 3, 4, 5);
```

#### 扩展运算符号

```js
let arr = [1, 2, 3];
let obj = {
  name: "xxx",
  age: 10,
};
console.log(...arr);
console.log(...obj);
```

#### 解构赋值

```js
var [x, y, z] = [1, "dd", false];
//相当与
var x = 1,
  y = "dd",
  z = fase;

//还可以
const [x, [a, b], y] = [10, [10, "dd"], "a"];

//交换两数方便
const [x, y] = [1, 2];
[x, y] = [y, x];

//函数可一次性返回多个数据
function xxx() {
  return ["jj", 3, true];
}
const [a, b, c] = xxx();

//不用按循序赋值
const {
  name,
  age,
  sex: x,
  ...res
} = {
  sex: "男",
  name: "xiaomi",
  age: 11,
  school: "xx",
};
console.log(name, age, x);
```

#### 模板语法

- 可用反引号代替单双引号括起字符串。其中的换行、代码缩进都会保留。
- 拼接字符串时可用`${变量/表达式/函数}`代替`+''+`

#### 新增数组方法

##### Array.every()

该方法测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值。如果所有元素都通过检测返回 true，否则返回 false。参数和 forEach()一致，也不能使用 break 和 continue，但可以实现他们的功能，在方法体内 return false 等同于 break， return true 等同于 continue， 如果不写 return 语句， 默认是 return false

```js
const arr = [0, 1, 2, 3, 4, 5, 6, 7];
let a = arr.every((value, index, arr2) => {
  //if (index == 2) continue //报错：SyntaxError: Illegal continue statement: no surrounding iteration statement
  //if (index == 5) break    报错：SyntaxError: Illegal break statement
  if (index == 2) return true; //实现continue功能
  if (index == 5) return false; //实现实现break功能
  value++;
  arr2[index] += 10;
  return true; //不可省略，否则默认为return false，相当于break
});
```

##### Array.some()

只要数组中存在某一个符合条件的就返回 true，全部不符合条件则返回 false 由于功能上的不同，有不一样的规则，在方法体内 return false 等同于 continue， return true 等同于 break， 如果不写 return 语句, 默认是 return true。完全和 every()相反，因为当 some()没有找到符合的元素时需要继续往下面找，而 every()遇到不符合的元素就直接停止了，some()遇到符合的元素直接停止

```js
const arr = [0, 1, 2, 3, 4, 5, 6, 7];
let a = arr.every((value) => value > 6); //判断数组是否存在一个元素大于6
console.log(a); //true
```

##### Array.includes()

检查数组中是否包含某个特定的值，如果数组包含该值，则返回 true，否则返回 false。

```js
let a = [1, 2, 3, 4];
a.includes(5); //false
```

##### Array.find()

在数组中查找符合条件的元素，只要找到一个符合条件的元素，就终止遍历，返回值为找到的元素。

```js
var array = [1, 2, 3, 45, 5];
var res = arr.find(function (item, idex, array) {
  //查找条件
  return item > 5;
});
console.log(res);
```

##### Array.findIndex()

只要找到一个符合条件的元素，就终止遍历，返回值为找到元素的索引。

```js
array.findIndex((item) => item > 20);
```

###### Array.filter()

找到符合条件的元素，返回新的数组

```js
const arr = [0, 1, 2, 3, 4, 5, 6, 7];
let newArr = arr.filter((value) => value > 3); //判断数组元素是否大于3
console.log(newArr); //[ 4, 5, 6, 7 ]
```

##### Array.reduce()

遍历数组全部元素，将函数处返回的数字累加到一起，并返回这个累加结果，可初始化一个值,不传初始值，则从第二项开始

```js
const arr = [0, 1, 2, 3, 4, 5, 6, 7];
let sum = arr.reduce((accumulator, currentValue) => {
  //accumulator为每次迭代后的累计值，currentValue为每次迭代时从arr传来的元素值
  return currentValue * 2 + accumulator;
}, 100); //将数组中每一个元素都乘以2累加并加上初始值100
console.log(sum); //156
```

##### Array.from()

将伪数组（没有真数组身上的方法）转换为真数组

```js
var aLid = document.getElementById("li1");
let aLis = Array.from(aLis);
aLis.push("test");

//第二个参数

let aLis = Array.from(aLis, (item) => item);

//可使用扩展运算符，转为真数组
let list = [...aLid];
```

##### Array.of()

将一组的值，转换为数组

```js
Array.of(3, 11, "xx", { name: "xx" }, [1, 2]);
```

##### Array.copyWithin()

第一个参数：从哪个下标开始
第二第三个参数：开始与结束的范围`[start,end)`
把范围里的元素从第一个参数开始覆盖。

```js
var arr = [2, 3, 4, 1, 4, 5, 1, 6, 7, 8, 9, 1, 3];
arr.copyWithin(2, 5, 7);
```

#### 新增对象方法

##### Object.assign();合并对象

浅拷贝对象，相同的值会被覆盖为新值
参数 1 为接受拷贝的对象，参数 2 为传递拷贝的对象
结果会改变第一个参数对象，同时会返回这个参数对象

```js
const target = { a: 1, b: 2 };
const source = { b: 3, c: 4 };

const result = Object.assign(target, source);
console.log(target); //  { a: 1, b: 3, c: 4 }
console.log(result); // { a: 1, b: 3, c: 4 }
console.log(target === result); // true
```

##### Object.keys()

用于获取给定对象的自身可枚举属性的属性名（键）

##### Object.values()

用于获取给定对象的自身可枚举属性的属性值（值）

##### Object.entries()

遍历键值对。

##### obj.hasOwnerProperty(属性名)

判断某属性在对象中是否存在：

##### Object.freeze()

> 冻结之后最外层对象不可修改，内层可以
> 冻结对象：其他代码不能删除或更改,添加任何属性
> 冻结一个对象后该对象的原型也不能被修改
> 。freeze() 返回和传入的参数相同的对象。

##### Object.isFrozen()方法

判断一个对象是否被冻结

```js
// 使用Object.freeze是冻结一个对象最方便的方法.
var frozen = { 1: 81 };
Object.isFrozen(frozen); //=== false
Object.freeze(frozen);
Object.isFrozen(frozen); //=== true

// 一个冻结对象也是一个密封对象.
Object.isSealed(frozen); //=== true

// 当然,更是一个不可扩展的对象.
Object.isExtensible(frozen); //=== false
//在 ES5 中，如果参数不是一个对象类型，将抛出一个TypeError异常。在 ES2015 中，非对象参数将被视为一个冻结的普通对象，因此会返回true。

Object.isFrozen(1);
// TypeError: 1 is not an object (ES5 code)

Object.isFrozen(1);
// true                          (ES2015 cod
```

##### Object.is()

比较两个值是否相同。所有 NaN 值都相等（这与==和===不同）。
方法判断两个值是否为同一个值。如果满足以下条件则两个值相等；
都是 undefined
都是 null
都是 true 或 false
都是相同长度的字符串且相同字符按相同顺序排列
都是相同对象（意味着每个对象有同一个引用）
都是数字且
都是 +0
都是 -0
都是 NaN
或都是非零而且非 NaN 且为同一个值

```js
Object.is("foo", "foo"); // true
Object.is(window, window); // true

Object.is("foo", "bar"); // false
Object.is([], []); // false

var foo = { a: 1 };
var bar = { a: 1 };
Object.is(foo, foo); // true
Object.is(foo, bar); // false

Object.is(null, null); // true

// 特例
Object.is(0, -0); // false
Object.is(-0, -0); // true
```

##### Object.defineProperty()

给对象添加一个属性并指定该属性的配置。

```js
/**
 *value； 给对象属性设置值
writable； 属性是否可写，默认 false 不可写
configurable；该属性是否是否可配置，可删除；默认 false 不可写
enumerable；该属性是否可枚举；默认 false 不可枚举
 * /
Object.defineProperty(obj, 'name', {
    get: function() {

        console.log('get方法被调用了');
        return this._myname + '1111';

    },
    set: function(value) {

        console.log('set方法被调用了');
        this._myname = value;

    },
    configurable: true,
    enumerable: true
});
```

##### Object.defineProperties()

给对象添加多个属性并分别指定它们的配置
`object.defineProperties(obj，{  属性名：{ }，属性名： { } })`

##### Object.getOwnProperty(obj，属性名)

获取单个属性： 返回一个对象，包含该属性所有配置

##### Object.getOwnPropertyDescriptor(obj)

获取全部属性： 返回一个对象，包含 obj 中各个属性所具有得配置

#### 基本数据类型 BigInt

是一种特殊的数字类型，它支持任意长度的整数。
要创建一个 bigint，可以在一个整数的末尾添加字符 n，或者调用函数 BigInt()。BigInt 函数使用字符串、数字等来创建一个 BigInt。

```js
const bigint = 1234567890123456789012345678901234567890n;
const sameBigint = BigInt("1234567890123456789012345678901234567890");
const bigintFromNumber = BigInt(10); // same as 10n

//BigInt可以像常规数字一样使用，例如:
alert(1n + 2n); // 3
alert(5n / 2n); // 2

//对 bigint 的所有操作都返回 bigint。我们不能混用 bigint 和常规数字：
alert(1n + 2); // Error: Cannot mix BigInt and other types

//如果需要，我们应该显式转换它们：使用BigInt()或Number()，如下所示：
let bigint = 1n;
let number = 2; // number to bigint
alert(bigint + BigInt(number)); // 3
// bigint to number
alert(Number(bigint) + number); // 3
//但是如果bigint太大，不适合数字类型，那么会进行截取操作

//bigint 不支持前置加号+
//请注意，由于 number 和 bitint 属于不同的数据类型，它们可以相等==，但不能严格相等===
//在 if 或其他布尔运算中时，bigint 的行为类似于数字。
```

#### 基本数据类型 Symbol

> 原始数据类型，表示独一无二的值(内存地址不同)
> 用于定义对象的私有变量

```js
const name = Symbol("xx");
const name2 = Symbol("xx");
console.log(name === name2); //false

let obj = {};
obj[name] = name;
//必须使用中括号
//无法遍历,不可枚举
console.log(obj[name]);

//可通过以下获取key
let s = Object.getOwnPropertySysmbols(obj);
console.log(s);
let m = Reflect.ownKeys(obj);
console.log(m);
```

#### 复杂数据类型 Set 集合(无重复值的有序列表)

- 增删查改

```js
let imgs = new Set();
//添加元素
imgs.add(100);
imgs.add(100);
imgs.add("ddd");
imgs.delete(100);
imgs.has(100); //false
imgs.add(new String("ddd"));
imgs.forEach((val, key) => {
  //键值是相等的
  console.log(val);
  console.log(key);
});
console.log(imgs, imgs.size);

//set中的对象引用无法被释放
let set3 = new Set();
let obj = { name: "xx" };
set3.add(obj);
obj = null;

//使用WeakSet代替
let set3 = new WeakSet();
set3.add(obj);
obj = null;
//它不能传入非对象类型的参数
//不能遍历
//没有size属性
```

- 遍历集合`for....of`

```js
for (let item of imgs.keys()) {
  //遍历键
  console.log(item);
}
for (let item of imgs.values()) {
  //只遍历值
  console.log(item);
}
for (let item of imgs.entries()) {
  //遍历键与值
  console.log(item);
}
```

- 数组与集合互转

```js
//数组变集合
var set = new Set([3, 2, 4, 5, 6, 1, 3, 4]);
console.log(set);
//集合变数组，将数据结构展开为数组
var arr = [...set];
console.log(arr);
```

#### 复杂数据类型 Map 映射(键值对的有序列表，键值可以是任意类型)

> 同等内存下 map 比 object 多存储更多的数据
> 插入性能更好，查找性能相当，删除性能更好（delete 仅仅解绑数据，内存还未释放）

```js
let map = new Map();
//添加数据
map.set("Tom", "fishman");
map.set("Jack", "woker");
map.set("Sim", "teacher");
map.set("Jack", "bussnessman");

//删除
map.delete("Sim");

//判断是否存在
map.has("Sim");
console.log(map);

//取值
console.log(map.get("Tom"));

//map遍历：for   of
for (let [key, value] of map) {
  console.log(key, value);
}
```

#### 类 class

> 是构造函数的语法糖

```js
//es5
function Person(name, sex, age) {
  this.name = name;
  this.sex = sex;
  this.age = age;
}
Person.prototype.sayName = function () {
  return this.name;
};

let p1 = new Person("xiaomi", "man", 10);
console.log(p1);
```

```js
//es6
class preson{
//class属性添加。
// constructor()是类的默认方法，通过new命令生成对象实例时，自动调用该方法
// 一个类必须有constructor()方法，如果没有显式定义，一个空的constructor()方法会被默认添加
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
function person(name, sex, age) {
  this.name = name;
  this.sex = sex;
  this.age = age;
}

person.prototype.showself = function () {
  console.log(this.name, this.age);
};

function woker(name, sex, age, job) {
  //1.构造函数的伪装，继承父级的属性
  Person.call(this, name, sex, age);
  this.job = job;
}
//2.原型链  继承父一级的方法
//<1>通过for ...in 遍历继承
for (var funcName in person.prototype) {
  worker.prototype[funcName] = person.prototype[funcName];
}

//<2>Object.create()
worker.prototype = Object.create(person.prototype);
//<3>调用构造函数继承
worker.prototype = new person();

worker.prototype.showjob = function () {
  console.log(this.job);
};
var w1 = new worker("Tom", "man", 22, "driver");
```

#### 迭代器 Iterator

> 新的遍历机制

```js
let arr = ["1", "2", "3"];
//创建迭代器
const ite = arr[Symbol.iterator]();
console.log(ite.next()); //{value:'1',done:false}
console.log(ite.next()); //{value:'2',done:false}
console.log(ite.next()); //{value:'3',done:false}
console.log(ite.next()); //{value:undefined,done:true}
```

#### 生成器 generator

> 只有在 generator 函数中才能使用 yield
> 通过 yield 关键字，将函数挂起,暂停执行
> 只能在函数内部使用 yield 表达式，将函数挂起

恢复执行：next 方法执行后会返回一个对象，对象中有 value 和 done 两个属性
value：暂停点后面接的值，也就是 yield 后面接的值
done：是否 generator 函数已走完，没走完为 false，走完为 true

```js
function* test() {
  console.log("one");
  yield 1;
  console.log("two");
  yield 2;
  console.log("end");
  return a + b;
}
let fn = test();
console.log(fn.next()); //one \n {value:'1',done:false}
console.log(fn.next()); //two \n {value:'2',done:false}
console.log(fn.next()); //end \n {value:undefined,done:true}
```

```js
//赋值
function* test(x) {
  console.log("one", x);
  let a = yield 1;
  console.log("two", a);
  yield 2;
  console.log("end", b);
  return a + b;
}
let fn = test(999);
console.log(fn.next()); //one 999 \n {value:'1',done:false}
console.log(fn.next(10)); //two 10 \n {value:'2',done:false}
console.log(fn.next(20)); //end 20 \n {value:30,done:true}
```

> 用于给不具备 interator 接口的对象提供遍历操作
> 用于解决回调地域

```js
//for...of.. foreach 不能遍历对象
//for...of..只能遍历带有iterator迭代器接口的,for...of底层利用iterator迭代器实现，遍历时会调用自身的next方法，让yield向下执行
//使用生成器，遍历对象
function* aobjectEntries(obj) {
  const keys = Object.keys(obj);
  for (let i of keys) {
    yield [i, obj[i]];
  }
}

const obj = { name: "xx", age: 10 };
obj[Symbol.iterator] = objectEntries;

for (let [key, value] of objectEntries(obj)) {
  console.log(key, value);
}
```

#### promise

> 通过 Promise 的链式调用可以解决回调地狱
> 回调函数：当一个函数当作参数传递给另一个函数的时候，这个函数就是回调函数
> 首先我们在调用 Promise 时，会返回一个 Promise 对象。
> 构建 Promise 对象时，需要传入一个 executor 函数，Promise 的主要业务流程都在 executor 函数中执行。
> 如果运行在 excutor 函数中的业务执行成功了，会调用 resolve 函数；如果执行失败了，则调用 reject 函数。
> Promise 的状态不可逆，同时调用 resolve 函数和 reject 函数，默认会采取第一次调用的结果
> Promise 是没有中断方法的,可以使用 race 来自己封装中断方法

```js
let p=new Promise((resolve,reject)=>{ //异步操作})
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

- 手写 promise
  promise 有三个状态：`pending`，`fulfilled`， `rejected`，promise 的默认状态是 pending
  执行了 resolve()，Promise 状态会变成 resolved/fulfilled，即 已完成状态
  执行了 reject()，Promise 状态会变成 rejected，即 被拒绝状态
  promise 的状态只可能从“等待”转到“完成”态或者“拒绝”态，不能逆向转换
  且状态只能改变一次，即改变之后无论变为成功还是失败, 都不会再改变
  Promise 中有 throw 的话，就相当于执行了 reject()

- 基础(then 本身未进行异步处理,链式调用)

```js
// 三个状态：PENDING、FULFILLED、REJECTED
const PENDING = 'PENDING';
const FULFILLED = 'FULFILLED';
const REJECTED = 'REJECTED';
class Promise{
   constructor(executor){
   // 默认状态为 PENDING
    this.status = PENDING;
    // 存放成功状态的值，默认为 undefined
    this.value = undefined;
    // 存放失败状态的值，默认为 undefined
    this.reason = undefined;

     // 存放成功的回调
    this.onResolvedCallbacks = [];
    // 存放失败的回调
    this.onRejectedCallbacks= [];

    // 调用此方法就是成功
    let resolve = (value) => {
      // 状态为 PENDING 时才可以更新状态，防止 executor 中调用了两次 resovle/reject 方法
      if(this.status ===  PENDING) {
        this.status = FULFILLED;
        this.value = value;
         // 依次将对应的函数执行
        this.onResolvedCallbacks.forEach(fn=>fn());
      }
    }

    // 调用此方法就是失败
    let reject = (reason) => {
      // 状态为 PENDING 时才可以更新状态，防止 executor 中调用了两次 resovle/reject 方法
      if(this.status ===  PENDING) {
        this.status = REJECTED;
        this.reason = reason;
        // 依次将对应的函数执行
        this.onRejectedCallbacks.forEach(fn=>fn());
      }
    }
      try{
      // 立即执行，将 resolve 和 reject 函数传给使用者
      executor(resolve,reject)
      //((resolve,reject)=>{resolve('xx')})(resolve,reject)
      }catch(error){
      // 发生异常时执行失败逻辑
      reject(error)
      }
   }
  // 包含一个 then 方法，并接收两个参数 onFulfilled、onRejected
  then(onFulfilled, onRejected) {
    if (this.status === FULFILLED) {
      onFulfilled(this.value)
    }

    if (this.status === REJECTED) {
      onRejected(this.reason)
    }
   // 如果promise的状态是 pending，需要将 onFulfilled 和 onRejected 函数存放起来，等待状 态确定后，再依次将对应的函数执行
   if (this.status === PENDING) {
       this.onResolvedCallbacks.push(() => {
         onFulfilled(this.value)
       });
       this.onRejectedCallbacks.push(()=> {
         onRejected(this.reason);
       })
    }


  }
}

}

```

- then 链式调用和异步
  结合 Promise/A+ 规范梳理一下思路：

then 的参数 onFulfilled 和 onRejected 可以缺省，如果 onFulfilled 或者 onRejected 不是函数，将其忽略，且依旧可以在下面的 then 中获取到之前返回的值；「规范 Promise/A+ 2.2.1、2.2.1.1、2.2.1.2」
promise 可以 then 多次，每次执行完 promise.then 方法后返回的都是一个“新的 promise"；「规范 Promise/A+ 2.2.7」
如果 then 的返回值 x 是一个普通值，那么就会把这个结果作为参数，传递给下一个 then 的成功的回调中；
如果 then 中抛出了异常，那么就会把这个异常作为参数，传递给下一个 then 的失败的回调中；「规范 Promise/A+ 2.2.7.2」
如果 then 的返回值 x 是一个 promise，那么会等这个 promise 执行完，promise 如果成功，就走下一个 then 的成功；如果失败，就走下一个 then 的失败；如果抛出异常，就走下一个 then 的失败；「规范 Promise/A+ 2.2.7.3、2.2.7.4」
如果 then 的返回值 x 和 promise 是同一个引用对象，造成循环引用，则抛出异常，把异常传递给下一个 then 的失败的回调中；「规范 Promise/A+ 2.3.1」
如果 then 的返回值 x 是一个 promise，且 x 同时调用 resolve 函数和 reject 函数，则第一次调用优先，其他所有调用被忽略；「规范 Promise/A+ 2.3.3.3.3」

```js
const resolvePromise = (promise2, x, resolve, reject) => {
  // 自己等待自己完成是错误的实现，用一个类型错误，结束掉 promise
  if (promise2 === x) {
    return reject(new TypeError('Chaining cycle detected for promise #<Promise>'))
  }
  // 只能调用一次
  let called;
  // 后续的条件要严格判断 保证代码能和别的库一起使用
  if ((typeof x === 'object' && x != null) || typeof x === 'function') {
    try {
      // 为了判断 resolve 过的就不用再 reject 了（比如 reject 和 resolve 同时调用的时候）
      let then = x.then;
      if (typeof then === 'function') {
        // 不要写成 x.then，直接 then.call 就可以了 因为 x.then 会再次取值，Object.defineProperty
        then.call(x, y => { // 根据 promise 的状态决定是成功还是失败
          if (called) return;
          called = true;
          // 递归解析的过程（因为可能 promise 中还有 promise）
          resolvePromise(promise2, y, resolve, reject);
        }, r => {
          // 只要失败就失败
          if (called) return;
          called = true;
          reject(r);
        });
      } else {
        // 如果 x.then 是个普通值就直接返回 resolve 作为结果
        resolve(x);
      }
    } catch (e) {
      if (called) return;
      called = true;
      reject(e)
    }
  } else {
    // 如果 x 是个普通值就直接返回 resolve 作为结果  Promise/A+ 2.3.4
    resolve(x)
  }
}

/*Promise类中的then方法*/
then(onFulfilled, onRejected) {
    //解决 onFufilled，onRejected 没有传值的问题
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v => v;
    //因为错误的值要让后面访问到，所以这里也要跑出个错误，不然会在之后 then 的 resolve 中捕获
    onRejected = typeof onRejected === 'function' ? onRejected : err => { throw err };
    // 每次调用 then 都返回一个新的 promise
    let promise2 = new Promise((resolve, reject) => {
      if (this.status === FULFILLED) {
        setTimeout(() => {
          try {
            let x = onFulfilled(this.value);
            // x可能是一个proimise
            resolvePromise(promise2, x, resolve, reject);
          } catch (e) {
            reject(e)
          }
        }, 0);
      }

      if (this.status === REJECTED) {
        setTimeout(() => {
          try {
            let x = onRejected(this.reason);
            resolvePromise(promise2, x, resolve, reject);
          } catch (e) {
            reject(e)
          }
        }, 0);
      }

      if (this.status === PENDING) {
        this.onResolvedCallbacks.push(() => {
          setTimeout(() => {
            try {
              let x = onFulfilled(this.value);
              resolvePromise(promise2, x, resolve, reject);
            } catch (e) {
              reject(e)
            }
          }, 0);
        });

        this.onRejectedCallbacks.push(()=> {
          setTimeout(() => {
            try {
              let x = onRejected(this.reason);
              resolvePromise(promise2, x, resolve, reject)
            } catch (e) {
              reject(e)
            }
          }, 0);
        });
      }
    });

    return promise2;
  }
```

#### async / await

> 是 generator 生成器的语法糖
> 将异步操作转为同步化操作
> async 返回一个 promise 对象
> 当执行到 await 时，函数暂停执行，直到 await 等待的 Promise 状态改变

```js
async function msg() {}
let jackson = async function () {};
let jackson = async () => {};

async function test() {
  return await "123";
  //throw new Error('err')
}
test().then((val) => {
  //val=='123'
});
```

### 模块化规范

- CommonJs 规范(nodejs)

- AMD 规范：

```js
//    声明：
define(function () {
  //代码
  return {
    outA: showA,
    outB: showB,
  };
});

//引用：（异步执行）
require("moduleA.js", function (moduleA) {
  moduleA.putA();
  moduleA.putB();
});
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
  import { age, sayName, name } from "./moduleA.js";
  sayName();
</script>
```

moduleA.js

```js
//默认导出
let obj = { name: "xx" };
//只能使用一次
export default obj;
```

```html
<script type="module">
  //导入
  import moduleA from "./moduleB.js";
  //或
  import * as a from "./moduleB.js";
  moduleA.outA();
  moduleA.outB();
</script>
```

### JavaScript 性能

1.  减少循环中的活动
2.  减少 DOM 访问
    与其他 JavaScript 相比，访问 HTML DOM 非常缓慢
3.  缩减 DOM 规模
    请尽量保持 HTML DOM 中较少的元素数量。
4.  避免不必要的变量
5.  延迟 JavaScript 加载
    请把脚本放在页面底部，使浏览器首先加载页面。
6.  避免使用 with
    请避免使用 with 关键词。它对速度有负面影响。它也将混淆 JavaScript 作用域。

### JavaScript JSON

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
var text =
  '{ "employees" : [' +
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

```js
function validateForm() {
  var x = document.forms["myForm"]["fname"].value;
  if (x == "") {
    alert("必须填写姓名");
    return false;
  }
}
```

```html
<form
  name="myForm"
  action="action\_page\_post.php"
  onsubmit="return validateForm()"
  method="post"
>
  姓名：<input type="text" name="fname" />
  <input type="submit" value="Submit" />
</form>
```

### js 执行机制（event loop 事件循环）

> javascript 是一门单线程语言，在最新的 HTML5 中提出了 Web-Worker，但 javascript 是单线程这一核心仍未改变。所以一切 javascript 版的"多线程"都是用单线程模拟出来的

1. 判断是同步任务还是异步任务
2. 同步的进入主线程，异步的进入 Event Table 并注册回调函数。
3. 当指定的事情完成时，Event Table 会将这个回调函数移入 Event Queue。
4. 主线程内的同步任务执行完毕为空，会去 Event Queue 读取对应的函数，进入主线程执行。
   上述过程会不断重复，也就是常说的 Event Loop(事件循环)。
   那怎么知道主线程执行栈为空啊？js 引擎存在 monitoring process 进程，会持续不断的检查主线程执行栈是否为空，一旦为空，就会去 Event Queue 那里检查是否有等待被调用的函数。

#### setTimeout

setTimeout(fn,0)的含义是，指定某个任务在主线程最早可得的空闲时间执行，关于 setTimeout 要补充的是，即便主线程为空，0 毫秒实际上也是达不到的。根据 HTML 的标准，最低是 4 毫秒。

#### setInterval

对于执行顺序来说，setInterval 会每隔指定的时间将注册的函数置入 Event Queue，如果前面的任务耗时太久，那么同样需要等待。
唯一需要注意的一点是，对于 setInterval(fn,ms)来说，我们已经知道不是每过 ms 秒会执行一次 fn，而是每过 ms 秒，会有 fn 进入 Event Queue。一旦 setInterval 的回调函数 fn 执行时间超过了延迟时间 ms，那么就完全看不出来有时间间隔了。

#### Promise 与 process.nextTick(callback)

> process.nextTick(callback)类似 node.js 版的"setTimeout"，在事件循环的下一次循环中调用 callback 回调函数。

我们进入正题，除了广义的同步任务和异步任务，我们对任务有更精细的定义：

macro-task(宏任务)：包括整体代码`script主代码块`，`setTimeout`，`setInterval`,`Ajax`、`DOM事件`。`setImmediate(Node.js 环境)` ,`postMessage`,`IO操作`,`MessageChannel`
micro-task(微任务)：`Promise`，`process.nextTick(Node.js 环境)`,`async`、`await`,`MutaionOberver（浏览器环境）`,`Object.observe`。

JS 的执行/运行机制，具体如下所示：
1、在执行栈中执行一个宏任务；
2、执行过程中遇到微任务，将微任务添加到微任务队列中；
3、当前宏任务执行完毕，立即执行微任务队列中的任务；
4、当前微任务队列中的任务执行完毕，检查渲染，GUI 线程接管渲染；
5、渲染完毕后，js 线程接管，开启下一次事件循环，执行下一次宏任务（事件队列中取）。

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

栈内存中的基本数据类型，可以直接通过操作系统进行处理，而堆内存中的引用数据类型的值大小不确定，因此需要 JS 的引擎通过垃圾回收机制进行处理。

#### 引用计数

- 当变量进行声明并赋值后，值的引用数为 1。
- 当同一个值被赋值给另一个变量时，引用数+1
- 当保存该值引用的变量被其它值覆盖时，引用数-1
- 当该值的引用数为 0 时，表示无法再访问该值了，此时就可以放心地将其清除并回收内存。

这种回收策略看起来很方便，但是当其进行循环引用时就会出现问题，会造成大量的内存不会被释放。当函数结束后，两个对象都不在作用域中，A 和 B 都会被当作非活动对象来清除掉,相比之下，引用计数则不会释放，也就会造成大量无用内存占用，这也是后来放弃引用计数，使用标记清除的原因之一。

#### 标记清除

- 垃圾收集器在运行时会给内存中的所有变量都加上一个标记
- 然后从各个根对象开始遍历，把还在被上下文变量引用的变量标记去掉标记
- 清理所有带有标牌机的变量，销毁并回收它们所占用的内存空间
- 最后垃圾回收程序做一次内存清理
  使用标记清除策略的最重要的优点在于简单，无非是标记和不标记的差异。通过标记清除之后，剩余的对象内存位置是不变的，也会导致空闲内存空间是不连续的，这就造成出现内存碎片的问题。内存碎片多了后，如果要存储一个新的需要占据较大内存空间的对象，就会造成影响。对于通过标记清除产生的内存碎片，还是需要通过标记整理策略进行解决。

#### V8 对于垃圾回收机制的优化

V8 的垃圾回收策略主要基于**分代式**垃圾回收机制，V8 中将堆内存分为新生代和老生代两区域，采用不同的垃圾回收器也就是不同的策略管理垃圾回收。
新生代的对象为存活时间较短的对象，简单来说就是新产生的对象，通常只支持 1 ～ 8M 的容量，而老生代的对象为存活事件较长或常驻内存的对象，简单来说就是经历过新生代垃圾回收后还存活下来的对象，容量通常比较大。V8 整个堆内存的大小就等于新生代加上老生代的内存，对于新老两块内存区域的垃圾回收，V8 采用了两个垃圾回收器来管控

##### 新生代内存回收

在 64 操作系统下分配为 32MB，32 位系统 16M 内存,因为新生代中的变量存活时间短，不太容易产生太大的内存压力，因此不够大也是能够理解。
对于新生代内存的回收，通常是通过 Scavenge 的算法进行垃圾回收，就是将新生代内存进行一分为二，正在被使用的内存空间称为使用区，而限制状态的内存空间称为空闲区。

- 新加入的对象都会存放在使用区，当使用区快写满时就进行一次垃圾清理操作。
- 在开始进行垃圾回收时，新生代回收器会对使用区内的对象进行标记
- 标记完成后，需要对使用区内的活动对象拷贝到空闲区进行排序
- 而后进入垃圾清理阶段，将非活动对象占用的内存空间进行清理
- 最后对使用区和空闲区进行交换，使用区->空闲区，空闲区->使用区
  新生代中的变量如果经过回收之后依然一直存在，那么会放入到老生代内存中，只要是已经经历过一次 Scavenge 算法回收的，就可以晋升为老生代内存的对象。
  新生代存放的是新分配的小量内存，如果达到以下条件中的一个，将被分配至老生代
  内存大小达到 From space 的 25%
  经历了 From space <-> To space 的一个轮回

##### 老生代内存回收

32 位系统 700M 左右
64 位系统 1.4G 左右
标记清除,标记整理进行老生代内存中的垃圾回收。
首先是标记阶段，从一组根元素开始，递归遍历这组根元素，遍历过程中能到达的元素称为活动对象，没有到达的元素就可以判断为非活动对象。清除阶段老生代垃圾回收器会直接将非活动对象，也就是数据清理掉。
同样的标记清除策略会产生内存碎片，因此还需要进行标记整理策略进行优化。

#### 内存泄漏

指在 JS 中已经分配内存地址的对象由于长时间未进行内存释放或无法清除

- 滥用闭包 -定时器或回调太多。与节点或数据相关联的计时器不再需要时，DOM 节点对象可以清除，整个回调函数也不再需要。可是，计时器回调函数仍然没有被回收（计时器停止才会被回收）。当不需要 setTimeout 或 setInterval 时，定时器没有被清除，定时器的糊掉函数以及其内部依赖的变量都不能被回收，会造成内存泄漏。解决方法：在定时器完成工作时，需要手动清除定时器。
- 太多无效的 DOM 引用。DOM 删除了，但是节点的引用还在，导致 GC 无法实现对其所占内存的回收。解决方法：给删除的 DOM 节点引用设置为 null。
- 滥用全局变量,全局变量是根据定义无法被垃圾回收机制进行收集的，因此需要特别注意临时存储和处理大量信息的全局变量。如果必须使用全局变量来存储数据，请确保将其指定为 null 或在完成后重新分配它。解决方法：使用严格模式。
