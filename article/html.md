---
title: HTML超文本标记语言
date: 2021-03-10 20:22:11
categories:
- web 
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

- HTML 标签是由尖括号包围的关键词，比如` <html>`
- HTML 标签通常是成对出现的，称为双标签,如` <html>` 和` </html>`.也有单标签：`<br/>`
- 标签对中的第一个标签是开始标签，第二个标签是结束标签
- 开始和结束标签也被称为开放标签和闭合标签 

网页：

```
<!DOCTYPE html>	//使用html5标准进行解析网页
<html lang>
<head>			//网页头部
<title>title</title>
</head>
<style></style>
<script></script>
<body></body>
</html>

```
- base

`<base href="" target="_top">`所有链接标签的默认链接

- meta视口标签

常用于指定网页描述、关键词、作者、事件等

`<meta name="viewport" content="width=device-width,user-scalable=no,initial-scale=1.0,maximum-scale=1.0">`

|属性|说明|
|-|-|
|name="viewport"|content="width=(设置的是viewport宽度,可设置device-width特殊值),initial-scale=(初始缩放比，大于0的数字小于10),maximum-scale=（最大缩放比，大于0的数字)，minimum-scale=(最小缩放比，大于0的数字)，user-scalable=(用户是否可以缩放)"|
|name="http-equiv"|额外添加头部内容|
|charset|编码|


### 常用标签

- 标题标签:`<h1>----<h6>`

- 段落标签：`<p>`

- 换行标签：`<br />`

- 分割线： `<hr>`

- 字体：`<h1>这是1号标题</h1> <font size="6">这是6号字体文本</font> 可以使用font-family（字体），color（颜色），和font-size（字体大小）属性来定义字体的样式,使用 text-align（文字对齐）属性指定文本的水平与垂直对齐方式`  

>嵌套问题：超链接不能嵌套超链接，p标签不能嵌套p标签，标题标签h1-h6也不能嵌套；

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
<!--定义了文档与外部资源的关系-->
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
<table width="500px" border="1px" cellspacing="0">  <!--定义表格标签,属性引号可要可不要-->
	<thead><!--头部-->
		<tr> 
			<th></th><!--表头，td->th加粗文字-->
			<td></td><!--列，必须嵌套在行中-->
		</tr>
	</thead>
	<tbody><!--主体-->
<col width="100px"><!--第1列的宽度-->
<col width="100px"><!--第2列的宽度-->
<colgroup span="6" width="100px"><!--接下来6列合成一组，一起设置宽度-->
		<tr> <!--行-->
			<td colspan="2"></td><!--合并单元格,需要删除多余的单元格-->
		</tr>
		<tr>
			<td rowspan=2"><!--合并行,需要在下面行中删除一个单元格--></td>
				<td></td>
		</tr>
		<tr height="100px">	<!--设置行高，宽无法设置-->
			<td width="100px">	<!--设置列宽，一整列都一致变化,哪个大用哪个-->
			</td>
			<td></td>
		</tr>
		<tr>
			<td></td>
		</tr>
	</tbody>

	<tfoot><!--页脚-->
	</tfoot>
</table>
```

表格属性：

|属性名|属性值|描述|
|-|-|-|
|align|left,center,right|表格相对周围元素的对齐方式|
|border|1或"像素值"|边框|
|cellpadding|像素值|单元边沿与内容的的距离|
|cellspacing|像素值|单元格间的空白|
|width，height||高，宽|

- 列表标签

   1. 无序标签：`<ul><li>列表</li><li>列表</li><li>列表</li></ul>`;去掉小圆点：`list-style:none`
   2. 有序标签：`<ol><li>列表</li><li>列表</li><li>列表</li></ol>`
   3. 自定义列表：`<dl><dt>名词</dt><<dd>名词解释</dd>dd>名词解释</dd><dl>`

- 表单域：

`<form action="url地址(服务器程序的url地址)" method="提交方式(get/post)" name="表单域名称">表单域控件</form>`
用`<input type="sumit" value="提交" />`提交数据，用`<input type="reset" value="重置" />`重置输入；input必须放在form标签内才能提交，所有提交的数据必须有name属性；

- input表单元素：
`<input type="属性值" name="名称;单选按钮与复选框应是相同的name值" value="规定input元素的值" checked="首次加载时选中" maxlength="输入字段的最大长度"/>`

- input 样式修改

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
<label\>for="sex"> 男\</label>
<input type="radio" name="sex" id="sex" />
```

- select:下拉列表
```
<select>
/*至少包含一对option*/
<option>\</option>
<option>\</option>
/*默认选中*/
<option selected="selected"\>xxx\</option>
</select>
```

- textarea:多行文本输入框

```
<textarea rows="3" cols="28">   /*cols:每行字符数； rows：行数*/
内容
</textarea>
```
---
### HTML5特性

#### 1.新增语义化标签

标签语义化指根据网页中的内容结构，选择适合的标签进行编写。

```
<article>
<aside>		元素页面主内容之外的某些内容（比如侧栏）
<details>	定义用户能够查看或隐藏的额外细节。
<figcaption> 	定义 <figure> 元素的标题。
<figure>	规定自包含内容，比如图示、图表、照片、代码清单等。
<footer>
<header>
<main>
<mark>	定义重要的或强调的文本。
<nav>
<section>	定义文档中的节。
<summary>	定义 <details> 元素的可见标题。
<time>
```

#### 2.新增多媒体标签

- \\\<video>

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

- \\\<audio>

`<audio src="" ></audio>`

|||
|-|-|
|autoplay|自动播放，谷歌禁止|
|loop|循环播放|
|controls|播放控件|

#### 3.input类型

|属性值|说明|
|-|-|
|type="email"|限制必须输入email类型|
|type="number"|数字类型|
|type="search"|搜索框|
|type="color"|颜色|
|type="tel"|手机号码|
|type="date"|日期|
|maxlength=""|允许输入最大长度,不会提示|
|max|允许输入最大长度|
|min|允许输入最大长度|

#### 4.表单属性

|属性|说明|
|-|-|
|placeholder|提示文本|
|multiple|可多选文件提交|
|autofocus|自动聚焦|
|required|不能为空，必填|

### 5. canvas

使用js在网页上绘制图像
`<canvas id=''  width='' height=''></canvas>`

### 本地存储localStorage

无时间限制，5M，只存储字符串

### 地理定位Geolocation

### socket通信：webscoket

### 后台js：webworkers

