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

## main.js

```
///引用的不再是vue的构造函数
import { createApp } from 'vue'
import App from './App.vue'
//创建应用实例对象
const app=createApp(App);
app.mount('#app')
//app 比 vm 轻量
//cosnt vm =new  Vue({
//   render(h=>h(App))
//})
//vm.$mount('#app')
```

## App.vue

> vue3 组件中的模版结构可以没有 div 根标签

### composition API 组合式 api

#### setup

> vue2 中的 data、methods、computed 写在 setup 中可以访问到 setup 中的属性和方法，但 setup 却不能访问到 vue2 中的配置。所以不建议混用。
> 如有重名，setup 中的优先，非异步组件，setup 中不能加 async，否则 return 返回的是 promise ，模版将读取不到
> setup 在 beforeCreate 前会执行一次，其中的 this 为 undefined

```
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

```
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

```
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

```
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

```
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

```
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

#### 自定义 hock 函数

> 本质是个函数，将 setup 中的函数进行封装,类似 vue2 中 mixin

- test.js

```
import {ref} from 'Vue'
function test(){
   return {
      a:2
   }
}
export defalut test
```

- xxx.vue

```
...
import test form './test.js'
setup(){
let a=test()
return {a}
}
```

#### toRef

> 创建一个 ref 对象，其 value 值指向另一个对象中的某个属性,将对象中的某个属性单独提供给外部使用

```
import {onBeforeMount} from 'Vue'
setup(){
   let person=reative({
      name:'xx',
      a:1,
      b:{c:2}
   })

   let t=toRefs(person)
   return {
      a:person.a,//不是响应式
      c:ref(person.b.c),//创建了一个新的实例对象，修改数据不会改变原数据
      c:toRef(person.b,'c')
      name:toRef(person,'name)
      ...toRefs(person)//全部
   }
}

```

#### shallowReative||shallowRef

> shallowReative 只处理对象最外层属性响应式,shallowRef 只处理基本数据响应式，不进行对象的响应式(提升性能)

```
let person = shallowReative({
   a:1,
   b:{
      c:1
   }
})
let person2=shallowRef({a:1})
```

#### readonly||shallowReadonly

```
let person=Reative({
a:1,
b:{
c:1
}
})
person =readonly(person)//深只读,保护数据
person =shallowReadonly(person)//浅只读
```

#### toRaw||markRaw

> 将 reactive 生成的响应式数据转换为普通对象,ref 的不行

```
let person=Reative({
a:1,
b:{
c:1
}
})
person =roRaw(person)//转化为原始数据
person =markRaw(person)//标记一个对象，使其永远不再成为响应式对象
```

#### provide||inject

> 祖孙组件通信

- 祖组件

```
let person=Reative({
a:1,
b:{
c:1
}
})
provide('test',person)
```

- 孙组件

```
let x=inject('test)
```

### teleport

```
<teleport to="body">
<div>xxx</div>
</teleport>
<teleport to="#xxx">
<div>xxx</div>
</teleport>
```

### vue3.0 响应式

```
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

- 组合 api

```

beforeCreate===>setup()
created===>setup()
beforeMount===>onBeforeMount
beforeUpdate===>onBeforeUpdate
Updated===>onUpdated
beforeUnmount===>onBeforeUnmount
unmounted===>onUnmounted
```

```
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

安装 Vue Router：`npm install vue-router@4`

- router/index.js

```
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

```
import {
    createApp
} from 'vue'
import App from './App.vue'
import router from './router'
createApp(App).use(router).mount('#app')
```
- 使用
```
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
```
import { createStore } from 'vuex'
export default createStore({
  state: {},
  getters: {},
  mutations: {},
  actions: {},
  modules: {}
})
```

## less
安装：`npm i less-loader less --save-dev`
## antdesign
安装antdesign

`npm i --save ant-design-vue@next`

```
import { createApp } from 'vue'
import Antd from 'ant-design-vue';
import 'ant-design-vue/dist/antd.css';

const app = createApp(App) 
app.use(Antd);
```

vue3引用ant-design-vue编译运行时，可能会报错can't read property undefine错误

执行下面命令就行

`npm i --save ant-design-vue@next -S`

```
vue3解析component: router-view失败  

解决：

//app.use(router) 需放在app.mount('#app')前面  不然加载时router-view、router-link等未被渲染
```

### 清空数组的几个方式
```
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