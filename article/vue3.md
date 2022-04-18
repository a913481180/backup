---
title: vue3
date: 2020-11-11 20:33:33
categories:
  - web
---

# VUE3.0

## 安装

1. 查看 vue-cli 版本，必须大于 4.5：`vue -V`
   - 升级 vue-cli：`npm install -g @vue/cli
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

vue2 中的 data、methods、computed 写在 setup 中可以访问到 setup 中的属性和方法，但 setup 却不能访问到 vue2 中的配置。所以不建议混用。
如有重名，setup 中的优先，setup 中不能加 async，否则 return 返回的是 promise ，模版将读取不到

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

### vue3.0响应式
```
let person={
   a:1,
   b:'xxx'
}
const p=new Proxy(person,{

get(target,propName){//读取属性
  //target:原对象
  //propName：读取的属性名 
   return target[propName]
},
set(target,propName,value){//修改属性或增加属性时调用
   //value：修改的值
   target[propName]=value
},
deleteProperty(target,propName){//删除属性
return delete target[propName]
},

})//代理
```

Reflect