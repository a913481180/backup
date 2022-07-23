---
title: react
date: 2022-12-09 20:00:00
categories: 
- linux 
---
# React

## 浏览器中编写

- 准备容器
```
<div id="test"></div>
```
- 引入库
```
<!--react核心库-->
<script type="text/javascript" src="react.development.js"></script>
<!--react-dom，用于支持react操纵dom-->
<script type="text/javascript" src="react-dom.development.js"></script>
<!--babel，用于将jsx转化为js-->
<script type="text/javascript" src="babel.min.js"></script>
```
- JSX
```
<script type="text/babel>/*此处要写babel*/
//创建虚拟dom
const Vdom=(<h1>hello</h1>)/*不用引号*/
//渲染虚拟DOM到页面
ReactDOM.render(Vdom,document.getElementById('test'));
</script>
```
- js
>jsx最终会翻译成js写法
```
<script type="text/javascript">
//创建虚拟dom
const Vdom=React.createElement('h1',{id:'title'},React.createElement('span',{},'hello'))
//渲染虚拟DOM到页面
ReactDOM.render(Vdom,document.getElementById('test'));
</script>
```

>虚拟DOM本质是一个对象，虚拟dom比较轻量，真实DOM比较完整，虚拟DOM是react内部用，因此无需真实DOM那么多的属性。最终虚拟DOM会被react转化为真实DOM呈现在页面上。

## JSX
>javascript XML,react定义的类似与XML的js扩展语法

xml：早期用于存储和传输数据的格式
```
<student>
<name>XIAOMI</name>
<age>19</age>
</student>
```

### jsx语法规则
- 定义虚拟dom，不要写引号
- 标签中混入js表达式要用｛｝，js表达式会产生一个值(返回值)，可以放在任何需要值的地方，如`a`、`a+b`、`demo(1)`、`arr.map()`、`function test(){}`。js语句：`if(){}`,`for(){}`,`switch(){}`
```
const a=1;
const Vdom=(
<span id={a}>{a.toString()}</span>
)
```
- 样式的类型指定不要用class，要用className
```
const Vdom=(
<span className="test" >xxx</span>
)
```
- 内联样式要用style={{fontSize:"20px"}}的写法
- 虚拟dom只有一个根标签
```
const Vdom=(
<div>
<span className="test" >xxx</span>
<span className="test" >xxx</span>
</div>
)
```
- 标签首字符为小写则直接转为html中同名元素，若为大写则去渲染对应组件

## 组件和模块

模块：提供特定功能的js程序
组件：实现局部功能效果的代码和资源集合（html、js、css、img等）

