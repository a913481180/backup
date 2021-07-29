--- 
title: Vue2.0+Vue3.0
date: 2021-01-01 20:11:33
categroie:
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

数据只能从data流向页面

`v-bind:href="xxx"`或简写为`:href="xxx"`
	
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

    - key原理
 	
给节点提供唯一标识
 
      1. 虚拟DOM中的key的作用：key是虚拟DOM对象的标识，当数据发送变化时，vue会根据新数据生成新的虚拟DOM，随后vue将新的虚拟DOM与旧的虚拟DOM进行差异比较
      2. 对比规则为：
         - 旧虚拟DOM中找到与新虚拟DOM相同的key：   
            - 若虚拟DOM中的内容没有改变，则直接使用之前的真实DOM
            - 若虚拟DOM中的内容改变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM
         - 旧虚拟DOM中未找到与新虚拟DOM相同的key：创建新的真实DOM，随后渲染到页面。
      3. index作为key可能发生的问题：对数据进行逆序添加、删除时，会产生没有必要的真实dom，界面虽然没问题，但效率低。当结构中存在输入类的DOM时，界面会出现问题。



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
{{ time|timeFormater1()|timeFormater2}}
<!--或者v-bind:属性="xxx|过滤器名"-->
<!--过滤器可额外接收参数，多个过滤器可串联，原始数据未改变，产生的是新的数据-->

</h3>
<body>

```

## 绑定样式

适用于动态指定类名
`<div class='box' :class='xxx' :style="{fontSize: n +'px' }" >test</div>`;xxx可为字符串、数组['box1','box2']、对象{box1:false,box2:true}。



## 生命周期


