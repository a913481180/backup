---
title: react
date: 2022-12-09 20:00:00
categories:
  - linux
---

## 浏览器中编写

- 准备容器

```jsx
<div id="test"></div>
```

- 引入库

```jsx
<!--react核心库-->
<script type="text/javascript" src="react.development.js"></script>
<!--react-dom，用于支持react操纵dom-->
<script type="text/javascript" src="react-dom.development.js"></script>
<!--babel，用于将jsx转化为js-->
<script type="text/javascript" src="babel.min.js"></script>
```

- JSX

```jsx
<script type="text/babel">
  /*此处要写babel*/ //创建虚拟dom const Vdom=(<h1>hello</h1>)/*不用引号*/
  //渲染虚拟DOM到页面 ReactDOM.render(Vdom,document.getElementById('test'));
</script>
```

- js
  > jsx 最终会翻译成 js 写法

```jsx
<script type="text/javascript">
//创建虚拟dom
const Vdom=React.createElement('h1',{id:'title'},React.createElement('span',{},'hello'))
//渲染虚拟DOM到页面
ReactDOM.render(Vdom,document.getElementById('test'));
</script>
```

> 虚拟 DOM 本质是一个对象，虚拟 dom 比较轻量，真实 DOM 比较完整，虚拟 DOM 是 react 内部用，因此无需真实 DOM 那么多的属性。最终虚拟 DOM 会被 react 转化为真实 DOM 呈现在页面上。

## JSX

> javascript XML,react 定义的类似与 XML 的 js 扩展语法，脚手架通过@babel/plugin-transform-react-jsx 包解析成 React.createElemnt('h1',{className:'test'},'xx')函数

xml：早期用于存储和传输数据的格式

```xml
<student>
<name>XIAOMI</name>
<age>19</age>
</student>
```

### jsx 语法规则

- 定义虚拟 dom，不要写引号
- 标签中混入 js 表达式要用｛｝，js 表达式会产生一个值(返回值)，可以放在任何需要值的地方，如`a`、`a+b`、`demo(1)`、`arr.map()`、`function test(){}`。js 语句：`if(){}`,`for(){}`,`switch(){}`

```jsx
const a = 1;
const Vdom = <span id={a}>{a.toString()}</span>;
```

- 样式的类型指定不要用 class，要用 className

```jsx
const Vdom = <span className="test">xxx</span>;
```

- 内联样式
  要用`style={{fontSize:"20px"}}`的写法

- 虚拟 dom 只有一个根标签

```jsx
const Vdom = (
  <div>
    <span className="test">xxx</span>
    <span className="test">xxx</span>
  </div>
);
```

- 标签首字符为小写则直接转为 html 中同名元素，若为大写则去渲染对应组件

### 列表渲染

```jsx
return (
  <ul>
    {list.map((item) => (
      <li key={item.id}>item.name</li>
    ))}
  </ul>
);
```

### 条件渲染

三元表达式，逻辑&&||运算，函数

```jsx
const isTrue = false;
const genSpan = (i) => {
  if (i) {
    return <span>x</span>;
  } else {
    return <b>x</b>;
  }
};
return (
  <div>
    {isTrue && <span>xx</span>}
    {isTrue ? <span>xx</span> : <b>xx</b>}
    {genSpan(isTrue)}
  </div>
);
```

## 样式控制

- 内联

```jsx
const style = { color: "red" };
return;
<>
  <div style={{ color: "red" }}>xx</div>
  <div style={style}>xx</div>
</>;
```

- 类名
  不能用对象和数组，只能是字符串

```jsx
return;
<>
  <div className="test">xx</div>
  <div className={isTrue ? "test" : ""}>xx</div>
</>;
```

## 组件和模块

模块：提供特定功能的 js 程序
组件：实现局部功能效果的代码和资源集合（html、js、css、img 等）

### 函数式组件

```jsx
function Demo() {
  //组件必须首字母大写
  //this 为undefind,经过babel编译后开启严格模式，this便指向undefind，不开启严格模式则是window
  return <h1>test</h1>;
  //必须有返回值，若没有内容返回null
}

ReactDOM.render(<Demo />, document.getElementById("test"));
//react解析组件标签,找到Demo组件，发现此组件是函数定义的，便调用该函数，将虚拟dom渲染成真实dom
```

### 类式组件

#### 简单组件

```jsx
//在Demo的原型对象上
class Demo extends React.Component {
  render() {
    //必须提供render,极其返回值
    //this指向Demo的实例对象
    return <h1>test</h1>;
  }
}
//react解析组件标签,找到Demo组件，发现此组件是类定义的，便new 出该类实例，通过该实例调用原型对象上的render方法。将虚拟dom渲染成真实dom
ReactDOM.render(<Demo />, document.getElementById("test"));
```

#### 复杂组件

##### state

> state 为组件的重要属性，值是对象，通过更新组件的 state 来更新对应的页面显示（重新渲染组件）

###### 传统写法

```jsx
class Demo extends React.Component{
  constructor(prop){
    super(prop)
    this.state={
      a:1,
      b:2
    }
  }
 changValue(){
 //类中的方法默认开启局部严格模式
 //所以this为undefind
 //只有通过Demo实例调用此实例，this才为Demo的实例
 //changeValue作为onClick的回调，不是实例调用
  }
  render(){
    return (
      <h1 onClick={test}>test{this.state.a}</h1>
      <h1 onClick={this.changeValue}>test{this.state.b}</h1>
    )
  }

}

function test(){}
ReactDOM.render(<Demo/>,document.getElementById('test'))

```

解决 this 指向

```jsx
class Demo extends React.Component {
  constructor(prop) {
    super(prop); //构造器是否接受props，是否传给super取决于是否希望在构造器中通过this访问props
    this.state = {
      a: 1,
      b: 2,
    };
    //改变this指向
    this.changValue = this.changValue.bind(this);
  }
  changValue() {
    //state不能直接更改
    this.state.a = 1;
    //必须通过api
    this.setState({ a: 2 }); //更新为合并，不是替换
  }
  render() {
    return <h1 onClick={this.changeValue}>test{this.state.b}</h1>;
  }
}
ReactDOM.render(<Demo />, document.getElementById("test"));
```

###### 简写

```jsx
class Demo extends React.Component {
  //名称固定，就叫state
  state = {
    //等于号，代表给该类的实例追加属性
    a: 1,
    b: 2,
  };
  //必须用箭头函数,不需要通过constructor修改this指向
  changValue = () => {
    this.setState({ a: 2 }); //更新为合并，不是替换
  };
  render() {
    return <h1 onClick={this.changeValue}>test{this.state.b}</h1>;
  }
}
ReactDOM.render(<Demo />, document.getElementById("test"));
```

##### 状态不可变

不要直接修改 state 状态的值，而是基于当前状态创建新的状态值

