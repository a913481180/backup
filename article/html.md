---
title: HTML超文本标记语言
date: 2020-11-10 20:22:11
categories:
- study
---
# HTML 是用来描述网页的一种语言

emmet语法：
```
div*3快速生成3个<div></div>
父子级关系：ul>li
兄弟关系：div+p
生成带有类名的：.demo或#two
有顺序的：加上$符号
在生成的标签内写内容：{}
```

浏览器内核:

|浏览器|内核|
|-|-|
|IE|Trident|
|firefox|Gecko|
|safari|Webkit|
|chrome/Opera|Blink|

## HTML 标签

>HTML 标记标签通常被称为 HTML 标签 (HTML tag)。

- HTML 标签是由尖括号包围的关键词，比如 <html>
- HTML 标签通常是成对出现的，称为双标签,如 <html> 和 </html>.也有单标签：<br/>
- 标签对中的第一个标签是开始标签，第二个标签是结束标签
- 开始和结束标签也被称为开放标签和闭合标签 

### 常用标签

- 标题标签:`<h1>----<h6>`

- 段落标签：`<p>`

- 换行标签：`<br />`

- 分割线： `<hr>`

- 字体：`<h1>这是1号标题</h1> <font size="6">这是6号字体文本</font> 可以使用font-family（字体），color（颜色），和font-size（字体大小）属性来定义字体的样式,使用 text-align（文字对齐）属性指定文本的水平与垂直对齐方式`  

- 内部样式表
```
当单个文件需要特别样式时，就可以使用内部样式表。你可以在<head> 部分通过 <style>标签定义内部样式表:<head>
<style type="text/css">
body {background-color:yellow;}
p {color:blue;}
</style>
</head>
```

- 外部样式表
```
当样式需要被应用到很多页面的时候，外部样式表将是理想的选择。
使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观。

<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```
- 背景色属性:`<p style="background-color:green;">这是一个段落。</p>`

- 加粗：`<strong>或<b>`

- 斜体：`<em>或<i>`

- 下划线：`<ins>或<u>`

- 盒子: `<div>与<span>`

- 图像标签和路径：`<img src="图像路径"/>`

|属性|说明|
|-|-|
|src|图片路径|
|alt|图片不显示时提醒文字|
|title||图片文字提醒|
|border|边框|
|width,height|宽，高|

- 超链接标签: `<a href="跳转目标(外连接如www.xxx.com，内链接如xxx.html,空链接:#，下载链接)" target="弹出方式(_self在本窗口打开，_blank在新窗口打开,_top跳出弹窗,">文本图像</a>`


- 锚点: `<a href="#one"></a>   <h3 id="one">标题</h3>`

- 特殊符号:

|||
|-|-|
|空格|`&nbsp;`|
|小于号|`&lt;`|
|大于号|`&gt;`|

- 表格标签：
```
<table>  <!--定义表格标签-->
<thead><!--头部-->
<tr>  <!--行-->
<th></th><!--表头，可加粗-->
<td></td><!--列，必须嵌套在行中-->
</tr></thead>
<tbody><!--主体-->
<tr><td colspan="2"></td></tr><!--合并单元格-->
<tr><td rowspan=2"><td></td></td></tr>
<tr><td><td></td></td></tr>
<tr><td></td></tr></tbody>
</table>
```
表格属性：

|属性名|属性值|描述|
|-|-|-|
|align|left,center,right|表格相对周围元素的对齐方式|
|border|1或""|边框|
|cellpadding|像素值|单元边沿与内容的的距离|
|cellspacing|像素值|单元格间的空白|
|width，height||高，宽|

- 列表标签

   1. 无序标签：`<ul><li>列表</li><li>列表</li><li>列表</li></ul>`;去掉小圆点：`list-style:none`
   2. 有序标签：`<ol><li>列表</li><li>列表</li><li>列表</li></ol>`
   3. 自定义列表：`<dl><dt>名词</dt><<dd>名词解释</dd>dd>名词解释</dd><dl>`

