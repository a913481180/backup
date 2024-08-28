---
title: css
date: 2021-01-11 20:22:22
categories:
  - web
tags:
  - web
---

## 位置

```html
<!--可以写在任何地方，建议写在<head>标签中-->
<style>
  h1 {
  }
  .test {
  }
  #idd {
  }
</style>

<!--引入外部css,可以触发浏览器缓存-->
<link rel="stylesheet" href="./test.css" />
```

## 响应式

响应式布局：在不同设备之间缩放网页

- 媒介查询

```css
//大范围在上面
@media screen and (max-width:768px){
当屏幕小于该尺寸时采用如下布局
...
}
@media screen and(max-width:1020px){
   ...
}
```

- 网页适配移动端

  - 百分比
    `.item{width:55%;}`

  - 利用 rem 单位
    > rem 相对与根文本字体大小
    > em 相对与自己的字体或父元素的字体大小

```css
.htmlfont-size:100px;}
.item{width: 0.4rem;}
```

- dpr=物理像素/设备独立像素
  `<meta name="viewport" content= "width=device-width,initial-scale=1/dpr>`

### 选择器

> 后来者居上

#### 1.基础选择器

- 标签选择器

```css
p {
  color: red;
}
div {
  color: red;
}
```

- 类选择器：

```html
<style>
  .类名 {
    /*不能用纯数字*/
    /*.....*/
  }
</style>

<!--后写的class会失效-->
<div class="test" class="test1"></div>

<!--生效顺序由css中的位置决定-->
<div class="test3 test2"></div>
```

- id 选择器：

```html
<style>
  #id名 {
    /*只能使用一次*/
    /*不能用数字开头*/
  }
</style>
<div id="xx"></div>
```

- 通配符选择器：

```css
* {
  margin: 0;
  padding: 0;
  /*不需要调用，自动给所有的元素使用*/
}
```

#### 2.组合选择器

- 后代选择器:

```css
/*
元素1 元素2 {
样式 ;
选择元素1里面的所有元素2(后代元素);
元素1，2用空格隔开
}

如：
*/
ul li {
  color: red;
}
```

- 子选择器

```css
/*
元素1>元素2 {
选择元素1里的所有直接后代元素2;
元素2必须是亲儿子
}
如：
*/
div > p {
  /*选择div里最近一级p标签元素*/
  color: red;
}
```

- 兄弟选择器

```css
div + p {
  /*选择div之后，和div同级且相邻的第一个p元素*/
  color: red;
}
div ~ p {
  /*选择div之后，和div同级的所有p元素*/
  color: red;
}
```

- 并集选择器：

```css
/*
元素1，元素2{xxxxx同时选择元素1和2}
*/
p,
a {
  color: red;
}
```

### 3.伪类选择器

> 选择元素的特殊状态

- 链接伪类选择器

```css
/*顺序不能乱写*/
a:link {
  /*没有访问过*/
}
a:visited {
  /*访问过的*/
}
a:hover {
  /*鼠标悬浮*/
}
a:active {
  /*点击时*/
}

input:focus {
  /*点击时获取焦点*/
}
input:checked {
  /*勾选时的复选框或单选框*/
}
input:disabled {
  /*禁用的input元素*/
}
input:enabled {
  /*未禁用的input元素*/
}
```

- 结构性伪类

```css
li:first-child {
  /*选择第一个子元素为li的元素*/
}
li:last-child {
  /*选择最后一个子元素为li的元素*/
}
li:only-child {
  /*选择没有兄弟li节点的li元素*/
}
li:nth-child(3) {
  /*
   选择第3个子元素为li的元素,
   不写则都不生效，
   写n则选择全部符合的，
   2n则为偶数的,
   2n+1则选择奇数的,
   -n+5前五个
   */
}

li:nth-last-child(3) {
  /*倒数第三个*/
}

li:first-of-type {
  /*选择第一个为li的元素*/
}
li:last-of-type {
  /*选择最后一个为li的元素*/
}
li:only-of-type {
  /*选择没有同级li元素的li节点*/
}
li:nth-of-type(2n) {
  /*选择序号（同类型）为偶数的xxx元素*/
}

li:nth-last-of-type(3) {
  /*倒数第三个*/
}

:root {
  /* 选中根元素,即html标签 */
}
div:empty {
  /* 选择div标签体为空的元素 */
}
```

- 否定伪类

```css
div:not(.test) {
  /*选择div中类名不是test的元素*/
}

div:not([title="xx"]) {
  /*选择div,排除属性为title='xx'的div元素*/
}

div:not(:nth-child(2)) {
  /*选择div,排除第二个*/
}
```

- 目标伪类

```css
div:target {
  /* 锚点选中的元素 */
}
```

- 语言伪类

```css
:lang(en) {
  /* 元素的lang属性为en的节点 */
}
div:lang(en) {
  /* div元素的lang属性为en的div节点 */
}
```

- 伪元素选择器

> 选择元素中的特殊位置
> 可以只写一个冒号，除了 placeholder 和 selection

```css
(伪元素)div::after{xxx}
div::first-letter{
   /* div中第一个文字 */
}
div::first-line{
   /* div中第一行 */
}
div::selection{
   /* div中选中的颜色 */
}

input::placeholder{
   /* 选中input中的提示文字 */
   color:red
}

/* 在元素前面创建一个元素 */

div::before{
content:'';
display:xxx;
color:xxx;
}

/* 在元素后面创建一个元素 */
div::after{
content:'';
display:xxx;
color:xxx;
}

```

### 4. 属性选择器

```css
[attr(属性名称)] {
  color: red;
}
[attr(属性名称)="val(属性值)"] {
  color: red;
}

/*选择attr的值为a开头的元素*/
[attr(属性名称)^="a"] {
  color: red;
}

/*选择attr的值为a结尾的元素*/
[attr(属性名称)$="a"] {
  color: red;
}

/*选择attr的值包含a的元素*/
[attr(属性名称)*="a"] {
  color: red;
}
```

---

#### Css 的三种样式表