- 错误修改

```js
state = { count: 0, list: [1, 2, 3], person: { name: "xx", age: 18 } };
this.state.count++;
this.state.count += 1;
this.state.count = 1;

this.state.list.push(1);
this.state.list.splice(1, 1);

this.state.person.name = "aaa";
```

- 正确修改

```js
this.setState({
  count:this.state.count+1,
  //增加
  list:[...this.state.list,4]
  //删除
  list:this.state.list.filter(item=>item!==1)

  person:{
    ...this.state.person,
    name:'aaa'
  }
})
```

##### props

> 组件的所有属性都保存在 props 中，props 是只读的，不能修改。可以传递任何数据包括函数，jsx
> 类式组件通过 this.props 获取 props 对象
> 函数式组件通过参数获取 props 对象
> 可使用 ts 来配置

```jsx
class Demo extends React.Component {
  render() {
    return <h1>test{this.props.b}</h1>;
  }
}
ReactDOM.render(<Demo a={1} b="ss" />, document.getElementById("test1")); //数据类型要用｛｝
const obj = { a: 1, b: 2 };
ReactDOM.render(<Demo {...obj} />, document.getElementById("test2")); //批量传属性
```

- props 规则限制

> 常见规则：
> 必传：isRequired
> 基本类型：boolean , number,...
> 节点：element
> 指定对象：shape({})

```jsx
//react16.x以上需引入prop-types.js
//导入prop-type包
//组件名.propTypes={}给组件添加规则校验
import PropTypes from 'prop-types'
class Demo extends React.Component{
render(){
return (
<h1 >test{this.props.b}</h1>
)

}
}
//限制类型
Demo.propTypes={
  a:PropTypes.number.isRequied
  test:PropTypes.func
}
//默认值
Demo.defaultProps={
  b:'xx'
}
function test(){}
ReactDOM.render(<Demo a={1} b='ss' handle="test"/>,document.getElementById('test1'))//数据类型要用｛｝
const obj={a:1,b:2}
ReactDOM.render(<Demo {...obj}/>,document.getElementById('test2'))//批量传属性
```

- 简写

```jsx
import PropTypes from 'prop-types'
class Demo extends React.Component{
render(){
return (
  <h1 >test{this.props.b}</h1>
)

}

static propTypes={ //加上static代表给类本身添加属性
  //react16.x以上需引入prop-types.js
  a:PropTypes.number.isRequied
  test:PropTypes.func
}
static defaultProps={
  b:'xx'
}}
function test(){}
ReactDOM.render(<Demo a={1} b='ss' handle="test"/>,document.getElementById('test1'))//数据类型要用｛｝
const obj={a:1,b:2}
ReactDOM.render(<Demo {...obj}/>,document.getElementById('test2'))//批量传属性
```

- 函数式组件使用 props

```jsx

import propTypes from 'prop-types'
function Demo(props){
return (
  <h1>test{props.b}</h1>
)

}
 Demo.propTypes={
  //react16.x以上需引入prop-types.js
  a:PropTypes.number.isRequied
  test:PropTypes.func
}
Demo.defaultProps={
  b:'xx'
}}
function test(){}
ReactDOM.render(<Demo a={1} b='ss' handle="test"/>,document.getElementById('test1'))//数据类型要用｛｝
```

```jsx

import propTypes from 'prop-types'
//官方推荐
function Demo({b:'xxx'}){
return (
  <h1 >test{b}</h1>
)

}
Demo.propTypes={
  a:PropTypes.number.isRequied
  test:PropTypes.func
}

function test(){}
  ReactDOM.render(<Demo a={1} b='ss' handle="test"/>,document.getElementById('test1'))//数据类型要用｛｝
```

##### refs

> 组件内的标签可使用 ref 属性标识自己类似 id='test'

```jsx
class Demo extends React.Component{
render(){
return (
<h1 ref='title1' >官方不推荐String类型的ref写法，效率不高</h1>
<h1 ref={(node)=>{this.title2=node}} >ref的回调写法，内联写法在每次更新时会调用两次，第一次时为了清空旧的ref，所以为null，第二次才为DOM，可用类绑定函数替换</h1>
<h1 ref={title3} >使用createRef</h1>
)
}
title3=createRef()//调用后返回一个容器，用于存储ref标识的节点，只能存一个
test=()=>{
console.log(this.refs.title1)
console.log(this.title2)
console.log(this.title3.current)
}
}
```

##### 受控组件

> input 框的状态（value）被 React 组件状态（state）控制，就是可以被 react 状态控制的组件

- 在状态 state 中声明一个组件的状态数据
- 将状态数据设置为 input 标签元素的 value 属性的值
- 为 input 添加 change 时间
- 通过事件对象 e 获取文本框的值
- 调用 setState 方法，将文本框的值作为 state 状态的最新值

##### 非受控组件

> 就是手动操作 dm 的方式获取文本的值，文本框的状态不受 react 组件的 state 中的状态控制

- 导入 createRef 函数
- 调用 createRef 函数，创建一个 ref,存储到名为 myref 的实例属性中
- 为 input 添加 ref 属性
- 通过 myref.current 即可拿到 input 对应的 dom 元素

##### 绑定事件

1. 通过 onXxx 属性指定事件处理函数（注意大小写）

   - react 使用的自定义事件（原生小写->大写，原生大写->小写），而不是原生 DOM 事件
   - react 中的事件是通过事件委托方式处理的（委托给组件最外层的元素,事件冒泡）

   ```jsx
   //函数式组件
   function App(){
    return <div onClick={(event)=>{}}>xx<div>
   }
   //类式组件
   class App extends React.Component{

      /*避免this指向出现问题(undefined),使回调中的this指向当前组件实例对象*/
     handleClick=(event)=>{

    }
      //this为undefined,需要在constructor中通过bind改变this指向
     handleClick2(event){

    }

    render(){
      //render函数中的 this已被react修正为当前组件实例对象
      return (
      <>
      <div onClick={this.handleClick}></div>
      {/*不通过constuctor改变this指向，可用以下这种方法*/}
      <div onClick={()=>this.handleClick2()}></div>
      </>)
    }
   }
   ```

2. 可通过 event.target 得到的发生事件的 DOM 元素对象
3. 阻止默认行为 event.preventDefault()

#### 生命周期

> 类式组件才有生命周期,因为函数式组件不能实例化

##### 旧(<16.4)

- 初始化阶段

```js
constructor(); //构造器
componentWillMount(); //组件将要挂载
render();
componentDidMount(); //组件挂载完毕
```

- 更新数据时

```js
setState();
shouldComponentUpdate(); //默认返回true，返回false时不执行更新
componentWillUpdate(); //组件将要更新
render();
componentDidUpdate(); //组件更新更新完毕
```

