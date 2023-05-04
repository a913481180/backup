---
title: vue3
date: 2020-11-11 20:33:33
categories:
  - web
---

# VUE3.0

## 安装

1. 查看 vue-cli 版本，必须大于 4.5：`vue -V`
   - 升级 vue-cli：`npm install -g @vue/cli`
   - 创建：`vue create test` 选择 vue3
2. vite 创建：`vue init vite-app test`
   - `vue install`
   - `npm run dev`
     或者`npm init vite@latest`输入工程名称,选择框架为 vue
   - `vue install`
   - `npm run dev`

     > <https://vitejs.cn/>

## main.js

- vue3

```js
///引用的不再是vue的构造函数
import { createApp } from 'vue'
import App from './App.vue'
//创建应用实例对象
const app=createApp(App);
app.mount('#app')

```

- vue2

```js
//app 比 vm 轻量，属性少
cosnt vm =new  Vue({
   render(h=>h(App))
})
vm.$mount('#app')
```


## 响应式 API：核心

### App.vue

> vue3 组件中的模版结构可以没有 div 根标签

#### setup

> vue2 中的 data、methods、computed 写在 setup 中可以访问到 setup 中的属性和方法，但 setup 却不能访问到 vue2 中的配置。所以不建议混用。
> 如有重名，setup 中的优先，非异步组件，setup 中不能加 async，否则 return 返回的是 promise ，模版将读取不到
> setup 在 beforeCreate 前会执行一次，其中的 this 为 undefined

```js
import {h} from 'vue'
export default{
   name:'App',
   setup(){
      //非响应式数据
      let a=1;
      let b=2;
     function test(){
      console.log(a,b)
      }

     return {//返回对象，可在模板中直接使用
         c:1,d:'xx',test
      }
      //也可以返回渲染函数，但此时模板中的内容将全部无效
      //return ()=>{h('div','xxxx')}

   }
}
```

- 传值

```js
<child a="1" b="2" @test="handle" > <template v-slot:xxx>s</template></child>
//子组件
props:['a','b'],
emits:['test']
setup(props,context){
//context.attrs->组件外传来但没有在props声明的属性
//context.emits
//context.slots
}
```

#### ref

> 定义一个响应式数据,模板中读取数据不用`.value`;基本类型使用的是`Object.defineProperty()`的 getter、setter 实现的，对象数据使用 reactive 函数

```js
import {ref} from 'vue'
export default{
   name:'App',
   setup(){
      //响应式数据
      let a=ref(1);//引用实例对象
      let b=ref({c:1});
     function test(){
        a.value=2;
        b.value.c=3;
      console.log(a,b)
      }

     return {//返回对象，可在模板中直接使用
         a,b,test
      }
   }
}
```

#### reative

> 定义一个对象类型的响应式数据（基本数据类型不用它)

```js
import {ref,reactive} from 'vue'
export default{
   name:'App',
   setup(){
      //响应式数据
      let a=ref(1);//引用实例对象
      let b=reactive({c:1});
     function test(){
        a.value=2;
        b.c=3;//不用.value
      console.log(a,b)
      }

     return {//返回对象，可在模板中直接使用
         a,b,test
      }
   }
}
```

#### computed

```js
import {computed} from 'vue'
export default{
   name:'App',
   setup(){
      //响应式数据
      let a=1;//引用实例对象

     let d= computed(()=>{
        return 100
      })
let b=computed({
   get(){
      return 1
   },
   set(value){}
})
     return {//返回对象，可在模板中直接使用
         a,b,test,d
      }
   }
}
```

#### watch

