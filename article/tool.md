---
title: DOM/BOM
date: 2021-01-01 10:12:33
categories: 
- web
tags:
- 前端
---

## DOM（文档对象模型document object model）
>
>用来提供一套操作网页内容的功能
HTML DOM 方法是您能够（在 HTML 元素上）执行的动作。
HTML DOM 属性是您能够设置或改变的 HTML 元素的值。
dom树：以树状结构，体现标签与标签之间的关系

- 通过这个对象模型，JavaScript 获得创建动态 HTML 的所有力量：

  - JavaScript 能改变页面中的所有 HTML 元素
  - JavaScript 能改变页面中的所有 HTML 属性
  - JavaScript 能改变页面中的所有 CSS 样式
  - JavaScript 能删除已有的 HTML 元素和属性
  - JavaScript 能添加新的 HTML 元素和属性
  - JavaScript 能对页面中所有已有的 HTML 事件作出反应
  - JavaScript 能在页面中创建新的 HTML 事件

- 节点类型:
  - 元素节点：<div></div>
  - 属性节点：id=“div1”
  - 文本节点： 文本

### 结点的获取

- document.getElementById(id);

- node.getElementsByTagName(标签名);
通过标签名获取符合条件的元素节点，返回：伪数组

```js
//获取ol下的li节点
bar oOl=document.getElementById("ol1");
var lis=oOl.getElementsByTagName("li");
//返回的是数组
```

