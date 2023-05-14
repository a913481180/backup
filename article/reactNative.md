---
title: react Native
date: 2021-01-09 20:00:00
categories: 
- web 
tags:
- app
---
# React

## 环境搭建

- node.js python2(不能python3) jdk android_studio
- 配置环境变量


## 创建项目

- 使用React Native内建的命令工具进行创建，也可以使用node自带的npx命令来安装
`npx react-native init test_project`
-准备 Android 设备
你需要准备一台 Android 设备来运行 React Native Android 应用。这里所指的设备既可以是真机，也可以是模拟器。后面我们所有的文档除非特别说明，并不区分真机或者模拟器。Android 官方提供了名为 Android Virtual Device（简称 AVD）的模拟器。此外还有很多第三方提供的模拟器如Genymotion、BlueStack 等。一般来说官方模拟器免费、功能完整，但性能较差。第三方模拟器性能较好，但可能需要付费，或带有广告。

使用 Android 真机
你也可以使用 Android 真机来代替模拟器进行开发，只需用 usb 数据线连接到电脑，然后遵照在设备上运行这篇文档的说明操作即可。

使用 Android 模拟器
你可以使用 Android Studio 打开项目下的"android"目录，然后可以使用"AVD Manager"来查看可用的虚拟设备,如果你刚刚才安装 Android Studio，那么可能需要先创建一个虚拟设备。点击"Create Virtual Device..."，然后选择所需的设备类型并点击"Next"，然后选择S API Level 31 image.
- 编译并运行 React Native 应用
确保你先运行了模拟器或者连接了真机，然后在你的项目目录中运行`yarn android`或者`yarn react-native run-android`。此命令会对项目的原生部分进行编译，同时在另外一个命令行中启动Metro服务对 js 代码进行实时打包处理（类似 webpack）。Metro服务也可以使用yarn start命令单独启动。

如果配置没有问题，你应该可以看到应用自动安装到设备上并开始运行。注意第一次运行时需要下载大量编译依赖，耗时可能数十分钟。此过程严重依赖稳定的代理软件，否则将频繁遭遇链接超时和断开，导致无法运行。

也可以尝试阿里云提供的maven 镜像，将android/build.gradle中的jcenter()和google()分别替换为maven { url 'https://maven.aliyun.com/repository/jcenter' }和maven { url 'https://maven.aliyun.com/repository/google' }（注意有多处需要替换）。

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

- 内联样式
要用`style={{fontSize:"20px"}}`的写法

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

### 函数式组件

```
function Demo(){  //组件必须首字母大写
//this 为undefind,经过babel编译后开启严格模式，this便指向undefind，不开启严格模式则是window
return <h1>test</h1>
}

ReactDOM.render(<Demo/>,document.getElementById('test'))
//react解析组件标签,找到Demo组件，发现此组件是函数定义的，便调用该函数，将虚拟dom渲染成真实dom
```

### 类式组件

#### 简单组件
```
class Demo extends React.Component{
render(){//在Demo的原型对象上
//this指向Demo的实例对象
return <h1>test</h1> 
}
}
ReactDOM.render(<Demo/>,document.getElementById('test'))
//react解析组件标签,找到Demo组件，发现此组件是类定义的，便new 出该类实例，通过该实例调用原型对象上的render方法。将虚拟dom渲染成真实dom

```

#### 复杂组件

##### state
>state为组件的重要属性，值是对象，通过更新组件的state来更新对应的页面显示（重新渲染组件）

```
class Demo extends React.Component{
constructor(prop){
super(prop)
this.state={
a:1,
b:2
}
//改变t
this.changValue=this.changValue.bind(this)
}

render(){
return (
<h1 onClick={test}>test{this.state.a}</h1> 
<h1 onClick={this.changeValue}>test{this.state.b}</h1> 
)

changValue(){
	//类中的方法默认开启局部严格模式
	//所以this为undefind
	//只有通过Demo实例调用此实例，this才为Demo的实例
	//changeValue作为onClick的回调，不是实例调用
}
}
}
function test(){}
ReactDOM.render(<Demo/>,document.getElementById('test'))

```

