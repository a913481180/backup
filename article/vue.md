--- 
titile: Vue2.0+Vue3.0
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
<a href='www.baidu.com' v-on:click.prevent.stop="showhello()"> test <a>	<!--阻止a标签自动跳转,可连续写-->
```

- 键盘事件

   - vue中的按键别名`回车enter`,`删除delete` , `退出esc`, `空格space`, `换行tap(必须配合keydown使用)`, `上up下down左left右right` 
   - vue中未提供的按键别名，可使用按键原始名去绑定，注意要转换为kebab-case（短横线命名）
   - 系统修饰键：`ctrl`,`alt`,`shift`,`meta`;
      - 配合keyup使用时：需再按下其他键，随后释放其他键才能触发
      - 配合keydown使用时：正常触发
   - Vue.config.keyCodes.自定义键名=键码

`<input type="text" placeholder="tips" @keyup.enter="showhello">

- 计算属性

内部有缓存机制，效率更高

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
