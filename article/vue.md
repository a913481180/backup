--- 
title: Vue2.0+Vue3.0
date: 2021-01-01 20:11:33
catergories:
- study
---

# Vue2.0

一套用于构建用户界面的渐进式javascrip框架

```
<!DOCTYPE html>
<html lang="en">
        <head>
                <meta charset="utf8">
                <title>vue</title>
                </head>
                <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"> //引入 Vue</script>
                <script>
                window.onload=function(){
        Vue.config.productionTip=false;         //阻止vue启动时的生成的提醒
        ///创建vue实例
       const vu= new Vue({
        el:'#root',     //指定容器,可写类名，第二种写法可用vu.$mount('#root');代替
        data:{
                msg:'hello',
                name:'xiaomi',
                }
	//data第二种写成函数式,由Vue管理的函数不能写成箭头函数，对象里的方法可省略:function
	/*
	data:function(){
	return{
		name:'xiaoming'
	}
	}
*/
                });
        }
                </script>
                <body>
                <div id='root'>{{msg}} {{name}} </div>  <!-- {{}}中可写js表达式-->
                </body>
</html>
```

- MVVM模型
一种软件架构

   - M:模型model:对应data中的数据
   - V：视图view:模板   
   - VM：视图模型view model：vue实例

- 数据代理

通过一个对象对另一个对象中的属性的操作
`Object.defineproperty()`第一个参数为对象，第二个为对象的属性，第三个为配置项

如添加属性：

```
let person{
name:'xiaomei'
}
Object.defineproperty(person,'age',{
value:18,
//默认情况下添加的属性不可枚举，即无法遍历出来
enumerable:true		//true表示可以枚举
//默认情况下添加的属性的值无法更改
writable:true	//控制属性是否可修改
//默认情况下添加的属性无法删除
configurable: true	////控制属性是否可删除


//当读取person的age属性时，get函数会被调用，且返回age的值
get:function(){
return 'hello'
}
//当修改person的age属性时，set函数会被调用
set:function(value){
age=value;
}
})
```

vue中的数据代理：通过vm对象来代理data对象中属性的操作

通过Object.defineProperty()把data对象中所有属性添加到vm上，为每一个添加到vm上的属性，都指定一个getter/setter方法,在它们内部去对data中的数据进行操作

- 数据监听


通过`vm._data`也可以访问到data中的数据,但其是经过加工的,即形成getter、setter形式

示例：
```
//###只能监测一层
let data={name:'xiaomei'}
//创建一个监视的实例对象，用于监视data中的属性变化
const obs=new Observer(data);
function Observer(obj){
//汇总对象中所有的属性形成一个数组
const keys=Object.keys(obj);
//遍历
keys.forEach((k)=>{
Object.defineProperty(this,k,{
get(){
return obj;
},
set(val){
console.log('数据被修改了');
}
})
});
}
```

原理：通过setter实现监测，且要在new Vue时就传入要监测的数据;

   - 对象中后追加的属性，vue默认不做响应式处理；如要给后添加的属性做响应式处理，需要用` Vue.set(对象,'添加的属性','属性值')或vm.$set(对象,'添加的属性','属性值')`

   - 数组更新检测
	vue将被侦听的数组的变更方法进行了包裹，所以会触发视图的更新，即先调用原生方法对数组进行修改，接着重新解模板，进行更新页面。被包裹的方法有`push()`,`pop()`,`shift()`,`unshift()`,`splice()`,`sort()`,`reverse()`.或使用` Vue.set(数组,下标,'修改的值')或vm.$set(数组,下标,'修改的值')`

>!注意：Vue.set();和vm.$set()不能给vm或vm的根数据对象添加属性
   



##模板语法

- 插值语法
用于解析标签体内容
`{{xxx}}`xxx为js表达式，可以直接读取到data中的属性

- 指令语法
用于解析标签

`v-bind:href="xxx"`或简写为`:href="xxx"`

   - 数据绑定

       - 单向数据绑定
数据只能从data流向页面;`v-bind:href="xxx"`或简写为`:href="xxx"`
	
       - 双向数据绑定

数据能从data流向页面,也能从页面流向data
`v-model:value="xxx"`或简写`v-model="xxx"`

>v-model指令只能用在表单类（输入类input、select）元素上

