---
title: CSS
date: 2020-11-11 20:22:22
categories:
- study
---
--- 

# css

响应式布局：在不同设备之间缩放网页



```
@media screen and (max-width:768px){
当屏幕小于该尺寸时采用如下布局
...
}
```

- 网页适配移动端

   - 百分比
`.item{width:55%;}`

   - 利用rem单位

```
.html{font-size:100px;}
.item{width: 0.4rem;}
```
   - dpr=物理像素/设备独立像素
`<meta name="viewport" content= "width=device-width,initial-scale=1/dpr>`




### 选择器

#### 1.基础选择器：

- 标签选择器

```
p{
   color:red;
}
div{
   color:red;
}
```

- 类选择器：

```
.类名{
   .....
}
```

- id选择器：

```
#id名{
   /*只能使用一次*/
}
<\div id="xx"> <\/div>
```

- 通配符选择器：

```
*{
   margin:0;
   padding:0;
   /*不需要调用，自动给所有的元素使用*/
}
```


#### 2.符合选择器：

- 后代选择器: 

```
元素1 元素2 {
样式 ;
选择元素1里面的所有元素2(后代元素);
元素1，2用空格隔开
}

如：
ul li {xxxx}
```

- 子选择器

```
元素1>元素2 {
选择元素1里的所有直接后代元素2;
元素2必须是亲儿子
}
如：
div>p{xxxx选择div里最近一级p标签元素}
```

- 并集选择器：

```
元素1，元素2{xxxxx同时选择元素1和2}
```

- 链接伪类选择器

```
a:link{}
a:visited{}
a:hover{鼠标经过}
a:active{}
input:focus{点击时获取焦点}
```
结构性伪类

```
----------------------------
xxx:nth-of-type(even){选择序号（同类型）为偶数的xxx元素}
xxx:nth-of-type(odd){选择序号（同类型）为奇数的xxx元素}
xxx:nth-child(n){选择序号为n的xxx元素}
```
- 属性选择器

```
xxx[attr(属性名称)=val(属性值)]{xxx}
```

- 伪元素选择器

```
(伪元素)xxx::after{xxx}
```

---

#### Css的三种样式表

1. 行内样式表
2. 内部样式表
3. 外部样式表`<link rel="stylesheet" href="css文件路径">`

*一般情况下，优先级如下：*

`（内联(行内)样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式`

`!important>行内(内联)样式 > id 选择器 > 类选择器 = 伪类选择器 = 属性选择器 > 标签选择器 = 伪元素选择器>继承>通用选择器`

权重越大，优先级越高：`通用选择器0、标签（元素）选择器1、类选择器10、ID选择器：100、行内样式1000`

如
```
#box p .tt{xxx}		//权重为100+1+10

#box .tt{xxx}		//权重100+10
```


注意：如果外部样式放在内部样式的后面，则外部样式将覆盖内部样式。

---

#### 元素显示模式

1. 块元素：`<h1> <p> <div> <ul>....`独占一行；宽高边距可控；文字列元素内不能使用块级元素如`<p>`与`<h1>`内不能放`<div>`;图片`<img>`可放在`<p>`内调整位置

2. 行内(内联)元素：`<a>,<span>,<b>,<em>...`一行多个；宽高无法直接设置；只能容纳文本或其他行内元素；

>特别的`<a>`内可以放块级元素，给`<a>`转换一下块级模式最安全

3. 行内块元素：`<img />,<input />,<td>`高宽可设置；一行多个

#### 显示模式转换：

1. 转换为块元素：`dispaly:block`

```
a{
   width:22xp;
   ....
   display:block;
}
```

2. 转换为行内元素 `dispaly:inline`
3. 行内块元素：`display:inline-block`
---

### 浮动

多个块级元素纵向排列用标准流，横向排列用浮动
特性：
1. 脱离标准流的控制，不再保留原先的位置
2. 所有浮动一行对齐.上对齐,宽高位置只跟上一个浮动元素有关,父级宽度不够时会换行
3. 具有行内块特性