- 强制更新时

```js
forceUpdate(); //不想更改状态中的数据，更新页面
componentWillUpdate(); //组件将要更新
render();
componentDidUpdate(); //组件更新更新完毕
```

- 父组件渲染

```js
componentWillReceiveProps(props); //第一次不调用，接受新的props才调用
shouldComponentUpdate(); //默认返回true，返回false时不执行更新
componentWillUpdate(); //组件将要更新
render();
componentDidUpdate(); //组件更新更新完毕
```

- 卸载组件

```js
//由React.unmountComponentAtNode()触发
componentWillUnmount();
```

##### 新

> 因为新版本的异步渲染，因此 componentWillUpdate，componentWillReceive，componentWillMount 即将废弃，所以要加上 UNSAFE\_防止误解和滥用

- 初始化阶段

```jsx
constructor()//构造器,初始化state,创建ref,bind解决this指向
//UNSAFE_componentWillMount()
static getDerivedStateFromProps(props,state){//组件将要挂载，不能加到实例上，要加上static。
//必须返回状态对象，若包含state中的数据，将覆盖它。用于state的值任何时候取决于props
return {a:1}
}
render()//每次组件渲染都会触发，不要在里面调用setState()
componentDidMount()//组件挂载完毕
```

- 更新数据时，调用 setState()、forceUpdate()或父组件新 Props

```jsx
static getDerivedStateFromProps(props,state)//组件将要挂载，不能加到实例上，要加上static。
shouldComponentUpdate()//默认返回true，返回false时不执行更新
getSnapshotBeforeUpdate(preProps,preState){//更新前,必须和componentDidupdate一起使用
    //返回值会传入componentDidUpdate中
    return 'xxx'
}
render()
componentDidUpdate(preProps,preState,snapshotValue)//之前的prop时和状态,不要在里面调用setState()
```

- 卸载组件

```js
//由React.unmountComponentAtNode()触发
componentWillUnmount();
```

#### react 中的 key 作用

key 是虚拟 Dom 对象的标识，当状态中的数据变化时，react 会根据新数据形成新的虚拟 DOM，
随后将新的虚拟 DOM 与旧的虚拟 DOM 进行 diff 比较。
若旧虚拟 dom 中到了与新虚拟 dom 中相同的 key：

- 虚拟 dom 中内容没有变化，使用之前的真实 dom
- 虚拟 dom 中内容变化了，则生成新的真实 dom，随后替换掉页面中的真实 dom

若旧虚拟 dom 未找到与新虚拟 dom 相同的 key，则创建新的真实 dom 渲染到页面中

用 index 做 key 引发的问题

1. 若对数据进行逆序添加，逆序删除等破坏顺序操作时，产生没必要的真实 dom 更新，虽然界面没有问题
2. 若结构中包含输入类的 DOM，会产生错误 DOM 更新，界面会出现异常
3. 若仅用于渲染列表展示，没有逆序添加，逆序删除等破坏顺序操作，则用 index 做 key 没有任何问题

#### 脚手架

安装：`npm i create-react-app -g`

创建文件夹：`create-ract-app test`

进入创建的文件夹：`npm start`

- 入口文件 index.js

```jsx
//核心库
import React from "react";
//渲染
import ReactDOM from "react-dom";
import App from "./App";
//react16.x
ReactDOM.render(<App />, document.getElementById("root"));

//react 18.x
//const root=ReactDOM.createRoot(document.getElementById("root"))
//root.render(
//严格模式会影响useEffect的执行时机，为了检测额外的副作用，会让每一个useEffect执行两次
//<React.StrictMode>
//<App/>
//</React.StrictMode>
//)
```

- App.js

```jsx
import React from 'react'
Class App extends React.Component{

render(){
return (<div>xx</div>)
}

}
export default App
```

- 样式模块化

index.module.css

```css
.title {
  color: red;
}
```

```jsx
import hello form './index.module.css'

export default calss Test extends Compont{
render(){
return (
<div className={hello.title}>xx</div>
}}
}

```

- 代理

setupProxy.js

```js
const proxy=require('http-proxy-middleware');
module.exports=function (app) {
app.use(
proxy.createProxyMiddleware('/apis',{
target:'http://xxx:xxx',
changeOrigin:true,//控制服务器收到的响应头中的Host字段值
pathRewrite:{'^/apis':''}
})，
proxy.createProxyMiddleware('/api2',{
target:'http://xxx:xxx',
changeOrigin:true,//控制服务器收到的响应头中的Host字段值,true时为代理服务ip,false为当前ip，默认为false
pathRewrite:{'^/api2':''}
})，
)

}
```

#### 组件间通信

- 子->父
  父给子组件间传入回调函数，子组件调用回调函数把数据传给父组件

- 消息订阅与发布
  第三方库 pubsub.js

#### 路由

> 依赖 H5 BOM 浏览器对象上的 history 或 hash 锚点，监听路由变化，匹配对应的组件
> BrowserRouter 使用的 h5 的 histrory API,不兼容 IE9 以下版本
> HashRouter 使用的 URL 的哈希值

- react-router-dom

```jsx
//要统一由一个router管理，可包围在App外侧
<BrowserRouter>
<Link to='/test'></Link>
<Route path="/test" component={Test}>
</BrowserRouter>

<HashRouter>
<Link to='/test'></Link>
<Route path="/test" component={Test}>
</HashRouter>
```

- 路由组件
  > 接受的 props 默认有 history，location，match

```jsx
<Route path="/test" component={Test}>
```

- 一般组件
  props 传什么有什么

```jsx
<Test />
```

- NavLink
  > 可以通过 activeClassName 指定样式名,标签体内容是个特殊属性，通过 this.props.children 获取

```jsx
<NavLink activeClassName="active_color">test
</NavLink>
<NavLink activeClassName="active_color" children='test'>
</NavLink>
```

- Switch
  > 路由默认会全部匹配完，加上 switch 可实现单一匹配，提高效率

```jsx
<Switch>
<Route path="/test" component={Test}>
<Route path="/test" component={Test}>
//默认模糊匹配,exact严格匹配，开启后无法匹配二级路由
<Route exact path="/test" component={Test}>
</Switch>
```

- Redirect
  > 重定向，写在路由最下方，当路由都不匹配时，跳转到 redirect 指定的路由

```jsx
<Redirect to="/test" >
```

- 二级路由

```jsx
<Switch>
<Route path="/test" component={Test}>
<Route path="/test/a" component={Test}>
<Redirect to="/test" >
</Switch>
```

- 路由参数

```jsx
//params参数，this.props.match.params
<HashRouter>
<Link to=`/test/${123}/${123}`></Link>
<Route path="/test/:id/:count" component={Test}>
</HashRouter>
```