- 表单域：
`<form action="url地址(服务器程序的url地址)" method="提交方式(get/post)" name="表单域名称">表单域控件</form>`

- input表单元素：
`<input type="属性值" name="名称;单选按钮与复选框应是相同的name值" value="规定input元素的值" checked="首次加载时选中" maxlength="输入字段的最大长度"/>`
```
去除边框：
border:none;
ouline:none;
居中：
vertical-align:middle;
隐藏radio的按钮:
display:none
```

|type属性值|描述|
|-|-|
|button|按键|
|checkbox|定义复选框|
|file|供文件上传|
|hidden|隐藏输入字段|
|image|图像形式提交按钮|
|password|密码字段|
|radio|定义单选按钮|
|reset|重置按钮|
|submit|提交按钮|
|text|输入字段|

- label标签：用于绑定表单域，点击自动调整到输入框
```
<label>for="sex"> 男</label>
<input type="radio" name="sex" id="sex" />
```

- select:下拉列表
```
<select>
/*至少包含一对option*/
<option></option>
<option></option>
/*默认选中*/
<option selected="selected">xxx</option>
</select>
```

- textarea:多行文本输入框
```
<textarea rows="3" cols="28">   /*cols:每行字符数； rows：行数*/
内容
</textarea>
```
---
--- 

# css

### 选择器

#### 1.基础选择器：

- 标签选择器
```
......
<style>
p{
   color:red;
}
div{
   color:red;
}
</style>
....
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
   ....
   /*只能使用一次*/
}
.....
<div id="xx">
...
</div>
```

- 通配符选择器：
```
*{
   ....
   margin:0;
   padding:0;
   /*不需要调用，自动给所有的元素使用*/

}
```


#### 2.符合选择器：

- 后代选择器: 
```
元素1 元素2 {样式 ;选择元素1里面的所有元素2(后代元素);元素1，2用空格隔开}

如：
ul li {xxxx}
```

- 子选择器
```
元素1>元素2 {xxxx选择元素1里的所有直接后代元素2; 元素2必须是亲儿子}
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
---

#### Css的三种样式表

1. 行内样式表
2. 内部样式表
3. 外部样式表`<link rel="stylesheet" href="css文件路径">`

一般情况下，优先级如下：
`（内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式`
`!important>内联样式 > id 选择器 > 类选择器 = 伪类选择器 = 属性选择器 > 标签选择器 = 伪元素选择器>继承`

注意：如果外部样式放在内部样式的后面，则外部样式将覆盖内部样式。

---

#### 元素显示模式

1. 块元素：`<h1> <p> <div> <ul>....`独占一行；宽高边距可控；文字列元素内不能使用块级元素如`<p>`与`<h1>`内不能放`<div>`;图片`<img>`可放在`<p>`内调整位置

2. 行内元素：`<a>,<span>,<b>,<em>...`一行多个；宽高无法直接设置；只能容纳文本或其他行内元素；特别的`<a>`内可以放块级元素，给`<a>`转换一下块级模式最安全

3. 行内块元素：`<img />,<input />,<td>`高宽可设置；一行多个

显示模式转换：
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

#### 字体属性

- font-family:默认使用第一个字体,` font-family:"Microsoft yahie" ,"hiragino Sans GB",Arial; `

- font-size: 单位px

- font-weight: `nomal或400`不加粗;`bold或700`加粗；`100-900`

- font-style:`normal`默认；`italic`斜体；

- color：支持十六进制、rgb（255,5,5）、预设颜色值red

- text-align:文本内容对齐left,right,center;

- text-decoration:文本装饰；`none`没有装饰线；`underline` 下划线；`line-through`删除线；

- text-indent:缩进；单位em代表1个文字大小；px像素

- line-height:行高，单位px;

- 字体复合属性：`font: font-style font-weight font-size/line-height font-family;`顺序不能换，必须保留`font-size` 和`font-family`属性

--- 
css背景

- 背景颜色：`background-color`
- 背景图片：`background-image:url()`
- 背景图片平铺：`background-repeat:no-repeat|repeat-x|repeat-y`
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
### 盒子模型

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
可分开设置
margin-left:
nargin-bottom:
复合写法与padding一致
让块级盒子居中：盒子有宽度，左右设为auto；
margin：0，auto；
行内元素只需要在父级元素添加text-align:center即可；

嵌套元素塌陷问题：
父元素与子元素有上外边距时，父元素会塌陷较大的外边距值
解决方法：
为父元素设置：上边框或上内边距
为父元素添加overflow：hidden；
```
清除内外边距
```
* {
   margin:0;
   padding:0;
}
```
- 圆角边框：`border-radius:10px|10%|顺时针四个参数`;
- 盒子阴影：`box-shadow:水平阴影 垂直阴影 模糊距离 阴影尺寸 颜色 inset（内阴影）`不占空间
- 文字阴影：`text-shadow:水平阴影 垂直阴影 模糊距离 颜色`