```
<!--收集表单数据-->
<input type="text"  v-model="keyWords/>	<!--用户输入就是value值，v-model收集的就是value值;v0model的三个修饰符：lazy失去焦点在收集数据，number输入的字符串转为有效数字，trim去除首尾的空格-->
<input type="radio" v-model="radio" value="radio1" /> <!--单选框-->
<input type="checkbox" v-model="hobby" value="study"/> <!--多选框-->
<input type="checkbox" v-model="hobby" value="reading"/> <!--若没有配置value，则当v-model初始值为数组时，勾选时收集的是null，为字符串时，收集的是布尔值-->
<input type="checkbox" v-model="hobby" value="sports"/>

new Vue({
el:'#root',
data:{
	keyWords:'',
	hobby:[],
}
});
```


   - 数据处理

```
<button v-on:cllick="showhello">test</button>
<!--或简写成-->
<button @cllick="showhello($event,22)">test</button>

<script type="text/javascript">
new Vue({
el:'.button1',
data:{
name:'xiaomei'
},
methods:{
showhello(e,n){
alert('hello');
console.log(e.target,n);
}
});
</script>
```

   - 事件修饰符
vue中的事件修饰符：
      - prevent:阻止默认事件
      - stop:阻止事件冒泡
      - once：事件只触发一次
      - capture:使用事件的捕获模式
      - sefl:只有event.target是当前操作的元素时才触发事件
      - passive:事件的默认行为立即执行，无需等待事件回调执行完毕


```
<a href='www.baidu.com' v-on:click.prevent.stop="showhello()">test<a>	<!--阻止a标签自动跳转,可连续写-->
```

   - 键盘事件
      - vue中的按键别名`回车enter`,`删除delete` , `退出esc`, `空格space`, `换行tap(必须配合keydown使用)`, `上up下down左left右right` 
      - vue中未提供的按键别名，可使用按键原始名去绑定，注意要转换为kebab-case（短横线命名）
      - 系统修饰键：`ctrl`,`alt`,`shift`,`meta`;
         - 配合keyup使用时：需再按下其他键，随后释放其他键才能触发
         - 配合keydown使用时：正常触发
      - Vue.config.keyCodes.自定义键名=键码

`<input type="text" placeholder="tips" @keyup.enter="showhello">`

   - 条件渲染

      `<h2 v-show="false"> test</h2>	<!--元素还存在-->`
	
```
   <h2 v-if="false"> test</h2>	<!--元素直接消失,中间不能被打断-->
   <template v-else-if="true"> <a>test</a></template>	<!--template不影响结构，但必须和v-if配合使用-->
   <h2 v-else> test</h2>	
```        

   - 渲染文本

   `v-text` 指令,向其所在的节点中渲染文本内容,其会替换掉节点中的内容，`{{xxx}}`插值语法则不会。
```
<h3 v-text="keyword"></h3>
```

   `v-html` 指令,向其所在的节点中渲染文本内容,其会替换掉节点中的内容,且会解析html标签，`v-text`则不会。其具有安全性问题，在网页上动态渲染html标签是非常危险的，易导致XSS攻击。

   - 列表渲染

```
<div id="#root"> 
<ul>
<!--遍历数组-->
<li v-for="n in persons" :key="n.id" > 
{{index}}	<!--索引值-->
</li>
</ul>

<!--遍历对象-->
<li v-for="(k,index2) of persons" :key="index2" > 
{{index2}}	<!--索引值-->
</li>
</ul>
</div>
<!-- 除此之外还可以遍历字符串，数字-->

<script>
const vm=new Vue({
el:'#root',
data:{
	persons:[
		{id:001,name:'xiaoMei',age:18},
		{id:002,name:'xiaoHong',age:22},
		{id:003,name:'xiaoLan',age:12},
		]
},
});
</script>
```

*key原理*
 	
给节点提供唯一标识
1. 虚拟DOM中的key的作用：key是虚拟DOM对象的标识，当数据发送变化时，vue会根据新数据生成新的虚拟DOM，随后vue将新的虚拟DOM与旧的虚拟DOM进行差异比较
2. 对比规则为：
   - 旧虚拟DOM中找到与新虚拟DOM相同的key：   
        - 若虚拟DOM中的内容没有改变，则直接使用之前的真实DOM
         - 若虚拟DOM中的内容改变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM
   - 旧虚拟DOM中未找到与新虚拟DOM相同的key：创建新的真实DOM，随后渲染到页面。
      3. index作为key可能发生的问题：对数据进行逆序添加、删除时，会产生没有必要的真实dom，界面虽然没问题，但效率低。当结构中存在输入类的DOM时，界面会出现问题。