```js
import {watch,ref} from 'vue'
export default{
   name:'App',
   setup(){
      //响应式数据
      let a=ref(1);//引用实例对象
      let b=ref(1);//引用实例对象
  watch(a,(newVal,oldVal)=>{//监听ref定义的响应式数据,不用加.value

      },{immediate:true,depp:true})
watch([a,b],(newVal,oldVal)=>{//监听多个ref定义的响应式数据
newVal->[.....]
      })
      let person=ref({
         a:1,
         b:{c:2}
      })
watch(person,(newVal,oldVal)=>{})//不会有效果，因为value的值为proxy属性，只有它变才会检测到，内部的数据变不会触发
//应使用
watch(person.value,(newVal,oldVal)=>{})//或
watch(person,(newVal,oldVal)=>{},{deep:true})
//-------------------------------------------------
      //使用reative定义的数据，无法正确获取oldvalue,无法关闭深度监视，
      let person=reative({
         a:1,
         b:{c:2}
      })
watch(person,(newVal,oldVal)=>{})
//监听reative定义的对象的某一属性,必须写成函数
watch(()=>{return person.a},(newval)=>{})
//监听reative定义的对象的某些属性,必须写成函数
watch([()=>{return person.a},...],(newval)=>{})
 //监听reative定义的对象的某个对象,必须加上deep
watch(()=>{return person.b},(newval)=>{},{deep:true})
//-----------------------------------------------------------
watchEffect(()=>{//内部用到谁，就会监测谁
let x=person.a
})
     return {//返回对象，可在模板中直接使用
         a,b,
      }
   }
}
```

## 响应式 API：工具函数

#### 自定义 hooks 函数(组合式函数)

> 本质是个函数，将 setup 中的函数进行封装,类似 vue2 中 mixin

- test.js

```js
import {ref ,onMount} from 'Vue'
function test(){
   let b=ref(2)
   onMount(()=>{
      b.value++
   })

   return {
      a:2
      ,b
   }
   
}
export defalut test
```

- xxx.vue

```js
...
import test form './test.js'
setup(){
let a=test()
return {a}
}
```
#### isRef|unRef

```js
//#检查某个值是否为 ref。
let foo: unknown
if (isRef(foo)) {
  foo.value
}

//如果参数是 ref，则返回内部值，否则返回参数本身
//val = isRef(val) ? val.value : val //语法糖
const unwrapped = unref(foo)


```

#### toRef|toRefs

> 创建一个 ref 对象，其 value 值指向另一个对象中的某个属性,将对象中的某个属性单独提供给外部使用（这样创建的 ref 与其源属性保持同步：改变源属性的值将更新 ref 的值，反之亦然）

```js
import {onBeforeMount} from 'Vue'
setup(){
   let person=reative({
      name:'xx',
      a:1,
      b:{c:2}
   })

   let t=toRefs(person) //创建多个
   return {
      a:person.a,//不是响应式
      c:ref(person.b.c),//创建了一个新的实例对象，修改数据不会改变原数据
      c:toRef(person.b,'c')
      name:toRef(person,'name')
      ...toRefs(person)//全部
   }
}

```

#### shallowReative||shallowRef

> shallowReative 只处理对象最外层属性响应式,shallowRef 只处理基本数据响应式，不进行对象的响应式(提升性能)

```js
let person = shallowReative({
   a:1,
   b:{
      c:1
   }
})
let person2=shallowRef({a:1})
```

#### readonly||shallowReadonly

```js
let person=Reative({
a:1,
b:{
c:1
}
})
person =readonly(person)//深只读,保护数据
person =shallowReadonly(person)//浅只读
```
#### isProxy()||isReactive()||isReadonly()

- isReactive()​
检查一个对象是否是由 reactive() 或 shallowReactive() 创建的代理。
- isProxy()​
检查一个对象是否是由 reactive()、readonly()、shallowReactive() 或 shallowReadonly() 创建的代理。
- isReadonly()
检查传入的值是否为只读对象。只读对象的属性可以更改，但他们不能通过传入的对象直接赋值。
通过 readonly() 和 shallowReadonly() 创建的代理都是只读的，因为他们是没有 set 函数的 computed() ref。

#### toRaw||markRaw

> 将 reactive 生成的响应式数据转换为普通对象,ref 的不行

```js
let person=Reative({
   a:1,
   b:{
      c:1
   }
})
person =roRaw(person)//转化为原始数据
person =markRaw(person)//标记一个对象，使其永远不再成为响应式对象
```

#### customRef()​

>创建一个自定义的 ref，显式声明对其依赖追踪和更新触发的控制方式。
customRef() 预期接收一个工厂函数作为参数，这个工厂函数接受 track 和 trigger 两个函数作为参数，并返回一个带有 get 和 set 方法的对象。

