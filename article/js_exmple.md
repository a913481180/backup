---
title: JS实例
date: 2021-05-22 20:22:11
categories:
- web 
---

## 防抖

单位时间内，频繁触发事件，只执行最后一次事件（回城）

```js
function debounce(fn,t){
  let timer=null
  return ()=>{
    if(timer){
      clearTimeout(timer)
      timer=null
    }
    timer=setTimeout(()=>{fn()},t)
  }
}

```

## 节流

单位时间内，频繁触发事件，必须等待第一次事件执行完毕（技能）

```js
function throttle(fn,t){
  let timer=null
  return ()=>{
    if(timer===null){
    timer=setTimeout(()=>{
      fn()
      clearTimeout(timer)
      timer=null
    },t)}
  }
}
```

## 图片懒加载

- 图片到浏览器顶部的距离offsetTop< 视口高度clientHeight+滚动轴距离scrollTop

```html
    <div id="container" style="height:400px;position:relative;overflow:auto"> 
      <img class="rwo" src="image/load.gif" >
      <img class="rwo" src="image/load.gif" >
      <img class="rwo" data-src="image/load.gif" >
      <img class="rwo" data-src="image/load.gif" >
      <img class="rwo" data-src="image/load.gif" >
      <img class="rwo" data-src="image/load.gif" >
      <img class="rwo" data-src="image/load.gif" >
      <img class="rwo" data-src="image/load.gif" >
    </div>

    <script>

      // 父元素要使用定位,否则需要计算偏移
      window.onload = () => {
      let el = document.getElementById("container");
      let height = el.clientHeight;
      el.onscroll = (e) => {
        let scrollTop = e.target.scrollTop;
        let imgs = el.getElementsByTagName("img");
        for (let item of imgs) {
          if (!item.src) {
          let top = item.offsetTop;
            //图片顶部小于等于滚动条加父元素高度就出现在可视区域
            // console.log(top, height, scrollTop);
            if (top <= height + scrollTop + 10) {
              item.src = item.dataset.src;
            } else {
              break;
            }
          }
        }
      };
    };
    </script>
```

- Element.getBoundingClientRect() 方法返回元素的大小及其相对于视口的位置。我们可以取得它的top值，它的top值就是元素左上角到视口顶部的距离。

```js
//需要计算父容器偏移
 let el = document.getElementById("container");
      let height = el.clientHeight;
      let offset = el.offsetTop;
      el.onscroll = () => {
        let imgs = el.getElementsByTagName("img");
        for (let item of imgs) {
          if (!item.src) {
          let top = item.getBoundingClientRect().top;
            //图片顶部出现在可视区域
            if (top <= heigh+offset + 10) {
              item.src = item.dataset.src;
            } else {
              break;
            }
          }
        }
    };
```

## 轮播图

- 动态切换img标签的src
- 将所有图片横向排列，然后依次向左移动至显示区域内

```html
<body>
 <div
        style={{
          width: 210,
          height: 210,
          border: "10px solid skyblue",
          overflow: "hidden",
          position: "relative",
        }}
      >
        <ul
          id="uli"
          style={{
            listStyle: "none",
            position: "absolute",
            left: 0,
            padding: 0,
            margin: 0,
            whiteSpace: "nowrap",
            height: 200,
          }}
        >
          <li style={{ border: "red 5px solid", display: "inline-block" }}>
            <img width="200" src="./logo512.png" alt="err" />
          </li>
          <li style={{ border: "blue 5px solid", display: "inline-block" }}>
            <img width="200" src="./logo512.png" alt="err" />
          </li>
          <li style={{ border: "green 5px solid", display: "inline-block" }}>
            <img width="200" src="./logo512.png" alt="err" />
          </li>
          <li style={{ border: "pink 5px solid", display: "inline-block" }}>
            <img width="200" src="./logo512.png" alt="err" />
          </li>
          <li style={{ border: "red 5px solid", display: "inline-block" }}>
            <img width="200" src="./logo512.png" alt="err" />
          </li>
        </ul>
      </div>
</body>
<style>
  let ul = document.getElementById("uli");
    setInterval(() => {
      ul.style.left = parseInt(ul.style.left) - 210 + "px";
      if (ul.offsetLeft <= -210 * 4) {
        ul.style.left = "0px";
      }
    }, 2000);
 </style>
```

### 拖拽