---

   - 其他指令

   `v-cloak`指令，Vue实例创建完毕并接管容器后，会删除所有v-cloak属性，常用于解决网速不好加载vue慢而显示{{xxx}}问题

```
<style>
[v-cloak]{display:none;}
</style>
<h2 v-cloak> {{keyword}}</h2>

```

   `v-once`指令，其所在的节点在初次渲染后，就视为静态内容了，以后即使数据变化了，其也不再变化

```
<h2 v-once> {{keyword}}</h2>

```

   `v-pre`指令，跳过其所在节点的编译过程，可用于跳过没有指令语法、插值语法的节点，加快编译速度。
   

   - 自定义指令

```
new Vue({
el:'#root',
data:{},
directives:{ 
//局部指令
//指令与元素成功绑定时即一开始就被调用，当指令所在模板被重新解析时会被调用。
//简写
test(element,binding){	//element,binding为规定写法
console.log(this);	//这里的this为window
element.innerText=bindding.value;
}
//标准写法
test2:{	//命名：多个单词用-分割，使用时不用-,后一个首字母改为大写
bind(element,binding){//指令与元素成功绑定时调用},
inserted(element,binding){//指令所在元素插入页面时调用},
update(element,binding){//指令所在模板被重新解析时被调用}
}
}
})
//全局指令
Vue.directive(指令名，回调函数);
;
```


	
	

## 计算属性

内部有缓存机制，效率更高,不能开启异步任务如:`setTimeout()`

```
new Vue({
el:'.box',
data:{
name:'xiaomei',
hoby:'reading'
},
computed:{
fullName:{
//初次读取fullName时，get会被调用;所依赖的数据改变时会调用
get(){
return this.name+'-'+this.hoby;
},
//当fullname被修改时，会被调用
set(){}
}
}

/*
//当数据只读不修改时，可采用简写
fullName:function(){
return this.name+'-'+this.hoby;
}
*/

});
```


## 侦听属性

能开启异步操作如:`setTimeout(()=>{},1000)`;所有不被vue所管理的函数最好写成箭头函数，这样this的指向才是vm

```
<script>
const vm = new Vue({
	el:'#test',
	data:{
	istrue: false	
	number:{
		a:1,
		b:2},
	},
	methods:{},
	computed:{},
	wantch{
	'istrue':{
	immediate:true,	///初始化时调用一下handler
	//当istrue发生改变时调用	
	handler(newValue,oldValue){
	console.log(newValue,oldValue);	
	}
	}	
	//简写，当只有handler()时
	/*istrue(newValue,oldValue){console.log('test');}*/
	'number':{
	deep:true,	//深度监视，当对象内的属性发生变化时,能检测到多层级内容的改变
	handler(newValue,oldValue){
	console.log(newValue,oldValue);	
	}}
 });
/*//第二种写法
vm.$watch('istrue',{ handler(){} } );
//简写
vm.$watch('istrue',function(){ console.log('test');});
*/
</script>
```


## 过滤器

对要显示的数据进行特定格式化后在显示（常用于一些简单逻辑的处理）

```
<script>
new Vue({
el:'#root'
data:{
time:24356765432,
},
computed:{},
//注册过滤器
filters:{
timeFormater1(value){
console.log(value);
return value;	//返回数据
},
timeFormater2(value){
console.log(value);
return value;	//返回数据
},
}
});

//注册全局过滤器
Vue.filter('myName',function(value){return value;});
</script>

<body>
<h3>
<!--使用过滤器-->
{\{time|timeFormater1()|timeFormater2}}
<!--或者v-bind:属性="xxx|过滤器名"-->
<!--过滤器可额外接收参数，多个过滤器可串联，原始数据未改变，产生的是新的数据-->
</h3>
<body>
```

## 绑定样式

适用于动态指定类名
`<div class='box' :class='xxx' :style="{fontSize: n +'px' }" >test</div>`;xxx可为字符串、数组['box1','box2']、对象{box1:false,box2:true}。



## 生命周期

又名生命周期回调函数、生命周期函数、生命周期钩子，是vue在关键时刻帮我们调用的一些特殊名称的函数，其名字不可更改，其中的this指向vm或组件实例对象。