1. 行内样式表`<div style=""></div>`
2. 内部样式表`<style></>`
3. 外部样式表`<link rel="stylesheet" href="css文件路径">`

> css 继承:可继承文字相关的样式;而不能继承布局相关的样式

_一般情况下，优先级如下：_

`（内联(行内)样式）Inline style > （内部样式）Internal style sheet =（外部样式）External style sheet > 浏览器默认样式`

`!important>行内(内联)样式 > id 选择器 > 类选择器 = 伪类选择器 = 属性选择器 > 标签选择器 = 伪元素选择器>通配符选择器>继承`

权重越大，优先级越高：(`ID选择器：100`、`类/伪类/属性选择器10`、 `标签（元素）/伪元素选择器1`)、若权重相同，则后写的生效

```css
#box p .tt{xxx}  //权重为100+1+10

#box .tt{xxx}  //权重100+10
```

注意：如果外部样式放在内部样式的后面，则外部样式将覆盖内部样式。

#### 元素显示模式

1. 块元素：`<h1> <p> <div> <ul>....`独占一行，默认占满父元素；宽高边距可控；文字列元素内不能使用块级元素如`<p>`与`<h1>`内不能放`<div>`;图片`<img>`可放在`<p>`内调整位置

2. 行内(内联)元素：`<a>,<span>,<b>,<em>...`一行多个；宽高无法直接设置；只能容纳文本或其他行内元素；
3. 行内块元素：`<img />,<input />,<td>`高宽可设置；一行多个
4. 特别的`<a>`内可以放块级元素，给`<a>`转换一下块级模式最安全

#### 显示模式转换