- node.getElementsByClassName(class名);  
通过class名字获取符合条件的元素节点；(IE8以下不兼容）

- document.getElementByName(name属性的值);
通过name属性的值获取符合条件的元素节点，一般用于表单元素,因为其他元素使用name属性没用

- document.querySelector(CSS选择器格式字符串);
返回一个元素节点，找到符合条件的第一个元素节点

```js
window.onload=function(){
//id=ol1
var node=document.querySelector("#ol1");

//tagName='li'
var node=document.querySelector("li");

//class=box
var node =document.querySelector(".box");
node.style.backgroundColor='red';
}
```

- document.querySelectorAll(CSS选择器格式字符串);
返回一个数组

#### js获取子节点的方式

1. 通过获取dom方式直接获取子节点
其中test的父标签id的值，div为标签的名字。getElementsByTagName是一个方法。返回的是一个数组。在访问的时候要按数组的形式访问。

```js
var a = document.getElementById("test").getElementsByTagName("div");
```

2. 通过`childNodes`获取子节点

使用childNodes获取子节点的时候，childNodes返回的是子节点的集合，是一个数组的格式。他会把文本，换行和空格也当成是节点信息。

```js
var b =document.getElementById("test").childNodes;
```

为了不显示不必须的换行的空格，我们如果要使用childNodes就必须进行必要的过滤。通过正则表达式式取掉不必要的信息。下面是过滤掉

```js
//去掉换行的空格
for(var i=0; i<b.length;i++){
  if(b[i].nodeName == "#text" && !/\s/.test(b.nodeValue)){
    document.getElementById("test").removeChild(b[i]);
  }
}
//打印测试
for(var i=0;i<b.length;i++){
  console.log(i+"---------")
  console.log(b[i]);
}
//补充 document.getElementById("test").childElementCount; 可以直接获取长度 同length
```

4. 通过`children`来获取子节点

利用children来获取子元素是最方便的，他也会返回出一个数组。对其获取子元素的访问只需按数组的访问形式即可。

```js
var getFirstChild = document.getElementById("test").children[0];
```

5. 获取第一个子节点

firstChild来获取第一个子元素，但是在有些情况下我们打印的时候会显示undefined，这是什么情况呢？？其实firstChild和childNodes是一样的，在浏览器解析的时候会把他当换行和空格一起解析，其实你获取的是第一个子节点，只是这个子节点是一个换行或者是一个空格而已。那么不要忘记和childNodes一样处理呀。

```js
var getFirstChild = document.getElementById("test").firstChild;
```

6. firstElementChild获取第一个子节点

使用firstElementChild来获取第一个子元素的时候，这就没有firstChild的那种情况了。会获取到父元素第一个子元素的节点 这样就能直接显示出来文本信息了。他并不会匹配换行和空格信息。

```js
var getFirstChild = document.getElementById("test").firstElementChild;
```

7. 获取最后一个子节点

lastChild获取最后一个子节点的方式其实和firstChild是类似的。同样的lastElementChild和firstElementChild也是一样的。不再赘余。

```js
var getLastChildA = document.getElementById("test").lastChild;
var getLastChildB = document.getElementById("test").lastElementChild;
```

一个包解决你所有的JS问题,点击获取

#### js获取父节点的方式

1. `parentNode`获取父节点

获取的是当前元素的直接父元素。parentNode是w3c的标准。

```js
var p = document.getElementById("test").parentNode;
```

2. `parentElement`获取父节点

parentElement和parentNode一样，只是parentElement是ie的标准。

```js
var p1 = document.getElementById("test").parentElement;
```

3. offsetParent获取所有父节点

一看offset我们就知道是偏移量 其实这个是于位置有关的上下级 ，直接能够获取到所有父亲节点， 这个对应的值是body下的所有节点信息。

```js
var p2 = document.getElementById("test").offsetParent;
```

#### js获取兄弟节点的方式

1. 通过获取父亲节点再获取子节点来获取兄弟节点

```js
var brother1 = document.getElementById("test").parentNode.children[1];
```

2. 获取上一个兄弟节点

在获取前一个兄弟节点的时候可以使用`previousSibling`和`previousElementSibling`。他们的区别是`previousSibling`会匹配字符，包括换行和空格，而不是节点。`previousElementSibling`则直接匹配节点。

```js
var brother2 = document.getElementById("test").previousElementSibling;
var brother3 = document.getElementById("test").previousSibling;
```

3. 获取下一个兄弟节点

同`previousSibling`和`previousElementSibling`，`nextSibling`和`nextElementSibling`也是类似的。

```js
var brother4 = document.getElementById("test").nextElementSibling;
var brother5 = document.getElementById("test").nextSibling;
```

### 获取样式

- node.currentStyle['height']; //IE兼容
- getComputedStyle[node]('height'); //火狐、谷歌

获取的是当前有效的样式

- node.getAttribute("class");

```js
//增加属性,支持自定义
node.setAttribute("class","box");
node.setAttribute("jjj","aaa");
//自定义属性data-
node.setAttribute("data-jjj","aaa");
node.setAttribute("data-a-b","aaa");
node.setAttribute("data-tB","aaa");
node.dataset.jjj
node.dataset.aB
node.dataset.tb

//删除属性
node.removeAttribute("class");

```

### 修改样式

- 通过style属性修改

```js
el.style.background='red'
```

- 通过className属性修改

```js
el.className='test'
```

- 通过classList属性修改

```js
//不会覆盖原有类名
el.classList.add('类名')
el.classList.remove('类名')
el.classList.toggle('类名')//有则移除，没有则添加
```

### 获取内容

- innerHTML
获取标签内容，写入时会解析标签
- innerText
获取标签间纯文本，不会解析标签
- outerHTML
会替换整个标签

```js
window.onload=function(){
var odiv=document.getElementById("div1");

console.log(odiv.innerHTML);
odiv.innerHTML="<h1>jjjjjjjj</h1>";

console.log(odiv.innerText);
console.log(odiv.outerHTML);
```

### 获取子节点

- childNode
访问当前节点下所有的子节点

- firstChild
访问子节点中的首位

- lastChild
访问子节点中的最后一位

- nextSibling 访问当前节点兄弟节点中的下一个节点

- previousSibling 访问当前节点兄弟节点中的上一个节点

>上面这些属性都包含文本节点

- children
- firstElementChild
- lastElementChild
- nextElementSibling
- previousElementSibling

>上面这些属性只获取子节点中的元素节点，不包含文本。

||nodeType|nodeName|nodeValue|
|-|-|-|-|
|元素节点|1|标签名|null|
|属性节点|2|属性名|属性值|
|文本节点|3|#text|文本内容|

```js
window.onload=function(){
var oDiv=document.getElementById("div1");
console.log(oDiv.childNodes.length); //返回数组，里面为全部子元素，包括换行，缩进
console.log(oDiv.childNodes[0].nodeName);
}
<body>
<div id="div1" > <em> text</em> divtext <strong>text</strong></div>
</body>
```

### 获取当前元素节点上的所有的属性节点

- attrbutes

获取当前元素节点上的所有的属性节点

```js
var oDiv=document.getElementById("div1");
console.log(oDiv.attributes);
```

返回的是集合：

   1. 无序
   2. 不重复

### 获取其中的某一个属性节点

```js
//div中title="hello"
console.log(oDiv.attributes.getNamedItem("title").nodeName);
console.log(oDiv.attributes.getNamedItem("title").nodeType);
console.log(oDiv.attributes.getNamedItem("title").nodeValue);
//或
console.log(oDiv.attributes["title"].nodeName);
console.log(oDiv.attributes["title"].nodeType);

```

### 节点操作

>document.write()会覆盖页面上的所有内容

- createElement()

```js
document.createElement();
参数：标签名
返回值：创建好的节点
```

- appendChild(插入元素)

```js
node1.appendChild(node2);
将node2节点插入到node1节点的子节点的末尾
```

- createTextNode()

```js
document.createTextNode(文本);
创建文本节点（纯文本）
```

- insertBefore(插入元素，在哪个元素前)

```js
box1的父节点.insertBefore(box2,box1);
将box2添加到box1前面
```

- replaceChild()

```js
box1的父节点.replaceChild(box2,box1);
用box2替换掉box1
```

- cloneNode(是否克隆后代节点)

```js
node.cloneNode(); //克隆自己
node.cloneNode(true); //克隆自己和本身下的所有子节点
//返回克隆的新节点
```

- removeChild(删除的元素)

>删除元素必须通过父元素删除

```js
box1的父节点.Child(box1);
删掉box1节点
```

实例

```js
window.onload=function(){
                var oDiv=document.getElementById("div1"); //div
               var newP=document.createElement('p');  //创建一个新的p标签节点
                var newText=document.createTextNode('jjjj'); //创建一个字符节点
                newP.appendChild(newText);   //将字符节点插入到p节点中
                oDiv.appendChild(newP);    //将p节点插入到div节点中
  var newb=document.createElement('input');  //新建一个input节点
                oDiv.insertBefore(newb,newP);   //将input节点添加到p节点前面
  //doument.body.insertBefore(oP,oDiv); //将oP节点插入到oDiv 前面
        }

```

### offset

- offsetWidth
- offsetHeight

- offsetLeft
- offsetTop

   1. 在父元素均不设置position属性时，在Chrome，opera和IE浏览器中offsetLeft是*元素边框外侧*到*浏览器窗口内侧*的距离且body.offsetLeft=0,在firefox浏览器中offsetLeft是元素边框外侧到body内侧的距离body.offsetLeft=-边框宽度

   2. 如果父元素是body且body设置了position属性，在Chrome和opera浏览器中offsetLeft是元素边框外侧到body边框外侧的距离，在IE和fireForx浏览器中offsetLeft是元素边框外侧到body边框内侧的距离

   3. 如果父元素不是body元素且设置了position属性时，offsetLeft为元素边框外侧到父元素边框内侧的距离（各浏览器情况一致）。

```js
window.onload=function(){
var oDiv=document.getElementById("div1");
//alert(getStyle(ODiv,"width"));
alert(oDiv.offsetWidth); //眼睛能看到的实际宽度（width+border+padding）

var oDiv2=document.getElementById("div2");
alert(oDiv2.offsetLeft);//眼睛能看到实际距离第一个有定位的父节点的距离。
}
```

#### DOM事件监听器

- addEventListerner
语法
`element.addEventListener(event, function, useCapture);`
第一个参数是事件的类型（比如 "click" 或 "mousedown"）。第二个参数是当事件发生时我们需要调用的函数。第三个参数是布尔值，指定使用事件冒泡还是事件捕获,默认false事件冒泡,此参数是可选的。

>注意：请勿对事件使用 "on" 前缀；请使用 "click" 代替 "onclick"。

实例

```js
//添加当用户点击按钮时触发的事件监听器：
document.getElementById("myBtn").addEventListener("click", displayDate);
```

addEventListener() 方法为指定元素指定事件处理程序。addEventListener() 方法为元素附加事件处理程序而不会覆盖已有的事件处理程序。您能够向一个元素添加多个事件处理程序。您能够向一个元素添加多个相同类型的事件处理程序，例如两个 "click" 事件。传统事件绑定重复添加会覆盖。您能够向任何 DOM 对象添加事件处理程序而非仅仅 HTML 元素，例如 window 对象。addEventListener() 方法使我们更容易控制事件如何对冒泡作出反应。当使用 addEventListener() 方法时，JavaScript 与 HTML 标记是分隔的，已达到更佳的可读性；即使在不控制 HTML 标记时也允许您添加事件监听器。

您能够通过使用 `removeEventListener()` 方法轻松地删除事件监听器。

格式：`node.removeEvenetListener()`
参数：第一个为事件类型，第二个为删除函数的名字；

#### IE事件处理函数

- attachEvent();
参数：事件名称和函数
格式：node.attchEvent(onclink,fn);

- detachEvent();
参数：事件名称和函数
格式：node.detachEvent(onclink,fn);

- 阻止超链接的默认行为

```js
window.onload=function(){
var a1=document.getElementById("a1");
a1.onclick=function(){
return confirm("are you sure?");
}
//或
a1.onclick=function(evt){
evt.preventDefault();
alert("done");
}
//or
a1.onclick=function(evt){
window.event.returnValue=false;   //IE兼容
alert("done");
}

<a id="a1" href="..."></a>
```

#### 委托

事件委托

   1. 找到当前节点的父节点或者祖先节点
   2. 将事件添加到你找到的这个父节点或者祖先节点上
   3. 找到触发对象，判断触发对象是否是想要的触发对象，进行后续操作

li委托ul将li变成红色：

```js
window.onload=function(){
var oUl=document.getElementById("ul1");
oUl.onclick=function(ev){
var e=ev||winow.event;
var target=e.target||winow.event.target;
if(target.nodeName.toLowerCase()=="li"){
target.style.backgroundColor="red";
}
}
}
```

## BOM浏览器对象模型

```js
window:{
    document:{anchors,forms,images,links,location},
    frames:{/*...*/},
    history:{/*...*/},
    location:{/*...*/},
    navigator:{/*...*/},
    screen:{/*...*/}
}
```

### window方法

- `alert();`弹出提示框
- `confirm();`弹出带确认的提示框，点确认返回true，取消返回false
- `prompt("tips","tips");`弹出带输入框的提示框，第一个参数为面板上的提示的内容，第二个为输入框里提示的内容
- `window.open(url, _blank|_top|_self|_parent, [新窗口的参数])`第一个参数的为跳转的url，第二个参数为新窗口的名字，第三个参数为设置打开窗口的属性；如width,height,top,left.

### history对象

 `window..history`掌管的是当前窗口的历史记录（只要加载的url不一样就会产生不一样的历史记录）

属性：

- `history.length`返回当前窗口的历史记录条数

方法：

- `history.back();`返回上一条历史记录
- `history.forward()；`前进下一条记录
- `history.go();`跳转到（0为刷新，正整数为前进，负整数为后退）

### location地址栏

方法：

- `location.assign(url)`当前窗口跳转到该url

- `location.replace(url)`在当前窗口替换成新的url

- `location.reload()`刷新当前窗口

- `location.reload(true)`不经过浏览器缓存，强制从服务器重载

---

*url:统一资源定位符*
*协议://IP（域名）//:端口号/路径/?查询字符串#锚点*

- `location.protocol`查看协议

>file:本地磁盘文件访问
>http:
>https:

- `location.hostname`查看主机名 IP

- `location.port`查看端口号(默认隐藏，无法查看)

- `location.pathname`查看路径

- `location.search`查看字符串如`?name1=Tom&name2=Tony`

- `location.hash`查看锚点

- `loaction.href`查看整个url

### localStorage

HTML5中年，加入了一个localStorage特性，用来作为本地存储使用的，解决了cookie存储空间不足的问题。localStorage中一般浏览器支持5M大小，cookie中每条cookie的存储空间为4k。

- 本地存储技术：

*localStorage(IE8以下不兼容);*

- 永久存储
- 最大可存储5M，相当于客户端一个微型数据库
- 只能存储string

- localStorage对象

  - setItem(name,value);

```js
if(!window.localStorage){
alert("当前页面不支持localStorage");
}
else{
localStorage.setItem("a","1");
localStorage.b="2";
localStorage["c"]="3";
console.log(localStorage.getItem("b"));
console.log(localStorage.b);
console.log(localStorage["a"]);
}
```

- getItem(name);
- removeItem(name);

*cookie*

会话跟踪技术,从一次会话开始到结束（浏览器的关闭），会全程跟踪记录客户端的状态，记录相关信息

- 可设置过期时间
- 最大可存储4KB
- 每一个域名下最多可存储50条数据

>只能存储字符串

- cookie的语法

name 键；value 值，都可自定义
`name=value;expires=date;path=path;domain=url;secure，其中name=value必选，剩余的可选`

   1. expires/max-age:过期时间，填写日期对象。系统会自动清理过期的cookie

```js
funciton afterOffDate(n){
var d=new Date();
var day=d.getDate();
d.setDate(n+day);
return d;
}
document.cookie="user='xxx';expires="+afterOffDate(7);
```

   2. path 限制访问路径，如果不设置默认加载当前.html文件的路径；
   3. domain 限制访问域名，如果不设置默认加载当前文件.html文件的服务器域名
   4. secure 加入这个字段后只能设置https协议加载cookie；
   5. httpOnly cookie的值不能修改；

>火狐浏览器支持本地加载的文件缓存cookie，谷歌只支持服务器加载文件缓存cookie

```js

//客户
//设置cookie
document.cookie='username=xiaomi';
//读取cookie
console.log(document.cookie);


//服务端设置
//在响应头中set-cookie
```

- cookie编解码

```js
document.cookie="name1="+encodeURIComponent("小明"); //编码写入
console.log(decodeURIComponent(document.cookie); //解码读取
```

*sessionStorage(结合后台使用)*