>浮动元素不会压住文字和图片、表单元素（输入框、单选按钮、下拉选择框、复选框等）；可做出文字环绕图片效果；

```
选择器 { 
   float:none|left|right；
}
```
>注意:假如某个div元素A是浮动的，如果A元素上一个元素也是浮动的，那么A元素会跟随在上一个元素的后边(如果一行放不下这两个元素，那么A元素会被挤到下一行)；如果A元素上一个元素是标准流中的元素，那么A的相对垂直位置不会改变，也就是说A的顶部总是和上一个元素的底部对齐。

### 常用标准流约束浮动位置

- 清除浮动： 父级盒子不设置高度时，子盒子浮动，脱标不占位置，则父级盒子高度为0；

```
//父元素清除浮动影响
选择器{
   clear:none|left(该元素左边不允许出现浮动元素)|right|both；
}
```
>注意:对于CSS的清除浮动(clear)，一定要牢记：这个规则只能影响使用清除的元素本身，不能影响其他元素。



  1. 额外标签法：在浮动元素的末尾添加一个空标签如`<div style="clear:both"></div>`
  2. 父级添加`overflow:hidden|auto|scroll;` 设置为auto时会出现滚动条，可选择隐藏`overflow-x:hidden;`或`overflow-y:auto`

- 修改滚动条样式

```
【1】IE
 
　　IE浏览器支持通过CSS样式来改变滚动条的部件的自定义颜色
 
　　scrollbar-face-color滚动条凸出部分的颜色
 
　　scrollbar-shadow-color立体滚动条阴影的颜色
 
　　scrollbar-highlight-color滚动条空白部分的颜色
 
　　scrollbar-3dlight-color滚动条亮边的颜色
 
　　scrollbar-darkshadow-color滚动条强阴影的颜色
 
　　scrollbar-track-color滚动条的背景颜色
 
　　scrollbar-arrow-color上下按钮上三角箭头的颜色
 
　　scrollbar-base-color滚动条的基本颜色

【2】webkit
::-webkit-scrollbar 滚动条整体部分
::-webkit-scrollbar-thumb  滚动条里面的小方块，能向上向下移动（或往左往右移动，取决于是垂直滚动条还是水平滚动条）
::-webkit-scrollbar-track  滚动条的轨道（里面装有Thumb）
::-webkit-scrollbar-button 滚动条的轨道的两端按钮，允许通过点击微调小方块的位置。
::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去）
::-webkit-scrollbar-corner 边角，即两个滚动条的交汇处
::-webkit-resizer 两个滚动条的交汇处上用于通过拖动调整元素大小的小控件

/////////////////////////////////////////////////////////////////////
/* 定义滚动条样式 */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
  background-color: rgba(240, 240, 240, 1);
}
 
/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track {
  box-shadow: inset 0 0 0px rgba(240, 240, 240, .5);
  border-radius: 10px;
  background-color: rgba(240, 240, 240, .5);
}
 
/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb {
  border-radius: 10px;
  box-shadow: inset 0 0 0px rgba(240, 240, 240, .5);
  background-color: rgba(240, 240, 240, .5);
}
```

  3. 父级添加伪元素法`:after`

  ```
  .clear:after{
     conten:"";
     display:block;
     height:0;
     clear:both;
     visibility:hidden;
  
  }
  .clear{
     /*IE6,7专有*/
     *zoom:1;
  }
  ```

  4. 父级双伪元素清除浮动

  ```
  .clear:before,.clear:after{
     content:"";
     display:table;
  }
  .clear:after{
     clear:both;
  }
  .clear{
     *zoom:1;
  }
  ```
---

### 定位

将盒子固定到某一个位置`定位=定位模式+边偏移`