```jsx
//search参数(为urlencoded编码字符串，需借助querystring解析)，this.location.match.search
import qs from 'querystring'
<HashRouter>
<Link to=`/test?a=${123}&b=${123}`></Link>
<Route path="/test" component={Test}>
//qs.parse(this.props.location.search)
</HashRouter>
```

```jsx
//state参数(不在地址栏显示，browser刷新页面不丢失，hashRouter刷新丢失)，this.props.location.state
<HashRouter>
<Link to={{pathname:'/test'state:{a=123,b=123}}}></Link>
<Route path="/test" component={Test}>
</HashRouter>
```

- 编程式路由

```jsx
goto(){
this.props.history.push('/test',{
xx:'xx'
})
this.props.history.replace('/test',{
xx:'xx'
})
}
```

- withRouter
  > 加工一般组件，让一般组件用上路由 api

```jsx
import { withRouter } from "react-router-dom";

class Test extends Component {}

export default withRouter(Test); //返回的是全新的组件
```

#### React Router 6

> 移除`<Switch/>` ，新增`<Routers/>`
> 移除`<Redirect/>`，新增`<Navigate/>` > `component={About}`变为 `element={<About/>}`
> 增加 hooks

- Routers

> 提供路由出口，满足条件的路由组件会渲染到组件内部，相当与 vue-router 里的<router-view></router-view>
> 必须用 Routers 包裹，不会匹配多个

```jsx
import {Routers,Route} form 'react-router-dom'
<Routers>
<Route path="/test" element={<Test/>}>
<Route path="/TEST" caseSensitive element={<Test2/>}>//区分大小写
</Routers>
```

- Navigate
  > 重定向,只要被渲染，就会切换路由

```jsx
import {Routers,Route,Navigate} form 'react-router-dom'
<Routers>
<Route path="/test" element={<Test/>}>
<Route path="/" element={<Navigate to="/test"  />}>
<Route path="/" element={<Navigate to="/test" replace />}>
</Routers>
```

- NavLink

```jsx
<NavLink className={()=>return 'activeColor'}></NavLink>
```

- 路由表

```jsx
import {lazy} from 'react'
import {useRoutes,Navigate} from 'react-router-dom'
//生成路由
cosnt About=lazy(()=>import('../pages/About'))
const element=useRoutes([
  {
    path:'/test',
    element:<Test/>
    children:[
      {
        path:'new',
        element:<New/>
      }
    ]
  },
  {
    path:'/about',
    element:<About/>
  },
  {
      path:'/',
      //实现路由重定向
      element:<Navigate to='/main'/>
  }
])


render(){
return (
<div>
{/*引用路由表*/}
{element}
</div>
)}
```

- Outlet

```jsx
function App() {
  return (
    <Routers>
      <Router path="/test" element={<Test />}>
        {/*二级路由嵌套*/}
        <Router path="test1" element={<Test1 />}></Router>
        <Router path="test2" element={<Test2 />}></Router>
      </Router>
    </Routers>
  );
}
```

```jsx
import {Outlet,NavLink} from 'react-router-dom'
function Test(){
// 若要显示子路由，需在父级路由组件中引入Outlet！！！
  render(){
    return (
      <div>
      <NavLink end to="new"></NavLink>//end代表匹配子路由时，自己失去高亮
        {/*指定路由组件呈现的位置*/}
         <Outlet />
      </div>
    )}
}
```

- 路由参数

```jsx
//params参数，this.props.match.params
<Link to=`/test/${123}/${123}`></Link>
...

import {useParams,useMath} from 'react-router-dom'
export default function Demo(){
const {a,b}=useParams()
const {a,b}=useParams(/test/:a/:b})
return ({a+b})
}
```

```jsx
<Link to=`/test?a=${123}&b=${123}`></Link>
import {useSearchParams,useLocation} from 'react-router-dom'
export default function Demo(){
const [search,setSearch]=useSearchParams()
const a=search.get('a')
let setData=()=>{setSearch(1234)}//更新a
const a1=useLocation()
return ({a})
}
```

```jsx
<Link to='/test' state={{a=123,b=123}}></Link>
import {useLocation} from 'react-router-dom'
export default function Demo(){
const obj=useLocation()
return ({obj.a})
}
```

- 编程式路由

```jsx
//导入useNavigate　函数
import {useNavigate} from 'react-router-dom'
const navigate=userNavigate()
goto(){
  //跳转
  navigate('/test')
  navigate('/test',{
    replace:false,
    state:{}
  })
  navigate(1)//前进
  navigate(-1)//后退
//传参searchParams
  navigate('/test?id=1&name=xigua')
//params参数
//需配置路由/test/:id/:name
  navigate('/test/1/xigua')
}

```

```jsx
import { useParams, useSearchParams } from "react-router-dom";
function Test() {
  const [searchParams] = useSearchParams();
  const id = searchParams.get("id");
  const { name } = searchParams.getAll();

  const [params] = useParams();
  const id = params.id;
  return <div></div>;
}
```

- 通配符

> 不匹配时，显示的内容

```jsx

<Routes>
  <Route path="/" element={<Home></Home>}>
  <Route path="*" element={<NotFount></NotFount>}>
</Routes>
```

#### 集中式状态管理工具 redux

> 状态管理 js 库，集中式管理 react 应用中多个组件共享的状态

- store：联系 action 和 reducer 的对象，提供`getState()获取state`,`dispatch()发送action`,`subscribe()注册监听，返回值注销监听`方法
  - action：本质是个对象，应用给 store 传递数据的载体，应用通过 store.dispatch()发送给 store
  - reducer:本质是个函数，用来响应发过来的 actions,处理后把 state 发给 store。（需要有 return 返回值，接受两个参数`初始化state`,`action`）
  - state:数据

##### 旧版

###### 使用

- countAction.js

```js
//创建一个为count组件服务的action，
export function test(data) {
  return { type: "xxxx", data };
}
```

- countReducer.js

```js
//创建一个为count组件服务的reducer，reducer本质是一个函数
//第一次调用时是store自动触发的，传递的preState是undefin
//必须是纯函数（同样输入必定得到同样输出）不要在reducer中发起网络请求、改写prestate、使用Date.now() , Math.random()
export default function (preState, action) {
  const { type, data } = action;
  switch (type) {
    case 0:
      return preState; //当preState没有变化时，不会引起页面刷新（浅比较）
    case 1:
      return preState + 1;
  }
}
```

- constants.js

```js
export const ADD = "add";
```

- test.jsx

```jsx
import store from '../store/store.js'
import {test} from '../store/countAction.js'

componentDidMount(){
store.subscribe(()=>{
  this.setState({})
})
}

increment(){
  store.dispatch(test(123))
}

render(){
return （<div>{{store.getState()}}</div>）
}
```

###### 中间件实现异步 action