1. 转换为块元素：`dispaly:block`

   ```css
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

> 浮动元素不会压住文字和图片、表单元素（输入框、单选按钮、下拉选择框、复选框等）；可做出文字环绕图片效果；

```css
选择器 {
  float: none|left|right；;
}
```

> 注意:假如某个 div 元素 A 是浮动的，如果 A 元素上一个元素也是浮动的，那么 A 元素会跟随在上一个元素的后边(如果一行放不下这两个元素，那么 A 元素会被挤到下一行)；如果 A 元素上一个元素是标准流中的元素，那么 A 的相对垂直位置不会改变，也就是说 A 的顶部总是和上一个元素的底部对齐。

### 常用标准流约束浮动位置

浮动影响：

- 父级盒子不设置高度时，子盒子浮动，脱标不占位置，则父级盒子高度为 0；
- 被浮动元素覆盖的块级元素中的文字会被挤出去；

```css
/* //父元素清除浮动影响 */
.test {
  clear: none|left(该元素左边不允许出现浮动元素) |right|both；;
}
```

> 注意:对于 CSS 的清除浮动(clear)，一定要牢记：这个规则只能影响使用清除的元素本身，不能影响其他元素。

1. 额外标签法：在浮动元素的末尾添加一个空标签如`<div style="clear:both"></div>`
2. 父级添加`overflow:hidden|auto|scroll;` 设置为 auto 时会出现滚动条，可选择隐藏`overflow-x:hidden;`或`overflow-y:auto`
3. 父级伪元素清除浮动

   ```css
   .clear::before,
   .clear::after {
     content: "";
     display: table;
   }

   .clear:after {
     conten: "";
     display: block;
     height: 0;
     clear: both;
     visibility: hidden;
   }
   .clear {
     /*IE6,7专有*/
     *zoom: 1;
   }
   ```

### 边距折叠问题

块的上外边距(margin-top)和下外边距(margin-bottom)有时合并(折叠)为单个边距，其大小为单个边距的最大值(或如果它们相等，则仅为其中一个)，这种行为称为边距折叠
有三种情况会形成外边距重叠：

- 同一层相邻元素之间，上下外边距合并
- 没有内容将父元素和后代元素分开；如果没有边框 border，内边距 padding，行内内容，则就会出现父块元素和其内后代块元素外边界重叠，重叠部分最终会溢出到父级块元素外面。 -空的块级元素：当一个块元素上边界 margin-top 直接贴到元素下边界 margin-bottom 时也会发生边界折叠。

- 解决方法：触发 BFC（块格式上下文）
  - 为父元素设置：上边框或上内边距
  - 为父元素添加 overflow：hidden；
  - 为父元素添加 dispaly:inline-block/tabl-cell;或 float:left;或 positon:absolute/fixed;

### 定位

将盒子固定到某一个位置`定位=定位模式+边偏移`

1. 定位模式：`position:static|relative|absolute|fixed`

   - 静态定位 static：不脱标，不能偏移；
   - 相对定位 relative：相对与自己原来的位置来移动；不脱标，继续保留原来的位置
   - 绝对定位 absolute：若没有祖先元素或祖先元素没有定位，则以浏览器为准；若祖先元素有定位（任何定位），则以最近一级有定位的祖先元素为参考点；
   - 固定定位 fixed：以浏览器可视窗口为参考点，跟父级元素没有任何关系；不随滚动条滚动，不在占有原先位置；
   - 粘性定位 sticky：离它最近的一个拥有滚动机制的祖先元素为参考点，即便这个祖先不是最近的真实可滚动祖先；占有原先的位置；必须添加 top,left,right,bottom 其中一个才生效；可以和浮动一起设置

     ```css
     <!-- 固定在版心右侧位置 -->
     left:50%;浏览器的一半位置
     margin-left:版心宽度的一半距离。
     ```

2. 边偏移：`top:33px;bottom:...;left:.....;right:...`
3. 叠放次序：`选择器{z-index:1;}`数值可正可负；数值越大盒子越靠上；只有定位的盒子才能有 z-index 属性；另外，有定位的在没有定位的上面，都有定位则后面写的在上面，

> 不能和浮动一起写(sticky 除外)，优先定位
> 子级是绝对定位的话，父级要用相对定位，子盒子不需要占有位置，父级需要占有位置；
> 行内元素添加绝对或固定定位，可直接设置宽高；块级元素添加后不设置宽高则默认是内容大小；相对定位还是保留元素之前的显示模式
> 浮动元素可以压住标准流盒子；但不会压住标准流盒子里的文字和图片；
> 绝对定位和固定定位，想让宽高和包含块一致，可同时设置 left:0;right:0 与 top:0;bottom:0

- flex 弹性布局
  - 采用 Flex 布局的元素，称为 Flex 容器（flex container），简称”容器”。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称”项目”。
  - 容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。
  - 主轴的开始位置（与边框的交叉点）叫做 main start，结束位置叫做 main end；
  - 交叉轴的开始位置叫做 cross start，结束位置叫做 cross end。
  - 项目默认沿主轴排列。单个项目占据的主轴空间叫做 main size，占据的交叉轴空间叫做 cross size。
  - 当一层 flex 布局的时候，设置子元素的 width:100%就没有问题。当页面中多层 flex 布局嵌套的时候，设置其中子元素的 width：100%会不起作用。可把元素设置为绝对定位来解决该问题。

| 容器属性                                                                                                                                         | 说明                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------- |
| flex-direction:row/row-reverse/column/column-reverse                                                                                             | 决定主轴的方向                                                 |
| flex-wrap:nowrap/wrap/wrap-reverse（最开始的一行在下面，新的在上面）;                                                                            | 如果一条轴线排不下，是否换行以及如何换行                       |
| flex-flow:`<flex-direction><flex-wrap>`                                                                                                          | flex-direction 与 flex-wrap 的简写形式                         |
| justify-content:flex-start/flex-end/center/space-between（两端对齐，项目间的间隔相等)/space-around（每个项目间的间隔相等，项目与边框的间隔较小） | 定义项目在主轴上的对齐方式                                     |
| align-items:stretch（项目无高度或设为 auto 时，将会占满整个容器高度）/center/flex-start/flex-end/baseline（项目的第一行文字的基线对齐）;         | 定义项目在交叉轴上的对齐方式                                   |
| align-content:stretch（项目无高度或设为 auto 时，将会占满整个容器高度）/center/flex-start/flex-end/space-between/space-around                    | 定义项目在多根轴线上的对齐方式，如果项目只有一根轴线则不起作用 |

| 项目属性                                                               | 说明                                                                                                                                                    |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| flex-grow: 数字;                                                       | 定义项目的放大比例，默认 0；若全部设为 1，则平分空间                                                                                                    |
| flex-shrink:数字;                                                      | 定义缩小比例，默认 1，都设为 1，空间不足，则都等比例缩小；若设为 0 则不会                                                                               |
| flex-basis:宽高/auto;                                                  | 定义项目占主轴的空间，默认 auto                                                                                                                         |
| flex:none/[`<flex-grow>`,flex-shrink，`<flex-basis>`];                 | flex-grow,flex-shrink，flex-basis 简写，后两个可选，默认 0,1，auto                                                                                      |
| order:数字;                                                            | 定义项目的排列顺序，越小越靠前，默认 0                                                                                                                  |
| align-self:auto / flex-start / flex-end / center / baseline / stretch; | 允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性。默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch。 |

```css
.box{
display:flex;
flex-direction:row/coloumn;
justify-content:flex-start(开始对齐)/flex-end（尾部对齐）/center/space-between（两端对齐）/space-around(自动分配间隔）
align-items:stretch/center/flex-start/flex-end/baseline;
}

```

### 元素的显示与隐藏

- display:none（隐藏）|block(除转换为块级元素外，还可显示元素);

> display 隐藏后不再占有原来的位置；

- visibility:visible(可视)|hidden；

> visibility:隐藏元素后，继续占有原来的位置；

- opacity:0;

#### 字体属性

- font-family:默认使用第一个字体,`font-family:"Microsoft yahie" ,"hiragino Sans GB",Arial;`

- font-size: 单位 px

- font-weight: `lighter或100-300`;`nomal或400`不加粗;`bold或700`加粗；`100-900`

- font-style:`normal`默认；`italic`斜体；`oblique`强制斜体

- color：支持十六进制、rgb（255,5,5）、预设颜色值 red

- text-align:文本内容对齐 left,right,center;

- line-height:行高，单位 px|百分比|倍数,不能为负数;

- text-decoration:文本装饰；`none`没有装饰线；`underline` 下划线；`line-through`删除线；可搭配 dotted 线|wavy 波浪线和颜色

- text-indent:缩进；单位 em 代表 1 个文字大小；px 像素
- letter-space:字间距；单位 em 代表 1 个文字大小；px 像素,可以为负数
- word-space:英文字母间距；单位 em 代表 1 个文字大小；px 像素,可以为负数

- 字体复合属性：`font: font-style font-weight font-size/line-height font-family;`顺序不能换，必须保留`font-size` 和`font-family`属性

#### css 背景

- 背景颜色：`background-color`
- 背景图片：`background-image:url()`
- 背景图片重复平铺：`background-repeat:no-repeat|repeat-x|repeat-y`
- 背景图片位置：`background-position:(方位名词：top|center|left|right|top left水平方向+垂直方向等)(坐标：x,y或只有x坐标与垂直居中)两个可混合使用`
- 背景固定：`background-attachment:scroll|fixed(固定)`
- 背景复合写法：`background:(没有顺序)一般颜色 图片 平铺 固定 位置。。`
- 背景半透明 ：`backround:rgba(225,22,0,0.5)最后一个参数代表透明度`

子标签会继承父标签的部分样式如字体属性，文本属性（除 vertical-algin）、颜色等，不会继承边框、背景、边距、宽高等
行高可以不跟单位：

```css
body {
  font: 12px/1.5; /*子元素字体大小的1.5倍*/
}
div {
  font-size: 14px;
}
```

### 盒模型

#### 边框:`border`

```css
/* 加边框后盒子会变大 */
border-width:单位px
border-stye:solid(实线)|dashed（虚线）|dotted（点线）|double(双实线)
border-color:
/* 复合写法：border：xxx xxx xxx没有循序 */
/* 每条边可单独设置: */
border-top:
border-bottom:
/* 合并相邻边框 */
border-collapse:collapse;
```

#### 内容:`content`

#### 内边界:`padding`

```css
/* 也会改变盒子大小,若盒子没有width/height属性则不会 */
/* 可分开设置上下左右边距 */
padding-left: 20px;
/* 复合写法：padding：（2个：上下，左右；3个：上，左右，下；4个：顺时针） */
/* 不能为负数 */
```

#### 外边距:`margin`

```css
 /*可分开设置 */
margin-left:1px
nargin-bottom:1px
/* 复合写法与padding一致 */
```

### 盒子居中

- 利用绝对定位，先将元素的左上角通过 top:50%和 left:50%定位到页面的中心，然后再通过`transform:translate(-50%,-50%)`来调整元素的中心点到页面的中心

- 使用 flex 布局，通过`align-items:center`和`justify-content:center`设置容器的垂直和水平方向上为居中对齐，然后它的子元素也可以实现垂直和水平的居中

- 父盒子`高已知`的情况,子盒子是`行内元素`或者`行内块`时,给父盒子设置`text-align:center;`会使子盒子水平居中,给单行文本设置`line-height=盒子高度`；使文本在盒子里垂直居中

- 父盒子 `display: table-cell;vertical-align: middle;text-align: center;`的情况,子盒子是`行内元素`或者`行内块`时(table-cell 表格单元格的形式呈现，类似于 td 标签)

- 利用绝对定位，先将元素的左上角通过 top:50%和 left:50%定位到页面的中心，然后再通过 margin 负值来调整元素的中心点到页面的中心。该方法适用于盒子`宽高已知`的情况

- 盒子`宽高已知`的情况，通过 margin 调整元素的中心点到页面的中心。(注意盒子的塌陷问题,margin-top 不用 50%的原因是，百分比默认的是父盒子宽度的百分比)

- 利用绝对定位，设置四个方向的值都为 0，并将 margin 设置为 auto，由于宽高固定，因此对应方向实现平分，可以实现水平和垂直方向上的居中。该方法适用于盒子`有宽高`的情况

- 使用 JS 的思路大概给大家说下：

1、js 中只要获取到当前盒子具体的 left/top 值即可

2、一屏幕的宽高-盒子的宽高，最后除以 2，获取的值就是它应该具备的 left/top(这个值可以使盒子处于页面中间)

### 布局

- 单列布局
- 两列布局
  两栏布局（左侧宽度固定，右侧自适应）：左浮动或 absoulute，右 margin-left 调位置或者触发 BFC；使用 flex 布局；使用网格布局

- 三栏布局
  - 圣杯布局

```css
.content {
  padding: 30px 15%;
}

.fld {
  float: left;
  margin-left: -15%;
  width: 15%;
  height: 10rem;
}

.fcd {
  float: left;
  width: 100%;
}

.frd {
  float: right;
  margin-right: -15%;
  width: 15%;
  height: 10rem;
}
```

- 双飞翼布局

```css
.container {
  min-width: 600px; //确保中间内容可以显示出来，两倍left宽+right宽
}
.left {
  float: left;
  width: 200px;
  height: 400px;
  background: red;
  margin-left: -100%;
}
.center {
  float: left;
  width: 100%;
  height: 500px;
  background: yellow;
}
.center .inner {
  margin: 0 200px; //中间新增部分
}
.right {
  float: left;
  width: 200px;
  height: 400px;
  background: blue;
  margin-left: -200px;
}
```

> 两种布局实现方式对比:两种布局方式都是把主列放在文档流最前面，使主列优先加载。两种布局方式在实现上也有相同之处，都是让三列浮动，然后通过负外边距形成三列布局。两种布局方式的不同之处在于如何处理中间主列的位置：圣杯布局是利用父容器的左、右内边距+两个从列相对定位；双飞翼布局是把主列嵌套在一个新的父级块中利用主列的左、右外边距进行布局调整

- 粘连布局

特点：有一块内容`<main>`，当`<main>`的高康足够长的时候，紧跟在`<main>`后面的元素`<footer>`会跟在`<main>`元素的后面。当`<main>`元素比较短的时候(比如小于屏幕的高度),我们期望这个`<footer>`元素能够“粘连”在屏幕的底部

```html
<style>
  * {
    margin: 0;
    padding: 0;
  }
  html,
  body {
    height: 100%;
    /* //高度一层层继承下来  */
  }
  #content {
    min-height: 100%;
    background: pink;
    text-align: center;
    overflow: hidden;
  }
  #content .main {
    padding-bottom: 50px;
  }
  #footer {
    height: 50px;
    line-height: 50px;
    background: deeppink;
    text-align: center;
    margin-top: -50px;
  }