1. 定位模式：`position:static|relative|absolute|fixed`
   - 静态定位static：不脱标，不能偏移；
   - 相对定位relative：相对与自己原来的位置来移动；不脱标，继续保留原来的位置
   - 绝对定位absolute：若没有祖先元素或祖先元素没有定位，则以浏览器为准；若祖先元素有定位，则以最近一级有定位的祖先元素为参考点；
   - 固定定位fixed：以浏览器可视窗口为参考点，更父级元素没有任何关系；不随滚动条滚动，不在占有原先位置；
   - 粘性定位sticky：以浏览器可视窗口为参考点；占有原先的位置；必须添加top,left,right,bottom其中一个才生效；

```
  固定在版心右侧位置
  left:50%;浏览器的一半位置
  amrgin-left:版心宽度的一半距离。
```

2. 边偏移：`top:33px;bottom:...;left:.....;right:...`
3. 叠放次序：`选择器{z-index:1;}`数值可正可负；数值越大盒子越靠上；只有定位的盒子才能有z-index属性；另外，有定位的在没有定位的上面，都有定位则后面写的在上面，

>子级是绝对定位的话，父级要用相对定位，子盒子不需要占有位置，父级需要占有位置；
>行内元素添加绝对或固定定位，可直接设置宽高；块级元素添加后不设置宽高则默认是内容大小；
>浮动元素可以压住标准流盒子；但不会压住标准流盒子里的文字和图片；

### 布局

- flex
属性：
   - flex-wrap:nowrap/wrap/wrap-reverse;
   - align-items:stretch/center/flex-start/flex-end/baseline;
   - flex-grow: 数字;
   - flex-shrink:数字;

```
.box{
display:flex;
flex-direction:row/coloumn;
justify-content:flex-start(开始对齐)/flex-end（尾部对齐）/center/space-between（两端对齐）/space-around(自动分配间隔）
align-items:stretch/center/flex-start/flex-end/baseline;
}

```

### 元素的显示与隐藏

- display:none（隐藏）|block(除转换为块级元素外，还可显示元素);
>display隐藏后不再占有原来的位置；

- visibility:visible(可视)|hidden；
>visibility:隐藏元素后，继续占有原来的位置；

---

#### 字体属性

- font-family:默认使用第一个字体,` font-family:"Microsoft yahie" ,"hiragino Sans GB",Arial; `

- font-size: 单位px

- font-weight: `nomal或400`不加粗;`bold或700`加粗；`100-900`

- font-style:`normal`默认；`italic`斜体；

- color：支持十六进制、rgb（255,5,5）、预设颜色值red

- text-align:文本内容对齐left,right,center;

- line-height:行高，单位px;

- text-decoration:文本装饰；`none`没有装饰线；`underline` 下划线；`line-through`删除线；

- text-indent:缩进；单位em代表1个文字大小；px像素

- 字体复合属性：`font: font-style font-weight font-size/line-height font-family;`顺序不能换，必须保留`font-size` 和`font-family`属性

--- 
css背景

- 背景颜色：`background-color`
- 背景图片：`background-image:url()`
- 背景图片重复平铺：`background-repeat:no-repeat|repeat-x|repeat-y`
- 背景图片位置：`background-position:(方位名词：top|center|left|right|top left等)(坐标：x,y或只有x坐标与垂直居中)两个可混合使用`
- 背景固定：`background-attachment:scroll|fixed(固定)`
- 背景复合写法：`background:(没有顺序)一般颜色 图片 平铺 固定 位置。。`
- 背景半透明 ：`backround:rgba(0,0,0,0.5)最后一个参数代表透明度`

---
子标签会继承付标签的部分样式如字体、颜色等
行高可以不跟单位：

```
body{
font:12px/1.5; /*子元素字体大小的1.5倍*/
}
div{
   font-size:14px;
}
```
---
### 盒子

- 边框:`border`

```
加边框后盒子会变大
border-width:单位px
border-stye:solid(实线)|dashed（虚线）|dotted（点线）
border-color:
复合写法：border：xxx xxx xxx没有循序
每条边可单独设置:
border-top:
border-bottom:
合并相邻边框
border-collapse:collapse;
```

- 内容:content
- 内边界:`padding`

```
也会改变盒子大小,若盒子没有width/height属性则不会
可分开设置上下左右边距
padding-left:
复合写法：padding：（2个：上下，左右；3个：上，左右，下；4个：顺时针）
```

