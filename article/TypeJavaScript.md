---
title: TypeScript
date: 2022-06-01 20:00:00
categories:
  - web
tags:
  - web
---

# TypeScript

> TypeScript 是一种由微软开发的开源、跨平台的编程语言。它是 JavaScript 的超集，主要提供了类型系统和对 ES6+ 的支持,最终会被编译为 JavaScript 代码。

## 安装 TypeScript npm 包

- 安装
  `npm install -g typescript`
  安装完成后我们就可以使用 TypeScript 编译器，名称叫 tsc，可将编译结果生成 js 文件。
- 使用
  `tsc hello.ts`

## 基础类型

> 在 TS 中，某些没有明确指定类型的情况下，TS 的类型推论机制会自动提供类型,有些情况下的类型注解可以省略不写
> ts 的常用基础类型分为两种：

基本类型：number/string/boolean/null/undefined/symbol/bigint
引用类型：array、 Tuple(元组)、 object(包含 Object 和{})、function
特殊类型：any、unknow、void、never、Enum(枚举)
其他类型：类型推理、字面量类型、交叉类型

### 数字

> 和 JavaScript 一样，TypeScript 里的所有数字都是浮点数。 这些浮点数的类型是 number。 除了支持十进制和十六进制字面量，TypeScript 还支持 ECMAScript 2015 中引入的二进制和八进制字面量。

```ts
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

### Null 和 Undefined

> TypeScript 里，undefined 和 null 两者各自有自己的类型分别叫做 undefined 和 null。 和 void 相似，它们的本身的类型用处不是很大：

```ts
let u: undefined = undefined;
let n: null = null;
```

默认情况下 null 和 undefined 是所有类型的子类型。 就是说你可以把 null 和 undefined 赋值给 number 类型的变量。然而，当你指定了--strictNullChecks 标记，null 和 undefined 只能赋值给 void 和它们各自。

### 数组

```ts
let list: number[] = [1, 2, 3];
let list: (number | string)[] = [1, 2, "3"];
let list: Array<number> = [1, 2, 3]; //使用数组泛型Array<元素类型>
let arr3: Array<number | string> = [1, 2, "3"]; //ok
```

### 元组 Tuple

> 元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 string 和 number 类型的元组。

```ts
let x: [string, number];
x = ["hello", 10]; // OK
x = [10, "hello"]; // Error
```

> 当访问一个越界的元素，会使用联合类型替代：

```ts
x[3] = "world"; // OK, 字符串可以赋值给(string | number)类型
console.log(x[5].toString()); // OK, 'string' 和 'number' 都有 toString
x[6] = true; // Error, 布尔不是(string | number)类型
```

### 枚举

enum 类型是对 JavaScript 标准数据类型的一个补充。 像 C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。
枚举的类型只能是 string 或 number
定义的名称不能为关键字

- 数字枚举

```ts
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;
//默认情况下，从0开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 1开始编号：
enum Color {
  Red = 1,
  Green,
  Blue,
}
let c: Color = Color.Green;
//或者，全部都采用手动赋值：
enum Color {
  Red = 1,
  Green = 2,
  Blue = 4,
}
let c: Color = Color.Green;
// 字枚举除了支持 从成员名称到成员值 的普通映射之外，它还支持 从成员值到成员名称 的反向映射
//枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为2，但是不确定它映射到Color里的哪个名字，我们可以查找相应的名字：
enum Color {
  Red = 1,
  Green,
  Blue,
}
let colorName: string = Color[2];
let colorValue: Color = Color.Green;
console.log(colorName); // 显示'Green'因为上面代码里它的值是2
```

- 字符串枚举

```ts
enum Direction {
  NORTH = "NORTH",
  SOUTH = "SOUTH",
  EAST = "EAST",
  WEST = "WEST",
}