|生命周期|说明|
|-|-|
|初始化：生命周期、事件|但数据代理还未开始|
|beforeCreate|无法通过vm访问到data中的数据已经methods中的方法|
|初始化：数据检测、数据代理||
|created|可以通过vm可访问到data中的数据、methods中的方法|
|解析模板，生成虚拟DOM|此时页面还不能显示解析好的内容|
|beforeMount|页面呈现的是未经Vue编译的DOM结构，此时所有对DOM的操作，最终都不奏效|
|创建vm $el|将内存中的虚拟DOM转为真实DOM插入页面|
|mounted|页面中呈现的是经过Vue编译的DOM，对DOM的操作均有效，到此初始化过程结束。一般在此进行开启定时器、发送网络请求、订阅消息、绑定自定义事件等操作|
|beforeUpdate|此时数据是新的，但页面是旧的，即页面未与数据保持同步|
|根据新的数据，生成新的虚拟DOM，随后与旧的虚拟DOM进行比较，最终完成页面的更新|
|updated|此时数据和页面保持同步|
|beforeDestroy|此时vm中所有的data、methods、指令等都处于可用状态，马上要执行销毁过程，一般在此进行关闭定时器、取消订阅消息、解绑自定义事件等操作|
|destroyed|销毁vue实例后自定义事件会失效，但原生DOM事件依然有效|

## 组件

实现应用中局部功能代码和资源的集合

>模块：向外提供特定功能的js程序

- 非单文件组件：一个文件中包含有n个组件

```
创建school组件
//简写
const school={};
//标准写法
const school=Vue.extend({
//组件定义时，不要写el配置项,最终所有的组件都要进过一个vm的管理
//名字
name:'xuexiao',

//使用template可以配置组件结果。
template:`
<div>
<h3>test</h3>
</div>
`,

data(){	//data必须写成函数，为避免组件被复用时，数据存在引用关系
return {
	schoolName:'BST',
	address:'beijing'
}
}
});

//定义app组件
const app=Vue.extend({});

//创建vm
new Vue({
el:'#root',
template:'<app></app>',
//局部注册组件
components:{
	xuexioa:school,		//组件名：一个单词通常首字母大小，多个单词通常用-分隔，或首字母大写（需要Vue脚手架支持）
	app
}
});

//全局注册组件
Vue.component('xuexiao',school);

//使用组件
<xuexiao></xuexiao>
<xuexiao /><!--需要脚手架支持，否则多个组件时，后续组件将不能渲染-->
```

关于VueComponent，组件本质是一个名为VueComponent的构造函数，是由Vue.extend生成的。当我们写<xuexiao></xuexiao>，Vue解析时会帮我们创建school组件的实例对象，即vue帮我们执行`new VueComponent();`。当我们每次调用Vue.extend()时,返回的都是一个全新的VueComponent。在组件配置中，data函数、methods中的函数、watch中的函数等，它们的this都指向VueComponent实例对象。

```
//定义一个构造函数
function Demo(){
this.a=1;
this.b=2;
}
//创建一个Demo实例对象
const d=new Demo();
console.log(Demo.prototype);	//显示原型属性
console.log(d.__proto__);	//隐式原型属性
//显示原型属性与隐式原型属性都指向原型对象
//通过显示原型属性操作原型对象
Demo.prototype.e=21;
```

>!!重要内置关系：VueComponent.prototype.__proto__===Vue.prototype;其可以让组件实例对象访问到Vue原型上的属性和方法。

- 单文件组件：一个文件中只包含有一个组件
   - school.vue组件
```
<template>
<!--组件的结构-->
	<div class="demo">
	<h2>{{schoolName}}</h2>
	<h2>{{address}}</h2>
	</div>

</template>

<script>
//组件交互的代码
	export default{
	name:'School',
	data(){
	return{
	  schoolName:'daxue'
	  address:'beijing'
		}
	},
	methods:{
	   showName(){
		alert(this.schoolName);
		}
	},
}
</script>

<style>
/*样式*/
.demoo{
background-color:red;
}
</style>
```
   - App.vue组件
```
<template>
<div>
<School/>
</div>
</template>

<script>
//引入组件
import School from './School'

export default{
name:'App',
components:{School}
}
</script>

<style>
</style>
```
   - main.js
```
import App from './App.vue'
new Vue({
el:'#root',
components:{App},
});
```
   - index.html