- mousedown

记录鼠标按下的位置和被拖拽物体相对距离

```js
//clientX为鼠标位置，offsetLeft为盒子位置
var offsetX=e.clientX-node.offsetLeft;
var offsetY=e.clientY-node.offsetTop;
```

- mousemove

```js
//鼠标移动的距离减去相对距离=盒子移动的距离
node.style.left=e.clientX-offsetX+'px';
node.style.top=e.clientY-offsetY+'px';
```

- mouseup

取消拖拽

```html
<style>
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
    document.onmousedown=null;
    document.onmouseup=null;
  }
}
</style>
```

### 动态创建表单并提交

```js
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

```vue
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

## js获取选择的文本

⾮IE浏览器获取选中⽂本：

1. `document.getSelection().toString()`
如果想获取选中部分的html代码，由于getSelection()⽅法返回的是⼀个Range对象，就需要⽤到Range对象的cloneContents⽅法，cloneContents⽅法把Range对象的内容复制到⼀个DocumentFragment对象中，我们需要创建⼀个dom元素，然后将该DocumentFragment对象添加到创建的dom元素中，通过获取它的innerHTML来获取选择部分的html代码.
2. `document.selection.createRange().text`;
3. `document.selection.createRange().htmlText`;

## 数组深拷贝

```js
// es5:
// 方法一：适用 单层 数组嵌套的深拷贝
var ary2 = ary1.concat();
 
// 方法二：适用 多层 数组嵌套的深拷贝
var ary2 = JSON.parse(JSON.stringify(ary1));
　　//此方法适用于Oject的深度拷贝，因为Array属于Oject类型，所以也适用于此处；
　　//需要注意的是：作为Oject的深度拷贝时，要复制的function会直接消失，所以这个方法只能用在单纯只有数据的对象。
 
 
es6:
// 方法三：适用 单层 数组嵌套的深拷贝
var ary2 = [...ary1];
 
// 方法四：适用 单层 数组嵌套的深拷贝
var [...ary2] = ary1;
 
//方法五：通过递归实现 多层 的深拷贝

function deepCopy(source){
        if (typeof source !== "object") {
           return source;
        }
        if (source === null) {
            return source;
        }
        var newObj = source.constructor === Array ? [] : {};  //开辟一块新的内存空间
        for (var i in source) {
             newObj[i] = deepCopy(source[i]);
        }
        return newObj;
}
```

## js复制文本

简介
目前，一共有三种方法可以实现剪贴板操作。

1. `Document.execCommand()`方法
2. 异步的 `Clipboard API`
3. copy事件和paste事件

一、`Document.execCommand()` 方法
`Document.execCommand()`是操作剪贴板的传统方法，各种浏览器都支持。

它支持复制、剪切和粘贴这三个操作。

```js
document.execCommand('copy')//（复制）
document.execCommand('cut')//（剪切）
document.execCommand('paste')//（粘贴）
```

（1）复制操作

复制时，先选中文本，然后调用document.execCommand('copy')，选中的文本就会进入剪贴板。

```js
const inputElement = document.querySelector('#input');
inputElement.select();
document.execCommand('copy');
```

上面示例中，脚本先选中输入框inputElement里面的文字（inputElement.select()），然后document.execCommand('copy')将其复制到剪贴板。

注意，复制操作最好放在事件监听函数里面，由用户触发（比如用户点击按钮）。如果脚本自主执行，某些浏览器可能会报错。

（2）粘贴操作

粘贴时，调用document.execCommand('paste')，就会将剪贴板里面的内容，输出到当前的焦点元素中。

```js
const pasteText = document.querySelector('#output');
pasteText.focus();
document.execCommand('paste');
```

（3）缺点

`Document.execCommand()`方法虽然方便，但是有一些缺点。

首先，它只能将选中的内容复制到剪贴板，无法向剪贴板任意写入内容。

其次，它是同步操作，如果复制/粘贴大量数据，页面会出现卡顿。有些浏览器还会跳出提示框，要求用户许可，这时在用户做出选择前，页面会失去响应。

为了解决这些问题，浏览器厂商提出了异步的 Clipboard API。

二、异步 `Clipboard` API
Clipboard API是下一代的剪贴板操作方法，比传统的document.execCommand()方法更强大、更合理。

它的所有操作都是异步的，返回 Promise 对象，不会造成页面卡顿。而且，它可以将任意内容（比如图片）放入剪贴板。