// ES5
("use strict");
var Direction;
(function (Direction) {
  Direction["NORTH"] = "NORTH";
  Direction["SOUTH"] = "SOUTH";
  Direction["EAST"] = "EAST";
  Direction["WEST"] = "WEST";
})(Direction || (Direction = {}));
```

- 异构枚举
  异构枚举的成员值是数字和字符串的混合：

```ts
enum Enum {
  A,
  B,
  C = "C",
  D = "D",
  E = 8,
  F,
}
```

- 常量枚举
  它是使用 const 关键字修饰的枚举，常量枚举会使用内联语法，不会为枚举类型编译生成任何 JavaScript。

```ts
const enum Direction {
  NORTH,
  SOUTH,
  EAST,
  WEST,
}

let dir: Direction = Direction.NORTH;

//es5
var dir = 0; /* NORTH */
```

### Any

不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 any 类型来标记这些变量：

```ts
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false;

let list: any[] = [1, true, "free"];
list[1] = 100;
```

### unknow

与 any 一样，都可以作为所有类型的顶级类型，但 unknow 更加严格，那么可以说除了 any 之下的第二大类型，接下来对比下 any,主要严格于一下两点：

- unknow 会对值进行检测，而类型 any 不会做检测操作，说白了，any 类型可以赋值给任何类型，但 unknow 只能赋值给 unknow 类型和 any 类型
- unknow 不允许定义的值有任何操作（如 方法，new 等），但 any 可以

```ts
let u: unknown;
let a: any;
u = "1"; //ok
u = 2; //ok
u = true; //ok
u = [1, 2, 3]; //ok
u = {}; //ok
u.set(); // error
a.set(); //ok
u(); // error
a(); //ok
new u(); // error
new a(); //ok
```

### Void

某种程度上来说，void 类型像是与 any 类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 void：

```ts
function warnUser(): void {
  console.log("This is my warning message");
}
//声明一个void类型的变量没有什么大用，因为你只能为它赋予undefined和null：
let unusable: void = undefined;
```

### Never

表示一个函数永远不存在返回值，TS 会认为类型为 never，那么与 void 相比, never 应该是 void 子集， 因为 void 实际上的返回值为 undefined，而 never 连 undefined 也不行
符合 never 的情况有：当抛出异常的情况和无限死循环
变量也可能是 never 类型，当它们被永不为真的类型保护所约束时。never 类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是 never 的子类型或可以赋值给 never 类型（除了 never 本身之外）。 即使 any 也不可以赋值给 never。

下面是一些返回 never 类型的函数：

```ts
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
  return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
  while (true) {}
}
let error = (): never => {
  // 等价约 let error = () => {}
  throw new Error("error");
};
let error1 = (): never => {
  while (true) {}
};
```

### Object

- object 表示非原始类型，也就是除 number，string，boolean，symbol，null 或 undefined 之外的类型。
  在定义上直接使用 object 是可以的，但你要更改对象的属性就会报错，原因是并没有使对象的内部具体的属性做限制，所以需要使用 `{}` 来定义内部类型

```ts
let obj1: object = { a: 1, b: 2 };
obj1.a = 3; // error

let obj2: { a: number; b: number } = { a: 1, b: 2 };
obj2.a = 3; // ok
```

- Object(大写的 O),代表所有的原始类型或非原始类型都可以进行赋值,除了 null 和`undefined

```ts
let obj: Object;
obj = 1; // ok
obj = "a"; // ok
obj = true; // ok
obj = {}; // ok
obj = Symbol(); //ok
obj = 10n; //ok

obj = null; // error
obj = undefined; // error
```

## 其他

### 字面量类型

在 TS 中，我们可以指定参数的类型是什么，目前支持字符串、数字、布尔三种类型。比如说我定义了 str 的类型是 '小杜杜' 那么 str 的值只能是小杜杜

```ts
let str: "小杜杜";
let num: 1 | 2 | 3 = 1;
let flag: true;
str = "小杜杜"; //ok
str = "Donmesy"; // error
num = 2; //ok
num = 7; // error
flag = true; // ok
flag = false; // error`
```

### 函数

```ts
// 可选参数
//在实际使用时，需要注意的是可选参数要放在普通参数的后面，不然会导致编译错误。
function createUserId(name: string, id: number, age?: number): string {
  return name + id;
}