```
<!DOCTYPE html>
<html>
<head>
	<meta charset='utf-8"/>
	<title>test</title>
</head>
<body>
<div id="root"><div>
<script type="text/javascript" src="./vue.js"></script>
<script type="text/javascript" src="./main.js"></script>
</body>
</html>
```


## 脚手架vue.cli(command line interface)

1. 全局安装@vue/cli:`npm install -g @vue/cli`
2. 切换到要创建项目的目录，然后创建项目：`vue create xxxx`
3. 启动项目：`npm run serve`

>1. 配置npm淘宝镜像：`npm config set registry https://registry.npm.taobao.org`
>2. Vue脚手架隐藏了所有webpack相关的配置，若想查看具体webpack配置，执行：`vue inspect > output.js`


- main.js项目的入口文件
```
//引入vue.runtime.xxx.js，其是运行版的vue,只包含核心功能，没有模板解析器,而完整版的Vue才有。
import Vue from 'vue'		
//引入App组件
import App from './App.vue'
//关闭生产提示
Vue.config.productionTip=false;

//创建VUe实例对象
new Vue({
el:'#root',
//将App组件放入容器中
render:h=>h(app),	//没有模板解析器，不能使用template配置项，需要使用render函数接收到的createElement函数去指定具体内容
});
```

- vue.config.js配置

使用vue.config.js可以对脚手架进行个性化定制
```
module.exports={
	pages:{
	  index:{
		//入口
		entry:'src/main.js',
		}
		},
	lintOnSave:false	//关闭语法检查
}
```

- 脚手架文件结构

|--node_modules
|--public
|--src
    |--assets:静态资源文件夹
    |--component:存放组件文件夹
    |--App.vue：汇总所有组件
    |--mian.js：入口文件
|--.gitignore: git版本管制忽略的配置
|--babel.config.js 
|--package.json: 应用包配置文件
|--package-lock.json: 包版本控制文件


- ref属性

id的替代者,用来给元素或组件注册引用信息,应用在html标签上获取的是真实DOM元素，应用组件标签上获取的是组件的实例对象

```
<template>
<div>
<h1 v-text="msg" ref="title"></h1>
<button ref="btn" @click="showDOM">outPut_DOM</button>
<School ref="sch" />
</div>
</template>

<script>
import School from './component/school'
export default {
	name:'App',
	components:{School}
	data(){
	return{
	msg:'hello'
	}
	},
	methods:{
	showDOM(){
	console.log(this.$ref.title);	//真实DOM元素
	console.log(this.$ref.sch);	//School组件的实例对象
}
}
}
</script>
```

- props配置项

让组件接收外部传来的数据,props是只读的，Vue底层会监测(非深度监测)props，若发生修改会发出警告。若要修改数据，请复制一份到data中,再修改。

>v-modle绑定的值不应是props传过来的值，当其为对象类型时，修改对象中的属性时，vue无法发现

子组件
```
new Vue({
data:{},
//简单声明接收
props:['name','age','sex']
//接收的同时对数据进行类型限制
props:{
name:String,
age:Number,
sex:String
}
});
//接收的同时对数据进行类型限制和默认值的指定
props:{
name:{
	type:String,
  	required:true,	//name必填
		},
	}
```

父组件
```
<template>
<div>
	<!--传递数据-->
	<Student name="xiaoMei" sex="female" :age="12"
</div>
</template>
<script>
import Student from './components/Student'
export default{
	name:'App",
	components:{Student}
}
</script>
```

>props适用于父组件和子组件的通信，子组件与父组件的通信要求父先给子一个函数，子再通过形参的形式传递数据


## 自定义事件

适用于子组件与父组件通信
App.vue
```
<template>
<!-- 通过父组件给子组件绑定一个自定义事件：用于子给父传递数据 -->
<Student v-on:test="getStudentName"/>
<!-- 第二种写法,使用ref-->
<!-- <student ref="student"/>  -->
<!-- <student ref="student"  @click.native="show"/>  组件绑定原生DOM事件，需要使用native修饰符 -->
</template>

<script>
import School from './components/School.vue'
import Student from './components/Student'

export default{
name:'App', components:{School,Student}
data(){
	return {
	   msg: 'test1!!!!!!!!'
	}}
methods:{
	getStudentName(name){
		console.log(name);
		}
	},
	// 第二种写法,使用ref-->
	/*
	mounted(){
	this.$refs.student.$on('test',this.getStudentName);
	//this.$refs.student.$on('test',(name,...params)=>{...//要用箭头函数，否则this指向出错});
	}
	*/
}
</script>

<style>
</style>
```