</style>

<div id="content">
  <div class="main"></div>
</div>
<footer></footer>
```

> (1)footer 必须是一个独立的结构，与 wrap 没有任何嵌套关系；(2)wrap 区域的高度通过设置 min-height，变为视口高度；(3)footer 要使用 margin 为负来确定自己的位置；(4)在 main 区域需要设置 padding-bottom。这也是为了防止负 margin 导致 footer 覆盖任何实际内容。

### 清除内外边距

> (body 会默认有个边距)

```css
* {
  margin: 0;
  padding: 0;
}
```

## CSS3

> 伪类选择器，伪元素选择器
> 支持更多颜色：HSL，HSLA,RGBA,opacity
> 弹性盒子
> 2d、3d 变换
> 圆角、阴影、渐变
> 过渡效果
> web 字体，显示电脑上没有的字体

### 私有前缀

> 浏览器厂商用于测试新的 css 特性
> [兼容性查询网站](https:www.caniuse.com)

- chrome/safari/edge：`-webkit-`
- firefox：`-moz-`
- opera：`-o-`
- IE：`-ms-`

### 单位

- `rem`：根元素字体的倍数
- `vh`：视口高的百分比
- `vw`：视口宽的百分比
- `vmax`：视口宽和高中大的那个的百分比
- `vmin`：视口宽和高中小的那个的百分比

### 盒模型属性

- `box-sizing`：`border-box`怪异盒模型，盒子的长宽包含内边距和边框,`content-box`标准盒模型，长宽不包含内边距和边框
- 调节盒子大小 `resize`：需添加`overflow`属性。值有（`horizontal`水平方向可调教大小,`vertical`垂直方向,`both`都可以）
- 盒子阴影`box-shadow`：`3px水平阴影 0px垂直阴影 5px模糊距离 0px阴影尺寸 #fff颜色 inset（内阴影）`不占空间
- 透明：`opacity:0.5`子元素也会跟着变化