> 异步 action 指 action 返回值为函数，同步 action 指 action 返回值为对象
> redux-thunk 能让我们的 action 不仅能返回一个对象还能返回一个函数，在函数里面在进行一次 dispatch，这样就能把异步请求抽离到 action 里面

- 安装：`yarn add redux-thunk`

- store.js

```js
import {createStore,applyMiddleware}form 'redux'
//引入异步action中间件
import thunk  from 'redux-thunk'
import countReducer from './countReducer'
const store=createStore(countReducer,applyMiddleware(thunk))
export default store
```

- action.js

```js
import store from './store'
export const add=data=({type:'add',data})//同步action,直接返回对象
export const addAsync=(data,a,b,c)=>{//异步action,返回函数，函数参数是dispatch方法
  return (dispatch)=>{
      setTimeout(()=>{
          dispatch({type:'add',data:123}})
      },50000)
}
}

```

###### 多个 reducer

- store.js

```js
import {createStore，applyMiddleware,combineReducers}form 'redux'
import countReducer1 from './reducer/count1'
import countReducer2 from './reducer/count2'

let allReducer=combineReducers({
  count1:countReducer1
  count2:countReducer2
})

export default createStore(allReducer)
```

##### react-redux

官方用于配合 react 的库，使组件更加方便从 store 中读取数据，分发 action

> 组件通过 props 传递状态、操作方法
> 不用再写 store.subscribe()

###### provide

该组件能使整个 app 都能读取到 store 中的数据

```js
//index.js
import store from "./store";
import { Provider } from "react-redux";
//自动匹配容器组件，添加上store
ReactDOM.render(
  <ProVider store={store}>
    <App />
  </ProVider>,
  document
);
```

###### connect

该方法能使组件和 store 进行关联,使用`connect(参数)(组件)`
常用参数

- `mapStateToProps(state,ownProps)函数`将 store 中的数据作为 props 绑定到组件上
- `mapDispatchToProps(dispatch,ownProps)函数`将 action 作为 props 绑定到组件上

```jsx

class Test extend Componet{
  render(){
    return <div onClick={()=>{this.props.add()}}>{this.props.state.value}<div>
  }
}

let mapStateToProps = (state) => {
  return {
    a: 1,
    b: state,
  };
};

let mapDisoatchToProps = (dispatch) => {
  return {
    d: (data) => {
      dispatch({ type: "xxx", data });
    },
  };
};
export default connect(
 mapStateToProps,
 mapDisoatchToProps
)(Test)
```

##### 新版

使用 Redux Toolkit 简化 Redux

`npm install @reduxjs/toolkit react-redux`
react-redux 也需要单独安装

- configureStore

  > configureStore 替代 createStore 配置简单,设置默认值也方便
  > src/store/index.js

  ```js
  // 引入
  import { configureStore } from "@reduxjs/toolkit";
  import counterSlice from "../pages/basic/counterSlice";
  import mySlice from "../pages/mySlice";

  export default configureStore({
    reducer: {
      rootCounter: counterSlice,
      rootMy: mySlice,
    },
  });
  ```

这里 reduer 直接合并成一个唯一的根 root 了,原有的 `combineReducers` 这个合并函数就用不到了
注意自己配置的 reducer 的 key 值 和 对应的 value 值

- 根组件配置 store

```js
入口index.js;

import React from "react";
import ReactDOM from "react-dom/client";
import store from "./store";
import { Provider } from "react-redux";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

- createAction
  创建一个 action，传入动作类型字符串，返回动作函数。
  createAction 语法： function createAction(type, prepareAction?)
  1，type：Redux 中的 actionTypes
  2，prepareAction：Redux 中的 actions

- createReducer
  创建一个 reducer，action type 映射到 case reducer 函数中，不用写 switch-case，并集成 immer。
  Builder 提供了三个方法：
  1，addCase： 根据 action 添加一个 reducer case 的操作。
  2，addMatcher： 在调用 actions 前，使用 matcher function 过滤
  3，addDefaultCase： 默认值，等价于 switch 的 default case;

- createSlice reducer 编写

  > createSlice 对 actions、Reducer 的一个封装。

  - 创建 slice
    使用 createSlice 方法创建一个 slice。每一个 slice 里面包含了 reducer 和 actions，可以实现模块化的封装。
    所有的相关操作都独立在一个文件中完成。

  - 关键属性:
    - `name`：命名空间，可以自动的把每一个 action 进行独立，解决了 action 的 type 出现同名的文件。在使用的时候默认会把使用 `name/actionName`
    - `initialState`：state 数据的初始值
  - reducers
    定义的 action。由于内置了 immutable 插件，可以直接使用赋值的方式进行数据的改变，不需要每一次都返回一个新的 state 数据。
  - 导出
    counterSlice.actions 导出所有的修改函数方便页面使用
    counterSlice.reducer 导出 reducer 在 store 里面使用
    具体 reducer 函数的参数
    参数 1: 当前 slice 的 state 数据
    参数 2: 对象{type:"",payload:传参}

type:counterSpace/decrement
type 就是之前的 actions 用 switc/case 来匹配很麻烦,现在简洁了
type 构成 slice 的 name 命名空间/具体的修改函数

payload 要和传的时候保持一致

```js
import { createSlice } from "@reduxjs/toolkit";
const initialState = {
  counter: 100,
  user: {
    name: "yzs",
    job: "全栈",
  },
};
/*
reducer切片
createSlice函数的作用：生成分片的reducer
内部调用的市createAction和createReducer
creatSlice可以帮助我们用更少的代码去生成配套的reducer和action,而且有很好的维护性
*/
export const counterSlice = createSlice({
  name: "counterSpace", // 命名空间，在调用action的时候会默认的设置为action的前缀,保证唯一.不重名
  initialState,
  reducers: {
    // reducer函数 state当前组件的数据
    //第二个参数为{payload:{},type:"""} 想想旧写法或者vuex
    increment(state) {
      state.counter += 100;
    },
    decrement(state, actions) {
      // actions == {payload:{},type:"""}
      console.log("decrement---actions", actions);
      state.counter -= actions.payload;
    },
    updateUser(state, { payload }) {
      console.log("updateUser-------payload", payload);
      // 引用类型 注意 赋值的写法
      state.user = {
        ...state.user,
        ...payload,
      };
    },
  },
});
/*
切片对象会自动地帮助我们生成action
切片对象会根据我们地reducers方法来自动地创建action对象，这些action对象会保存到切片对象的actions中
{type:name/函数名，payload:函数的参数}
*/
export const { increment, decrement, updateUser } = counterSlice.actions;
export const selectCount = (state) => state.rootCounter.counter;
export const selectUser = (state) => state.rootCounter.user;