Student.vue
```
<template>
<h2>{{name}}</h2>
<h2>{{sex}}</h2>
<button @click="sendStudentName">test</button>
</template>
<script>
export default {
name:'Student'
data(){
return{
name:'xiaoHu',
sex:'man'
}
},
methods:{
sendStudentName(){
//触发student组件实例身上的test(getStudentName)事件;即this.$emit('传过来的事件名',参数数据）；
this.$emit('test',this.name,xx,32);
}
unbind(){
//this.$off('demo1');	//解绑一个自定义事件
//this.$off({'xx','xx','xx'});	//解绑多个自定义事件
this.$off();	//解绑所有自定义事件
},
death(){
this.$destroy();	//销毁当前student组件的实例，销毁后所有student实例的自定义事件全部失效
}
 }
 </script>
```


## 全局事件总线

适用与任意组件间的通信


main.js
```
import Vue from 'vue'
import App from './App.vue'

new Vue({
	el:'#root',
	render:h=>h(App),
	beforeCreate(){
	   Vue.prototype.$bus=this	//安装全局事件总线
	},
});
```

School.vue
```
<template>
</template>
<script>
export default{
	name:'school',
	data(){
	return{
	name:'xuexiao',
	address:'biejing'
	}
		},
	mounted(){
	//接受数据
	this.$bus.$on('hello',(data)=>{
	console.log('recieve data',data);
	});
	},
	beforeDestroy(){
	//解绑事件
	this.$bus.$off('hello')}
	}
</script>
<style>
</style>
```

School.vue
```
<temlate>
</template>
<script>
export default({
	name:'School',
	data(){
	return{
	name:'小米',
	sex:'woman',
	}
	},
	methods:{
	sendStudentName(){
	//提供数据
	this.$bus.$emit('hello',32)
	}
	},
})
</script>
<style>
</style>
```


##  消息发布和订阅

适用于任意组件间通信

School.vue
```
<template>
</template>

<script>
//安装pubsub: npm i pubsub-js
import pubsub from 'pubsub-js'
export default{
	name:'School',
	data(){
	return{
	name:'xuexiao',
	address:'beijing'
	}
		},
	mounted(){
	//订阅消息，接收数据
	this.pubId=pubsub.subscribe('hello',function(msgName,data){
	console.log('有人发布hello消息',msgName,data);
	console.log(this);
		})
	},
	beforeDestroy(){
	pubsub.unsubscribe(this.pubId);
	}
}
</script>

<style>
</style>
```

student.vue
```
<script>
import pubsub from 'pubsub-js'
export default ({
	name:'Student',
	data(){
	return{
	name:'xiaomei',
	sex:'woman'
	}
	},
	methods:{
	///提供数据
	sendData(){
	pubsub.publish('hello',2222);
	}	
	},
	});
</script>
```

- nextTick

语法：`this.$nextTick(回调函数)`
作用：在下一次DOM更新结束后执行其指定的回调,用于当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行

- mixins(混入)配置项
	
可以把多个组件共用的配置提取成一个混入对象
school组件
```
<template>
<div>
<h2 @click="showName" >{{name}}</h2>
</div>
</template>

<script>
//引用一个test
import{test} from '../mixin'
//import{test,test2} from '../mixn'

export default{
	name:'School',
	data(){
	return {name:'daxue'}	
	},
	mixins:[test]
	//mixins:[test,test2]
}

//全局混入
//Vue.mixin(xxx)
</script>
```

mixin.js
```
//定义混合
export const test={
	methods:{
		showNamw(){alert(this.name);}
		},
	mounted(){}
}
export const test2={
	data(){
	return{
		x:2,
		y:1
		}
		},
	mounted(){}
}
```


- 插件:用于增强Vue

main.js
```
//引入Vue
import Vue from 'vue'
//引入APP
import APP from './App.vue'
//引入插件
import plugins from './plugins'

//使用插件
Vue.use(plugins,33,2);

//创建VM
new Vue({
	el:'#root',
	render:h=>h(App)
	});
```

plugins.js
```
//定义插件
export default{
install (Vue,x,y){
console.log(x,y);
//添加全局过滤器
Vue.filter(....)
//添加实例方法
Vue.prototype.$myMethod=function(){..}
}
}
```