// 默认参数
function createUserId(
  name: string = "semlinker",
  id: number,
  age?: number
): string {
  return name + id;
}

//剩余参数
function push(array, ...items) {
  items.forEach(function (item) {
    array.push(item);
  });
}

//函数重载
//函数重载或方法重载是使用相同名称和不同参数数量或类型创建多个方法的一种能力。
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: string, b: number): string;
function add(a: number, b: string): string;
function add(a: Combinable, b: Combinable) {
  // type Combinable = string | number;
  if (typeof a === "string" || typeof b === "string") {
    return a.toString() + b.toString();
  }
  return a + b;
}
```

### 交叉类型（&）

将多个类型合并为一个类型，使用&符号连
如果是相同的类型，合并后的类型也是此类型，那如果是不同的类型,会是 never

```ts
type AProps = { a: string; c: number };
type BProps = { b: number; c: string };

type allProps = AProps & BProps;

const Info: allProps = {
  a: "小杜杜",
  b: 7,
  c: 1, // error (property) c: never
  c: "Domesy", // error (property) c: never
};
```

## 断言

### 类型断言

类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript 会假设你，程序员，已经进行了必须的检查。

```ts
//类型断言有两种形式。 其一是“尖括号”语法：
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

//另一个为as语法：
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
//两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；然而，当你在TypeScript里使用JSX时，只有 as语法断言是被允许的。
```

### 非空断言

后缀表达式操作符 ! 可以用于断言操作对象是非 null 和非 undefined 类型。具体而言，x! 将从 x 值域中排除 null 和 undefined 。! 非空断言操作符会从编译生成的 JavaScript 代码中移除

```ts
// 忽略 undefined 和 null 类型
function myFunc(maybeString: string | undefined | null) {
  // Type 'string | null | undefined' is not assignable to type 'string'.
  // Type 'undefined' is not assignable to type 'string'.
  const onlyString: string = maybeString; // Error
  const ignoreUndefinedAndNull: string = maybeString!; // Ok
}

//调用函数时忽略 undefined 类型

type NumGenerator = () => number;
function myFunc(numGenerator: NumGenerator | undefined) {
  // Object is possibly 'undefined'.(2532)
  // Cannot invoke an object which is possibly 'undefined'.(2722)
  const num1 = numGenerator(); // Error
  const num2 = numGenerator!(); //OK
}
```

### 确定赋值断言

在 TS 2.7 版本中引入了确定赋值断言，即允许在实例属性和变量声明后面放置一个 ! 号，以告诉 TS 该属性会被明确赋值,是非 null 和非 undefined 类型。

```ts
let num: number;
let num1!: number;

const setNumber = () => (num = 7);
const setNumber1 = () => (num1 = 7);

setNumber();
setNumber1();

console.log(num); // error
console.log(num1); // ok
```

### 双重断言

断言失效后，可能会用到,如基础类型不能断言为接口

```ts
interface Info {
  name: string;
  age: number;
}

const name = "小杜杜" as Info; // error, 原因是不能把 string 类型断言为 一个接口
const name1 = "小杜杜" as any as Info; //ok
```

- 解构
  解构数组

```ts
let input = [1, 2];
let [first, second] = input;
[first, second] = [second, first];
function f([first, second]: [number, number]) {
  console.log(first);
  console.log(second);
}
//你可以在数组里使用...语法创建剩余变量
let [first, ...rest] = [1, 2, 3, 4];
console.log(first); // outputs 1
console.log(rest); // outputs [ 2, 3, 4 ]