export default counterSlice.reducer;
```

- 页面使用
  1. useSelector()返回指定的 state

```js
// 这样写太长了 麻烦
const counter = useSelector((state) => state.rootCouter.counter);
```

rootCouter 这个 key 来源于 根 store 里面配置的 reducer，这样写太长了，麻烦。在这个 slice 里面我做了统一处理

```js
export const selectCount = (state) => state.rootCounter.counter;
export const selectUser = (state) => state.rootCounter.user;
```

页面使用

```jsx
import { useSelector, useDispatch } from "react-redux";
import {
  increment,
  decrement,
  updateUser,
  selectCount,
  selectUser,
} from "./normalSlice";
export default function Counter() {
  const dispatch = useDispatch();

  // let counter = useSelector(state=>state.rootCouter.counter)
  const counter = useSelector(selectCount);
  console.log("页面---counter:", counter);
  const user = useSelector(selectUser);

// 这样可以直接解构出当前 slice所有的 state
// 具体用哪种 看自己心情
 const { counter } = useSelector((state) => state.rootCouter);

return ( <div> 布局看下面 </div>)

```

- useDispatch()
  payload 传参和 reducer 保持一致
  引用类型的 修改注意

```jsx
const dispatch = useDispatch(); 修改函数
  return (
    <div>
      <h1>reduxjs/toolkit 基础用法</h1>
      <hr />
      <button onClick={() => dispatch(increment())}>+</button>
      <span>{counter}</span>
      <button onClick={() => dispatch(decrement(666))}>-</button>
      <hr />
      <h1>姓名:{user.name} ---职业:{user.job}</h1>
      <button onClick={()=>{dispatch(updateUser({name:'Michael'}))}}>改名</button>
      <button onClick={()=>{
        dispatch(updateUser({job:'自由职业者'}))}
        }>转行</button>
    </div>
  );
}

```

- 异步 createAsyncThunk()
  内置了 redux-thunk 处理异步 , 足够解决绝大部分的问题. 还有其他中间件比如:redux-saga 、redux-observable。
  异步请求处理三种状态的 action :pending\fulfilled\rejected；
  这三种状态的 action 自动触发, 防止外部手动调用,则使用属性 extraReducers , 则不会生成对外的的 action creator .

```js
接受一个动作类型字符串和一个返回Promise函数，并生成一个pending/fulfilled/rejected基于该Promise分派动作类型的 thunk
用 fetch请求模拟一个异步
3.createAsyncThunk("counterSpace/getList",()=>{})
参数1: slice的name/命名空间/函数名

// return 不要忘记
export const getList = ( ) => {
return fetch("https://.XX.cn/api/news").then(res=>res.json());
};
export const  getListAsync =  createAsyncThunk("counterSpace/getList",async()=>{
  const res = await getList()
  return res// 此处的返回结果会在 .fulfilled中作为payload的值
});


```

- extraReducers
  异步函数配置

```js
createSlice({
    name: "counterSpace",
    initialState,
    reducers:{},
    extraReducers: (builder) => {
    builder
      .addCase(getListAsync.pending, (state) => {
         console.log("pending",state);
      })
      .addCase(getListAsync.rejected, (state, err) => {
        console.log("rejected 失败",err);

      })
      .addCase(getListAsync.fulfilled, (state, action) => {
         console.log("fulfilled 成功",state);
        console.log("fulfilled action",action);

        state.list = action.payload
      });
  },
});
这个配置基本就是套路
只需要把函数名字改为通过createAsyncThunk()创建的函数名
根据自己的业务场景 写赋值逻辑就行
```

页面使用异步函数

```jsx
<button onClick={()=>dispatch(getListAsync('异步模拟'))}>异步</button>
      <ul>
        {
          listData.map((news)=>{
            return <li key={news.id}>{news.title}</li>
          })
        }
      </ul>
```

##### 集中式状态管理工具 Mobx

> 简单：编写无模板的极简代码
> 轻松实现最优渲染：依赖自动追踪最小渲染优化
> 自由：可移植，测试

###### 安装

```bash
npm install mobx mobx-react
```

###### 基本使用

- stroe/counter.js

```js
import { makeAutoObservable, computed } from "mobx";
class CounterStore {
  //定义数据
  count = 0;
  constructor() {
    //把数据弄成响应式
    makeAutoObservable(this, {
      count_2: computed, //标记为计算属性
    });
  }
  //修改数据
  addCount = () => {
    this.conut++;
  };

  get count_2() {
    return this.count * 10;
  }
}
//实例化，导出
const counterStore = new CounterStore();
export default counterStore;
```

- App.js

```jsx
//导入store
import counterStore from "./store/counter.js";
//导入中间件，链接mobx，react,完成响应式
import { observer } from "mobx-react";
function App() {
  return (
    <div>
      {counterSotre.count}
      {counterSotre.count_2}

      <button onClick={counterStore.addCount}>add</button>
    </div>
  );
}
export default observer(App);
```

###### 模块化

- store/index.js

```js
//组合模块
import { CounterStore } from "./counter.Store.js";
import React from "react";

//声明rootStore
class RootStore {
  constructor() {
    this.counterStore = new CounterSotre();
  }
}
//实例化根store
const rootStore = new RootStore();
//使用context进行透传．也可以直接导出包一层observer
//查找机制：优先从Provider标签中value,若找不到则找createContext()方法中的传递过来的默认参数
const context = React.createContext(rootStore);
const useStore = () => React.useContext(context);
export { useStore };
```

- store/counter.Stroe.js

```js
import { makeAutoObservable, computed } from "mobx";
class CounterStore {
  //定义数据
  count = 0;
  constructor() {
    //把数据弄成响应式
    makeAutoObservable(this, {
      count_2: computed, //标记为计算属性
    });
  }
  //修改数据
  addCount = () => {
    this.conut++;
  };

  get count_2() {
    return this.count * 10;
  }
}
//实例化，导出
export default CounterStore;
```

App.js

```jsx
//导入store
import { useStore } from "./store/index.js";
import { observer } from "mobx-react";
function App() {
  const rootStore = useStore();

  return (
    <div>
      {rootStore.counterSotre.count}
      {rootStore.counterSotre.count_2}

      <button onClick={rootStore.counterStore.addCount}>add</button>
    </div>
  );
}

export default observer(App);
```

##### 解构赋值

```js
let obj = { a: { b: 1 } };
const { a } = obj;
const {
  a: { b },
} = obj; //连续解构赋值
const {
  a: { b: value },
} = obj; //连续解构赋值+重命名
```

##### 高阶组件

> 把一个组件当成另一个组件的参数传入，然后返回新的组件

```jsx
function AuthComponent({children}){
  const isToken=localStorage.get('token')
  if(isToken){
    return <>{children}</>
  }else{
    return <Navigate to="/login" replace />
  }
}

//使用
<AuthComponent>
  <Home>