一般来说，track() 应该在 get() 方法中调用，而 trigger() 应该在 set() 中调用。然而事实上，你对何时调用、是否应该调用他们有完全的控制权。
```js
import { customRef } from 'vue'

export function useDebouncedRef(value, delay = 200) {
  let timeout
  return customRef((track, trigger) => {
    return {
      get() {
        track()
        return value
      },
      set(newValue) {
        clearTimeout(timeout)
        timeout = setTimeout(() => {
          value = newValue
          trigger()
        }, delay)
      }
    }
  })
}
```
在组件中使用：

```vue
<script setup>
import { useDebouncedRef } from './debouncedRef'
const text = useDebouncedRef('hello')
</script>

<template>
  <input v-model="text" />
</template>
```



#### provide||inject

> 祖孙组件通信

- 祖组件

```js
let person=Reative({
   a:1,
   b:{
      c:1
   }
})
provide('test',person)
```

- 孙组件

```js
let x=inject('test)
```

### 内置组件

#### teleport
>
>是一个内置组件，它可以将一个组件内部的一部分模板“传送”到该组件的 DOM 结构外层的位置去。如：一个组件模板的一部分在逻辑上从属于该组件，但从整个应用视图的角度来看，它在 DOM 中应该被渲染在整个 Vue 应用外部的其他地方。

```html
<!--Teleport 接收一个 to prop 来指定传送的目标。to 的值可以是一个 CSS 选择器字符串，也可以是一个 DOM 元素对象，<Teleport> 挂载时，传送的 to 目标必须已经存在于 DOM 中-->
<teleport to="body">
<div>xxx</div>
</teleport>

<teleport to="#xxx">
<div>xxx</div>
</teleport>

<teleport :disabled="false" to="#xxx">
<div>xxx</div>
</teleport>
```

### vue3.0 响应式

```js
let person={
a:1,
b:'xxx'
}
const p=new Proxy(person,{
get(target,propName){//读取属性
//target:原对象
//propName：读取的属性名
//return target[propName]//普通操作
return Reflect.get(target,propName)//reflect 可减少写错误捕获
},
set(target,propName,value){//修改属性或增加属性时调用
//value：修改的值
//target[propName]=value
return Reflect.set(target,prop,value)
},
deleteProperty(target,propName){//删除属性
//return delete target[propName]
return Reflect.deleteProperty(target,prop)
},
})//代理
```

### vue3.0 生命周期

> beforeCreate->created->beforeMount->mounted->beforeUpdate->updated->beforeUnmount->unmounted

- beforeCreate===>setup()
- created===>setup()
- beforeMount===>onBeforeMount
- beforeUpdate===>onBeforeUpdate
- Updated===>onUpdated
- beforeUnmount===>onBeforeUnmount
- unmounted===>onUnmounted

```js
....
import {onBeforeMount} from 'Vue'
setup(){
onBeforeMount(()=>{
console.log()
})
},
//配置项写法优先级要低于组合式 api 写法
beforeMount(){
console.log('xxx')
}

```

## 路由

安装 Vue Router：`npm install vue-router`

- router/index.js

```js
// history模式
import {
    createRouter,
    createWebHashHistory,
} from 'vue-router'

import Home from '../pages/Home.vue'
import About from '../pages/About.vue'

const routes = [
// 路由的默认路径
    {
        path:'/',
        redirect:"/home"
    },
    {
        path: '/home',
         component: () => import('…/pages/Home.vue')
    },
    {
        path: '/about',
        component: About
    },
]

// 创建路由对象
const router = createRouter({
    history: createWebHashHistory(),
    routes
})
export default router;

```

- main.js

```js
import {
    createApp
} from 'vue'
import App from './App.vue'
import router from './router'
createApp(App).use(router).mount('#app')
```

- 使用

```js
import { useRouter } from 'vue-router';
  setup() {
    const router = useRouter();
    function goto(){
      router.push("/about");
    }
    return{
       goto  //一定要要放在return里才能在模板上面使用
    }
  }
```

## vuex

npm 安装 vuex。`npm install vuex@next --save`

- store/index.js

```js
import { createStore } from 'vuex'
export default createStore({
  state: {},
  getters: {},
  mutations: {},
  actions: {},
  modules: {}
})
```

首先从 vuex 中引入 useStore 函数，他的返回值就是一个 vuex 实例