let [first] = [1, 2, 3, 4];
console.log(first); // outputs 1
let [, second, , fourth] = [1, 2, 3, 4];
```

对象解构

```ts
let o = {
  a: "foo",
  b: 12,
  c: "bar",
};
let { a, b } = o;
//可以用没有声明的赋值,注意，我们需要用括号将它括起来，因为Javascript通常会将以 { 起始的语句解析为一个块。
({ a, b } = { a: "baz", b: 101 });

//可以在对象里使用...语法创建剩余变量：
let { a, ...passthrough } = o;
let total = passthrough.b + passthrough.c.length;
```

属性重命名

```ts
let { a: newName1, b: newName2 } = o;
//好像你写成了以下样子：
let newName1 = o.a;
let newName2 = o.b;
//这里的冒号不是指示类型的。 如果你想指定它的类型， 仍然需要在其后写上完整的模式。
let { a, b }: { a: string; b: number } = o;
```

默认值

```ts
默认值可以让你在属性为 undefined 时使用缺省值：
function keepWholeObject(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject;
}
```

函数声明
解构也能用于函数声明。 看以下简单的情况：

```ts
type C = { a: string; b?: number };
function f({ a, b }: C): void {
  // ...
}
// 但是，通常情况下更多的是指定默认值，解构默认值有些棘手。 首先，你需要在默认值之前设置其格式。

function f({ a = "", b = 0 } = {}): void {
  // ...
}
f();
// 上面的代码是一个类型推断的例子，将在本手册后文介绍。

// 其次，你需要知道在解构属性上给予一个默认或可选的属性用来替换主初始化列表。 要知道 C 的定义有一个 b 可选属性：

function f({ a, b = 0 } = { a: "" }): void {
  // ...
}
f({ a: "yes" }); // ok, default b = 0
f(); // ok, default to {a: ""}, which then defaults b = 0
f({}); // error, 'a' is required if you supply an argument
// 要小心使用解构。 从前面的例子可以看出，就算是最简单的解构表达式也是难以理解的。
```

- 展开
  > 展开操作符正与解构相反。 它允许你将一个数组展开为另一个数组，或将一个对象展开为另一个对象。 例如：

```ts
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];//[0, 1, 2, 3, 4, 5]
//展开操作创建了 first和second的一份浅拷贝。 它们不会被展开操作所改变。

//你还可以展开对象：
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { ...defaults, food: "rich" };
search的值为{ food: "rich", price: "$$", ambiance: "noisy" }。
// 对象的展开比数组的展开要复杂的多。 像数组展开一样，它是从左至右进行处理，但结果仍为对象。 这就意味着出现在展开对象后面的属性会覆盖前面的属性。 因此，如果我们修改上面的例子，在结尾处进行展开的话：
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { food: "rich", ...defaults };
//那么，defaults里的food属性会重写food: "rich"
```

对象展开还有其它一些意想不到的限制。 首先，它仅包含对象 自身的可枚举属性。 大体上是说当你展开一个对象实例时，你会丢失其方法：

```ts
class C {
  p = 12;
  m() {}
}
let c = new C();
let clone = { ...c };
clone.p; // ok
clone.m(); // error!
```

其次，TypeScript 编译器不允许展开泛型函数上的类型参数。 这个特性会在 TypeScript 的未来版本中考虑实现。

### in 映射类型

用来映射遍历枚举类型

```ts
type p = string | number | symbol; //支持这三种
type Props = {
  [i in p]: string;
};
```

## 别名 type

用来给一个类型起个新名字

```ts
type InfoProps = string | number;
const setInfo = (data: InfoProps) => {};
```

## 接口 interface

使用 interface 关键字来定义接口,接口可以用来描述对象

- 可读属性：当我们定义一个接口时，我们的属性可能不需要全都要，这是就需要 ? 来解决
- 只读属性：用 readonly 修饰的属性为只读属性，意思是指允许定义，不允许之后进行更改
- 任意属性：这个属性极为重要，它是可以用作就算没有定义，也可以使用，比如 [data: string]: any。比如说我们对组件进行封装，而封装的那个组件并没有导出对应的类型，然而又想让他不报错，这时就可以使用任意属性

```ts
 interface Props {
        a: string;
        b: number;
        c: boolean;
        d?: number; // 可选属性
        readonly e: string; //只读属性
        [f: string]: any //任意属性
    }
    let res: Props = {
        a: '小杜杜',
        b: 7,
        c: true,
        e: 'Domesy',
        d: 1, // 有没有d都可以
        h: 2 // 任意属性，之前为定义过h
    }

    let res.e = 'hi' // error, 原因是可读属性不允许更改
