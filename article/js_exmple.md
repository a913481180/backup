---
title: JS实例
date: 2021-05-22 20:22:11
categories:
- web 
---

# 实例

## 判断是否是pc设备

```
    function IsPC() {
        var userAgentInfo = navigator.userAgent;
        console.log(userAgentInfo);
        var Agents = ["Android", "iPhone","SymbianOS", "Windows Phone", "iPod"];
        var flag = true;
        for (var v = 0; v < Agents.length; v++) {
            if (userAgentInfo.indexOf(Agents[v]) > 0) {
                flag = false;
                break;
            }
        }
        if(window.screen.width>=768){
             flag = true;
        }
        return flag;
    }
```


## 轮播图

```
window.onload()=function(){
var oUl1=document.getElementById("ul1");
var oDiv1=document.getElementById("div1");
//将图片添加到末尾
oUl1.innerHTML+=oUl1.innerHTML;
//重新设置一下ul的宽
oUl1.style.width=220*8+"px";
setInterval(function(){
//让ul向左运动一个图片位置
startMove(oUl1,{left:oUl1.offsetLeft-220},function(){
if(oUl1.offsetLeft<=-oUl1.offsetWidth/2){
oUl1.style.left="opx";
}
})
},2000);
```


### 拖拽

- mousedown 

记录鼠标按下的位置和被拖拽物体相对距离

```
//clientX为鼠标位置，offsetLeft为盒子位置
var offsetX=e.clientX-node.offsetLeft;
var offsetY=e.clientY-node.offsetTop;
```


- mousemove

```
//鼠标移动的距离减去相对距离=盒子移动的距离
node.style.left=e.clientX-offsetX+'px';
node.style.top=e.clientY-offsetY+'px';
```

- mouseup

取消拖拽

*实例：*

```
//父盒子与子盒子要有定位
window.onload=function(){
var oDiv=document.getElementById("div1");
oDiv.onmousedown=function(ev){
var e=ev||window.event;
var offsetX=e.clientX-oDiv.offsetLeft;
var offsetY=e.clientY-oDiv.offsetTop;

document.onmousemove=function(ev){
	var e=ev||window.event;
oDiv.style.left=e.clientX-offsetX+"px";
oDiv.style.top=e.clientY-offsetY+"px";
}}
//绑定到document上，防止鼠标移动过快,离开节点
document.onmouseup=function(){
document.onmousemove=null;
}
}
```
### 动态创建表单并提交
```
// JavaScript 构建一个 form 
function MakeForm() 
{ 
  // 创建一个 form 
  var form1 = document.createElement("form"); 
  form1.id = "form1"; 
  form1.name = "form1"; 
  // 添加到 body 中 
  document.body.appendChild(form1); 
  // 创建一个输入 
  var input = document.createElement("input"); 
  // 设置相应参数 
  input.type = "text"; 
  input.name = "value1"; 
  input.value = "1234567"; 
  // 将该输入框插入到 form 中 
  form1.appendChild(input); 
  // form 的提交方式 
  form1.method = "POST"; 
  // form 提交路径 
  form1.action = "/servlet/info"; 
  // 对该 form 执行提交 
  form1.submit(); 
  // 删除该 form 
  document.body.removeChild(form1); 
} 
```

### 虚拟列表