```vue
<template>
  <h1>vuex中的数据{{ store.state.count }}</h1>
</template>
<script lang="ts">
import { defineComponent } from "vue"
import { useStore } from "vuex"
export default defineComponent({
  name: "index",
  setup() {
    const store = useStore()
    return { store }
  },
})
</script>
```

- vue3 的 router 文件引入 vuex

```js
//错误引入

import { useStore } from 'vuex'
const $store = useStore();
console.log($store) //undefined

//正确引入

import myStore from '@/store/index.js'
const $store = myStore;
console.log($store)
```

## less

安装：`npm i less-loader less --save-dev`

## sass

安装：`npm install node-sass sass-loader --save-dev`
文件使用 scss 后缀写 style 时声明 lang=scss

```html
<style scoped lang="scss">
//这里是scss不是sass，这个是因为scss是sass3引入进来的，scss语法有"{}",";"而sass没有，所以sass-loader对他们的解析是不一样的
# 这里写样式
</style>
```

可以声明使用变量

```scss
 $color:black;
 .container{
   a {
     color:$color;
   }
 }
```

可以封装函数，通过 @mixin 声明函数，该函数创建颜色 style,不传参默认黑色

```scss
@mixin create_color($color:black){
  color:$color;
}
```

通过@import 引入封装函数的文件，通过@include 调用函数

```scss
<style lang="scss" scoped>
     @import "src/assets/sass/mixin.scss";
    .container{
      a {
        @include create_color(red);
     }
    }
</style>
```

## antdesign

安装 antdesign

`npm i --save ant-design-vue@next`

```js
import { createApp } from 'vue'
import Antd from 'ant-design-vue';
import 'ant-design-vue/dist/antd.css';
const app = createApp(App)
app.use(Antd);
```

vue3 引用 ant-design-vue 编译运行时，可能会报错 can't read property undefine 错误

执行下面命令就行

`npm i --save ant-design-vue@next -S`

```js
vue3解析component: router-view失败
解决：
//app.use(router) 需放在app.mount('#app')前面  不然加载时router-view、router-link等未被渲染
```

### 清空数组的几个方式

```js
//使用ref()
const array = ref([1,2,3]);
array.value = [];

//使用slice,不过需要注意要使用ref
array.value = array.value.slice(0,0);

//length赋值为0,支持reactive,而且，这种只会触发一次watch
array.value.length = 0;

//使用splice,可以使用reactive,不过要注意，watch会触发多次
array.splice(0,array.length)
```

### Vue3 中废弃了$set 的概念

原因就是 Vue2 中的数据响应式是利用 object.definedProperty()实现的，它是无法深层监听数据的变化的。

而 Vue3，用的是 ES6 的 proxy，对数据响应式进行一个数据的代理。这个就厉害了啊，结合 Vue3 的 composition API。

Vue3 中的 reactivity API：

```js
reactive
readonly
ref
computed
```

如果想要让一个对象变为响应式数据，可以使用 reactive 或 ref

### vue3.0 中使用 nextTick

nextTick 是将回调推迟到下一个 DOM 更新周期之后执行。在更改了一些数据以等待 DOM 更新后立即使用它

```js
import { nextTick } from 'vue'
...
 setup () {
    let otherParam = reactive({
      showA:false
    })
    nextTick(()=>{
      otherParam.showA = true
    })
  return {
      otherParam

    }


}
```

## vue3 使用 ref 获取元素

```js
<template>
  <div ref="hello">xxxx</div>
</template>
<script setup lang="ts">
import { onMounted, ref } from "vue";
const hello = ref<any>(null);
onMounted(() => {
  console.log(hello.value); });
</script>
```

变量名称必须要与 ref 命名的属性名称一致。
通过 hello.value 的形式获取 DOM 元素。
必须要在 DOM 渲染完成后才可以获取 hello.value，否则就是 null

- v-for 中使用 ref

```vue
<template>
  <div v-for='i in 10' ref="hello">xxxx</div>
</template>
<script setup lang="ts">
import { onMounted, ref } from "vue";
const hello = ref([]);
onMounted(() => {
  console.log(hello.value); });
</script>
```

- ref 绑定函数

```vue
<template>
  <div v-for='i in 10' :ref="(el)=>{hello(el,i)}">xxxx</div>
</template>
<script setup lang="ts">
import { onMounted, ref } from "vue";
const hello = (el,index)=>{

};
</script>
```