- 外边距:`margin`

```
*可分开设置
margin-left:
nargin-bottom:
*复合写法与padding一致
```

### 盒子居中

* 让块级盒子水平居中：

   1. 通过手动计算 margin 左右边距
	 父盒子的定宽的，子盒子指定 margin-left 即可
   2. 盒子有宽度，左右设为auto；
	`margin：0，auto；`// 让子盒子左右自动适应，想当于 `left:auto; right:auto`
   3. 先让盒子左右边缘和父盒子垂直的中心线垂直，然后把子盒子往回移动自身宽度的一半

```
	/* 通过 transform 实现*/
	//子盒子
	margin-left: 50%;                // 先移动父盒子的一半
        transform: translateX(-50%);     // 再移动自身盒子一半,transform中translate使用百分比时相对的是自己的长宽，不是父盒子的。



	/*通过 定位实现*/
	//父盒子
	position: relative;
	//子盒子
	position: absolute;
        left: 50%;                       // 向右移动父盒子一半
        margin-left: -100px;             // 向左移动自身盒子一半
```

   4. 把盒子转成行内块,行内元素只需要在父级元素添加text-align:center即可；

```
	//父盒子
	text-align: center;               // 让父盒子设置水平居中

	//子盒子
	display: inline-block;            // 让子盒子显示为行内块模式
```

- 盒子垂直居中的方法

   1. 知道父盒子的高度，可以使用 margin 计算盒子的上下边距，来使盒子居中

```
	margin-top: 149px;         // 根据父盒子的高度指定子盒子 margin-top 即可
```

   2. 先让盒子的上下边缘和父盒子的水平中心线重叠，，然后再让子盒子往回移动自身一半的距离

```
/* 通过 transform 属性来移动*/
	//子盒子
	margin-top: 50%;                  // 向下移动父盒子的一半
        transform: translateY(-50%);      // 向上移动自身盒子的一半

/* 通过 定位来移动*/
	//父盒子
	position: relative;
	//子盒子
	position: absolute;
        top: 50%;                  // 先向下移动父盒子的一半
        margin-top: -100px;        // 再向上移动自身盒子的一半
```

   3. 使用表格的 vertical-align :middle 属性来实现盒子垂直居中

```
	//父盒子
	display: table-cell;         // 显示形式为表格
        vertical-align: middle;      // 里面内容为居中对齐
```
   4. 把盒子转成行内块

```
//父盒子line-height:500px 与 子盒子的vertical-align:middel共同作用使子盒子垂直居中。
.parent-box {
line-height: 500px;
text-align:center;
}
.child-box {
display:inline-block;
vertical-align:middle;
line-height:1rem;
color:white;
}
```

- 利用flex布局

flex布局，设置水平与竖直方向的内容居中。

```
.parent-box {
display: flex;
justify-content: center;
align-items: center;
}
```

-  position:absolute 配合定位与margin：auto

```
//不兼容低版本的IE浏览器
.parent-box {
position: relative;
}
.child-box {
position: absolute;
left: 0;
top: 0;
right: 0;
bottom:0;
margin: auto;
}
```

- 使用JS的思路大概给大家说下：

1、js中只要获取到当前盒子具体的left/top值即可

2、一屏幕的宽高-盒子的宽高，最后除以2，获取的值就是它应该具备的left/top(这个值可以使盒子处于页面中间)

###  嵌套元素塌陷问题：

父元素与子元素有上外边距时，父元素会塌陷较大的外边距值

- 解决方法：触发BFC（块格式上下文）
   - 为父元素设置：上边框或上内边距
   - 为父元素添加overflow：hidden；
   - 为父元素添加dispaly:inline-block/tabl-cell;或float:left;或positon:absolute/fixed;

### 清除内外边距

>(body会默认有个边距)

```
* {
   margin:0;
   padding:0;
}
```