`navigator.clipboard`属性返回 Clipboard 对象，所有操作都通过这个对象进行。

```js
const clipboardObj = navigator.clipboard;
```

如果navigator.clipboard属性返回undefined，就说明当前浏览器不支持这个 API。

由于用户可能把敏感数据（比如密码）放在剪贴板，允许脚本任意读取会产生安全风险，所以这个 API 的安全限制比较多。

首先，Chrome 浏览器规定，只有 `HTTPS` 协议的页面才能使用这个 API。不过，开发环境（localhost）允许使用非加密协议。

其次，调用时需要明确获得用户的许可。权限的具体实现使用了 Permissions API，跟剪贴板相关的有两个权限：clipboard-write（写权限）和clipboard-read（读权限）。
"写权限"自动授予脚本，而"读权限"必须用户明确同意给予。

也就是说，写入剪贴板，脚本可以自动完成，但是读取剪贴板时，浏览器会弹出一个对话框，询问用户是否同意读取。`

另外，需要注意的是，脚本读取的总是当前页面的剪贴板。这带来的一个问题是，如果把相关的代码粘贴到开发者工具中直接运行，可能会报错，因为这时的当前页面是开发者工具的窗口，而不是网页页面。

```js
(async () => {
  const text = await navigator.clipboard.readText();
  console.log(text);
})();
```

如果你把上面的代码，粘贴到开发者工具里面运行，就会报错。因为代码运行的时候，开发者工具窗口是当前页，这个页面不存在 Clipboard API 依赖的 DOM 接口。一个解决方法就是，相关代码放到setTimeout()里面延迟运行，在调用函数之前快速点击浏览器的页面窗口，将其变成当前页。

```js
setTimeout(async () => {
  const text = await navigator.clipboard.readText();
  console.log(text);
}, 2000);
```

上面代码粘贴到开发者工具运行后，快速点击一下网页的页面窗口，使其变为当前页，这样就不会报错了。

三、Clipboard 对象
Clipboard 对象提供了四个方法，用来读写剪贴板。它们都是异步方法，返回 Promise 对象。

4.1 Clipboard.readText()
Clipboard.readText()方法用于复制剪贴板里面的文本数据。

```js
document.body.addEventListener(
  'click',
  async (e) => {
    const text = await navigator.clipboard.readText();
    console.log(text);
  }
)
```

上面示例中，用户点击页面后，就会输出剪贴板里面的文本。注意，浏览器这时会跳出一个对话框，询问用户是否同意脚本读取剪贴板。

如果用户不同意，脚本就会报错。这时，可以使用try…catch结构，处理报错。

```js
async function getClipboardContents() {
  try {
    const text = await navigator.clipboard.readText();
    console.log('Pasted content: ', text);
  } catch (err) {
    console.error('Failed to read clipboard contents: ', err);
  }
}
```

4.2 Clipboard.read()
Clipboard.read()方法用于复制剪贴板里面的数据，可以是文本数据，也可以是二进制数据（比如图片）。该方法需要用户明确给予许可。

该方法返回一个 Promise 对象。一旦该对象的状态变为 resolved，就可以获得一个数组，每个数组成员都是 ClipboardItem 对象的实例。

```js
async function getClipboardContents() {
  try {
    const clipboardItems = await navigator.clipboard.read();
    for (const clipboardItem of clipboardItems) {
      for (const type of clipboardItem.types) {
        const blob = await clipboardItem.getType(type);
        console.log(URL.createObjectURL(blob));
      }
    }
  } catch (err) {
    console.error(err.name, err.message);
  }
}
```

ClipboardItem 对象表示一个单独的剪贴项，每个剪贴项都拥有ClipboardItem.types属性和ClipboardItem.getType()方法。

ClipboardItem.types属性返回一个数组，里面的成员是该剪贴项可用的 MIME 类型，比如某个剪贴项可以用 HTML 格式粘贴，也可以用纯文本格式粘贴，那么它就有两个 MIME 类型（text/html和text/plain）。

ClipboardItem.getType(type)方法用于读取剪贴项的数据，返回一个 Promise 对象。该方法接受剪贴项的 MIME 类型作为参数，返回该类型的数据，该参数是必需的，否则会报错。

4.3 Clipboard.writeText()
Clipboard.writeText()方法用于将文本内容写入剪贴板。

```js
document.body.addEventListener(
‘click’,
async (e) => {
await navigator.clipboard.writeText(‘Yo’)
}
)
```

上面示例是用户在网页点击后，脚本向剪贴板写入文本数据。

该方法不需要用户许可，但是最好也放在try…catch里面防止报错。

```js
async function copyPageUrl() {
  try {
    await navigator.clipboard.writeText(location.href);
    console.log('Page URL copied to clipboard');
  } catch (err) {
    console.error('Failed to copy: ', err);
  }
}
```

4.4 Clipboard.write()
Clipboard.write()方法用于将任意数据写入剪贴板，可以是文本数据，也可以是二进制数据。

该方法接受一个 ClipboardItem 实例作为参数，表示写入剪贴板的数据。

```js
try {
  const imgURL = 'https://dummyimage.com/300.png';
  const data = await fetch(imgURL);
  const blob = await data.blob();
  await navigator.clipboard.write([
    new ClipboardItem({
      [blob.type]: blob
    })
  ]);
  console.log('Image copied.');
} catch (err) {
  console.error(err.name, err.message);
}
```

上面示例中，脚本向剪贴板写入了一张图片。注意，Chrome 浏览器目前只支持写入 PNG 格式的图片。

ClipboardItem()是浏览器原生提供的构造函数，用来生成ClipboardItem实例，它接受一个对象作为参数，该对象的键名是数据的 MIME 类型，键值就是数据本身。

下面的例子是将同一个剪贴项的多种格式的值，写入剪贴板，一种是文本数据，另一种是二进制数据，供不同的场合粘贴使用。

```js
function copy() {
const image = await fetch(‘kitten.png’);
const text = new Blob([‘Cute sleeping kitten’], {type: ‘text/plain’});
const item = new ClipboardItem({
‘text/plain’: text,
‘image/png’: image
});
await navigator.clipboard.write([item]);
}
```

五、copy 事件，cut 事件
用户向剪贴板放入数据时，将触发copy事件。

下面的示例是将用户放入剪贴板的文本，转为大写。

```js
const source = document.querySelector('.source');

