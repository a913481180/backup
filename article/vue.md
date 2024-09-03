---
title: Vue2
date: 2021-06-07 20:00:00
categories:
  - web
tags:
  - vue
  - web
---

> 一套用于构建用户界面的渐进式 javascrip 框架

## 原生使用

- 引入 vue

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"> //引入 Vue</script>
<!--引入vue后，会出现全局属性Vue>
```

- 容器

```html
<div id="root">{{msg}} {{name}}</div>
<!-- {{}}中可写js表达式-->
```

- 使用 vue

```js
 window.onload=function(){
 Vue.config.productionTip=false;//阻止vue启动时的生成的提醒
//创建vue实例
const vu= new Vue({
  el:'#root', //挂载点，指定容器,可写类名，不能用在html和body上；第二种写法可用vu.$mount('#root');代替,el的优先级更高
  data:{
    msg:'hello',
    name:'xiaomi',
    test(){} //不建议在data中写函数，因为会data中的属性会进行数据代理，而方法并不需要进行数据代理
 }
});

```

## MVVM 模型

> 一种软件架构

- M:模型 model,代表数据模型
- V：视图 view,代表 UI 组件
- VM：视图模型 view model->vue 实例

model 和 view 分别通过和 viewmodel 双向数据绑定进行交互，开发者只需关注业务逻辑，不用手动操作 dom,复杂数据同步交给 vm 来管理

- 数据代理

`Object.defineproperty()`第一个参数为对象，第二个为对象的属性，第三个为配置项

如添加属性：

```js
let person{
name:'xiaomei'
}
Object.defineproperty(person,'age',{
value:18,
//默认情况下添加的属性不可枚举，即无法遍历出来
enumerable:true  //true表示可以枚举
//默认情况下添加的属性的值无法更改
writable:true //控制属性是否可修改
//默认情况下添加的属性无法删除
configurable: true ////控制属性是否可删除


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

通过一个对象对另一个对象中的属性的操作

```js
var object = {
  //原数据
  name: "xiaomei",
  sex: "female",
};
var test = {
  age: 3,
  hobby: "swing",
};
Object.defineProperty(object, "age", {
  //默认情况下添加的属性不可枚举，即无法遍历出来

  get() {
    console.log("getter");
    return test.age; //通过object中的age控制test中的age,即数据代理
  },
  set(n) {
    console.log("set", n);
    test.age = n;
  },
});
```

vue 中的数据代理：通过 vm 对象来代理 data 对象中属性的操作

通过 Object.defineProperty()把 data 对象中所有属性添加到 vm 上，为每一个添加到 vm 上的属性，都指定一个 getter/setter 方法,在它们内部去对 data 中的数据进行操作

### 数据监听

通过`vm._data`也可以访问到 data 中的数据,但其是经过加工的,即形成 getter、setter 形式

示例：

```js
//###只能监测一层
let data = { name: "xiaomei" };
//创建一个监视的实例对象，用于监视data中的属性变化
const obs = new Observer(data);
function Observer(obj) {
  //汇总对象中所有的属性形成一个数组
  const keys = Object.keys(obj);
  //遍历
  keys.forEach((k) => {
    Object.defineProperty(this, k, {
      get() {
        return obj[k];
      },
      set(val) {
        console.log("数据被修改了");
        obj[k] = val;
      },
    });
  });
}
```

原理：通过 setter 实现监测，且要在 new Vue 时就传入要监测的数据;

- 对象中后追加的属性，vue 默认不做响应式处理；如要给后添加的属性做响应式处理，需要用`Vue.set(对象,'添加的属性','属性值')或vm.$set(对象,'添加的属性','属性值')`
- 删除属性也会做响应式更新，可使用`this.$delete('属性')`删除
- 数组更新检测
  vue 将被侦听的数组的变更方法进行了包裹，所以会触发视图的更新，即先调用原生方法对数组进行修改，接着重新解模板，进行更新页面。被包裹的方法有`push()`,`pop()`,`shift()`,`unshift()`,`splice()`,`sort()`,`reverse()`.或使用`Vue.set(数组,下标,'修改的值')或vm.$set(数组,下标,'修改的值')`

> !注意：Vue.set();和 vm.$set()不能给 vm 或 vm 的根数据对象添加属性

## 语法

- 插值语法：用于解析标签体内容
  `{{xxx}}`xxx 为 js 表达式，可以直接读取到 data 中的属性

- 指令语法：用于解析标签
  `v-bind:href="xxx"`或简写为`:href="xxx"`

## 指令

### 数据绑定`v-model`|`v-bind`

#### 单向数据绑定

> 数据只能从 data 流向页面;

`v-bind:href="xxx"`或简写为`:href="xxx"`

#### 双向数据绑定

> 数据能从 data 流向页面,也能从页面流向 data

- `v-model:value="xxx"`或简写`v-model="xxx"`。
- `v-model`的三个修饰符：

  - `lazy`会将绑定的事件切换为 change 事件，只有在提交时（比如回车）才会触发，
  - `number`输入的字符串转为有效数字，
  - `trim`去除首尾的空格

- `v-model`指令只能用在表单类（输入类 input、select）元素上或自定义组件上

  ```html
  <!--输入框-->
  <input type="text" v-model.trim="keyWords/>

  <!--单选框，要加上value属性-->
  <input type="radio" v-model="radio" value="radio1" name="test" />
  <input type="radio" v-model="radio" value="radio2" name="test" />
  <!--多选框-->
  <!--若没有配置value，则当v-model初始值为数组时，勾选时收集的是null，为字符串时，收集的是布尔值-->
  <input type="checkbox" v-model="hobby" value="study" />
  <input type="checkbox" v-model="hobby" value="reading" />
  <input type="checkbox" v-model="hobby" value="sports" />
  ```

- v-model 双向绑定原理：

  - `v-bind绑定value属性的值；`
  - `v-on绑定input事件监听到函数中，函数会获取最新的值，赋值给绑定的属性中；`
    - 原生 input 元素的类型是 text/textarea，那么是使用它的 value 属性和 input 事件来实现双向数据绑定。
    - 原生 input 元 u 素的类型是 radio/checkbox，那么是使用它的 checked 属性和 change 事件来实现双向数据绑定。
    - 原生 select 元素，是使用它的 value 属性和 change 事件来实现双向数据绑定。

- 如果 `v-model` 绑定的是响应式对象上某个不存在的属性，那么 vue 会悄悄地增加这个属性，并让它响应式。

```js
// template中：
<el-input v-model="user.tel"></el-input>;
// script中：
export default {
  data() {
    return {
      user: {
        name: "xxx",
        //user 上会新增 tel 属性，并且 tel 这个属性还是响应式的
      },
    };
  },
};
```

- 组件上的双向绑定

> `v-model`默认绑定的值是 value 和 input 事件

```vue
<template>
  <p>子组件库存:{{ value }}</p>
  <button @click="addFun">增加1</button>
</template>
<script>
export default {
  props: {
    value: {
      type: Number,
      default: 0,
    },
  },
  /*
  model: { // 自定义v-model的格式
    prop: 'ame', // 代表 v-model 绑定的prop名
    event: 'zard' // 代码 v-model 通知父组件更新属性的事件名
  },
  */
  methods: {
    addFun() {
      this.$emit("input", this.value + 1);
    },
  },
};
</script>
```

### 事件绑定`v-on`

```html
<button v-on:cllick="showhello">test</button>
<!--或简写成-->
<button @cllick="showhello($event,22)">$event占位</button>

<script>
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

#### 事件修饰符

> 修饰符可以连续写，按写的顺序依次执行

- `prevent`:阻止默认事件
- `stop`:阻止事件冒泡
- `once`：事件只触发一次
- `capture`:使用事件的捕获模式
- `sefl`:只有 event.target 是当前操作的元素时才触发事件
- `passive`:事件的默认行为立即执行，无需等待事件回调执行完毕，

#### 键盘事件

- vue 中的按键别名：`回车enter`，`删除delete`，`退出esc`，`空格space`， `换行tap(必须配合keydown使用)`， `上up`，`下down`，`左left`，`右right`
- 系统修饰键：`ctrl`，`alt`，`shift`，`meta`
  - 配合 keyup 使用时：需再按下其他键，随后释放其他键才能触发
  - 配合 keydown 使用时：正常触发
- Vue.config.keyCodes.自定义键名=键码
- vue 中未提供的按键别名，可使用按键原始名去绑定，注意要转换为 kebab-case（短横线命名）

  ```html
  <input type="text" placeholder="tips" @keyup.enter="showhello" />`
  <input type="text" placeholder="tips" @keyup.ctrl.y="showhello" />
  ```

### 条件渲染`v-if`|`v-show`

v-if 与 v-show 的区别：

```html
<h2 v-show="false">test</h2>
<!--元素还存在-->
<h2 v-if="false">test</h2>
<!--元素直接消失,中间不能被打断-->
<template v-else-if="true"> <a>test</a></template>
<!--template不影响结构，但必须和v-if配合使用-->
<h2 v-else>test</h2>
```

### 列表渲染`v-for`

```html
<div id="#root">
  <ul>
    <!--遍历数组(item,index)-->
    <li v-for="n in persons" :key="n.id">
      {{index}}
      <!--索引值-->
    </li>

    <!--遍历对象(val,key,index)-->
    <li v-for="(val,k,index2) of persons" :key="index2">
      {{index2}}
      <!--索引值-->
    </li>
  </ul>
</div>
<!-- 除此之外还可以遍历字符串，数字-->
<script>
  const vm = new Vue({
    el: "#root",
    data: {
      persons: [
        { id: 001, name: "xiaoMei", age: 18 },
        { id: 002, name: "xiaoHong", age: 22 },
        { id: 003, name: "xiaoLan", age: 12 },
      ],
    },
  });
</script>
```

#### key 给节点提供唯一标识

- 不写 key，则默认用 index，index 作为 key 可能发生的问题：对数据进行逆序添加、删除时，会产生没有必要的真实 dom，界面虽然没问题，但效率低。当结构中存在输入类的 DOM 时，界面会出现问题。
- 虚拟 DOM 中的 key 的作用：key 是虚拟 DOM 对象的标识，当数据发送变化时，vue 会根据新数据生成新的虚拟 DOM，随后 vue 将新的虚拟 DOM 与旧的虚拟 DOM 进行差异比较

对比规则为：

- 旧虚拟 DOM 中找到与新虚拟 DOM 相同的 key：
  - 若虚拟 DOM 中的内容没有改变，则直接使用之前的真实 DOM
  - 若虚拟 DOM 中的内容改变了，则生成新的真实 DOM，随后替换掉页面中之前的真实 DOM
- 旧虚拟 DOM 中未找到与新虚拟 DOM 相同的 key：创建新的真实 DOM，随后渲染到页面。

### 渲染文本`v-text`|`v-html`

- `v-text` 指令,向其所在的节点中渲染文本内容,其会替换掉节点中的内容，`{{xxx}}`插值语法则不会,可以替换指定内容。

  ```html
  <h3 v-text="keyword"></h3>
  ```

- `v-html` 指令,向其所在的节点中渲染文本内容,其会替换掉节点中的内容,且会解析 html 标签，`v-text`则不会。其具有安全性问题，在网页上动态渲染 html 标签是非常危险的，易导致 XSS 攻击。

### 其他指令`v-cloak`|`v-once`|`v-pre`

#### `v-cloak`指令

Vue 实例创建完毕并接管容器后，会删除所有 v-cloak 属性，常用于配合属性选择器,解决网速不好加载 vue 慢而显示`{{xxx}}`问题

```html
<style>
  [v-cloak] {
    display: none;
  }
</style>
<h2 v-cloak>{{keyword}}</h2>
```

#### `v-once`指令

其所在的节点在初次渲染后，就视为静态内容了，以后即使数据变化了，其也不再变化

```html
<h2 v-once>{{keyword}}</h2>
```

#### `v-pre`指令

跳过其所在节点的编译过程，可用于跳过没有指令语法、插值语法的节点，加快编译速度。

### 自定义指令

```js
new Vue({
  el:'#root',
  data:{},
  directives:{
  //局部指令
  //指令与元素成功绑定时即一开始就被调用，当指令所在模板被重新解析时会被调用。
  //简写
  test(element,binding){ //element,binding为规定写法
    console.log(this); //这里的this为window
    element.innerText=bindding.value;
  }
  //标准写法
  test2:{ //命名：多个单词用-分割，使用时不用-,后一个首字母改为大写
    bind(element,binding){
      //指令与元素成功绑定时调用
    },
    inserted(element,binding){
      //指令所在元素插入页面时调用
    },
    update(element,binding){
      //指令所在模板被重新解析时被调用
    }
  }
  }
})

//全局指令
Vue.directive(指令名，回调函数);
```

## 计算属性

要用的属性不存在，利用已有属性计算而得来

内部有缓存机制，多次调用时只运行一次，效率更高,但不能开启异步任务如:`setTimeout()`

```js
new Vue({
  el: ".box",
  data: {
    name: "xiaomei",
    hoby: "reading",
  },
  computed: {
    fullName: {
      //初次读取fullName时，get会被调用;所依赖的数据改变时会调用,即data中的属性被改变时。
      get() {
        return this.name + "-" + this.hoby;
      },

      //当fullname被修改时，会被调用。使用v-model绑定时必须添加set，不然报错
      set(val) {
        this.name = val; //不能修改fullName自己,会报错
      },
    },
    //当数据只读不修改时，可采用简写
    fullName2() {
      return this.name + "-" + this.hoby;
    },
  },
});
```

## 监听属性

能开启异步操作如:`setTimeout(()=>{},1000)`;所有不被 vue 所管理的函数最好写成箭头函数，这样 this 的指向才是 vm

```html
<script>
  const vm = new Vue({
    el:'#test',
    data:{
    istrue: false
    number:{
      a:1,
      b:2},
    },
    wantch{
     'istrue':{
        immediate:true, ///初始化时调用一下handler
        //当istrue发生改变时调用
        handler(newValue,oldValue){
            console.log(newValue,oldValue);
        }
      }
      //简写，当只有handler()时
     istrue2(newValue,oldValue){console.log('test');}

    'number':{
      deep:true, //深度监视，当对象内的属性发生变化时,能检测到多层级内容的改变
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

```html
<body>
  <h3>
    <!--使用过滤器-->
    {{time|timeFormater1()|timeFormater2}}
    <!--或者v-bind:属性="xxx|过滤器名"-->
    <!--过滤器可额外接收参数，多个过滤器可串联，原始数据未改变，产生的是新的数据-->
  </h3>
  <body>
    <script>
      new Vue({
        el:'#root'
        data:{
        time:24356765432,
        },
        filters:{
          timeFormater1(value){
            console.log(value);
            return value; //返回数据
          },
          timeFormater2(value){
            console.log(value);
            return value; //返回数据
          },
        }
      });

      //注册全局过滤器
      Vue.filter('myName',function(value){return value;});
    </script>
  </body>
</body>
```

## mixins(混入)配置项

> mixins 属性和组件的属性相同时，组件的优先级更高，生命周期函数会一起执行(mixins 中的先执行)
> 可以把多个组件共用的配置提取成一个混入对象
> school 组件

```vue
<template>
  <div>
    <h2 @click="showName">{{ name }}</h2>
  </div>
</template>

<script>
//引用一个test
import { test } from "../mixin";
//import{test,test2} from '../mixn'

export default {
  name: "School",
  data() {
    return { name: "daxue" };
  },
  mixins: [test],
  //mixins:[test,test2]
};

//全局混入
//Vue.mixin(xxx)
//Vue.mixin(xxx2)
</script>
```

mixin.js

```js
//定义混合
export const test = {
  methods: {
    showNamw() {
      alert(this.name);
    },
  },
  mounted() {},
};
export const test2 = {
  data() {
    return {
      x: 2,
      y: 1,
    };
  },
  mounted() {},
};
```

## 插件

用于增强 Vue

- plugins.js

```js
//定义插件
export default {
  install(Vue, x, y) {
    console.log(x, y);
    //添加全局过滤器
    Vue.filter();
    //添加实例方法
    Vue.prototype.$myMethod = function () {};
  },
};
```

- main.js

```js
//引入Vue
import Vue from "vue";
//引入APP
import APP from "./App.vue";
//引入插件
import plugins from "./plugins";

//使用插件
Vue.use(plugins, 33, 2);

//创建VM
new Vue({
  el: "#root",
  render: (h) => h(App),
});
```

## scoped 样式

让样式只在本组件内有效，即局部生效，防止冲突。写法：`<style scoped></style>`
其在标签上加上属性`data-v-xxxxx`，配合标签属性选择器`demo[data-v-xxxxx]`

> 组件化编码流程：1.拆分成静态组件。2.实现动态组件。3.实现交互绑定事件

## vue 封装的动画与过渡

```vue
<template>
<div>
<transition name="hello" appear>
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

## 绑定样式

- 动态指定类名
  `<div class='box' :class='xxx' >test</div>`
  xxx 可为`字符串`、数组`['box1','box2']`、对象`{box1:false,box2:true}`。

- 动态指定内联样式
  `<div class='box'  :style="xxx" >test</div>`
  xxx 可为`字符串`、数组`[{color:'red'},{background:'red'}]`、对象`{color:'red'}`。

## 生命周期

又名生命周期回调函数、生命周期函数、生命周期钩子，是 vue 在关键时刻帮我们调用的一些特殊名称的函数，其名字不可更改，其中的 this 指向 vm 或组件实例对象。

| 生命周期                           | 说明                                                                                                                                             |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 初始化：生命周期、事件             | 但数据代理还未开始                                                                                                                               |
| beforeCreate                       | 无法通过 vm 访问到 data 中的数据、 methods 中的方法                                                                                              |
| 初始化                             | 数据检测、数据代理                                                                                                                               |
| created                            | 可以通过 vm 可访问到 data 中的数据、methods 中的方法                                                                                             |
| 解析模板，生成虚拟 DOM，（内存中） | 此时页面还不能显示解析好的内容                                                                                                                   |
| beforeMount                        | 页面呈现的是未经 Vue 编译的 DOM 结构，此时所有对 DOM 的操作，最终都不奏效                                                                        |
| 创建 vm $el                        | 将内存中的虚拟 DOM 转为真实 DOM 插入页面                                                                                                         |
| mounted                            | 页面中呈现的是经过 Vue 编译的 DOM，对 DOM 的操作均有效，到此初始化过程结束。一般在此进行开启定时器、发送网络请求、订阅消息、绑定自定义事件等操作 |
| beforeUpdate                       | 此时数据是新的，但页面是旧的，即页面未与数据保持同步                                                                                             |
| 根据新的数据，生成新的虚拟 DOM，   | 随后与旧的虚拟 DOM 进行比较，最终完成页面的更新                                                                                                  |
| updated                            | 此时数据和页面保持同步                                                                                                                           |
| beforeDestroy                      | 此时 vm 中所有的 data、methods、指令等都处于可用状态，马上要执行销毁过程，一般在此进行关闭定时器、取消订阅消息、解绑自定义事件等操作             |
| destroyed                          | 销毁 vue 实例后自定义事件会失效，但原生 DOM 事件依然有效                                                                                         |

## 组件

实现应用中局部功能代码和资源的集合

> 模块：向外提供特定功能的 js 程序

### 非单文件组件

> 一个文件中包含有 n 个组件

#### 创建 school 组件

```js
//简写
const school = {};
//标准写法
const school = Vue.extend({
  //组件定义时，不要写el配置项,最终所有的组件都要进过一个vm的管理
  //名字
  name: "xuexiao",

  //使用template可以配置组件结果。
  template: `
<div>
<h3>test</h3>
</div>`,

  data() {
    //data必须写成函数，为避免组件被复用时，数据存在引用关系
    return {
      schoolName: "BST",
      address: "beijing",
    };
  },
});
```

#### 定义 app 组件

```js
const app = Vue.extend({});
```

#### 创建 vm

```js
new Vue({
  el: "#root",
  template: "<app></app>",
  //局部注册组件
  components: {
    xuexioa: school, //组件名：一个单词通常首字母大小，多个单词通常用-分隔，或首字母大写（需要Vue脚手架支持）
    app,
  },
});

//全局注册组件
Vue.component("xuexiao", school);
```

```js
import App from "./compoents/App.vue";
const vm = new Vue({
  components: { App },
  render: (h) => h("App"),
});
```

```js
import App from "./compoents/App.vue";
const vm = new Vue({
  render: (h) => h(App),
});
```

#### 使用组件

```html
<xuexiao></xuexiao>
<xuexiao /><!--需要脚手架支持，否则多个组件时，后续组件将不能渲染-->
```

关于 VueComponent，组件本质是一个名为 VueComponent 的构造函数，是由 Vue.extend 生成的。当我们写`<xuexiao></xuexiao>`，Vue 解析时会帮我们创建 school 组件的实例对象，即 vue 帮我们执行`new VueComponent();`。当我们每次调用 Vue.extend()时,返回的都是一个全新的 VueComponent。在组件配置中，data 函数、methods 中的函数、watch 中的函数等，它们的 this 都指向 VueComponent 实例对象。

```js
//定义一个构造函数
function Demo() {
  this.a = 1;
  this.b = 2;
}
//创建一个Demo实例对象
const d = new Demo();
console.log(Demo.prototype); //显示原型属性
console.log(d.__proto__); //隐式原型属性
//显示原型属性与隐式原型属性都指向原型对象
//通过显示原型属性操作原型对象
Demo.prototype.e = 21;
```

> !!重要内置关系：`VueComponent.prototype.__proto__===Vue.prototype`;其可以让组件实例对象访问到 Vue 原型上的属性和方法。

### 单文件组件

> 一个文件中只包含有一个组件

#### school.vue 组件

```vue
<template>
  <!--组件的结构-->
  <div class="demo">
    <h2>{{ schoolName }}</h2>
    <h2>{{ address }}</h2>
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
.demoo {
  background-color: red;
}
</style>
```

#### App.vue 组件

```vue
<template>
  <div>
    <School />
  </div>
</template>

<script>
//引入组件
import School from "./School";

export default {
  name: "App",
  components: { School },
};
</script>

<style></style>
```

- main.js

```js
import App from "./App.vue";
new Vue({
  el: "#root",
  components: { App },
});
```

#### index.html

```html
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

## 脚手架 vue.cli(command line interface)

1. 全局安装@vue/cli:`npm install -g @vue/cli`
2. 切换到要创建项目的目录，然后创建项目：`vue create xxxx`
3. 启动项目：`npm run serve`

> 1. 配置 npm 淘宝镜像：`npm config set registry https://....`
> 2. Vue 脚手架隐藏了所有 webpack 相关的配置，若想查看具体 webpack 配置，执行：`vue inspect > output.js`

### main.js 项目的入口文件

```js
//引入vue.runtime.xxx.js，其是运行版的vue,只包含核心功能，没有模板解析器,而完整版的Vue才有。
import Vue from "vue";
//引入App组件
import App from "./App.vue";
//关闭生产提示
Vue.config.productionTip = false;

//创建VUe实例对象
new Vue({
  el: "#root",
  //将App组件放入容器中
  render: (h) => h(app), //没有模板解析器，不能使用template配置项，需要使用render函数接收到的createElement函数去指定具体内容
  render(createElement) {
    return createElement("h1", "test");
  },
});
```

### vue.config.js 配置

使用 vue.config.js 可以对脚手架进行个性化定制

```js
module.exports={
 pages:{
   index:{
  //入口
  entry:'src/main.js',
  }
  },
 lintOnSave:false, //关闭语法检查
 //代理
 devServer:{
  proxy:{
   '/api'::{
    target:'http://xxxx'
    changeOrigin:true,
    pathRewrite:{
     '^/api':'/api'
    }
   }
  }
 }
 configureWebpack:(config)=>{//地址
  config.resolve={
   extensions:['.js','.json','.vue'],
   alias:{
    '@':path.resolve(__dirname,'./src')
   }
  }
 }
}
```

### 脚手架文件结构

```text
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
```

## ref 属性

id 的替代者,用来给元素或组件注册引用信息,应用在 html 标签上获取的是真实 DOM 元素，应用组件标签上获取的是组件的实例对象

```vue
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
    console.log(this.$ref.title); //真实DOM元素
    console.log(this.$ref.sch); //School组件的实例对象
  }
}
}
</script>
```

## props 配置项

让组件接收外部传来的数据,props 是只读的，Vue 底层会监测(非深度监测)props，若发生修改会发出警告。若要修改数据，请复制一份到 data 中,再修改。

> Vue 使用 props 时父组件给子组件传值后，子组件可用 props 配置项接收，但不要忘了加引号！！！！！！！否则使用它会出现 undefined！
> v-modle 绑定的值不应是 props 传过来的值，当其为对象类型时，修改对象中的属性时，vue 无法发现
> props 和 data 的属性重复时，props 的优先级更高

### 子组件

```js
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
   required:true, //name必填
  },
 }
```

### 父组件

```vue
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

> props 适用于父组件和子组件的通信，子组件与父组件的通信要求父先给子一个函数，子再通过形参的形式传递数据

## 自定义事件

子组件跟父组件通信可使用 Vue 的自定义事件，父组件通过@xxx=“function(){}”给子组件绑定自定义事件，然后子组件通过$emit 触发事件，第一个参数为触发的事件，第二个为要传递的数据，便可实现子与父的通信。组件中使用原生事件需要`native`修饰符(`native`只对组件生效)
适用于子组件与父组件通信
App.vue

```vue
<template>
  <!-- 通过父组件给子组件绑定一个自定义事件：用于子给父传递数据 -->
  <Student v-on:test="getStudentName" />
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

<style></style>
```

Student.vue

```vue
<template>
  <h2>{{ name }}</h2>
  <h2>{{ sex }}</h2>
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
//this.$off('demo1'); //解绑一个自定义事件
//this.$off({'xx','xx','xx'}); //解绑多个自定义事件
this.$off(); //解绑所有自定义事件
},
death(){
this.$destroy(); //销毁当前student组件的实例，销毁后所有student实例的自定义事件全部失效
}
 }
</script>
```

## 全局事件总线

适用与任意组件间的通信

main.js

```js
import Vue from "vue";
import App from "./App.vue";

new Vue({
  el: "#root",
  render: (h) => h(App),
  beforeCreate() {
    Vue.prototype.$bus = this; //安装全局事件总线
  },
});
```

School.vue

```vue
<template></template>
<script>
export default {
  name: "school",
  data() {
    return {
      name: "xuexiao",
      address: "biejing",
    };
  },
  mounted() {
    //接受数据
    this.$bus.$on("hello", (data) => {
      console.log("recieve data", data);
    });
  },
  beforeDestroy() {
    //解绑事件
    this.$bus.$off("hello");
  },
};
</script>
<style></style>
```

School.vue

```vue
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

## 消息发布和订阅

适用于任意组件间通信，需要信息的组件订阅消息，提供信息的组件的发布消息

School.vue

```vue
<template></template>

<script>
//安装pubsub: npm i pubsub-js
import pubsub from "pubsub-js";
export default {
  name: "School",
  data() {
    return {
      name: "xuexiao",
      address: "beijing",
    };
  },
  mounted() {
    //订阅消息，接收数据
    this.pubId = pubsub.subscribe("hello", function (msgName, data) {
      console.log("有人发布hello消息", msgName, data);
      console.log(this);
    });
  },
  beforeDestroy() {
    pubsub.unsubscribe(this.pubId);
  },
};
</script>

<style></style>
```

student.vue

```vue
<script>
import pubsub from "pubsub-js";
export default {
  name: "Student",
  data() {
    return {
      name: "xiaomei",
      sex: "woman",
    };
  },
  methods: {
    ///提供数据
    sendData() {
      pubsub.publish("hello", 2222);
    },
  },
};
</script>
```

- nextTick

语法：`this.$nextTick(回调函数)`
作用：在下一次 DOM 更新结束后执行其指定的回调,用于当改变数据后，要基于更新后的新 DOM 进行某些操作时，要在 nextTick 所指定的回调函数中执行

## vue 脚手架配置代理服务

vue.config.js

```js
module.exports={

 pages:{
  index:{
   //入口
  entry:'src/main.js'
   },
 },
lintOnSave:false, //关闭语法检测
//开启代理服务器（一）:配置简单，但不能配置多个代理
/*
devServer:{
 proxy:'http://localhost:3333'
}*/
//开启代理服务器（二）:可以配置多个代理
devServer:{
proxy:{
'/api':{ //匹配所有以'/api'开头的请求路径
target:'<url>',
ws:true, //用于支持websocket
changeOrigin:true //用于控制请求头中的host值,true时，服务器请求头伪装成localhost：5050，false时为localhost:8080
},
'/test':{
target:'<other_url>'
},
'/test2'{
target:'http://localhost:5000',
pathRewrite:{'^/test2':''}, //清除地址前面的协议域名端口http://localhost:8080
}
}
 }

}
```

App.vue

```vue
<template></template>
<script>
import axios from "axios";
export default {
  name: "App",
  methods: {
    getStudents() {
      aixos.get("http://localhost:8081/students").then(
        (response) => {
          console.log("请求成功", data);
        },
        (error) => {
          console.log("请求fail", message);
        }
      );
      //aioxs.post(地址,{key:value,key2:value2}).then(function(response){},function(error){})
    },
  },
};
</script>
```

## 插槽

用于父组件与子组件的通信

Category.vue

```vue
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
<slot :games="test"  :games2:'list'>默认 当没有传递具体结构时，会显示</slot>
<div>
</template>

<script>
export default{
 name:'Category',
 data(){
 return {
 test:['xxx','0989','33'],
 list:['','','','']
 }
 },
 props:['listData','title']
}
</script>
```

App.vue

```vue
<template>
  <Category>
    <img slot="center" src="xxx" />
    <a slot="footer" href="xxxx"></a>
  </Category>
  <Category>
    <template scope="test1">
      <h1>{{ test1.games }}</h1>
      <h1>{{ test1.games2 }}</h1>
    </template>
  </Category>
</template>
```

## vuex

专门在 vue 中实现集中式状态（数据）管理的一个 vue 插件,对 vue 应用中多个组件的共享状态进行集中式的管理（读与写），也是一种组件间通信的方式，适用于任意组件间的通信。

使用时机：多个组件需要共享数据时

[![fJo8ZF.png](https://z3.ax1x.com/2021/08/10/fJo8ZF.png)](https://imgtu.com/i/fJo8ZF)

### store

创建文件：src/store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'
//使用vuex插件
Vue.use(Vuex);

//action--用于响应组件中的动作
const actions={}
//mutations--用于操作数据state
const mutations={}
//state--用于储存数据
const state={}

//创建store
const store =new Vuex.Store({
actions,
mutatuons,
state,
});

//暴露store
expore default store
```

在 main.js 中创建 vm 时传入 store 配置项

```js
import Vue form 'vue'
import App from '/App.vue'
//引用插件
import vueResource from 'vue-resource'
import Vuex form 'vuex'
import store form  './store'

//使用插件
Vue.use(vueResource);

new Vue({
el:'#app',
render:h=>h(App),
store,
beforeCreate(){
Vue.prototype.$bus=this
}
});
```

#### 基本使用

初始化数据、配置 actions、mutations，操作文件 store.js

```js
import Vue from 'vue'
import Vuex form 'vuex'
Vue.use (Vuex);

const actions={
//响应组件中的动组
jia:function(context,value){
 //context里中存放着一些方法
context.commit('JIA',value);
}
}

const mutations={
//执行jia
JIA:function(state,value){
state.sum+=value;
}
}

//初始化数据
const state={sum=0;
}

//创建并暴露store
export default new Vuex.store({
actions:actions,
mutations,
state,
});
```

组件中读取 vuex 中的数据：`$store.state.sum`
组件中修改 vuex 中的数据：`$store.dispatch('actions中的方法名',数据)`或`$store.commit('mutatuins中的方法名',数据)`

> 若没有网络请求或其他业务逻辑，组件中也可以越过 actions，即不写 dispatch,直接编写 commit

#### 四个 map 方法的使用

- mapState:用于帮助我们映射 state 中的数据为计算属性

```js
computed:{
//借助mapState生成计算属性，对象写法
...mapState({sum:'sum',school:'shchool',subject:'subject'});
//借助mapState生成计算属性，对象写法
...mapState(['sum','shchool','subject']);
}
```

- mapGetters: 帮助我们映射 getters 中的数据为计算属性

```js
computed:{
...mapGetters({b:'b'});
...mapGetters(['b']);
}
```

- mapActions 方法：帮助我们生成与 action 对法的方法，即包含`$store.dispatch(xxx)`的函数

```js
methods:{
...mapActions({inicrementOdd:'jiaOdd',incrementWait:'jiaWait'})
...mapActions(['jiaOdd','jiaWait'])
```

- mapMutations 方法：用于帮助我们生成与 mutations 对话的方法，即：包含$store.commit(xxx)的函数

```js
methods:{
...mapMutations({increment:'JIA',decrement:'JIAN'})
...mapMutations(['JIA','JIAN'])
}
```

> mapActions 与 mapMutations 使用时，若需要传递参数需要在模板中绑定事件时传递好参数，否则参数是事件对象

#### store

index.js

```js
//创建store
import vue from 'vue'
import vuex from 'vuex'
//应用vuex插件
Vue.use(vuex);

const test={
//namespaced:true,
action:{},
mutations:{},
state:{},
getters:{}
}
//store：actions、mutations、state、getters

//创建暴露store
export default new Vuex.store{
 modules:{
 a:test,
 b:xxx
}
}
```

test.vue

```vue
<template></template>

<script>
import {mapState,mapGetters,mapMutations,mapActions } form 'vuex'
export default {
name:'test'
data(){},
computed:{
personList(){
 return this.$store.state.personAbout
}
}

}
</script>
```

### 模块化+命名空间

为了让代码更好维护，让多种数据分类更加明确

修改 store.js

```js
const countAbout={
namespaced:true, //开启命名空间
state:{x:1},
mutatuions:{.........},
actions:{..},
getters:{

bigSum(state){
 return state.sum+1;

}
}
}

const personAbout={
namespaced:true,
state:{..},
mutations:{},
actions:{},
}

const store=new Vuex.Store({
modules:{
countAbout,
personAbout
}

});
```

开启命名空间后，组件读取 state 数据

```js
this.$store.state.personAbout.list
//借助mapState读取
...mapState('countAbout',['sum,'school','subject']);
```

开启命名空间后，组件读取 getters 数据

```js
this.$store.getters('personAbout/firstPersonName')
//借助mapGetters读取
...mapGetters('countAbout',['bigSum']);
```

开启命名空间后，组件读取 dispatch 数据

```js
this.$store.dispatch('personAbout/addPersonWang'person`)
//借助mapActions读取
...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'});
```

开启命名空间后，组件读取 commit 数据

```js
this.$store.commit('personAbout/ADD_PERSON',person)
...mapMutations('countAbout',{increment:"JIA",decrement:"JIAN"})
```

## 路由

路由就是一组映射（key-value）的对应关系,key 为路径，value 是 function 或 Component

- 后端路由:value 是 function,用于处理客户端提交的请求，即服务端根据请求路径找到匹配的函数处理数据后，返回响应数据

> 路由组件一般放在 pages 文件夹，一般组件则放在 Component 文件夹；通过切换，隐藏了的路由组件，默认是被销毁的，需要的时候在去挂载；每个组件都有$route属性，里面存储着自己的路由信息
；整个应用只有一个route，可通过组件的$router 属性获取到。

### vue-router

vue 的一个插件库，用来实现 SPA 应用（单个 web 应用，整个页面只有一个完整页面，只会做页面的局部更新，数据需要通过 ajax 请求获取）

src/router/index.js

```js
//引入路由
import VueRouter from 'vue-router'
//引入组件
import About form '../components/About.vue'
import Home form '../components/Home.vue'

//创建并暴露一个路由器
export default new VueRouter({

routes:[
{path:"/about",
component:About
},
{
path:"/home"
component:Home
children:[ //多级路由
{
path:'news', //不能写‘/news’
component:News,
},


]
}
]

})
```

main.js

```js
//引入Vue
import Vue from 'vue'
//引入APP
import App form './App.vue'
//引入VueRouter
import VueRouter from 'vue-router'
//引入路由器
import router form './router'

//应用插件
Vue.use(VueRouter)

//创建vm
new Vue({
el:"#app",
render:h=>h(App),
router:router
})

```

index.js 跳转

```vue
<template>
<!--用route-link代替a标签去跳转-->
<!--active-class属性：该元素激活时触发样式，可配置高亮样式-->
<router-link  class="" active-class="active" to="/about">About</router-link>
<router-link  class="" to="/home">Home</router-link>
<router-link  class="" to="/home/news">Home</router-link>
<!--跳转路由并携带query参数（字符串写法）-->
<router-link  class="" to="/home/news?id=xxx&titel=xxx">Home</router-link>
<!--跳转路由并携带query参数（对象写法）-->
<router-link  class="" to="
{path:'/home/news',
query:{id:xxx,titel:xxx}
}"></router-link>


<router-view></route-view> <!--指定组件呈现的位置-->
</template>
```

接收 query 参数

```js
$route.query.id;
```

#### 命名路由

简化路由跳转

```js
{
path:'/demo',
component:Demo,
children:[
{
 path:'test'
 component:Test,
 chilren:[
  {
  name:'hello' //给路由命名
  path:'welcome',
  component:Hello
  }
  ]
},
{
name:weather
path:'weather/:id/:title', //使用占位符声明接收params参数
component:Weather
}
]
}
```

跳转

```html
<router-link :to="name:hello">test</router-link>
<router-link :to="
{
name:hello,
query:{id:xxx,title:xxx}
}">test</router-link>
<!--跳转路由并携带params参数（字符串写法）-->
<router-link  class="" to="/home/news/id/title">Home</router-link>
<!--跳转路由并携带params参数（对象写法）-->
<router-link  class="" to="
{name:'weather',
params:{id:xxx,titel:xxx}
}"></router-link>
</template>
```

> 携带 params 参数时，若使用 to 的对象写法，则不能使用 path 配置项，必须用 name 配置下

接收 params 参数

```js
$route.params.id;
```

#### 路由的 props 配置

index.js

```js
export default new VueRouter({

routes:[
{
path:'/home',
component:Home,
children:[
{
name:'test',
path:'detail/:id/:title',
compomemt:Detail,
//props的第一种写法，值为对象，该对象中的所有key-value都会以props的形式传给Detail组件
props:{a:1,b:'hello'}
//props的第二种写法，值为布尔值，若布尔值为真，就会把该路由组件收到的所有params参数，以props的形式传给detail组件。只能传params参数
props：true
//props的第三种写法，值为函数,该函数返回的对象中每一组key-value都会通过props传给Detail组件
props($route){
return {id:$route.query.id,title:$route.query.title}
}
}
]
}
]
})
```

Detail.vue

```vue
<template>
  <ul>
    <li></li>
    <li></li>
  </ul>
</template>

<script>
export default {
  name: "Detail",
  props: ["id", "title"],
  computed: {},
  mounted() {},
};
</script>
```

- `<router-link>`的 replace 属性
  - 作用：控制路由跳转时操作的浏览器历史记录的模式
  - 历史记录的写入方式：`push`追加历史记录，`replace`替换当前记录；路由跳转时默认为`push`
  - `<router-link replace >test</router-link>`

message.vue

```vue
<script>
name:'massage',
data(){},
methods:{
pushShow(m){
this.$router.push({

 name:'test',
 query:{
  id:m.id,
  title:m.title
 }
})
}
}
```

Banner.vue

```vue
<script>

export default {
    name:'Banner'
 methods:{
 back(){this.$router.back()},
 forward(){this.$router.forward()}
 }
}
```

### 编程式路由导航

```js
this.$router.push({
  name: "test",
  params: { id: xxx, title: xxx },
});

this.$router.replace({
  name: "test",
  params: { id: xxx, title: xxx },
});
```

### 缓存路由组件

让不展示的路由组件保持挂载，不被销毁

```html
<keep-alive include="News">
  <router-view></router-view>
</keep-alive>
```

home.vue

```vue
<template>
  <div>
    <ul>
      <li>
        <router-link to="/home/new">news</router-link>
      </li>
      <li>
        <router-link to="/home/message">message</router-link>
      </li>
    </ul>
    <!--缓存多个路由组件-->
    <!-- <keep-alive :include="['','']">-->
    <!--缓存一个路由组价-->
    <keep-alive include="News">
      <router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
export default {
  name: "Home",
};
</script>
```

New.vue

```vue
<script>
 export default{
 name:'News',
 data(){
  return{
  opacity:1
  }
  },
 //路由组件所独有的两个钩子，用于捕获路由组件的激活状态
 activated(){console.log('News组件被激活了')}
 deactivated(){console.log('News组件失活了')}
}
```

### 路由守卫

对路由进行权限控制

- 全局守卫

index.js

```js
...
const router=new VueRounter({
 routes:[
  {
  name:'test',
  path:'/about',
  component:About
  meta:{isAuth:true,title:'新闻'},
  },
 ]
//全局前置路由守卫---初始化时被调用，每次路由切换之前被调用
router.beforeEach((to,from,next)=>{
if(to.meta.isAuth){//判断当前路由是否需要进行权限控制
 if(localStorage.getItem('shool')==='test'){next();}
 else{alert('学校名不对')}
}else{
next(); //放行
}
})

//全局前置路由守卫---初始化时被调用，每次路由切换之后被调用
router.afterEach((to,from,next)=>{
console.log('后置路由守卫');
dovument.title=to.meta.title||'testTitle'

})
})
export default router;
```

- 独享守卫

index.js

```js
// ......
name:'news',
path:'news',
component:News,
meta:{isAuth:true,title:'新闻'},
beforeEnter:(to,from,next)=>{
if(to.meta.isAuth){//判断当前路由是否需要进行权限控制
 if(localStorage.getItem('shool')==='test'){next();}
 else{alert('学校名不对')}
}else{
next(); //放行
}
}
```

- 组件内守卫

```js
//进入守卫：进入该组件时被调用
beforeRouteEnter(to,from,next){},
//离开守卫：离开该组件时被调用
beforeRouteLeave(to,from,next){}
```

### 路由器工作模式

对于一个 url 来说`#`及其后的内容就是 hash 值;hash 值不会包含在 http 请求中，即 hash 值不会带给服务器。

- hash 模式:

地址中永远带着#号，不美观；通过第三方手机 app 分享时可能会标记为不合法；但其兼容性好；

- history 模式:

地址干净，兼容比 hash 略差，应用部署上线时需要后端人员支持，解决刷新页面服务端 404 的问题；

## Vue UI 组件库

移动端：`Vant`,`Cube UI`,`Mint UI`

PC 端：`Element UI`, `IView UI`

main.js

```js
import Vue from 'vue'
import App from './App.vue'
//引入ElementUI组件库
import ElementUI from 'element-ui'
//引入ElementUI全部样式
import 'element-ui/lib/theme-chalk/index.css';

Vue.ues(ElementUI);

new Vue({
 el:'#app'
 render:h=>h(App),
})
```

balel.config.js

```js
module.exports = {
  presets: [
    "@vue/cli-plugin-babel/preset",
    ["@babel/present-env", { modules: false }],
  ],
  plugins: [
    [
      "component",
      {
        libraryName: "element-ui",
        styleLibraryName: "theme-chalk",
      },
    ],
  ],
};
```

---

### vue 刷新当前页面

用 vue-router 重新路由到当前页面，页面是不进行刷新的

```js
//###ctrl+F5刷新
location.reload()；
//或
this.router.go(0);

//局部刷新
1. 跳转到一个空白页，再跳转回来
2. v-if控制router-view的存亡来实现刷新
```

### VUE 实现 checkbox 的勾选

给选择按钮绑定 v-mode 变量，值为 true 时就是选中状态，绑定点击事件，执行勾选操作

### 自定义 checkbox 样式

通过 label 标签将 input 包裹住，通过 label for 绑定 input id，
将 input 设置 `opacity: 0`;不可见，再通过给设置 lable 标签或其中 div 来设置 checkbox 的默认样式及选中状态样式

### 特殊符号（Unicode）

`https://www.cnblogs.com/Whikiey/archive/2011/01/05/1926220.html`