- 解决 this指向
```

class Demo extends React.Component{
constructor(prop){
super(prop)//构造器是否接受props，是否传给super取决于是否希望在构造器中通过this访问props
this.state={
a:1,
b:2
}
//改变this指向
this.changValue=this.changValue.bind(this)
}

render(){
return (
<h1 onClick={this.changeValue}>test{this.state.b}</h1> 
)

changValue(){
//state不能直接更改
this.state.a=1
//必须通过api
this.setState({a:2})//更新为合并，不是替换
}
}
}
ReactDOM.render(<Demo/>,document.getElementById('test'))
```

- 简写
```
class Demo extends React.Component{
state={//等于号，代表给该类的实例追加属性
a:1,
b:2

}
render(){
return (
<h1 onClick={this.changeValue}>test{this.state.b}</h1> 
)

}
changValue=()=>{//必须用箭头函数
this.setState({a:2})//更新为合并，不是替换
}
}
ReactDOM.render(<Demo/>,document.getElementById('test'))
```


##### props
>组件的所有属性都保存在props中，props是只读的，不能修改

```
class Demo extends React.Component{
render(){
return (
<h1 >test{this.props.b}</h1> 
)

}
}
ReactDOM.render(<Demo a={1} b='ss'/>,document.getElementById('test1'))//数据类型要用｛｝
const obj={a:1,b:2}
ReactDOM.render(<Demo {...obj}/>,document.getElementById('test2'))//批量传属性
```

- props规则限制

```

class Demo extends React.Component{
render(){
return (
<h1 >test{this.props.b}</h1> 
)

}
}
//限制类型
Demo.propTypes={
//react16.x以上需引入prop-types.js
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

```