- scoped样式
让样式只在本组件内有效，即局部生效，防止冲突。写法：`<style scoped></style>`

>组件化编码流程：1.拆分成静态组件。2.实现动态组件。3.实现交互即绑定事件

- vue封装的动画与过渡

```
<template>
<div>
<ransition name="hello" appear>
</div>
</template>
<script>
</script>
<style>
/*动画*/
.hello-enter-active{
	animation: test 0.5s linear;
}
.hello-leave-active{
	animation: test 0.3s linear reverse;
}
@keyframes test{}

/*详细写法
/*进入的起点*/
.hello-enter{
	transform:translateX(-100x);
}
/*进入的终点*/
.hello-enter-to{
	transform:translateX(0px);
}
/**/
.hello-leave{
	transform:translateX(0px);
}
/**/
.hello-leave-to{
	transform:translateX(-100px);
}
*/
</style>
```

## vue脚手架配置代理服务


vue.config.js
```
module.export={

	pages:{
		index:{
			//入口
		entry:'src/main.js'
			},
	},
lintOnSave:false,	//关闭语法检测
//开启代理服务器（一）:配置简单，但不能配置多个代理
/*
devServer:{
	proxy:'http://localhost:3333'
}*/
//开启代理服务器（二）:可以配置多个代理
devServer:{
proxy:{
'/api':{	//匹配所有以'/api'开头的请求路径
target:'<url>',
ws:true,	//用于支持websocket
changeOrigin:true	//用于控制请求头中的host值,true时，服务器请求头伪装成localhost：5050，false时为localhost:8080
},
'/test':{
target:'<other_url>'
},
'/test2'{
target:'http://localhost:5000',
pathRewrite:{'^/test2':''},	//清除地址前面的协议域名端口http://localhost:8080
}
}
	}

}
```

App.vue
```
<template>
</template>
<script>
import axios from 'axios'
export default {
	name:'App',
	methods:{
	getStudents(){
	aixos.get('http://localhost:8081/students').then(
	response=>{
	console.log('请求成功',data);	
	},
	error=>{
	console.log('请求fail',message);	
	})
	}
	},
}

</script>
```

## 插槽

用于父组件与子组件的通信

Category.vue
```
<template>
<div class="category">
<h3>{{title}}</h3>
<!--定义插槽-->
<!-- 默认插槽 -->
<slot >默认 当没有传递具体结构时，会显示</slot>
<!-- 具名插槽 -->
<slot name="center">默认 当没有传递具体结构时，会显示</slot>
<slot name="footer">默认 当没有传递具体结构时，会显示</slot>
<!-- 作用域插槽:数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定 -->
<slot :games="test">默认 当没有传递具体结构时，会显示</slot>
<div>
</template>

<script>
export default{
	name:'Category',
	data(){
	return {
	test:['xxx','0989','33']	
	}	
	},
	props:['listData','title']
}
</script>
```

App.vue
```
<template>
<Category>
<img slot="center" src="xxx">
<a slot="footer" href="xxxx"></a>
</Category>
<Category>
<template scope="test1">
<h1>{{test1.test}}</h1>
</template>
</Category>
</template>

```
## webStorage

存储内容大小一般支持5MB左右
浏览器端通过Window.sessionStorage和Window.localStorage属性来实现本地存储。
相关api：
   1. xxx.Storage.setItem('key','value');接受一个键和值作为参数，把键值添加到存储中，若存在则更新。
   2. xxxxStorage.getItem('person');接受一个键名作为参数,返回键名对应的值。
   3. xxxStorage.removeItem('key);接受一个键名作为参数,删除键名和其对应的值。
   4. xxxStorage.clear();清空存储中的所有数据。

注意：1.SessionStorage存储内容会随浏览器窗口的关闭而消失；2.LocalStorage存储的内容，需手动清除才会消失。3.xxxxx.Storage.getItem(xx);若xx对应的值获取不到则返回null；4.JSON.parse(null)的结果依然是null。


## vuex

专门在vue中实现集中式状态（数据）管理的一个vue插件,对vue应用中多个组件的共享状态进行集中式的管理（读与写），也是一种组件间通信的方式，适用于任意组件间的通信。

使用时机：多个组件依赖于同一状态；来自不同组件的行为需要变更同一状态