```

### 接口继承 extends

与类一样，接口也存在继承属性，也是使用 extends 字段

```ts
interface nameProps {
  name: string;
}

interface Props extends nameProps {
  age: number;
}

const res: Props = {
  name: "小杜杜",
  age: 7,
};
```

### 函数类型接口

可以定义函数和类，

- 加 new 修饰的是类，
- 不加 new 的是函数

```ts
interface Props {
  (data: number): number;
}

const info: Props = (number: number) => number; //可定义函数

// 定义类
class A {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
}

interface PropsClass {
  new (name: string): A;
}

const info1 = (fun: PropsClass, name: string) => new fun(name);

const res = info1(A, "小杜杜");
console.log(res.name); // "小杜杜"
```

### keyof 关键字

获取一个对象接口的所有 key 值,可以检查对象上的键是否存在

```ts
interface Props {
  name: string;
  age: number;
  sex: boolean;
}

type PropsKey = keyof Props; //包含 name， age， sex

const res: PropsKey = "name"; // ok
const res1: PropsKey = "tel"; // error

// 泛型中的应用
const getInfo = <T, K extends keyof T>(data: T, key: K): T[K] => {
  return data[key];
};

const info = {
  name: "小杜杜",
  age: 7,
  sex: true,
};

getInfo(info, "name"); //ok
getInfo(info, "tel"); //error
```

### 索引访问操作符

```ts
interface Props {
  name: string;
  age: number;
  sex: boolean;
}

type age = Props["age"]; //包含 name， age， sex
```

### 别名和接口的区别

- type 和 interface 都可以定义 对象 和 函数
- type 可以定义其他数据类型，如字符串、数字、元祖、联合类型等，而 interface 不行

```ts
type A = string; // 基本类型
type B = string | number; // 联合类型
type C = [number, string]; // 元祖
const dom = document.createElement("div"); // dom元素
type D = typeof dom;
```

- interface 是通过 extends 来实现
- type 是通过 & 来实现

```ts
// interface 扩展 interface
interface A {
  a: string;
}
interface B extends A {
  b: number;
}
const obj: B = { a: `小杜杜`, b: 7 };

// type 扩展 type
type C = { a: string };
type D = C & { b: number };
const obj1: D = { a: `小杜杜`, b: 7 };

// interface 扩展为 Type
type E = { a: string };
interface F extends E {
  b: number;
}
const obj2: F = { a: `小杜杜`, b: 7 };

// type 扩展为 interface
interface G {
  a: string;
}
type H = G & { b: number };
const obj3: H = { a: `小杜杜`, b: 7 };
```

- interface 可以多次被定义，并且会进行合并，但 type 不行

```ts
interface A {
  a: string;
}
interface A {
  b: number;
}
const obj: A = { a: `小杜杜`, b: 7 };

type B = { a: string };
type B = { b: number }; // error
```

- Implements
  类可以以相同的方式实现接口或类型别名，但类不能实现使用类型别名定义的联合类型：

```ts
interface Point {
  x: number;
  y: number;
}

class SomePoint implements Point {
  x = 1;
  y = 2;
}

type Point2 = {
  x: number;
  y: number;
};

class SomePoint2 implements Point2 {
  x = 1;
  y = 2;
}

type PartialPoint = { x: number } | { y: number };