</AuthComponent>
```

##### 高阶函数

- 若 a 函数，接收的参数是一个函数，那么 a 就可以称为高阶函数
- 若 a 函数，调用的返回值依然是一个函数，那么 a 就可以称为高阶函数
  如 promise setTimeOut arr.map()

```jsx
test=(type){
return (e)=>{
arr.push( {[type]:e.target.value})
}
}
<input onChange="{test('1')}"/>
<input onChange="{test('2')}"/>
```

##### 函数的柯里化

通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式

```js
test=(type){
return (e)=>{
arr.push( {[type]:e.target.value})
}
}
```

```js
function sum(a, b, c) {
  return a + b + c;
}

function sum2(a) {
  return (b) => {
    return (c) => {
      return a + b + c;
    };
  };
}
sum(1)(2)(3);
```

### useState 和 setState 到底是同步还是异步

#### 18.x

均为异步

#### 18.x 以下

##### 在正常的 react 的事件流里（如 onClick 等）

- setState 和 useState 是异步执行的（不会立即更新 state 的结果）
- 多次执行 setState 和 useState，只会调用一次重新渲染 render
  不同的是，setState 会进行 state 的合并，而 useState 则不会

##### 在 setTimeout，Promise.then 等异步事件中

- setState 和 useState 是同步执行的（立即更新 state 的结果）
- 多次执行 setState 和 useState，每一次的执行 setState 和 useState，都会调用一次 render

#### 前端发请求

##### 　 xhr

- jquery
- axios（node 使用 axios 是封装 http 协议）

##### fetch

> 原生函数,老版本兼容性差

fetch('xxx').then(res=>{
//联系服务器成功
return res.json()
},err=>{return new Promis(()=>{})}).then(res=>{},err=>{})

##### setState

写法

```js
state = { a: 1 };

对象式setState;
this.setState({ a: 2 });
this.setState({ a: 2 }, () => {
  //状态更新以及页面也更新后，才调用
});

函数式setState;
this.setState(
  (state, props) => {
    return { a: 1 };
  },
  () => {}
);
```

#### lazyLoad

> 路由组件懒加载

```jsx
import {lazy,Component,Suspense} form 'react'
import Loading from "./component/Loading.jsx"
const Home=lazy(()=>{import ('./Home')})
export default class Demo extend Component{

<Suspense fallback={<Loading/>}>
<Route  path="/home" component={Home}>

</Route>
</Suspense>

}

```

#### Hooks

> hook 是 16.8 的新特性，可以让你在函数组件中使用 state 以及其他 react 特性
> hook 本质是一套能够使函数组件更强大，灵活的‘钩子’(某一时刻下自动执行的函数)
> 解决组件逻辑复用的问题：hook 出现前，react 先后尝试了 mixins、HOC 高阶组件、render-props 等模式，但各自都有对应的问题。如 mixin 的数据来源不明，高阶组件的嵌套问题
> 解决了类式组件自身的问题：如属性过多，生命周期、this 指向

##### state

useState 返回的值是数组

```jsx
import React from "react";
function Test() {
  const [a, set] = React.useState(123); //初始化会调用一次，下次调用时，会缓存数据，不会覆盖
  //不能在if/for/函数体中写（react会按照hooks调用顺序识别每一个hook）
  let add1 = () => {
    set(456);
  };
  let add2 = () => {
    set((a) => {
      return a + 1;
    });
  };
  return <div>{a}</div>;
}
```

##### effect

> 函数组件里使用生命周期

```jsx
import React form 'react'
function Test(){
const [count,setCount]=React.useState(123)

React.useEffect(()=>{},[a])//检测a,改变时调用------
React.useEffect(()=>{},[])//谁也不监测,仅在挂载和卸载的时候执行----
React.useEffect(()=>{})//检测所有,改变时调用----componentDidUpdate
React.useEffect(()=>{return ()=>{组件卸载前执行}},[])//componentWillUnmount
//不要在useEffectd 的回调函数外层直接包裹await,因为异步会导致清理函数无法立即返回
React.useEffect(async()=>{
  const res=await axios.get('...')
},[])
//正确写法
React.useEffect(()=>{
  async function getData(){
  const res=await axios.get('...')
}},[])
return (<div>a</div>)
}