source.addEventListener('copy', (event) => {
  const selection = document.getSelection();
  event.clipboardData.setData('text/plain', selection.toString().toUpperCase());
  event.preventDefault();
});
```

上面示例中，事件对象的clipboardData属性包含了剪贴板数据。它是一个对象，有以下属性和方法。

```js
Event.clipboardData.setData(type, data)：修改剪贴板数据，需要指定数据类型。
Event.clipboardData.getData(type)：获取剪贴板数据，需要指定数据类型。
Event.clipboardData.clearData([type])：清除剪贴板数据，可以指定数据类型。如果不指定类型，将清除所有类型的数据。
Event.clipboardData.items：一个类似数组的对象，包含了所有剪贴项，不过通常只有一个剪贴项。
```

下面的示例是拦截用户的复制操作，将指定内容放入剪贴板。

```js
const clipboardItems = [];

document.addEventListener(‘copy’, async (e) => {
e.preventDefault();
try {
let clipboardItems = [];
for (const item of e.clipboardData.items) {
if (!item.type.startsWith(‘image/’)) {
continue;
}
clipboardItems.push(
new ClipboardItem({
[item.type]: item,
})
);
await navigator.clipboard.write(clipboardItems);
console.log(‘Image copied.’);
}
} catch (err) {
console.error(err.name, err.message);
}
});
```

上面示例中，先使用e.preventDefault()取消了剪贴板的默认操作，然后由脚本接管复制操作。

cut事件则是在用户进行剪切操作时触发，它的处理跟copy事件完全一样，也是从Event.clipboardData属性拿到剪切的数据。

六、paste 事件
用户使用剪贴板数据，进行粘贴操作时，会触发paste事件。

下面的示例是拦截粘贴操作，由脚本将剪贴板里面的数据取出来。

```js
document.addEventListener('paste', async (e) => {
  e.preventDefault();
  const text = await navigator.clipboard.readText();
  console.log('Pasted text: ', text);
});
```

一、原理：
利用 input 框的 .select() 事件选中需要复制的文本。
利用 document.execCommand("copy") 来将文字复制到剪切板中。【MDN已废弃,部分浏览器可使用】

## js毫秒如何转时分秒

```js
formatDuring: function(mss) {

  var days = parseInt(mss / (1000 * 60 * 60 * 24));

  var hours = parseInt((mss % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));

  var minutes = parseInt((mss % (1000 * 60 * 60)) / (1000 * 60));

  var seconds = (mss % (1000 * 60)) / 1000;

  return days + " 天 " + hours + " 小时 " + minutes + " 分钟 " + seconds + " 秒 ";

}
```

## 关闭页面发送请求

- 同步ajax

在用户将页面完全关闭前，有时需要向后台传输一些必要数据（比如：用户行为分析数据）。为了避免数据丢失，一些网站会使用同步的 `XMLHttpRequest()` 请求，直到数据完全传递到服务器，再关闭页面。这种处理方式，可能会导致诸如页面延迟几秒后再关闭的糟糕体验，其实我们有更好的解决这类问题的方式。
XMLHttpRequest 规范提到 同步 XMLHttpRequest 已经计划从 Web 平台删除，开发者禁止将

```js
new XMLHttpRequest().open(method, url [, async = true [, username = null [, password = null]]]) 
```

方法中 async 参数赋值为 false 使用。Chrome 80 中迈出了第一步，开始禁止在如下这些事件处理器中使用同步 XMLHttpRequest：`beforeunload`、`unload`、`pagehide`、`visibilitychange`。Webkit 最近也有一个实现此行为的提交。

- `fetch keepalive`
Fetch API  提供了一套健壮的与服务器端交互的方式，提供了跨越不同平台 API 的一致接口。它提供了一个选项 `keepalive`，保证不管发送请求的页面关闭与否，请求都会持续直到结束。

```js
window.addEventListener('unload', {
  fetch('/siteAnalytics', {
    method: 'POST',
    body: getStatistics(),
    keepalive: true
  });
}
```

fetch() 方法的优点是可以更好地控制发送到服务器的内容。fetch() 方法会返回一个 Promise 对象，resolve 值是 Response 对象。 本例中，我没有使用 fetch() 返回值做任何其他的事情，是因为这个请求发生在页面卸载之时，因此并不一定会保证执行。

- SendBeacon()

SendBeacon() 方法底层的使用的是 Fetch API。这样就能明白为什么它也有 `64 KB`  的上传数据限制，也能明白为什么它还能在页面卸载后继续请求。它的主要优点是简单，只要用一行代码就能搞定。

```js
window.addEventListener('unload', {
  navigator.sendBeacon('/siteAnalytics', getStatistics());//post请求
}
```

## v-for遍历对象获取下标

```js
v-for="(value,key,index) in obj"

```

## 去重

- 方法1：先排序后去重

1）对数组进行双重循环；如果外循环的值大于内循环的值；就利用第三方变量对其交换值 ；这儿也可以用 sort

```js
a.sort((a,b)=>{return a-b}); // 排序
```

2）当排序完成以后；再次循环进行比较相邻的两个值

```js
var a =[1,2,3,4,5,2,3,5,5,5,5]
function info(a){
            var arr = []
            for(var i=0;i<a.length;i++){
                for(var j=0;j<a.length-1;j++){
                    if(a[i]>a[j]){
                        var list = a[i]
                        a[i] = a[j]
                        a[j] = list
                    }
                }
            }
            for(var e=0;e<a.length;e++){
                if(a[e] != a[e+1]){
                    arr.push(a[e])
                }
            }
            return arr
 }
        let list = info(a)
        console.log(list); // 输出结果 [1, 2, 3, 4, 5]
```

- 方法2：indexOf去重

此去重主要是利用 indexOf 去查找是否有相同的值；如果没有就返回-1

```js
var a =[1,2,3,4,5,2,3,5,5,5,5]
function info(arr){
            let a = []
            for(var i in arr){
                if(a.indexOf(arr[i])==-1){
                    a.push(arr[i])
                }
            }
            return a
  }
        let list = info(a)
        console.log(list); // 输出结果 [1, 2, 3, 4, 5]
 ```

- 方法3：双重for循环去重

这儿需要注意一点的就是内循环的时候不要 a.length-1；否则的话就会出现[1, 2, 3, 4, 5, 5]这个结果

```js
var a =[1,2,3,4,5,2,3,5,5,5,5]
for(let i=0;i<a.length;i++){
            for(let j=i+1;j<a.length;j++){
                if(a[i]===a[j]){
                a.splice(j,1);
                    j--;
                }
            }
     }
  console.log(a); // 输出结果 [1, 2, 3, 4, 5]
```

方法4：Set去重
set去重应该说是简单的去重方式了

```js
var a = [1,2,3,4,5,2,3,5,5,5,5]
let list = [...new Set(a)]
console.log(list); // 输出结果 [1, 2, 3, 4, 5]
```

## 判断是否是pc设备

```js
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

## 树

列表数据转树型结构

话不多说，直接进入正题~
列表数据中字段如下：

```js
[
    {
        id: 1,
        text: '节点1',
        parentId: 0 //这里用0表示为顶级节点
    },
    {
        id: 2,
        text: '节点1_1',
        parentId: 1 //通过这个字段来确定子父级
    }
    ...   
]
```

我们预期的结果：

```js
[
    {
        id: 1,
        text: '节点1',
        parentId: 0,
        children: [
            {
                id:2,
                text: '节点1_1',
                parentId:1
            }
        ]
    }
]
```

先说一下实现的思路：
1、先把列表数据转成以id为key，节点本身为value的对象temp；
2、定义空列表treeData，然后遍历temp对象
2-1、如果当前节点不是顶级节点（parentId不为0），那找到以parentId作为key的节点，并把当前节点push到该节点的children中；
2-2、如果当前节点是顶级节点，只需把该节点push到treeData中,当temp遍历结束后，treeData就是最后的树型数据。

具体代码：

```js
var temp = {};
var treeData = [];
for(var i=0; i<data.length; i++){
    temp[data[i].id] = data[i];
}
for(var i in temp){
     if(temp[i].parentId != "") {
         if(!temp[temp[i].parentId].children) {
             temp[temp[i].parentId].children = new Array();
         }
         temp[temp[i].parentId].children.push(temp[i]);
     } else {
         treeData.push(temp[i]);
     }
}
```

这里特别说一下有个稍微底层一点的东东，也是该方法实现的关键所在。。
大家想一想，逐步跟一下这个过程，在遍历temp时，假如顶级节点1先被push到了treeData中，而后面又有子节点被添加到了节点1中，那么这个子节点最后能不能被带入到treeData中的节点1里呢？答案是可以的~为节点1本身是个Object对象，它指向了一个内存地址，而无论后续节点1怎么改变，被push到treeData中的节点1都会指向同一个地址，所以节点1被什么时候push都没有关系，最后就是完整的数据。

## 前端实现吸顶的三种方式

- 一、position: sticky
设置粘性定位并且添加某个方位才可生效，但是兼容性较差。

- 二、动态修改元素的position
监听页面的滚动，比较 元素初始状态距离顶部的高度offsetTop 与 页面此时的滚动高度 。当后者大于前者时，修改元素的position方式为fixed。

- 三、障眼法
复制一个相同的元素，将其固定于顶部并让其先隐藏。
监听页面的滚动，比较 元素初始状态距离顶部的高度offsetTop 与 页面此时的滚动高度 ，当后者数值大于前者数值时，原来的元素刚刚好触顶，且正与新元素的位置重合。
此时我们在监听方法中，让这个新元素显示。
当页面继续滚动的时候，原来的元素已经被滚上去了，而新的元素一直被固定在顶部并且此时已经显示出来，所以这个过程发生的时候，会让人有一种元素改变了定位方式的错觉。

## 如何强制完全重新加载,location.reload（true）已弃用

由于该forceReload参数已被弃用，它也已经被一些浏览器忽略了。

`location.reload();`如果您想执行强制重新加载（如使用Ctrl+完成F5）以便从服务器而不是浏览器缓存重新加载所有资源，则使用 only不是解决方案。

```js
//执行POST对当前位置的请求，因为这总是会使浏览器重新加载所有内容。
//location.reload()只是执行一个GET请求。于是，我来到了解决办法将一个空的形式对角应用与method="POST"和action="https://example.org/"，然后提交从代码隐藏表单想要当。
forceReload() {
    if(environment.production) {
        const form = document.createElement('form');
        form.method = "POST";
        form.action = location.href;
        document.body.appendChild(form);
        form.submit();
    } else {
        window.location.reload();
    }
}
```

```js
//由于window.location.reload(true)已被弃用。您可以使用：
window.location.href = window.location.href,
//强制重新加载并清除缓存。
```