// A class can only implement an object type or
// intersection of object types with statically known members.
class SomePartialPoint implements PartialPoint {
  // Error
  x = 1;
  y = 2;
}
```

## 联合类型 Union Types

表示取值可以为多种类型中的一种,未赋值时联合类型上只能访问两个类型共有的属性和方法，如：

```ts
const setInfo = (name: string | number) => {};
```

### 可辨识联合

定义了 A、B、C 三次接口，但这三个接口都包含 type 属性，那么 type 就是可辨识的属性,而其他属性只跟特性的接口相关。

然后通过可辨识属性 type，才能使用其相关的属性

```ts
interface A {
  type: 1;
  name: string;
}

interface B {
  type: 2;
  age: number;
}

interface C {
  type: 3;
  sex: boolean;
}

// const setInfo = (data: A | B | C) => {
//   return data.type // ok 原因是 A 、B、C 都有 type属性
//   return data.age // error，  原因是没有判断具体是哪个类型，不能确定是A，还是B，或者是C
// }

const setInfo1 = (data: A | B | C) => {
  if (data.type === 1) {
    console.log(`我的名字是${data.name}`);
  } else if (data.type === 2) {
    console.log(`我的年龄是${data.age}`);
  } else if (data.type === 3) {
    console.log(`我的性别是${data.sex}`);
  }
};

setInfo1({ type: 1, name: "小杜杜" }); // "我的名字是小杜杜"
setInfo1({ type: 2, age: 7 }); // "我的年龄是7"
setInfo1({ type: 3, sex: true }); // "我的性别是true"
```

## 泛型

是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性

```ts
const calcArray = (data: any): any[] => {
  let list = [];
  for (let i = 0; i < 3; i++) {
    list.push(data);
  }
  return list;
};
```

但我们现在想要的效果是：无论我们传什么类型，都能返回对应的类型，针对这种情况怎么办？所以此时泛型就登场了

```ts
const calcArray = <T>(data: T): T[] => {
  let list: T[] = [];
  for (let i = 0; i < 3; i++) {
    list.push(data);
  }
  return list;
};

const res: string[] = calcArray<string>("d"); // ok
const res1: number[] = calcArray<number>(7); // ok
type Props = {
  name: string;
  age: number;
};
const res3: Props[] = calcArray<Props>({ name: "小杜杜", age: 7 }); //ok
```

### 语法

```ts
function identity<T>(value: T): T {
  return value;
}
```

### 多类型传参

我们有多个未知的类型占位，我们可以定义任何的字母来表示不同的参数类型

```ts
const calcArray = <T, U>(name: T, age: U): { name: T; age: U } => {
  const res: { name: T; age: U } = { name, age };
  return res;
};
const res = calcArray<string, number>("小杜杜", 7);
console.log(res); // {"name": "小杜杜", "age": 7}
```

### 泛型接口

```ts
interface A<T> {
  data: T;
}

const Info: A<string> = { data: "1" };
console.log(Info.data); // "1"
```

### 泛型类

```ts
class clacArray<T> {
  private arr: T[] = [];

  add(value: T) {
    this.arr.push(value);
  }
  getValue(): T {
    let res = this.arr[0];
    console.log(this.arr);
    return res;
  }
}

const res = new clacArray();

res.add(1);
res.add(2);
res.add(3);

res.getValue(); //[1, 2, 3]
console.log(res.getValue); // 1
```

### 泛型类型别名

```ts
type Info<T> = {
  name?: T;
  age?: T;
};

const res: Info<string> = { name: "小杜杜" };
const res1: Info<number> = { age: 7 };
```

### 泛型默认参数

```ts
const calcArray = <T = string>(data: T): T[] => {
  let list: T[] = [];
  for (let i = 0; i < 3; i++) {
    list.push(data);
  }
  return list;
};
```

### 泛型常用字母

T：代表 Type，定义泛型时通常用作第一个类型变量名称
K：代表 Key，表示对象中的键类型；
V：代表 Value，表示对象中的值类型；
E：代表 Element，表示的元素类型；

### 泛型工具类型

#### infer

可以用 infer 声明一个类型变量并且对它进行使用。如

```ts
type Info<T> = T extends { a: infer U; b: infer U } ? U : never;