class Demo extends React.Component{
render(){
return (
<h1 >test{this.props.b}</h1> 
)

}

static propTypes={	//加上static代表给类本身添加属性
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


- 函数式组件使用props

```
function Demo(props){
return (
<h1 >test{props.b}</h1> 
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

##### refs
>组件内的标签可使用ref属性标识自己类似id='test'

```
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

##### 绑定事件

1. 通过onXxx属性指定事件处理函数（注意大小写）
   - react使用的自定义事件（原生小写->大写，原生大写->小写），而不是原生DOM事件
   - react中的事件是通过事件委托方式处理的（委托给组件最外层的元素,事件冒泡）
2. 可通过event.target得到的发生事件的DOM元素对象


#### 生命周期
##### 旧

- 初始化阶段
```
constructor()//构造器
componentWillMount()//组件将要挂载
render()
componentDidMount()//组件挂载完毕
```

- 更新数据时
```
setState()
shouldComponentUpdate()//默认返回true，返回false时不执行更新
componentWillUpdate()//组件将要更新
render()
componentDidUpdate()//组件更新更新完毕
```

- 强制更新时
```
forceUpdate()//不想更改状态中的数据，更新页面
componentWillUpdate()//组件将要更新
render()
componentDidUpdate()//组件更新更新完毕
```

- 父组件渲染
```
componentWillReceiveProps(props)//第一次不调用，接受新的props才调用
shouldComponentUpdate()//默认返回true，返回false时不执行更新
componentWillUpdate()//组件将要更新
render()
componentDidUpdate()//组件更新更新完毕

```

- 卸载组件
```
由React.unmountComponentAtNode()触发
componentWillUnmount()
```
##### 新
>因为新版本的异步渲染，因此componentWillUpdate，componentWillReceive，componentWillMount即将废弃，所以要加上UNSAFE_防止误解和滥用
- 初始化阶段
```
constructor()//构造器
//UNSAFE_componentWillMount()
static getDerivedStateFromProps(props,state){//组件将要挂载，不能加到实例上，要加上static。
//必须返回状态对象，若包含state中的数据，将覆盖它。用于state的值任何时候取决于props
return {a:1}
}
render()
componentDidMount()//组件挂载完毕
```

- 更新数据时，调用setState()、forceUpdate()或父组件新Props
```
static getDerivedStateFromProps(props,state)//组件将要挂载，不能加到实例上，要加上static。
shouldComponentUpdate()//默认返回true，返回false时不执行更新
static getSnapshotBeforeUpdate(preProps,preState){//更新前

return 'xxx'

}
render()
componentDidUpdate(preProps,preState,snapshotValue)//之前的prop时和状态
```


- 卸载组件
```
由React.unmountComponentAtNode()触发
componentWillUnmount()
```

#### react 中的key作用

key是虚拟Dom对象的标识，当状态中的数据变化时，react会根据新数据形成新的虚拟DOM，
随后将新的虚拟DOM与旧的虚拟DOM进行diff比较。
若旧虚拟dom中到了与新虚拟dom中相同的key：
- 虚拟dom中内容没有变化，使用之前的真实dom
- 虚拟dom中内容变化了，则生成新的真实dom，随后替换掉页面中的真实dom

若旧虚拟dom未找到与新虚拟dom相同的key，则创建新的真实dom渲染到页面中

用index做key引发的问题
1. 若对数据进行逆序添加，逆序删除等破坏顺序操作时，产生没必要的真实dom更新，虽然界面没有问题
2. 若结构中包含输入类的DOM，会产生错误DOM更新，界面会出现异常
3. 若仅用于渲染列表展示，没有逆序添加，逆序删除等破坏顺序操作，则用index做key没有任何问题

 
#### 脚手架

安装：`npm i create-react-app -g`

创建文件夹：`create-ract app test`

进入创建的文件夹：`npm start`

- 入口文件index.js
```
//核心库
import React from 'react'
//渲染
import ReactDOM from 'react-dom'
import App from './App'
ReactDOM.render(<App/>,document.getElementById('root'))
```

- App.js
```
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
```
.title{color:red}
```

```
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
```
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
第三方库pubsub.js


#### 路由
>依赖H5 BOM浏览器对象上的history或hash锚点，监听路由变化，匹配对应的组件
>BrowserRouter使用的h5的histrory API,不兼容IE9以下版本
>HashRouter使用的URL的哈希值

- react-router-dom
```
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
>接受的props默认有history，location，match
```
<Route path="/test" component={Test}>
```
- 一般组件
props传什么有什么
```
<Test/>
```

- NavLink
>可以通过activeClassName指定样式名,标签体内容是个特殊属性，通过this.props.children获取
```
<NavLink activeClassName="active_color">test
</NavLink>
<NavLink activeClassName="active_color" children='test'>
</NavLink>
```
- Switch
>路由默认会全部匹配完，加上switch可实现单一匹配，提高效率
```
<Switch>
<Route path="/test" component={Test}>
<Route path="/test" component={Test}>
//默认模糊匹配,exact严格匹配，开启后无法匹配二级路由
<Route exact path="/test" component={Test}>
</Switch>
```
- Redirect
>重定向，写在路由最下方，当路由都不匹配时，跳转到redirect指定的路由
```
<Redirect to="/test" >
```

- 二级路由

```
<Switch>
<Route path="/test" component={Test}>
<Route path="/test/a" component={Test}>
<Redirect to="/test" >
</Switch>
```
- 路由参数
```
//params参数，this.props.match.params
<HashRouter>
<Link to=`/test/${123}/${123}`></Link>
<Route path="/test/:id/:count" component={Test}>
</HashRouter>
```

```
//search参数(为urlencoded编码字符串，需借助querystring解析)，this.location.match.search
import qs from 'querystring'
<HashRouter>
<Link to=`/test?a=${123}&b=${123}`></Link>
<Route path="/test" component={Test}>
//qs.parse(this.props.location.search)
</HashRouter>
```

```
//state参数(不在地址栏显示，browser刷新页面不丢失，hashRouter刷新丢失)，this.props.location.state
<HashRouter>
<Link to={{pathname:'/test'state:{a=123,b=123}}}></Link>
<Route path="/test" component={Test}>
</HashRouter>
```

- 编程式路由

```
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
>加工一般组件，让一般组件用上路由api
```
import {withRouter} from 'react-router-dom'

class Test extends Component {}

export default withRouter(Test)//返回的是全新的组件
```

#### React Router 6

>移除<Switch/> ，新增<Routers/>
>移除<Redirect/> ，新增<Navigate/>
>component={About}变为element={<About/>}
>增加hooks

- Routers
>必须用Routers包裹，不会匹配多个
```
import {Routers,Route} form 'react-router-dom'
<Routers>
<Route path="/test" element={<Test/>}>
<Route path="/TEST" caseSensitive element={<Test2/>}>//区分大小写
</Routers>
```

- Navigate
>重定向,只要被渲染，就会切换路由
```
import {Routers,Route,Navigate} form 'react-router-dom'
<Routers>
<Route path="/test" element={<Test/>}>
<Route path="/" element={<Navigate to="/test"  />}>
<Route path="/" element={<Navigate to="/test" replace />}>
</Routers>
```

- NavLink

```
<NaviLink className={()=>return 'activeColor'}></NaviLink>
```

- 路由表

```
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

```

// 若要显示子路由，需在父级路由组件中引入Outlet！！！
render(){
return (
<div>
<NavLink end to="new"></NavLink>//end代表匹配子路由时，自己失去高亮
{/*指定路由组件呈现的位置*/}
<Outlet />
</div>
)}

```

- 路由参数
```
//params参数，this.props.match.params
<Link to=`/test/${123}/${123}`></Link>
...

import {useParams,useMath} from 'react-router-dom'
export default function Demo(){
const {a,b}=userParams()
const {a,b}=userParams(/test/:a/:b})
return ({a+b})
}
```

```
<Link to=`/test?a=${123}&b=${123}`></Link>
import {useSearchParams,useLocation} from 'react-router-dom'
export default function Demo(){
const [search,setSearch]=userSearchParams()
const a=search.get('a')
let setData=()=>{setSearch(1234)}//更新a
const a1=useLocation()
return ({a})
}
```

```
<Link to='/test' state={{a=123,b=123}}></Link>
import {useLocation} from 'react-router-dom'
export default function Demo(){
const obj=useLocation()
return ({obj.a})
}
```
- 编程式路由

```
import {useNavigate} from 'react-router-dom'
const navigate=userNavigate()
goto(){
navigate('/test',{
replace:false,
state:{}
})
navigate(1)//前进
navigate(-1)//后退
}
```


#### redux

>状态管理js库，集中式管理react应用中多个组件共享的状态

#####  简单
- store.js
```
import {createStore}form 'redux'
import countReducer from './countReducer'
const store=createStore(countReducer)
export default store
```

- countReducer.js
```
//创建一个为count组件服务的reducer，reducer本质是一个函数
第一次调用时是store自动触发的，传递的preState是undefin
export default function (preState,action){
const {type,data}=action
return preState
}
```


- test.jsx
```
import store from '../store/store.js'

componentDidMount(){
store.subscribe(()=>{
this.setState({})
})
}

increment(){
store.dispatch({type:'xx",data:123})
}

render(){
return （<div>{{store.getState()}}</div>）
}
```
##### 完整
- countAction.js
```
//创建一个为count组件服务的action，
export  function test(data){
return {type:'xxxx',data}
}
```


- countReducer.js
```
//创建一个为count组件服务的reducer，reducer本质是一个函数
//第一次调用时是store自动触发的，传递的preState是undefin
//必须是纯函数（同样输入必定得到同样输出）不要在reducer中发起网络请求、改写prestate、使用Date.now() , Math.random()
export default function (preState,action){
const {type,data}=action
switch(type){
case 0:
return preState//当preState没有变化时，不会引起页面刷新（浅比较）
case 1:
return preState+1
}
}
```
- constants.js
```
export const ADD='add'
```


- test.jsx
```
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

##### 异步action
>异步action指action返回值为函数，同步action指action返回值为对象
- store.js
```
import {createStore,applyMiddleware}form 'redux'
import countReducer from './countReducer'
//引入异步action中间件
//yarn add redux-thunk
import thunk  from 'redux-thunk'
const store=createStore(countReducer,applyMiddleware(thunk))
export default store
```


- action.js
```
import store from './store'
export const add=data=({type:'add',data})//同步action
export const addAsync=(data,a,b,c)=>{
return (dispatch)=>{
setTimeout(()=>{
dispatch(add(data)})
},50000)
}
}

```

##### react-redux
>UI组件应该包裹一个容器组件，它们为父子关系
>UI组件不能使用任何redux api，容器组件可以
>容器给UI组件通过props传递状态、操作方法
>不用在写store.subscribe()

- 容器组件
/containers/test/index.jsx
```
//yran add react-redux
import testUI from '../../components/test.jsx'
//引入connect用于连接UI组件与redux
import {connect} from 'react-redux'

let mapStateToProps=(state)=>{
//通过props传给UI组件状态
return {
a:1,b:state
}}

let mapDisoatchToProps=(dispatch)=>{
//通过props传给UI操作方法
return {
d:(data)=>{
dispatch({type:'xxx',data})
},
c:()=>{}
}}


const testContainer=connect(mapStateToProps,mapDisoatchToProps)(testUI)

export default testContainer
```

App.jsx
```
import store form '../../redux/store.js'
import Test from '../../containers/test'

render(){
return (
<Test store={store}/>
)
}
```


简写
```
import {createIncreamentAction} from '../store/action.js'
const testContainer=connect(
state=>{a:state},
{
add:createIncreamentAction
}
)(testUI)
```

##### provide

index.js
```
import store from './store'
import {Provider} from 'react-redux'
//自动匹配容器组件，添加上store
ReactDOM.render(<ProVider store={store}><App/></ProVider>,document)
```

##### 整合容器和UI组件
```
import {createIncreamentAction} from '../store/action.js'

class Test extend Componet{}
export default connect(
state=>{a:state},
{
add:createIncreamentAction
}
)(Test)
```

##### 多个reducer
- store.js
```
import {createStore，applyMiddleware,combineReducers}form 'redux'
import countReducer1 from './reducer/count1'
import countReducer2 from './reducer/count2'
let allReducer=combineReducers({
count1:countReducer1
count2:countReducer2
})
export default createStore(allReducer)
```




##### 解构赋值

```
let obj={a:{b:1}}
const {a}=obj;
const {a:{b}}=obj;//连续解构赋值
const {a:{b:value}}=obj;//连续解构赋值+重命名 

```

##### 高阶函数
- 若a函数，接收的参数是一个函数，那么a就可以称为高阶函数
- 若a函数，调用的返回值依然是一个函数，那么a就可以称为高阶函数
如promise setTimeOut arr.map()

 
```
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

```
test=(type){
return (e)=>{
arr.push( {[type]:e.target.value})
}
}
```

```
function sum(a,b,c){return a+b+c}

function sum2(a){
return (b)=>{
return (c)=>{
return a+b+c
}
}
}
sum(1)(2)(3)
```

#### 前端发请求

#####　xhr
- jquery
- axios（node使用axios是封装http协议）
##### fetch
>原生函数,老版本兼容性差

fetch('xxx').then(res=>{
//联系服务器成功
return res.json()
},err=>{return new Promis(()=>{})}).then(res=>{},err=>{})

##### setState
写法
```
state={a:1}

对象式setState
this.setState({a:2})
this.setState({a:2},()=>{
//状态更新以及页面也更新后，才调用
})

函数式setState
this.setState((state,props)=>{
return {a:1}},()=>{})

```

#### lazyLoad
>路由组件懒加载
```
import {lazy,COmponent,Suspense} form 'react'
const Home=lazy(()=>{import ('./Home')})
export default class Demo extend Component{

<Suspense fallback={<Loading/>}>
<Route  path="/home" component={Home}>

</Route>
</Suspense>

}

```

#### Hooks
>hook是16.8的新特性，可以让你在函数组件中使用state以及其他react特性

##### state
```
import React form 'react'
function Test(){
const [a,set]=React.useState(123) //初始化会调用一次，下次调用时，会缓存数据，不会覆盖
let add1=()=>{set(456)}
let add2=()=>{set((a)=>{return a+1})}
return (<div>a</div>)}
```

##### effect
>函数组件里使用生命周期
```
import React form 'react'
function Test(){
const [count,setCount]=React.useState(123) 

React.userEffect(()=>{},[a])//检测a,改变时调用------
React.userEffect(()=>{},[])//谁也不监测,只调用一次---- componentDidMount
React.userEffect(()=>{})//检测所有,改变时调用----componentDidUpdate
React.userEffect(()=>{return ()=>{组件卸载前执行}},[])//componentWillUnmount

return (<div>a</div>)
}
```
##### ref
```
import React form 'react'
function Test(){
const myRef=React.useRef() 
function show(){myRef.style.color='red'}
return (<div ref={myRef}>a</div>)
}
```

#### Fragment
>可以不用必须有一个真实DOM根标签，编译后会移除
```
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
>常用与祖组件和后代组件通信
>一般不用Context,而是它的封装react-redux

类式组件
```
const CountContext=React.createContext()//创建一个上下文,必须所有组件都访问得到
class A extends Component{
state={a:1}
render(){
return (<CountContext.Provider value={this.state.a}><B/></CountContext.Provider>)}
}
class B extends Component{
render(){
return (<C/>)}
}
class C extends Component{
static contextType=CountContext//声明接收context
render(){
return (this.context)}
}
```
- 所有组件
```
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
>只要执行setState即使不更新数据，组件也会重新render9)
>只要render()调用，其子组件也会更新
>原因：shouldComponentUpdate()默认总是为true