### 边框属性

- 圆角边框`border-radius`：`10px|10%|顺时针四个参数`最大值为 50%;

```css
border-top-left-radius: 10px;
border-top-right-radius: 10px;
border-bottom-left-radius: 100px 200px; /* 椭圆x轴半径 , y轴半径*/
/*椭圆*/
border-raduis: 10px 20px 30px 40px /10px 20px 40px 10px;
```

- 外轮廓：

```css
/*不占位置*/
.test {
  outline-width: 20px;
  outline-color: red;
  outline-style: solid;
  outline-offset: 10px;
}
.test2 {
  outline: 20px solid blue;
  outline-offset: 10px;
}
```

### 文本属性

- 文字阴影：`text-shadow:水平阴影 垂直阴影 模糊距离 颜色`

- 文本换行`white-space`：`normal`,`pre`原文显示,`pre-wrap`超出边界换行.`pre-line`超出边界换行，只识别文本中的换行，行前行尾空格会忽略.`no-wrap`不换行

- 文本移除`text-overflow`：`ellipsis`需配合`overflow:hidden(非visible);white-space:no-wrap`使用|`clip`默认值
- 文本修饰

```css
text-decoration-line: overline;
text-decoration-style: solid;
text-decoration-line: red;
text-decoration: overline solid red;
```

- 文本描边(wekit)

```css
-webkit-text-stroke-color: red;
-webkit-text-stroke-width: 10px;
-webkit-text-stroke: 10px green;
```

### 背景

- 背景图原点`background-origin`：`paddding-box`默认，从 padding 区域开始；`border-box`从 border 区域开始，`content-box`：从内容区域开始

- 背景图向外裁切区域`background-clip`：`paddding-box`默认，从 padding 区域开始；`border-box`从 border 区域开始，`content-box`：从内容区域开始；`text`背景图显示在文字上，需要加上-webkit-前缀

- 背景图位置`background-position`：移动的距离是这个目标图片的 x，y 坐标，一般情况是向上或左移动，所以数值是负值；

> 精灵图
> 将多个小背景图整合到一张大图片中，减少了服务器的请求

- 背景图大小`background-size`：`300px 200px`不能为负值；`100% 20%`不能为负值；`auto`默认；`contain`图片整张放在 div 中，可能图片会重复铺满；`cover`图片充满 div,可能会裁切一部分

- 复合写法`background:color url repeat position / size origin clip`size 必须写在 position 后面且用斜杠分割，origin 和 clip 相同可以少写后一个，
- 多个背景`background:url('xx.jpg') no-repeat right top,url('xx.jpg') no-repeat right bottom,url('xx.jpg') no-repeat left top,`

### 渐变

> 本质是图片

- 线性渐变背景：`background:linear-gradient(100px|100px 20px半径,to bottom|to top|to right|to left|to left top|20deg,#fff(开始颜色),#3a3 10%,#3a3 100px,#000(结束颜色));`

- 径向渐变背景：`background:radial-gradient(at bottom|at top|at right|at left|at left top|circle|at 20px 100px,#fff(开始颜色),#3a3 10%,#3a3 100px,#000(结束颜色));`

- 重复线性渐变背景：`background:repeating-linear-gradient(100px|100px 20px半径,to bottom|to top|to right|to left|to left top|20deg,#fff(开始颜色),#3a3 10%,#3a3 100px,#000(结束颜色));`

- 重复径向渐变背景：`background:repeating-radial-gradient(at bottom|at top|at right|at left|at left top|circle|at 20px 100px,#fff(开始颜色),#3a3 10%,#3a3 100px,#000(结束颜色));`

### 2d/3d 变换

- 位移属性（不能用于行内元素）：`transform:translateX(20%)|translateY(-10%)|translateZ(20px)|translate(10px,10px,0px);`位移时默认是元素的中心位置,不脱离文档流
- 缩放（不能用于行内元素）：`transform:scaleX(1.5)|scaleY(1.5)|scaleZ(1.5)|scale(1.5)|scale3d(1.5);`默认为 1 正常大小，通过缩放可实现字体小于浏览器的最小字体限制；`transform-origin:50% 50%;`元素缩放中心点位置
- 旋转：`transform:rotateX()|rotateY()|rotate3d()|rotateZ(30deg)|rotate(30deg);`z 轴是垂直与屏幕的
- 透视：`transform:perspective(1000px);`透视距离，必须写在前面；

- 过渡动画：`transition: width(要过渡的属性) 1s(过渡时间) ease-in(过渡方式) 2s(延迟时间)`

> transition 过渡方式 ：ease：慢速开始，然后变快，然后慢速结束；ease-in：以慢速开始的过渡效果；ease-out：以慢速结束的过渡效果；ease-in-out：以慢速开始和结束的过渡

- 循环动画：

```css
/* //定义动画 */
@keyframe beat {
  /* //关键帧 */
  30%: {
    transform: scale(1.3);
  }
  100%: {
    transform: scale(1);
  }
}

/* //使用动画 */

span.heart {
  animation: beat(动画名称) 1.5s (动画时长) infinite(循环动画);
}
```

---

- box-shadow 可同时设置多个阴影，如

```css
.test {
  box-shadow: #fff 22px -15px 0 6px, #fff 57px -6px 0 2px, #fff 87px 4px 0 -4px,
    #fff 33px 6px 0 6px;
}
```

- 使用 clip-path 可快速裁剪盒子形状，使用 shape-outsizes 可实现文字不规则环绕效果（注意兼容问题）
- transform 中的 rotate 属性旋转后，其内部内容会随之旋转。
- 设置字间距可使用 letter-spacing 属性增加或减少字符间的空白（字符间距）。该属性定义了在文本字符框之间插入多少空间(允许使用负值)
- CSS 中用于设置过渡特效的属性是 transition，但是其不能同时使用在多个属性身上。

