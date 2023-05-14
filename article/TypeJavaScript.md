---
title: TypeScript
date: 2022-06-01 20:00:00
categories:
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

- 布尔值

```ts
let isDone: boolean = false;
```

- 数字
  > 和 JavaScript 一样，TypeScript 里的所有数字都是浮点数。 这些浮点数的类型是 number。 除了支持十进制和十六进制字面量，TypeScript 还支持 ECMAScript 2015 中引入的二进制和八进制字面量。

```ts
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

- 字符串

```ts
let name: string = "bob";
let name: string = 'bob';
let name: string = `bob`;
```

- 数组

```ts
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];//使用数组泛型Array<元素类型>
```

-元组 Tuple

> 元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 string 和 number 类型的元组。

```ts
let x: [string, number];
x = ['hello', 10]; // OK
x = [10, 'hello']; // Error
```

> 当访问一个越界的元素，会使用联合类型替代：

```ts
x[3] = 'world'; // OK, 字符串可以赋值给(string | number)类型
console.log(x[5].toString()); // OK, 'string' 和 'number' 都有 toString
x[6] = true; // Error, 布尔不是(string | number)类型
```

- 枚举
  > enum 类型是对 JavaScript 标准数据类型的一个补充。 像 C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```ts
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
//默认情况下，从0开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 1开始编号：
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
//或者，全部都采用手动赋值：
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;
//枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为2，但是不确定它映射到Color里的哪个名字，我们可以查找相应的名字：
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];
console.log(colorName);  // 显示'Green'因为上面代码里它的值是2
```

- Any
  不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 any 类型来标记这些变量：

```ts
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false;

let list: any[] = [1, true, "free"];
list[1] = 100;
```

- Void
  某种程度上来说，void 类型像是与 any 类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 void：

```ts
function warnUser(): void {
    console.log("This is my warning message");
}
//声明一个void类型的变量没有什么大用，因为你只能为它赋予undefined和null：
let unusable: void = undefined;
```

- Null 和 Undefined
  > TypeScript 里，undefined 和 null 两者各自有自己的类型分别叫做 undefined 和 null。 和 void 相似，它们的本身的类型用处不是很大：

```ts
let u: undefined = undefined;
let n: null = null;
```

默认情况下 null 和 undefined 是所有类型的子类型。 就是说你可以把 null 和 undefined 赋值给 number 类型的变量。然而，当你指定了--strictNullChecks 标记，null 和 undefined 只能赋值给 void 和它们各自。 这能避免 很多常见的问题。 也许在某处你想传入一个 string 或 null 或 undefined，你可以使用联合类型 string | null | undefined。

- Never
  never 类型表示的是那些永不存在的值的类型。 例如， never 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never 类型，当它们被永不为真的类型保护所约束时。never 类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是 never 的子类型或可以赋值给 never 类型（除了 never 本身之外）。 即使 any 也不可以赋值给 never。

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
    while (true) {
    }
}
```

- Object
  object 表示非原始类型，也就是除 number，string，boolean，symbol，null 或 undefined 之外的类型。

```ts
declare function create(o: object | null): void;
create({ prop: 0 }); // OK
create(null); // OK
create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

- 类型断言
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
    c: "bar"
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
let {a, b}: {a: string, b: number} = o;
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
type C = { a: string, b?: number }
function f({ a, b }: C): void {
    // ...
}
// 但是，通常情况下更多的是指定默认值，解构默认值有些棘手。 首先，你需要在默认值之前设置其格式。

function f({ a="", b=0 } = {}): void {
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
  m() {
  }
}
let c = new C();
let clone = { ...c };
clone.p; // ok
clone.m(); // error!
```

其次，TypeScript 编译器不允许展开泛型函数上的类型参数。 这个特性会在 TypeScript 的未来版本中考虑实现。

## 接口

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

给ts的CSS文件加上类型的声明
*.d.ts文件：ts专用的类型声明文件，只包含类型的声明，不包含逻辑，不会被编译，也不会被webpack打包

```ts
declare module "*.css" {
  const css: {[key: string]: string}
  export default css
  }
  declare module '*.scss' {
  const content: { [className: string]: string};
  export = content;
}

```

- `“AsyncThunkAction<any，void，{}>”`类型的参数不能分配给“AnyAction”类型的参数

```ts

// app/store.ts

import { configureStore } from '@reduxjs/toolkit'
// ...

export const store = configureStore({
  reducer: {
    posts: postsReducer,
    comments: commentsReducer,
    users: usersReducer,
  },
})

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>
// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch

// app/hooks.ts

import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux'
import type { RootState, AppDispatch } from './store'

// Use throughout your app instead of plain `useDispatch` and `useSelector`
export const useAppDispatch = () => useDispatch<AppDispatch>()
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector
```