##### 解决
- 手动比较新旧值,不一样更新，否则禁止更新
```
shouldComponentUpdate(nextProps,nextState){//新的
console.log(this.props,this.state)//旧的
return true
return false
}
```

```
import {PureComponent} from 'react'

class Test extends PureComponent {
//依赖的数据不更新时，不重新render

const obj=this.state
obj.name='xx'
this.setState(obj)//浅比较，数据不会更新

}

```

#### render Props
>vue中的插槽
```
class A extends Component{
state={a:1}
render(){
return (
<B>aaa</B>
 <B render={(val)=>{<C b={val} />}/>
)}
}
class B extends Component{
state={b:1}
render(){
return (
<span>{this.props.children}</span>
this.props.render(this.state.b)
)}//aaa
}
function C(){
static contextType=CountContext//声明接收context
render(){
return (<div><Consumer>{value=>{return (<span>value</span>)}</Consumer></div>)}
}
```


#### ErrorBoundary错误边界
>防止子组件出错导致整个页面出错,只能在生产环境使用
>只能捕获后代组件生命周期产生的错误，不能捕获自己的
```
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
```
children props
render props
```
- 消息订阅-发布
```
pubs-sub event
```
- 集中式管理
```
redux、dva
```
- conText
```
生产者-消费者模式
``

- 搭配
父子：props
兄弟: 集中式管理、消息订阅发布
祖孙: 集中式管理、消息订阅发布、context（开发用的少，封装插件用得多）


### 关闭eslint
第一步：执行以下命令：
`npm run eject`
第二步：在package.json 中修改代码

```
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