### 字体图标和 web 字体

精灵图放大会失真，字体图标不会，且可以改变颜色大小，只适合简单图形；

字体图标下载网站：`http://icomoon.io` `https://iconfont.cn`

使用方法： 1.添加字体声明在`<style></style>`中

```css
@font-face {
  font-family: "xxname";
  src: url("./xxx.ttf");
  src: url("./xxx.ttf"), url("./xxx.ttf"), url("./xxx.ttf") format("woff");
}
h1 {
  font-family: "xxname";
  font-size: 19px;
}
```

2.使用:从官网复制图标粘贴到相应位置并设置样式:`span{font-family:'xxname';font-size:....}`

### 三角形

盒边距设置为 0，边框宽度控制大小，边框背景设置为透明

```css
.box {
  width: 0px;
  height: 0px;
  border: 20px solid transparent;
  border-left-color: red;
  /*照顾兼容性*/
  line-height: 0;
  font-size: 0;
}
```

直角三角形

```css
width: 0;
height: 0;
/*上边框调大*/
border-top: 100px solid transparent;
border-right: 50px solid red;
/*左边和下边边框设置为0*/
border-bottom: 0;
border-left: 0;
```

### 鼠标样式

```css
li {
  /*cursor:default|pointer(手指)|move|text(文本)|not-allowed(禁止);*/
  cursor: url("xxx.png") pointer;
}
```

### 防止文本域被拖拽

`textarea{resize:none;}`

### vertical-align

用于设置行内块垂直对齐方式(必须作用与 inline-block 元素)
`vertical-align:baseline(基线)|top|middle|bottom;`

- 解决图片底部空白缝隙问题：添加 vertical-align 属性；或转换为块级元素`display:block;`

### 溢出文字省略号表示

- 单行

```css
 {
  /* 1.强制一行显示文本 */
  white-space: nowrap;
  /* 2.超出部分隐藏 */
  overflow: hidden;
  /* 3.超出文字用省略号表示 */
  text-overflow: ellipsis;
}
```

- 多行

```css
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

```css
ul li {
  list-style: none;
  float: left;
  width: 111px;
  height: 222px;
  border: 2px solid red;
  margin-left: -1px;
}
```

鼠标移动显示边框

```css
/*若盒子有定位，则利用z-index提高层级*/
ul ll:hover {
  z-index: 1;
  border: 1px solid red;
}
/*若盒子没有定位则鼠标经过时添加相对定位*/
ul li:hover {
  position: relative;
  border: 2px solid red;
}
```

- 修改滚动条样式

```css
/* 【1】IE */
/* IE浏览器支持通过CSS样式来改变滚动条的部件的自定义颜色 */
/*  
　　scrollbar-face-color滚动条凸出部分的颜色
 
　　scrollbar-shadow-color立体滚动条阴影的颜色
 
　　scrollbar-highlight-color滚动条空白部分的颜色
 
　　scrollbar-3dlight-color滚动条亮边的颜色
 
　　scrollbar-darkshadow-color滚动条强阴影的颜色
 
　　scrollbar-track-color滚动条的背景颜色
 
　　scrollbar-arrow-color上下按钮上三角箭头的颜色
 
　　scrollbar-base-color滚动条的基本颜色 */

/* 【2】webkit */
/* 
::-webkit-scrollbar 滚动条整体部分
::-webkit-scrollbar-thumb  滚动条里面的小方块，能向上向下移动（或往左往右移动，取决于是垂直滚动条还是水平滚动条）
::-webkit-scrollbar-track  滚动条的轨道（里面装有Thumb）
::-webkit-scrollbar-button 滚动条的轨道的两端按钮，允许通过点击微调小方块的位置。
::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去）
::-webkit-scrollbar-corner 边角，即两个滚动条的交汇处
::-webkit-resizer 两个滚动条的交汇处上用于通过拖动调整元素大小的小控件
 */
/* ///////////////////////////////////////////////////////////////////// */
/* 定义滚动条样式 */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
  background-color: rgba(240, 240, 240, 1);
}

/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track {
  box-shadow: inset 0 0 0px rgba(240, 240, 240, 0.5);
  border-radius: 10px;
  background-color: rgba(240, 240, 240, 0.5);
}

/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb {
  border-radius: 10px;
  box-shadow: inset 0 0 0px rgba(240, 240, 240, 0.5);
  background-color: rgba(240, 240, 240, 0.5);
}
```

## 预处理器 less

### 浏览器用法

> 运行时编译
> 下载 `less.js` and include it in a `<script></script>` tag in the `<head>` element of your page:

```html
<style text="text/less">
  .test {
    .test2 {
      color: red;
    }
  }
</>
<script src="less.js" type="text/javascript"></script>
```

### vscode

easyless 插件，预编译生产 css

### 注释

```less
//不会编译到css文件中
/*会编译到css文件中*/
.test {
  .test2 {
    color: red;
  }
}
```

### 变量

```less
//使用＠声明一个变量,变量可重复定义，遵循块级作用域
@color1:red;//值，可直接使用
@m:margin;//属性名，使用需要加花括号
@selector:#test3;//类名，使用需要加花括号
@url:"../../bg.jpg";//地址链接，使用需要加花括号
@themes: "../../src/themes";

@import "@{themes}/tidal-wave.less";
.test{
   .test2{
      color:@color1;
      @{m}:0px 1px 2px 3px
   }
   @{selector}{
      color:red;
      background:url(@{url})
   }

}

@primary:  green;
@color: primary;

.section {
    color: @@color;
}
```

### 嵌套

```less
.test {
  //&表示当前选择器的父级,代表所有父选择器（不仅仅是最近的祖先）
  & .test2 {
  }
  .test3 {
  }
}
```

### 混入 mix-in

> 将一系列属性从一个规则集引入到另一个规则集的方法(处理重复 css 样式)

- 普通混合

```less
//会编译到css文件中
.common {
  color: red;
  margin: 10px;
}