```

在 React 通知到 Renderer 渲染器后，渲染器又分了三个子阶段来处理：

beforeMutation 阶段（渲染视图前）
mutation 阶段（渲染试图）
layout 阶段（渲染视图后）
渲染器会在 mutation 阶段完成后， 在 layout 阶段同步的调用 useLayoutEffect，在子组件嵌套中于是如此。在类组件中，调用的是 componentDidMount 生命周期函数。也就是说，在 useLayoutEffect 中，无论是否有重新触发 setState，也不会在当前渲染里，重新更新界面。
而在整个渲染器渲染阶段（其实也叫 commit）渲染完成后，react 才会异步的执行 useEffect。当在 useEffect 中如果有 setState，则会重新触发渲染器，更新界面。

结论：

useLayoutEffect 的是在渲染器执行当前渲染界面任务时，同步执行。
在当前一轮的 Reconciler 任务调度过程中，在渲染器执行完当前任务后，才会异步调用 useEffect。
useLayoutEffect 先于 useEffect 执行，并且子组件优先执行。
componentDidMount()完全等价于 useLayoutEffect( fn , [ ] )，但是不等价于 useEffect( fn , [ ] )。

##### ref

> 获取 dom 元素获取组件对象

```jsx
import React form 'react'
function Test(){
const myRef=React.useRef()
function show(){myRef.style.color='red'}
return (<div ref={myRef}>a</div>)
}
```

#### Fragment

> 可以不用必须有一个真实 DOM 根标签，编译后会移除

```jsx
render(){

return (
<Fragment key={}>//只能有一个属性：key
<>//简写，但不能写任何属性
</>
</Fragment>
)
}
```

#### Context

> 常用与祖组件和后代组件通信
> 一般不用 Context,而是它的封装 react-redux

类式组件

```jsx
const CountContext = React.createContext(); //创建一个上下文,必须所有组件都访问得到
class A extends Component {
  state = { a: 1 };
  render() {
    return (
      <CountContext.Provider value={this.state.a}>
        <B />
      </CountContext.Provider>
    );
  }
}
class B extends Component {
  render() {
    return <C />;
  }
}
class C extends Component {
  static contextType = CountContext; //声明接收context
  render() {
    return this.context;
  }
}
```

- 所有组件

```jsx
const CountContext=React.createContext()//创建一个上下文,必须所有组件都访问得到
const {Consumer,Provider}=CountContext
class A extends Component{
state={a:1}
render(){
return (<Provider value={this.state.a}><B/></Provider>)}
}
class B extends Component{
render(){
return (<C/>)}
}
function C(){
static contextType=CountContext//声明接收context
render(){
return (<div><Consumer>{value=>{return (<span>value</span>)}</Consumer></div>)}
}
```

#### PureComponent

> 只要执行 setState 即使不更新数据，组件也会重新 render9)
> 只要 render()调用，其子组件也会更新
> 原因：shouldComponentUpdate()默认总是为 true

##### 解决

- 手动比较新旧值,不一样更新，否则禁止更新

```jsx
shouldComponentUpdate(nextProps,nextState){//新的
console.log(this.props,this.state)//旧的
return true
return false
}
```

```jsx
import {PureComponent} from 'react'

class Test extends PureComponent {
//依赖的数据不更新时，不重新render

const obj=this.state
obj.name='xx'
this.setState(obj)//浅比较，数据不会更新

}

```

#### render Props

> 组件标签内可以直接写多个文本，标签、函数、jsx,会传入到组件 prpos 中的 chidren 属性中
> vue 中的插槽

```jsx
class A extends Component{
state={a:1}
render(){
return (
  <>
<B>aaa</B>
 <B render={(val)=>{<C b={val} />}/>
 </>
)}
}
class B extends Component{
state={b:1}
render(){
return (
  <>
<span>{this.props.children}</span>
this.props.render(this.state.b)
</>
)}//aaa
}
function C(){
static contextType=CountContext//声明接收context
render(){
return (<div><Consumer>{value=>{return (<span>value</span>)}</Consumer></div>)}
}
```

#### ErrorBoundary 错误边界

> 防止子组件出错导致整个页面出错,只能在生产环境使用
> 只能捕获后代组件生命周期产生的错误，不能捕获自己的

```jsx
state={error:''}
//生命周期函数
static getDerivedStateFromError(err){//它的子组件出现报错时会调用
componentDidCath(err,info){}//发生错误时调用
return {error:err}//返回新的state，在render前触发
}
render(){
return (
<div>
{error?error:''}
</div>
)
}
```

#### 组件间通信

- props

```js
children props
render props
```

- 消息订阅-发布

```js
pubs-sub event
```

- 集中式管理

```js
redux、dva
```

- conText

```js
生产者 - 消费者模式;
```

- 搭配
  父子：props
  兄弟: 集中式管理、消息订阅发布，借助父组件
  祖孙: 集中式管理、消息订阅发布、context（开发用的少，封装插件用得多）

### 关闭 eslint

第一步：执行以下命令：

`npm run eject`

第二步：在 package.json 中修改代码

```json
"eslintConfig": {
"extends": [
"react-app",
"react-app/jest"
],
"rules":{
"no-undef":"off",
"no-restricted-globals": "off",
"no-unused-vars": "off"
}
},

```

第三步：重启项目

### 常见错误

#### 并发模式下在 dev 时 render-phase 会执行两次

这个是 react 的一个用来突出显示应用程序中潜在问题的工具（严格模式）

有一项检测意外的副作用，严格模式不能自动检测到你的副作用，但它可以帮助你发现它们，使它们更具确定性。通过故意重复调用以下函数来实现的该操作。

注意：这仅适用于开发模式。生产模式下生命周期不会被调用两次。strictMode，故意在开发环境中执行多次，暴雷出代码隐藏的 bug,把标签去掉即可

#### antd+form，initialValue 值变化后不更新

当我们第一次点开 Modal 的时候， 会得到一个 initialValue,但是这个值只在组件挂载的时候执行了一次，后续数据的更新并不会造成重新渲染，所以当我们再次打开 Modal 窗口的时候并不会更新。

解决方案：
方法一：使用 form.resetFields()
使用 resetFields 方法会直接重置为 initialValue 的值，这样再次打开编辑表单就是我们想要的数据啦。

方法二：使用 form.setFieldsValue
对于 initialValue 不更新问题官方文档已经给出了解决方法

#### react 获取上一轮的 props 和 state（接用 useEffect, useRef 实现）

如果只是 想实现 这个效果 下面的代码 也行 。就不用借助其它的了。 这个思路就是，在 改变 state 之前 就 备份一下 值 。

effect 的执行时机

与 componentDidMount、componentDidUpdate 不同的是，传给 useEffect 的函数会在浏览器完成布局与绘制之后，在一个延迟事件中被调用。这使得它适用于许多常见的副作用场景，比如设置订阅和事件处理等情况，因为绝大多数操作不应阻塞浏览器对屏幕的更新。

然而，并非所有 effect 都可以被延迟执行。例如，一个对用户可见的 DOM 变更就必须在浏览器执行下一次绘制前被同步执行，这样用户才不会感觉到视觉上的不一致。（概念上类似于被动监听事件和主动监听事件的区别。）React 为此提供了一个额外的 useLayoutEffect Hook 来处理这类 effect。它和 useEffect 的结构相同，区别只是调用时机不同。

此外，从 React 18 开始，当它是离散的用户输入（如点击）的结果时，或者当它是由 flushSync 包装的更新结果时，传递给 useEffect 的函数将在屏幕布局和绘制之前同步执行。这种行为便于事件系统或 flushSync 的调用者观察该效果的结果。

注意

这只影响传递给 useEffect 的函数被调用时 — 在这些 effect 中执行的更新仍会被推迟。这与 useLayoutEffect 不同，后者会立即启动该函数并处理其中的更新。

即使在 useEffect 被推迟到浏览器绘制之后的情况下，它也能保证在任何新的渲染前启动。React 在开始新的更新前，总会先刷新之前的渲染的 effect。
useRef

useRef 返回一个可变的 ref 对象，其 .current 属性被初始化为传入的参数（initialValue）。返回的 ref 对象在组件的整个生命周期内持续存在。
本质上，useRef 就像是可以在其 .current 属性中保存一个可变值的“盒子”。

你应该熟悉 ref 这一种访问 DOM 的主要方式。如果你将 ref 对象以 `<div ref={myRef} />` 形式传入组件，则无论该节点如何改变，React 都会将 ref 对象的 .current 属性设置为相应的 DOM 节点。

然而，useRef() 比 ref 属性更有用。它可以很方便地保存任何可变值，其类似于在 class 中使用实例字段的方式。

这是因为它创建的是一个普通 Javascript 对象。而 useRef() 和自建一个 {current: ...} 对象的唯一区别是，useRef 会在每次渲染时返回同一个 ref 对象。

请记住，当 ref 对象内容发生变化时，useRef 并不会通知你。变更 .current 属性不会引发组件重新渲染。如果想要在 React 绑定或解绑 DOM 节点的 ref 时运行某些代码，则需要使用回调 ref 来实现。

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  const prevCountRef = useRef();
  useEffect(() => {
    prevCountRef.current = count;
  });
  const prevCount = prevCountRef.current;

  return (
    <h1>
      Now: {count}, before: {prevCount}
    </h1>
  );
}
```
