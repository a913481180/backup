---
title: DOM/BOM
date: 2021-01-01 10:12:33
categories:
  - web
tags:
  - web
---

## DOM（文档对象模型 document object model）

> 用来提供一套操作网页内容的功能
> HTML DOM 方法是您能够（在 HTML 元素上）执行的动作。
> HTML DOM 属性是您能够设置或改变的 HTML 元素的值。
> dom 树：以树状结构，体现标签与标签之间的关系

- 通过这个对象模型，JavaScript 获得创建动态 HTML 的所有力量：

  - JavaScript 能改变页面中的所有 HTML 元素
  - JavaScript 能改变页面中的所有 HTML 属性
  - JavaScript 能改变页面中的所有 CSS 样式
  - JavaScript 能删除已有的 HTML 元素和属性
  - JavaScript 能添加新的 HTML 元素和属性
  - JavaScript 能对页面中所有已有的 HTML 事件作出反应
  - JavaScript 能在页面中创建新的 HTML 事件

- 节点类型:
  - 元素节点：`<div></div>`
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

- node.getElementsByClassName(class 名);  
  通过 class 名字获取符合条件的元素节点；(IE8 以下不兼容）

- document.getElementByName(name 属性的值);
  通过 name 属性的值获取符合条件的元素节点，一般用于表单元素,因为其他元素使用 name 属性没用

- document.querySelector(CSS 选择器格式字符串);
  返回一个元素节点，找到符合条件的第一个元素节点

```js
window.onload = function () {
  //id=ol1
  var node = document.querySelector("#ol1");

  //tagName='li'
  var node = document.querySelector("li");

  //class=box
  var node = document.querySelector(".box");
  node.style.backgroundColor = "red";
};
```

- document.querySelectorAll(CSS 选择器格式字符串);
  返回一个数组

#### js 获取子节点的方式

1. 通过获取 dom 方式直接获取子节点
   其中 test 的父标签 id 的值，div 为标签的名字。getElementsByTagName 是一个方法。返回的是一个数组。在访问的时候要按数组的形式访问。

```js
var a = document.getElementById("test").getElementsByTagName("div");
```

2. 通过`childNodes`获取子节点

使用 childNodes 获取子节点的时候，childNodes 返回的是子节点的集合，是一个数组的格式。他会把文本，换行和空格也当成是节点信息。

```js
var b = document.getElementById("test").childNodes;
```

为了不显示不必须的换行的空格，我们如果要使用 childNodes 就必须进行必要的过滤。通过正则表达式式取掉不必要的信息。下面是过滤掉

```js
//去掉换行的空格
for (var i = 0; i < b.length; i++) {
  if (b[i].nodeName == "#text" && !/\s/.test(b.nodeValue)) {
    document.getElementById("test").removeChild(b[i]);
  }
}
//打印测试
for (var i = 0; i < b.length; i++) {
  console.log(i + "---------");
  console.log(b[i]);
}
//补充 document.getElementById("test").childElementCount; 可以直接获取长度 同length
```

4. 通过`children`来获取子节点

利用 children 来获取子元素是最方便的，他也会返回出一个数组。对其获取子元素的访问只需按数组的访问形式即可。

```js
var getFirstChild = document.getElementById("test").children[0];
```

5. 获取第一个子节点

firstChild 来获取第一个子元素，但是在有些情况下我们打印的时候会显示 undefined，这是什么情况呢？？其实 firstChild 和 childNodes 是一样的，在浏览器解析的时候会把他当换行和空格一起解析，其实你获取的是第一个子节点，只是这个子节点是一个换行或者是一个空格而已。那么不要忘记和 childNodes 一样处理呀。

```js
var getFirstChild = document.getElementById("test").firstChild;
```

6. firstElementChild 获取第一个子节点

使用 firstElementChild 来获取第一个子元素的时候，这就没有 firstChild 的那种情况了。会获取到父元素第一个子元素的节点 这样就能直接显示出来文本信息了。他并不会匹配换行和空格信息。

```js
var getFirstChild = document.getElementById("test").firstElementChild;
```

7. 获取最后一个子节点

lastChild 获取最后一个子节点的方式其实和 firstChild 是类似的。同样的 lastElementChild 和 firstElementChild 也是一样的。不再赘余。

```js
var getLastChildA = document.getElementById("test").lastChild;
var getLastChildB = document.getElementById("test").lastElementChild;
```

一个包解决你所有的 JS 问题,点击获取

#### js 获取父节点的方式

1. `parentNode`获取父节点

获取的是当前元素的直接父元素。parentNode 是 w3c 的标准。

```js
var p = document.getElementById("test").parentNode;
```

2. `parentElement`获取父节点

parentElement 和 parentNode 一样，只是 parentElement 是 ie 的标准。

```js
var p1 = document.getElementById("test").parentElement;
```

3. offsetParent 获取所有父节点

一看 offset 我们就知道是偏移量 其实这个是于位置有关的上下级 ，直接能够获取到所有父亲节点， 这个对应的值是 body 下的所有节点信息。

```js
var p2 = document.getElementById("test").offsetParent;
```

#### js 获取兄弟节点的方式

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

- node.currentStyle['height']; //IE 兼容
- getComputedStyle[node]('height'); //火狐、谷歌

获取的是当前有效的样式

- node.getAttribute("class");

```js
//增加属性,支持自定义
node.setAttribute("class", "box");
node.setAttribute("jjj", "aaa");
//自定义属性data-
node.setAttribute("data-jjj", "aaa");
node.setAttribute("data-a-b", "aaa");
node.setAttribute("data-tB", "aaa");
node.dataset.jjj;
node.dataset.aB;
node.dataset.tb;

//删除属性
node.removeAttribute("class");
```

### 修改样式

- 通过 style 属性修改

```js
el.style.background = "red";
```

- 通过 className 属性修改

```js
el.className = "test";
```

- 通过 classList 属性修改

```js
//不会覆盖原有类名
el.classList.add("类名");
el.classList.remove("类名");
el.classList.toggle("类名"); //有则移除，没有则添加
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

> 上面这些属性都包含文本节点

- children
- firstElementChild
- lastElementChild
- nextElementSibling
- previousElementSibling

> 上面这些属性只获取子节点中的元素节点，不包含文本。

|          | nodeType | nodeName | nodeValue |
| -------- | -------- | -------- | --------- |
| 元素节点 | 1        | 标签名   | null      |
| 属性节点 | 2        | 属性名   | 属性值    |
| 文本节点 | 3        | #text    | 文本内容  |

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
var oDiv = document.getElementById("div1");
console.log(oDiv.attributes);
```

返回的是集合：

1.  无序
2.  不重复

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

> document.write()会覆盖页面上的所有内容

- createElement()

```js
document.createElement();
参数：标签名
返回值：创建好的节点
```

- appendChild(插入元素)

```js
node1.appendChild(node2);
将node2节点插入到node1节点的子节点的末尾;
```

- createTextNode()

```js
document.createTextNode(文本);
创建文本节点（纯文本）
```

- insertBefore(插入元素，在哪个元素前)

```js
box1的父节点.insertBefore(box2, box1);
将box2添加到box1前面;
```

- replaceChild()

```js
box1的父节点.replaceChild(box2, box1);
用box2替换掉box1;
```

- cloneNode(是否克隆后代节点)

```js
node.cloneNode(); //克隆自己
node.cloneNode(true); //克隆自己和本身下的所有子节点
//返回克隆的新节点
```

- removeChild(删除的元素)

> 删除元素必须通过父元素删除

```js
box1的父节点.Child(box1);
删掉box1节点;
```

实例

```js
window.onload = function () {
  var oDiv = document.getElementById("div1"); //div
  var newP = document.createElement("p"); //创建一个新的p标签节点
  var newText = document.createTextNode("jjjj"); //创建一个字符节点
  newP.appendChild(newText); //将字符节点插入到p节点中
  oDiv.appendChild(newP); //将p节点插入到div节点中
  var newb = document.createElement("input"); //新建一个input节点
  oDiv.insertBefore(newb, newP); //将input节点添加到p节点前面
  //doument.body.insertBefore(oP,oDiv); //将oP节点插入到oDiv 前面
};
```

### offset

- offsetWidth
- offsetHeight

- offsetLeft
- offsetTop

  1.  在父元素均不设置 position 属性时，在 Chrome，opera 和 IE 浏览器中 offsetLeft 是*元素边框外侧*到*浏览器窗口内侧*的距离且 body.offsetLeft=0,在 firefox 浏览器中 offsetLeft 是元素边框外侧到 body 内侧的距离 body.offsetLeft=-边框宽度

  2.  如果父元素是 body 且 body 设置了 position 属性，在 Chrome 和 opera 浏览器中 offsetLeft 是元素边框外侧到 body 边框外侧的距离，在 IE 和 fireForx 浏览器中 offsetLeft 是元素边框外侧到 body 边框内侧的距离

  3.  如果父元素不是 body 元素且设置了 position 属性时，offsetLeft 为元素边框外侧到父元素边框内侧的距离（各浏览器情况一致）。

```js
window.onload = function () {
  var oDiv = document.getElementById("div1");
  //alert(getStyle(ODiv,"width"));
  alert(oDiv.offsetWidth); //眼睛能看到的实际宽度（width+border+padding）

  var oDiv2 = document.getElementById("div2");
  alert(oDiv2.offsetLeft); //眼睛能看到实际距离第一个有定位的父节点的距离。
};
```

#### DOM 事件监听器

- addEventListerner
  语法
  `element.addEventListener(event, function, useCapture);`
  第一个参数是事件的类型（比如 "click" 或 "mousedown"）。第二个参数是当事件发生时我们需要调用的函数。第三个参数是布尔值，指定使用事件冒泡还是事件捕获,默认 false 事件冒泡,此参数是可选的。

> 注意：请勿对事件使用 "on" 前缀；请使用 "click" 代替 "onclick"。

实例

```js
//添加当用户点击按钮时触发的事件监听器：
document.getElementById("myBtn").addEventListener("click", displayDate);
```

addEventListener() 方法为指定元素指定事件处理程序。addEventListener() 方法为元素附加事件处理程序而不会覆盖已有的事件处理程序。您能够向一个元素添加多个事件处理程序。您能够向一个元素添加多个相同类型的事件处理程序，例如两个 "click" 事件。传统事件绑定重复添加会覆盖。您能够向任何 DOM 对象添加事件处理程序而非仅仅 HTML 元素，例如 window 对象。addEventListener() 方法使我们更容易控制事件如何对冒泡作出反应。当使用 addEventListener() 方法时，JavaScript 与 HTML 标记是分隔的，已达到更佳的可读性；即使在不控制 HTML 标记时也允许您添加事件监听器。

您能够通过使用 `removeEventListener()` 方法轻松地删除事件监听器。

格式：`node.removeEvenetListener()`
参数：第一个为事件类型，第二个为删除函数的名字；

#### IE 事件处理函数

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

1.  找到当前节点的父节点或者祖先节点
2.  将事件添加到你找到的这个父节点或者祖先节点上
3.  找到触发对象，判断触发对象是否是想要的触发对象，进行后续操作

li 委托 ul 将 li 变成红色：

```js
window.onload = function () {
  var oUl = document.getElementById("ul1");
  oUl.onclick = function (ev) {
    var e = ev || winow.event;
    var target = e.target || winow.event.target;
    if (target.nodeName.toLowerCase() == "li") {
      target.style.backgroundColor = "red";
    }
  };
};
```

### HTML 事件

HTML 事件可以是浏览器或用户做的某些事情。

写法：

1. 内联模式
2. 脚本模式/外联模式

```html
<button onclick="btnClick()">内联模式</button>

<button id="btn">外联模式</button>

<script>
  var oBtn=document.getElementById("btn");
  0Btn.onclick=function(){
  console.log("click button");
  }
</script>
```

绑定事件格式：`元素节点.on+事件类型=匿名函数`；

> 系统会在事件绑定完成时，生成一个事件对象。触发事件时，系统会自动去调用事件绑定的函数，将事件对象当做第一个参数传入。

```js
function show() {
  console.log(argument.length);
}

window.onload = function () {
  var oBtn = document.getElement("btn1");
  //oBtn.onclick=show;

  /*
button属性：左键0；中键1；右键2；

鼠标位置：(原点位置不同）
clientX    clientY  //可视窗口的左上角
pageX    pageY   //整个页面的左上角（滚动也包括）
screenX    screenY  //电脑屏幕的左上角
*/

  oBtn.onclick = function (e) {
    //事件对象获取的方式，固定写法
    var ev = e || window.event;
    console.log(ev);
    //console.log(ev.button);
    //console.log(ev.clientX);
  };
};
```

循环添加

```js
var Oblock = document.querySelectorAll("div");
//console.log(Oblock);
for (var i = 0; i < Oblock.length; i++) {
  Oblock[i].index = i;
  Oblock[i].onmousedown = function (e) {
    console.log(this.index);
  };
}
```

- window 事件

  - load 当页面加载完后会触发
  - unload 当页面解析的时候触发（如刷新界面、关闭当前界面）IE 浏览器兼容
  - scroll 页面滚动
  - resize 窗口大小发生变化的时候触发

- 表单事件

  - blur 失去焦点
  - focus 获取焦点
  - select 在输入框内选中文本时触发
  - change 修改输入框内容并失去焦点时触发

  - sumit 只对 sumit 按钮有效
  - reset 只对 reset 按钮有效

下面是一些常见的 HTML 事件：

| 事件         | 描述                                                            |
| ------------ | --------------------------------------------------------------- |
| onchange     | HTML 元素已被改变                                               |
| onclick      | 用户点击了 HTML 元素                                            |
| ondblclick   | 双击                                                            |
| onmouseover  | 用户把鼠标移动到 HTML 元素上(经过子节点会重复触发)              |
| onmouseout   | 用户把鼠标移开 HTML 元素(经过子节点会重复触发)                  |
| onmouseenter | 用户把鼠标移动到 HTML 元素上(经过子节点不会重复触发,IE8 不兼容) |
| onmouseleave | 用户把鼠标移开 HTML 元素(经过子节点不会重复触发,IE8 不兼容)     |
| onmousedown  | 鼠标任一按钮被按下                                              |
| onmousemove  | 鼠标被移动,会不停触发                                           |
| onmouseup    | 鼠标按键被松开                                                  |
| onkeypress   | 用户按下键盘按键(只支持字符建，shift 等没用)                    |
| onkeydown    | 用户按下键盘按键                                                |
| onkeyup      | 用户抬起键盘按键                                                |
| onload       | 浏览器已经完成页面加载                                          |

键盘事件对象属性：

- shiftKey //按下 shift 键为 true，默认为 false
- altKey
- ctrlKey
- metaKey //按下 win 键
- keyCode 键码 //不区分大小写，只返回大写 ASCII 码，支持 shift、ctrl 等非字符键；只在 keydown 下支持,可用 which 兼容

```js
window.onload = function () {
  window.onkeydown = function (ev) {
    var e = ev || window.event;
    var w = e.which || e.keyCode;
    console.log(w);
  };
};
```

- charCode 键码 //区分大小写，只支持字符键，只在 keypress 下支持,可用 which 兼容

```js
window.onload = function () {
  window.onkeypress = function (ev) {
    var e = ev || window.event;
    var w = e.which || e.keyCode;
    console.log(w);
  };
};
```

- target
  目标对象/触发对象

IE8 以下不兼容，可使用 window.event.srcElement

```js
window.onload = function () {
  var oUl = document.getElementById("ul1");

  window.onkeypress = function (ev) {
    var e = ev || window.event;
    var target = e.target || window.event.srcElement;
    console.log(this.tagName);
    console.log(target.innerHTML);
  };
};
```

- 事件冒泡：由里向外逐级触发

> 事件冒泡：同样的事件会在该元素的所有祖先元素中依次被触发(从里到外)
> 事件捕获：从根元素开始去执行对应事件(从外到里)

事件对象的属性和方法:
`cancelBubble=true;`
`stopPropagation();`(捕获阶段也有效)

```js
window.onload = function () {
  var oDiv = document.getElementById("div1");
  for (var i = 0; i < aDivs.length; i++) {
    window.onkeypress = function (ev) {
      var e = ev || window.event;
      console.log(this.tagName);
      e.cancelBubble = true;
      //e.stopProgagation();
    };
  }
};
```

> mouseover 和 mouseout 会有冒泡效果
> mouseenter 和 mouseleave 没有冒泡效果

## BOM 浏览器对象模型

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

### 输入输出语句

- alert(msg):浏览器弹窗提示
- console.log(msg):控制台打印信息
- innerHTML:写入 HTML 元素

> innerHTML 和 outerHTML 的区别：1）innerHTML:从对象的起始位置到终止位置的全部内容, 不包括 HTML 标签。2）outerHTML:除了包含 innerHTML 的全部内容外, 还包含对象标签本身。
> 如需访问 HTML 元素，JavaScript 可使用 document.getElementById(id) 方法。如：`document.getElementById("demo").innerHTML = 5 + 6;`

- document.write()：写入 HTML 输出

> 会解析标签
> 注意：在 HTML 文档完全加载后使用 document.write() 将会删除所有的标签 ;

- prompt(info):浏览器弹出，用户输入

### window 方法

- `alert();`弹出提示框
- `confirm();`弹出带确认的提示框，点确认返回 true，取消返回 false
- `prompt("tips","tips");`弹出带输入框的提示框，第一个参数为面板上的提示的内容，第二个为输入框里提示的内容
- `window.open(url, _blank|_top|_self|_parent, [新窗口的参数])`第一个参数的为跳转的 url，第二个参数为新窗口的名字，第三个参数为设置打开窗口的属性；如 width,height,top,left.

### history 对象

`window..history`掌管的是当前窗口的历史记录（只要加载的 url 不一样就会产生不一样的历史记录）

属性：

- `history.length`返回当前窗口的历史记录条数

方法：

- `history.back();`返回上一条历史记录
- `history.forward()；`前进下一条记录
- `history.go();`跳转到（0 为刷新，正整数为前进，负整数为后退）

### location 地址栏

方法：

- `location.assign(url)`当前窗口跳转到该 url

- `location.replace(url)`在当前窗口替换成新的 url

- `location.reload()`刷新当前窗口

- `location.reload(true)`不经过浏览器缓存，强制从服务器重载

---

_url:统一资源定位符_
_协议://IP（域名）//:端口号/路径/?查询字符串#锚点_

- `location.protocol`查看协议

> file:本地磁盘文件访问
> http:
> https:

- `location.hostname`查看主机名 IP

- `location.port`查看端口号(默认隐藏，无法查看)

- `location.pathname`查看路径

- `location.search`查看字符串如`?name1=Tom&name2=Tony`

- `location.hash`查看锚点

- `loaction.href`查看整个 url

### localStorage

HTML5 中年，加入了一个 localStorage 特性，用来作为本地存储使用的，解决了 cookie 存储空间不足的问题。localStorage 中一般浏览器支持 5M 大小，cookie 中每条 cookie 的存储空间为 4k。

- 本地存储技术：

_localStorage(IE8 以下不兼容);_

- 永久存储
- 最大可存储 5M，相当于客户端一个微型数据库
- 只能存储 string

- localStorage 对象

  - setItem(name,value);

```js
if (!window.localStorage) {
  alert("当前页面不支持localStorage");
} else {
  localStorage.setItem("a", "1");
  localStorage.b = "2";
  localStorage["c"] = "3";
  console.log(localStorage.getItem("b"));
  console.log(localStorage.b);
  console.log(localStorage["a"]);
}
```

- getItem(name);
- removeItem(name);

_cookie_

会话跟踪技术,从一次会话开始到结束（浏览器的关闭），会全程跟踪记录客户端的状态，记录相关信息

- 可设置过期时间
- 最大可存储 4KB
- 每一个域名下最多可存储 50 条数据

> 只能存储字符串

- cookie 的语法

name 键；value 值，都可自定义
`name=value;expires=date;path=path;domain=url;secure，其中name=value必选，剩余的可选`

1.  expires/max-age:过期时间，填写日期对象。系统会自动清理过期的 cookie

```js
funciton afterOffDate(n){
var d=new Date();
var day=d.getDate();
d.setDate(n+day);
return d;
}
document.cookie="user='xxx';expires="+afterOffDate(7);
```

2.  path 限制访问路径，如果不设置默认加载当前.html 文件的路径；
3.  domain 限制访问域名，如果不设置默认加载当前文件.html 文件的服务器域名
4.  secure 加入这个字段后只能设置 https 协议加载 cookie；
5.  httpOnly cookie 的值不能修改；

> 火狐浏览器支持本地加载的文件缓存 cookie，谷歌只支持服务器加载文件缓存 cookie

```js
//客户
//设置cookie
document.cookie = "username=xiaomi";
//读取cookie
console.log(document.cookie);

//服务端设置
//在响应头中set-cookie
```

- cookie 编解码

```js
document.cookie="name1="+encodeURIComponent("小明"); //编码写入
console.log(decodeURIComponent(document.cookie); //解码读取
```

_sessionStorage(结合后台使用)_