```
<template>
  <div class="cube-recycle-list" id="container">
    <!-- 可见区域列表 -->
    <div :style="{ height: heights + 'px' }">
      <div
        v-for="(item, index) in visibleItems"
        :key="index"
        class="cube-recycle-list-item"
        :style="{ transform: 'translate(0,' + item.top + 'px)' }"
      >
      <!-- 内容 -->
        <div
          :style="
            'height: ' +
            (index + startIndex + 1) * 10 +
            'px; width: 100%; background-color: aqua'
          "
        >
          {{ index + startIndex }}
        </div>
      </div>
    </div>

      <!-- 预加载,用于动态获取宽高 -->
    <div
      class="cube-recycle-list-item"
      v-for="(item, index) in preItems"
      :ref="(el) => itemDom(el, item.index, index)"
    >
      <!-- 内容 -->
      <div
        :style="
          'height: ' +
          (item.index + 1) * 10 +
          'px; width: 100%; background-color: aqua'
        "
      >
        {{ item.index }}
      </div>
    </div>
  </div>
</template>

<script>
import { nextTick, ref, computed, onMounted, onBeforeUnmount } from "vue";

export default {
  name: "List",
  setup() {
    let n = 0;
    let items = ref([]);
    let list = ref([]);
    let loadings = ref(false);
    let heights = ref(0);
    let pageNo = ref(0);
    let startIndex = ref(0);
    let refs = ref([]);
    let itemDom = (el, index, i) => {//获取预加载的元素节点
      if (i == 0) {
        refs.value = [];
      }
      refs.value.push({ index, el });
    };
    let size = ref(15); //每页
    let visibleItems = computed(() => {
      return items.value.slice(
        Math.max(0, startIndex.value - size.value),
        Math.min(items.value.length, startIndex.value + size.value)
      );
    });
    let preItems = computed(() => {//先加载元素获取其宽高，然后过滤去掉元素
      return items.value.filter(function (item) {
        return !item.height;
      });
    });
    //更新每个item顶部距离和 整个列表的高
    let updateItemTop = () => {
      let height = 0;
      let pre;
      let current;
      // loop all items to
      for (let i = 0; i < items.value.length; i++) {
        pre = items.value[i - 1];
        current = items.value[i];
        // it is empty in array
        /* istanbul ignore if */
        if (!items.value[i]) {
          height += 0;
        } else {
          current.top = pre ? pre.top + pre.height : 0;
          height += current.height;
        }
      }
      heights.value = height;
    };
    //更新可见区域的起始序号
    let updateStartIndex = () => {
      // update visible items start index
      let top = document.getElementById("container").scrollTop;
      let item;
      for (let i = 0; i < items.value.length; i++) {
        item = items.value[i];
        if (!item || item.top > top) {
          startIndex.value = Math.max(0, i - 1);
          break;
        }
      }
    };
    ///加载每一页数据的高度
    let loadItemsByIndex = () => {
      const start = (pageNo.value - 1) * size.value;
      const end = pageNo.value * size.value;
      let promiseTasks = [];
      let item;
      for (let i = start; i < end; i++) {
        //处理原数据
        item = items.value[i];
        if (!item) {
          items.value[i] = {
            index: i,
            data: list.value[i] || {},
            height: 0,
            top: -1000,
          };
          promiseTasks.push(
            nextTick().then(() => {
              /////  动态获取每个item的高
              for (let j = 0; j < refs.value.length; j++) {
                if (refs.value[j].index == i) {
                  let node = refs.value[j].el;
                  if (node) {
                    items.value[i].height = node.offsetHeight;
                  } else {
                    items.value[i].height = 0;
                  }
                  break;
                }
              }
            })
          );
        }
      }
      //加载一页的高和item的顶
      window.Promise.all(promiseTasks).then(() => {
        console.log(items.value);
        updateItemTop();
        updateStartIndex();
      });
    };

    //获取数据
    let load = () => {
      if (!loadings.value) {
        console.log("load");
        let arr = [];
        const promiseFetch = new Promise((resolve) => {
          setTimeout(() => {
            arr = [];
            for (let i = 0; i < size.value; i++) {
              arr.push({ n: n++ });
            }
            resolve(arr);
          }, 1000);
        });
        pageNo.value++;
        loadings.value = true; //加载中
        promiseFetch.then((res) => {
          loadings.value = false; //取消加载中
          //追加数据
          list.value.push(...res);
          ///加载item的高度
          loadItemsByIndex();
          //if (res.length < size.value) {
          //没有更多数据
          // updateItemTop();
          //updateStartIndex();
          // }
        });
      }
    };
    let _onScroll = () => {
      if (
        document.getElementById("container").scrollTop +
          document.getElementById("container").offsetHeight >
        heights.value - 100
      ) {
        //判断是否到底
        load();
      }
      //不停更新可见区域索引
      updateStartIndex();
    };
    let _onResize = () => {
      //重新加载全部数据的高度
      items.value.forEach((item) => {
        item.loaded = false;
      });
    };
    onMounted(() => {
      let node = document.getElementById("container");

      console.log(node);
      node.addEventListener("scroll", _onScroll);
      window.addEventListener("resize", _onResize);
      load();
    });
    onBeforeUnmount(() => {
      document
        .getElementById("container")
        .removeEventListener("scroll", _onScroll);
      window.removeEventListener("resize", _onResize);
    });
    return {
      items,
      heights,
      startIndex,
      itemDom,
      size,
      visibleItems,
      preItems,
    };
  },
};
</script>
<style scoped>
.cube-recycle-list {
  position: relative;
  height: 100%;
  overflow-x: hidden;
  overflow-y: auto;
}
.cube-recycle-list-item {
  width: 100%;
  position: absolute;
  box-sizing: border-box;
}
</style>

``` 