.test {
  .test2 {
    font-size: 12;
    .common();
  }
  .test3 {
    font-size: 16;
    .common();
  }
}
```

- 不带输出的混合

```less
//不会编译到css文件中
.common() {
  color: red;
  margin: 10px;
}

.test {
  .test2 {
    font-size: 12;
    .common();
  }
  .test3 {
    font-size: 16;
    .common();
  }
}
```

- 带参数的混合
  参数只用逗号分隔，但后来添加了分号以支持将逗号分隔的列表值传递给单个参数。
  逗号分隔的默认值： `.name(@param1: red, blue;1px;)`.
  从 Less 4.0 开始，您可以使用括号转义 `[~()]` 来包装列表值，例如 `.name(@param1: ~(red, blue))`。 这类似于引用转义语法：`~"quote"`

```less
//不会编译到css文件中
.common(@w:red,@h) {
  color: @w;
  margin: @h;
}

.test {
  .test2 {
    font-size: 12;
    .common(red,10px);
  }
  .test3 {
    font-size: 16;
    .common(pink,20px);
  }
}
```

- `!important` 关键字
  在 mixin 调用后使用 !important 关键字将其继承的所有属性标记为 !important：

```less
.foo (@bg: #f5f5f5; @color: #900) {
  background: @bg;
  color: @color;
}

.important {
  .foo() !important;
}