- 透明：`opacity:0.5`子元素也会跟着变化
- 圆角边框：`border-radius:10px|10%|顺时针四个参数`;
- 盒子阴影：`box-shadow:3px水平阴影 0px垂直阴影 5px模糊距离 0px阴影尺寸 #fff颜色 inset（内阴影）`不占空间
- 文字阴影：`text-shadow:水平阴影 垂直阴影 模糊距离 颜色`
- 渐变背景：`background:linear-gradient(to bottom|to top|to right|to left,#fff(开始颜色),#000(结束颜色));
- 过渡动画：`transion: width(要过渡的属性) 1s(过渡时间) ease-in(过渡方式) 2s(延迟时间)`
- 位移属性：`transform:translateX|translateY|translateZ(20px);`位移时默认是元素的中心位置
- 缩放：`transform:scale(1.5);`默认为1正常大小；`transform-origin:50% 50%;`元素缩放中心点位置
- 旋转：`transform:rotateX|rotateY|rotateZ(30deg);`
- 透视：`transform:perspective(1000px);`透视距离，必须写在前面；
- 循环动画：
```
//定义动画
@keyframe beat{		//关键帧
30%:{
transform:scale(1.3);
}
100%:{
transform:scale(1);
}
}

//使用动画

span.heart{
animation:beat(动画名称) 1.5s(动画时长) infinite(循环动画);
}	
```





### 精灵图 

将多个小背景图整合到一张大图片中，减少了服务器的请求

移动背景位置：`background-position `移动的距离是这个目标图片的x，y坐标，一般情况是向上或左移动，所以数值是负值；

### 字体图标

精灵图放大会失真，字体图标不会，且可以改变颜色大小，只适合简单图形；

字体图标下载网站：`http://icomoon.io` `https://iconfont.cn`

使用方法：
1.添加字体声明在`<style></style>`中

```
@font-face{
   font-family: `name`;
   src:url()
   ....
}
```

2.使用:从官网复制图标粘贴到相应位置并设置样式:`span{font-family:'name';font-size:....}`

### 三角形

盒边距设置为0，边框宽度控制大小，边框背景设置为透明

```
.box{
   width:0px;
   height:0px;
   border:20px solid transparent;
   border-left-color:red;
   /*照顾兼容性*/
   line-height:0;
   font-size:0;
}
```

直角三角形

```
width:0;
height:0;
/*上边框调大*/
border-toP:100px solid transparent;
border-right:50px solid red;
/*左边和下边边框设置为0*/
border-bottom:0;
border-left:0;
```

### 鼠标样式

`li{ cursor:default|pointer(手指)|move|text(文本)|not-allowed(禁止)}`

### 防止文本域被拖拽

`textarea{resize:none;}`

### vertical-align

用于设置行内块垂直对齐方式(必须作用与inline-block元素)
`vertical-align:baseline(基线)|top|middle|bottom;`

- 解决图片底部空白缝隙问题：添加vertical-align属性；或转换为块级元素`display:block;

### 溢出文字省略号表示

- 单行

```
1.强制一行显示文本
white-space:nowrap;
2.超出部分隐藏
overflow:hidden;
3.超出文字用省略号表示
text-overflow:ellipsis;
```
- 多行

```
兼容性差，适合移动端和wwebkit浏览器
overflow:hidden;
text-overflow:ellipsis;
/*弹性盒子显示*/
display: -webkit-box;
/*限时一个快元素显示的文本行数*/
-webkit-line-clap:2;
/*设置伸缩盒对象的子元素的排列方式和*/
-webkit-box-orient:vertical;
```

### 盒子间的边框问题

防止边框重叠

```
ul li{
   list-style:none;
   float:left;
   width:111Px;
   height:222px;
   border: 2px solid red;
   margin-left: -1px;
}
```

鼠标移动显示边框

```
/*若盒子有定位，则利用z-index提高层级*/
ul ll:hover{
   z-index:1;
   border:1px solid red;
}
/*若盒子没有定位则鼠标经过时添加相对定位*/
ul li:hover{
   position:relative;
   border: 2px solid red;
}
```