---
浮动
多个块级元素纵向排列用标准流，横向排列用浮动
特性：
1. 脱离标准流的控制，不再保留原先的位置
2. 所有浮动一行对齐.上对齐,父级宽度不够时会换行
3. 具有行内块特性
```
选择器 { 
   float:none|left|right；
}
```
常用标准流约束浮动位置

- 清除浮动： 父级盒子不设置高度时，子盒子浮动，脱标不占位置，则父级盒子高度为0；
```
选择器{
   clear:属性值left|right|both；
}
```
  1. 额外标签法：在浮动元素的末尾添加一个空标签如`<div style="clear:both"></div>`
  2. 父级添加`overflow:hidden|auto|scroll;`
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
3. 叠放次序：`选择器{z-index:1;}`数值可正可负；数值越大盒子越靠上；只有定位的盒子才能有z-index属性；

>子级是绝对定位的话，父级要用相对定位，子盒子不需要占有位置，父级需要占有位置；
>行内元素添加绝对或固定定位，可直接设置宽高；块级元素添加后不设置宽高则默认是内容大小；
>浮动元素可以压住标准流盒子；但不会压住标准流盒子里的文字和图片；

### 元素的显示与隐藏

- display:none（隐藏）|block(除转换为块级元素外，还可显示元素);
>display隐藏后不再占有原来的位置；

- visibility:visible(可视)|hidden；
>visibility:隐藏元素后，继续占有原来的位置；

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

用于设置行内块垂直对齐方式
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
### 布局技巧
利用浮动元素不会压住文字；可做出文字环绕图片效果；

### HTML5特性

#### 新增语义化标签

```
<header>; 
<nav>;
<article>;
<setcion>;
<footer>;
<aside>
```

#### 新增多媒体标签

- <video>

`<video src="" controls="controls" ></video>`

属性：

|||
|-|-|
|autoplay|自动播放，谷歌禁止，可添加muted属性解决|
|controls|播放控件|
|width;height|宽高|
|loop|循环播放|
|preload|auto(加载视频);none(不加载)|
|poster|imgurl(加载等待图片)|
|muted|静音播放|

- <audio>

`<audio src="" ></audio>`

|||
|-|-|
|autoplay|自动播放，谷歌禁止|
|loop|循环播放|
|controls|播放控件|

#### input类型

|属性值|说明|
|-|-|
|type="email"|限制必须输入email类型|
|type="number"|数字类型|
|type="search"|搜索框|
|type="color"|颜色|
|type="tel"|手机号码|
|type="date"|日期|

#### 表单属性

|属性|说明|
|-|-|
|placeholder|提示文本|
|multiple|可多选文件提交|
|autofocus|自动聚焦|
|required|不能为空，必填|