//编译结果
.important {
  background: #f5f5f5 !important;
  color: #900 !important;
}
```

- 重载混入
  定义多个具有相同名称和参数数量的混合是合法的。 Less 将使用所有可以应用的属性。 如果您使用带有一个参数的 mixin，例如 .mixin(green);，然后将使用具有一个强制参数的所有 mixin 的属性：

```less
.mixin(@color) {
  color-1: @color;
}
.mixin(@color, @padding: 2) {
  color-2: @color;
  padding-2: @padding;
}
.mixin(@color, @padding, @margin: 2) {
  color-3: @color;
  padding-3: @padding;
  margin: @margin @margin @margin @margin;
}
.some .selector div {
  .mixin(#008000);

//编译成
.some .selector div {
  color-1: #008000;
  color-2: #008000;
  padding-2: 2;
}
```

- 命名参数
  mixin 引用可以通过名称而不是位置来提供参数值。 任何参数都可以通过其名称来引用，并且它们不必按任何特殊顺序排列：

```less
.mixin(@color: black; @margin: 10px; @padding: 20px) {
  color: @color;
  margin: @margin;
  padding: @padding;
}
.class1 {
  .mixin(@margin: 20px; @color: #33acfe);
}
.class2 {
  .mixin(#efca44; @padding: 40px);

```

- `@arguments`
  `@arguments` 在 mixin 中有特殊含义，它包含调用 mixin 时传递的所有参数。 如果您不想处理单个参数，这很有用：

```less
.box-shadow(@x: 0, @y: 0, @blur: 1px, @color: #000) {
  -webkit-box-shadow: @arguments;
  -moz-box-shadow: @arguments;
  box-shadow: @arguments;
}
.big-block {
  .box-shadow(2px, 5px);
}

//结果
.big-block {
  -webkit-box-shadow: 2px 5px 1px #000;
  -moz-box-shadow: 2px 5px 1px #000;
  box-shadow: 2px 5px 1px #000;
}
```

- 高级参数和 `@rest` 变量
  mixin 接受可变数量的参数，你可以使用 `...`。 在变量名之后使用它会将这些参数分配给变量。

```less
.mixin(...) {        // matches 0-N arguments
.mixin(@a: 1, ...) { // matches 0-N arguments
.mixin(@a, @rest...) {
   // @rest is bound to arguments after @a
   // @arguments is bound to all arguments
}
```

- 模板匹配
  希望根据传递给它的参数更改混入的行为

````less
.mixin(dark, @color) {//它期望 dark 作为第一个参数
  color: darken(@color, 10%);
}
.mixin(light, @color) {//它需要 light
  color: lighten(@color, 10%);
}
.mixin(@_, @color) {//它期望任何值。
  display: block;
}


//使用
@switch: light;

.class {
  .mixin(@switch, #888);
}
//编译
.class {
  color: #a2a2a2;
  display: block;
} ```
````

- 覆盖混入值
  如果您有多个匹配的 mixins，所有规则都会被评估和合并，并返回具有该标识符的最后一个匹配值。

```less
// library.less
#library() {
  .mixin() {
    color: red;
    prop: foo;
  }
}

// customize.less
@import "library";
#library() {
  .mixin() {
    color: red;
    prop: bar;
  }
}
.box {
  my-value: #library.mixin[prop]; //可以使用属性/变量访问器从已评估的混入规则中选择一个值
}
//编译
.box {
  my-value: bar;
}
```

- 递归混入

```less
.loop(@counter) when (@counter > 0) {
  .loop((@counter - 1));    // next iteration
  width: (10px * @counter); // code for each iteration
}

div {
  .loop(5); // launch the loop
}

.generate-columns(4);

.generate-columns(@n, @i: 1) when (@i =< @n) {
  .column-@{i} {
    width: (@i * 100% / @n);
  }
  .generate-columns(@n, (@i + 1));



//编译
div {
  width: 10px;
  width: 20px;
  width: 30px;
  width: 40px;
  width: 50px;
}

.column-1 {
  width: 25%;
}
.column-2 {
  width: 50%;
}
.column-3 {
  width: 75%;
}
.column-4 {
  width: 100%;
}
```

- 混入守卫

守卫中可用的比较运算符的完整列表是： `>`、`>=`、`=`、`=<`、`<`。 此外，关键字 `true` 是唯一的真值

```less
.mixin(@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin(@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin(@a) {
  color: @a;
}

//使用
.class1 {
  .mixin(#ddd);
}
.class2 {
  .mixin(#555);
}

//编译
.class1 {
  background-color: black;
  color: #ddd;
}
.class2 {
  background-color: white;
  color: #555;
}
```

逻辑运算符
使用 `and` 关键字组合守卫：

```less
.mixin(@a) when (isnumber(@a)) and (@a > 0) {
  ...;
}
```

您可以通过用逗号 `,` 分隔守卫来模拟 `or` 运算符。 如果任何一个守卫评估为真，它被认为是一场比赛：

```less
.mixin(@a) when (@a > 10), (@a < -10) {
  ...;
}
```

使用 `not` 关键字否定条件：

```less
.mixin(@b) when not (@b > 0) {
  ...;
}
```

类型检查函数
`iscolor`
`isnumber`
`isstring`
`iskeyword`
`isurl`

检查一个值除了是一个数字之外是否在一个特定的单位中
`ispixel`
`ispercentage`
`isem`
`isunit`

- 别名
  Mixins 可以赋值给一个变量来作为变量调用调用，也可以用于 map lookup。

```less
#theme.dark.navbar {
  .colors(light) {
    primary: purple;
  }
  .colors(dark) {
    primary: black;
    secondary: grey;
  }
}

.navbar {
  @colors: #theme.dark.navbar.colors(
    dark
  ); //分配给变量和不带参数调用的 mixin 调用始终需要括号
  @colors: #theme.dark.navbar.colors; //错误
  background: @colors[primary];
  border: 1px solid @colors[secondary];
}
```

### 继承

Extend 是一个 Less 伪类，它将它所放置的选择器与与其引用的匹配的选择器合并。

```less
//两种写法
.a:extend(.b, .c) {
}
.a {
  &:extend(.b,
  .c);
}
//继承所有属性
.c:extend(.d all) {
  // extends all instances of ".d" e.g. ".x.d" or ".d.x"
}

.c:extend(.d) {
  // extends only instances where the selector will be output as just ".d"
}
```

- 继承附加到选择器

附加到选择器的扩展看起来像一个普通的伪类，将选择器作为参数。 一个选择器可以包含多个扩展子句，但所有扩展都必须位于选择器的末尾。

- 在选择器之后扩展： `pre:hover:extend(div pre)`.
- 选择器和扩展之间的空间是允许的： `pre:hover :extend(div pre)`.
- 允许多个扩展： `pre:hover:extend(div pre):extend(.bucket tr)` - 注意这与 `pre:hover:extend(div pre, .bucket tr)` 相同
- 这是不允许的： `pre:hover:extend(div pre).nth-child(odd)`. 扩展必须在最后

### 合并

允许将来自多个属性的值聚合到单个属性下的逗号或空格分隔列表中,对于背景和转换等属性很有用。

- 逗号`+`

```less
mixin() {
  box-shadow+: inset 0 0 10px #555;
}
.myclass {
  .mixin();
  box-shadow+: 0 0 20px black;
}

//编译
.myclass {
  box-shadow: inset 0 0 10px #555, 0 0 20px black;
}
```

- 空格`+_`

```less
.mixin() {
  transform+_: scale(2);
}
.myclass {
  .mixin();
  transform+_: rotate(15deg);
}

//编译
.myclass {
  transform: scale(2) rotate(15deg);
}
```

### `@import` 规则

在标准 CSS 中，@import at 规则必须在所有其他类型的规则之前。 但是 Less 不关心你把 @import 语句放在哪里。如果它没有扩展名，则会附加 .less 并将其作为导入的 Less 文件包含在内。

#### 导入选项

- reference: 导入外部文件，但除非引用，否则不将导入的样式添加到编译输出。
- inline: 在输出中包含源文件但不处理它
- less: 将文件视为 Less 文件，无论文件扩展名是什么
- css: 将文件视为 CSS 文件，无论文件扩展名是什么
- once: 只包含文件一次（这是默认行为）,这意味着该文件仅导入一次，该文件的后续导入语句将被忽略。
- multiple: 允许导入多个同名文件。 这是与一次相反的行为。
- optional: 允许仅在文件存在时导入该文件。 如果没有 optional 关键字，Less 会在导入找不到的文件时抛出 FileError 并停止编译。

### `@plugin` 规则

```js
// my-plugin.js
install: function(less, pluginManager, functions) {
    functions.add('pi', function() {
        return Math.PI;
    });
}
```

使用

```less
@plugin "my-plugin";
.show-me-pi {
  value: pi();
}

//编译
.show-me-pi {
  value: 3.141592653589793;
}
```

## react 使用 less

在终端中输入`npm install less-loader less --save`
我们在项目根目录中找到刚才生成的 config 文件，找到里面的`webpack.config.js`这个文件：
然后搜索`sassRegex`就可以找到如下代码：

```js
// style files regexes
const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;
const sassRegex = /\.(scss|sass)$/;
const sassModuleRegex = /\.module\.(scss|sass)$/;
//增加
const lessRegex = /\.less$/;
const lessModuleRegex = /\.module\.less$/;
```

接着再搜索`sassRegex`，声明了变量肯定要用，添加如下代码：

```js
// Opt-in support for less (using .scss or .sass extensions).
// By default we support less Modules with the
// extensions .module.less
{
  test: lessRegex,
  exclude: lessModuleRegex,
  use: getStyleLoaders(
    {
      importLoaders: 3,
      sourceMap: isEnvProduction && shouldUseSourceMap,
    },
    'less-loader'
  ),
  // Don't consider CSS imports dead code even if the
  // containing package claims to have no side effects.
  // Remove this when webpack adds a warning or an error for this.
  // See https://github.com/webpack/webpack/issues/6571
  sideEffects: true,
},
// Adds support for CSS Modules, but using less
// using the extension .module.less
{
  test: lessModuleRegex,
  use: getStyleLoaders(
    {
      importLoaders: 3,
      sourceMap: isEnvProduction && shouldUseSourceMap,
      modules: {
        getLocalIdent: getCSSModuleLocalIdent,
      },
    },
    'less-loader'
  ),
},

```

另外，导入 less 还会报错，在根目录的 react-app-env.d.ts 加入

```ts
declare module "*.module.less" {
  const classes: { readonly [key: string]: string };
  export default classes;
}
```