type Props = Info<{ a: string; b: number }>; // Props类： string | number

type Props1 = Info<number>; // Props类型： never
```

#### Partial|Required|Readonly

`Partial<T>` 作用：将所有属性变为可选的 ?
`Required<T>` 作用：将所有属性变为必选的，与 Partial 相反
`Readonly<T>` 作用：将所有属性都加上 readonly 修饰符来实现。也就是说无法修改

```ts
interface Props {
  name: string;
  age: number;
}

const info: Props = {
  name: "小杜杜",
  age: 7,
};

const info1: Partial<Props> = {
  name: "小杜杜",
};
const info1: Required<Props> = {
  name: "小杜杜",
};
```

#### Record

`Record<K extends keyof any, T>`将 K 中所有的属性的值转化为 T 类型。
`K extends keyof any`其类型可以是:string、number、symbol

```ts
    interface Props {
        name: string,
        age: number
    }

    type InfoProps = 'JS' | 'TS'

    const Info: Record<InfoProps, Props> = {
        JS: {
            name: '小杜杜',
            age: 7
        },
        TS: {
            name: 'TypeScript',
            age: 11
        }
   //从上述代码上来看, InfoProps的属性分别包含Props的属性
```

#### Pick

`Pick<T, K extends keyof T>`将某个类型中的子属性挑出来，变成包含这个类型部分属性的子类型。

```ts
interface Props {
  name: string;
  age: number;
  sex: boolean;
}

type nameProps = Pick<Props, "name" | "age">;

const info: nameProps = {
  name: "小杜杜",
  age: 7,
};
//从上述代码上来看, Props原本属性包括name、age、sex三个属性，通过 Pick我们吧name和age挑了出来，所以不需要sex属性
```

#### Exclude|Extra|Omit

`Exclude<T, U>`将 T 类型中的 U 类型剔除
`Extra<T, U>`将 T 可分配给的类型中提取 U。与 Exclude 相反
`Omit<T, U>`将已经声明的类型进行属性剔除获得新类型,与 Exclude 的区别：Omit 返回的是新的类型，原理上是在 Exclude 之上进行的，Exclude 是根据自类型返回的

```ts
// 数字类型
type numProps = Exclude<1 | 2 | 3, 1 | 2>; // 3
type numProps1 = Exclude<1, 1 | 2>; // nerver
type numProps2 = Exclude<1, 1>; // nerver
type numProps3 = Exclude<1 | 2, 7>; // 1 2

// 字符串类型
type info = "name" | "age" | "sex";
type info1 = "name" | "age";
type infoProps = Exclude<info, info1>; //  "sex"

// 类型
type typeProps = Exclude<string | number | (() => void), Function>; // string | number

// 对象
type obj = { name: 1; sex: true };
type obj1 = { name: 1 };
type objProps = Exclude<obj, obj1>; // nerver
//从上述代码上来看,我们比较了下类型上的，当 T 中有 U 就会剔除对应的属性，如果 U 中又的属性 T 中没有，或 T 和 U 刚好一样的情况都会返回 nerver，且对象永远返回nerver
```

```ts
type numProps = Extract<1 | 2 | 3, 1 | 2>; // 1 | 2
```

#### NonNullable

`NonNullable<T>` 作用：从 T 中排除  null  和  undefined

#### ReturnType|Parameters

`ReturnType<T>`用于获取 函数 T 的返回类型。
`Parameters<T>` 作用：用于获取 获取函数类型的参数类型

```ts
type Props = ReturnType<() => string>; // string
type Props1 = ReturnType<<T extends U, U extends number>() => T>; // number
type Props2 = ReturnType<any>; // any
type Props3 = ReturnType<never>; // any
// 从上述代码上来看， ReturnType可以接受 any 和 never 类型，原因是这两个类型属于顶级类型，包含函数
```

```ts
type Props = Parameters<() => string>; // []
type Props1 = Parameters<(data: string) => void>; // [string]
type Props2 = Parameters<any>; // unknown[]
type Props3 = Parameters<never>; // never
```

## 类型守卫

是可执行运行时检查的一种表达式，用于确保该类型在一定的范围内

### in 关键字

用于判断这个属性是那个里面的

```ts
interface Info {
  name: string;
  age: number;
}

interface Info1 {
  name: string;
  flage: true;
}

const setInfo = (data: Info | Info1) => {
  if ("age" in data) {
    console.log(`我的名字是：${data.name}，年龄是：${data.age}`);
  }

  if ("flage" in data) {
    console.log(`我的名字是：${data.name}，性别是：${data.flage}`);
  }
};

setInfo({ name: "小杜杜", age: 7 }); // "我的名字是：小杜杜，年龄是：7"
setInfo({ name: "小杜杜", flage: true }); // "我的名字是：小杜杜，性别是：true"
```

### 类型谓词(is)

```ts
function isNumber(x: any): x is number {
  //默认传入的是number类型
  return typeof x === "number";
}

console.log(isNumber(7)); // true
console.log(isNumber("7")); //false
console.log(isNumber(true)); //false
```

### typeof 关键字

用于判断基本类型

### instanceof 关键字

用于判断一个实例是不是构造函数，或使用类的时候

## react+ts

新建项目使用 typescript
如果你是要新建一个使用 typescript 的 react 项目，并且你用脚手架 Create React App 去创建，那没就非常的容易，你只需要在创建的时候将命令改为
`yarn create react-app xxx --template typescript`

已有 react 项目
1、安装 ts
npm i typescript -g 全局安装
npm i typescript -D 当前项目安装

tsc --init
修改 tsconfig 配置文件

```js
{
  "compilerOptions": {
     "target": "es5", /**指定ECMAScript目标版本**/
     "module": "commonjs", /**指定生成哪个模块系统代码**/
     "allowJs": true,  /**允许编译js文件**/
     "jsx": "preserve",  /**支持JSX**/
     "outDir": "build",  /**编译输出目录**/
     "strict": true, /**启用所有严格类型检查选项**/
     "noImplicitAny": false, /**在表达式和声明上有隐含的any类型时报错**/
     "skipLibCheck": true,  /**忽略所有的声明文件的类型检查**/
     "forceConsistentCasingInFileNames": true   /**禁止对同一个文件的不一致的引用**/
  },
  "include": ["src"] /**指定编译目录**/
}
```

安装 react 的声明文件
`npm install --save typescript @types/node @types/react @types/react-dom @types/jest`

## 常见错误

- Cannot use JSX unless the '--jsx' flag is provided 报错或 React‘ refers to a UMD global 或者 Cannot use JSX unless the ‘--jsx‘ flag is provided.
  在 tsconfig.json 中加入：
  "jsx": "react-jsx",

- Cannot find module ‘./index.module.less’ or its corresponding type declarations

给 ts 的 CSS 文件加上类型的声明
\*.d.ts 文件：ts 专用的类型声明文件，只包含类型的声明，不包含逻辑，不会被编译，也不会被 webpack 打包

```ts
declare module "*.css" {
  const css: { [key: string]: string };
  export default css;
}
declare module "*.scss" {
  const content: { [className: string]: string };
  export = content;
}
```

- `“AsyncThunkAction<any，void，{}>”`类型的参数不能分配给“AnyAction”类型的参数

```ts
// app/store.ts

import { configureStore } from "@reduxjs/toolkit";
// ...

export const store = configureStore({
  reducer: {
    posts: postsReducer,
    comments: commentsReducer,
    users: usersReducer,
  },
});

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>;
// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch;

// app/hooks.ts

import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
import type { RootState, AppDispatch } from "./store";

// Use throughout your app instead of plain `useDispatch` and `useSelector`
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```
