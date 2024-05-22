---
title: HTML超文本标记语言
date: 2021-03-10 20:22:11
categories:
  - web
---

# HTML 是用来描述网页的一种语言

> Hyper text markup Language(超文本标记语言)
> 超文本：通过超链接将文字＼图片＼音频＼动画等各种信息组织在一起

- w3c(world wide web consortium 万维网联盟)

  - 结构化标准语言(html,xml)
  - 表现标准语言(css)
  - 行为标准（dom,ecmaScript）

- emmet 语法：

```html
div*3快速生成3个
<div></div>
父子级关系：ul>li 兄弟关系：div+p 生成带有类名的：.demo或#two
有顺序的：加上$符号 在生成的标签内写内容：{}
```

- 浏览器内核（渲染引擎）:

> 对代码进行解析渲染

| 浏览器       | 内核    | 说明｜                     |
| ------------ | ------- | -------------------------- |
| IE           | Trident | IE，猎豹，百度，360 浏览器 |
| firefox      | Gecko   | 火狐浏览器内核             |
| safari       | Webkit  | 苹果浏览器内核             |
| chrome/Opera | Blink   | Blink 也是 webkit 的分支   |

## HTML 标签

> HTML 标记标签通常被称为 HTML 标签 (HTML tag)。

- HTML 标签是由尖括号包围的关键词，比如`<html>`
- HTML 标签通常是成对出现的，称为双标签,如`<html>` 和`</html>`.也有单标签：`<br/>`
- 标签对中的第一个标签是开始标签，第二个标签是结束标签
- 开始和结束标签也被称为开放标签和闭合标签
- 标签中的同名属性，后写的会失效

网页结构：

```html
<!--告诉浏览器使用什么规范．使用html5标准进行解析网页-->
<!DOCTYPE html>
<html lang>
  <!--网页头部-->
  <head>
    <!--网页标题-->
    <title>title</title>
  </head>
  <style></style>
  <script></script>
  <body></body>
</html>
```

- base

`<base href="" target="_top">`所有链接标签的默认链接

- meta 视口标签

> 常用于指定网页描述、关键词、作者、事件等
> seo

`<meta name="viewport" content="width=device-width,user-scalable=no,initial-scale=1.0,maximum-scale=1.0">`

| 属性                         | 说明                                                                                                                                                                                                                                         |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name="viewport"              | content="width=(设置的是 viewport 宽度,可设置 device-width 特殊值),initial-scale=(初始缩放比，大于 0 的数字小于 10),maximum-scale=（最大缩放比，大于 0 的数字)，minimum-scale=(最小缩放比，大于 0 的数字)，user-scalable=(用户是否可以缩放)" |
| name="http-equiv"            | 额外添加头部内容                                                                                                                                                                                                                             |
| http-equiv="refresh"         | `content="10;url=http://xxx"` 自动刷新                                                                                                                                                                                                       |
| http-equiv="X-UA-Compatible" | `content="IE=edge"` ie 兼容                                                                                                                                                                                                                  |
| charset='UTF-8'              | 编码                                                                                                                                                                                                                                         |
| name="author"                | content="xxx,xxx,xx"网页作者                                                                                                                                                                                                                 |
| name="copyright"             | content="2002-2022@版权"网页版权                                                                                                                                                                                                             |
| name="keywords"              | content="xxx,xxx,xx"网页关键词                                                                                                                                                                                                               |
| name="description"           | content="xxx,xxx,xx"网页描述                                                                                                                                                                                                                 |
| name="robots"                | content="noindex \| all"爬虫索引的范围                                                                                                                                                                                                       |

### 常用标签

> 块级元素：元素独占一行，标签体内几乎能写任何元素
> 行内标签：由内容撑开,不独占一行, 只能写行内元素,写块级元素
> 行内块元素：

- 标题标签:`<h1>----<h6>`

- 段落标签：`<p>`标签体内不能写块元素,若写，会放到ｐ标签外面或脚手架报错

- 换行标签：`<br />`

- 分割线： `<hr>`
- 保持原有格式显示：`<pre></pre>`

- 字体：`<h1>这是1号标题</h1> <font size="6">这是6号字体文本</font> 可以使用font-family（字体），color（颜色），和font-size（字体大小）属性来定义字体的样式,使用 text-align（文字对齐）属性指定文本的水平与垂直对齐方式`

> 嵌套问题：超链接不能嵌套超链接，p 标签不能嵌套 p 标签，；

- 内部样式表

```html
当单个文件需要特别样式时，就可以使用内部样式表。你可以在<head>
  部分通过
  <style>
    标签定义内部样式表:<head > <style type="text/css" > body {
      background-color: yellow;
    }
    p {
      color: blue;
    }
  </style>
</head>
```

- 外部样式表

```html
当样式需要被应用到很多页面的时候，外部样式表将是理想的选择。
使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观。
<head>
  <!--定义了文档与外部资源的关系-->
  <link rel="stylesheet" type="text/css" href="mystyle.css" />
</head>
```

- 背景色属性:`<p style="background-color:green;">这是一个段落。</p>`

- 加粗：`<strong>或<b>`

- 斜体：`<em>或<i>`

- 下划线：`<ins>或<u>`

- 删除线：`<del>或<s>`

- 盒子: `<div>与<span>`

- 图像标签和路径：`<img src="图像路径"/>`

| 属性         | 说明                                     |
| ------------ | ---------------------------------------- |
| src          | 图片路径                                 |
| alt          | 图片不显示时提醒文字                     |
| title        | 图片文字提醒                             |
| border       | 边框                                     |
| width,height | 宽，高，只写其中一个，会等比例缩放另一个 |

| 图片分类   | 说明                                                                                         |
| ---------- | -------------------------------------------------------------------------------------------- |
| webp       | 支持无损和有损压缩，有损压缩体积比 jpg 小 1/3 ，无损比 png 小 1/4,支持有损半透明，支持动图　 |
| jpg(jpeg)  | 有损压缩，支持的色彩丰富，占用空间小，不支持透明背景和动图　                                 |
| png        | 无损压缩，支持的色彩丰富，占用空间大，支持透明背景(0-256)，不支持动图　                      |
| apng       | 无损压缩，支持的色彩丰富，占用空间大，支持透明背景(0-256)，支持动图　                        |
| bmp        | 不支持压缩，支持的色彩丰富，占用空间极大，支持透明背景，不支持动图　                         |
| gif        | 无损压缩， 仅支持 256 种颜色，支持简单透明背景(0,1bit)和动图　                               |
| tif        | 支持保存图层，　                                                                             |
| avf        | 视频软件有损压缩产物，有损压缩体积比 jpg 小 1/2，打开速度慢,需解码 　                        |
| heif(heic) | 收费，视频软件有损压缩产物，有损压缩体积比 jpg 小 1/2，打开速度慢,需解码 　                  |

- 超链接标签: `<a href="跳转目标(外连接如www.xxx.com，内链接如xxx.html,空链接:#，下载链接)" target="弹出方式(_self在本窗口打开，_blank在新窗口打开,_top跳出弹窗," download="下载文件">文本图像</a>`

- 锚点:

```html
<a href="#one"></a>
<h3 id="one">标题</h3>
<a name="one"></a>

<a href="#">回到顶部</a>
<a href="">刷新页面</a>
<a href="javascript:alert(999);">执行脚本</a>
<a href="tel:10010">打电话</a>
<a href="mailto:xxx@qq.com">邮件</a>
<a href="sms:10086">短信</a>
```

- 字符实体:

|        |            |
| ------ | ---------- |
| 空格   | `&nbsp;`   |
| 小于号 | `&lt;`     |
| 大于号 | `&gt;`     |
| `&`    | `&amp;`    |
| `￥`   | `&yen;`    |
| 版权   | `&copy;`   |
| 乘号   | `&times;`  |
| 除号   | `&divide;` |

- 表格标签：

```html
<!--table标签的width和heigth是最小值-->
<table width="500px" height="500" border="1px" cellspacing="0">
  <!--定义表格标签,属性引号可要可不要-->
  <caption></caption><!--表格标题-->
 <thead height="500" align="center|left|right" valign="top|middle|bottom"><!--表格头部-->
  <tr>
   <th></th><!--表头，td->th加粗文字-->
   <td></td><!--列，必须嵌套在行中-->
  </tr>
 </thead>
 <tbody height="500" align="center|left|right" valign="top|middle|bottom"><!--主体,tbody标签的height与thead、tfoot标签的height相加不能小于table标签的height-->
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

  <tr height="100px"  align="center|left|right" valign="top|middle|bottom"> <!--设置行高，宽无法设置-->
   <td width="100px" align="center|left|right" valign="top|middle|bottom"> <!--设置列宽，一整列都一致变化,哪个大用哪个-->
   </td>
   <td></td>
  </tr>
  <tr>
   <td></td>
  </tr>
 </tbody>

 <tfoot height="500" align="center|left|right" valign="top|middle|bottom"><!--页脚-->
 </tfoot>
</table>


<table>
  <thead>
    <tr>
      <th>name</th>
      <th>sex</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>xiaomi</td>
      <td>man</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
```

表格属性：

| 属性名        | 属性值            | 描述                       |
| ------------- | ----------------- | -------------------------- |
| align         | left,center,right | 表格相对周围元素的对齐方式 |
| border        | 1 或"像素值"      | 边框                       |
| cellpadding   | 像素值            | 单元边沿与内容的的距离     |
| cellspacing   | 像素值            | 单元格间的空白             |
| width，height |                   | 高，宽                     |

- 列表标签

  - 无序标签：

  ```html
  <ul>
    <li>列表</li>
    <li>列表</li>
    <li>列表</li>
  </ul>
  ```

  去掉小圆点：`list-style:none`

  - 有序标签：

  ```html
  <ol>
    <li>列表</li>
    <li>列表</li>
    <li>列表</li>
  </ol>
  ```

  - 自定义列表：

  ```html
  <dl>
    <dt>名词</dt>
    <dd>名词解释</dd>
    <dd>名词解释</dd>

    <dt>分类</dt>
    <dd>分类１</dd>
  </dl>
  ```

- 表单域：

```html
<form
  action="url地址(服务器程序的url地址)"
  method="提交方式(get/post)"
  name="表单域名称"
>
  表单域控件
</form>
```

用`<input type="sumit" value="提交" />`提交数据，用`<input type="reset" value="重置" />`重置输入；input 必须放在 form 标签内才能提交，所有提交的数据必须有 name 属性；

- input 表单元素：
  `<input type="属性值" name="名称;单选按钮与复选框应是相同的name值" value="规定input元素的值" checked="首次加载时选中" maxlength="输入字段的最大长度" hidden readonly disabled placeholder="xx" required pattern="[0-9]"/>`

- input 样式修改

```css
input {
  /*除边框：*/
  border: none;
  ouline: none;

  /*居中：*/
  vertical-align: middle;

  /*隐藏radio的按钮:*/
  display: none;
}
```

| type 属性值 | 描述                                                                                 |
| ----------- | ------------------------------------------------------------------------------------ |
| button      | 按键                                                                                 |
| checkbox    | 定义复选框,name 写成同样的则为一组，值为 value。设置 checked 则默认选中              |
| file        | 供文件上传                                                                           |
| hidden      | 隐藏输入字段                                                                         |
| image       | 图像形式提交按钮                                                                     |
| password    | 密码字段                                                                             |
| radio       | 定义单选按钮,name 写成同样的则为一组，值为 value,不写则为 on,设置 checked 则默认选中 |
| reset       | 重置按钮,value 则为标签体的值,不能写 name                                            |
| submit      | 提交按钮,value 则为标签体的值,不能写 name                                            |
| text        | 输入字段，value 默认值，maxlength 最大长度                                           |

- label 标签：用于绑定表单域，点击自动调整到输入框

```html
<label>for="sex"> 男\</label> <input type="radio" name="sex" id="sex" />
```

- select:下拉列表

```html
<select>
  /*至少包含一对option*/
  <option>\</option>
  <option>\</option>
  /*默认选中*/
  <option selected="selected" \>xxx\</option>
</select>
```

- textarea:多行文本输入框

```html
<textarea rows="3" cols="28">
   /*cols:每行字符数； rows：行数*/
内容
</textarea>
```

- iframe

```html
<iframe src="xxx" name="mainFrame" width="100px" height="100px" frameborder="0">
  <!-- name属性可以和超链接或表单的target配合用来显示不同内容 -->
  <a target="mainFrame">xxx</a>
  <form target="mainFrame">
    <input type="reset" value="重置按钮" />
    <button>默认表达提交按钮</button>
    <button type="button">普通按钮</button>
  </form></iframe
>
```

---

### HTML5 特性

#### 1.新增语义化标签

标签语义化指根据网页中的内容结构，选择适合的标签进行编写。

- 布局标签

```html
<header></header>
<nav></nav>
<aside>元素页面主内容之外的某些内容（比如侧栏）</aside>
<main></main>
<article>
  <section>定义文档中的节。</section>
</article>
<footer></footer>
```

```html
<details>
  <summary>定义 details 元素的可见标题。</summary>
  定义用户能够查看或隐藏的额外细节。
</details>

<figcaption>定义 figure 元素的标题。</figcaption>
<figure>
  规定自包含内容，比如图示、图表、照片、代码清单等。
  <figure>
    <sub></sub>下标文字 <sup></sup>上标文字 <code></code>代码块
    <mark> 定义重要的或强调的文本。</mark>
    <time></time>
    <!--//文本注音-->
    <ruby>
      <span>汉</span>
      <rt>han</rt>
    </ruby>

    <!--进度条-->
    <progress max="100" value="10"></progress>
    <meter min="0" max="100" value="30">
      <meter>
        <!--列表-->
        <datalist>
          <option value="xx">test</option>
        </datalist>
      </meter>
    </meter>
  </figure>
</figure>
```

#### 2.新增多媒体标签

- `<video>`

`<video src="" controls="controls" ></video>`

属性：

| 属性         | 说明                                                       |
| ------------ | ---------------------------------------------------------- |
| autoplay     | 自动播放，谷歌默认禁止，可添加 muted 属性解决              |
| controls     | 播放控件                                                   |
| width;height | 宽高                                                       |
| loop         | 循环播放                                                   |
| preload      | auto(加载视频);none(不加载);metadata(加载基本信息，如时长) |
| poster       | imgurl(加载等待图片)                                       |
| muted        | 静音播放                                                   |

- `<audio>`

`<audio src="" ></audio>`

| 属性     | 说明                                                       |
| -------- | ---------------------------------------------------------- |
| src      | 音频路径                                                   |
| muted    | 静音                                                       |
| autoplay | 自动播放，谷歌根据媒体参与度来判断是否自动自动播放         |
| loop     | 循环播放                                                   |
| preload  | auto(加载视频);none(不加载);metadata(加载基本信息，如时长) |
| controls | 播放控件                                                   |

#### 3.input 类型

| 属性值                | 说明                      |
| --------------------- | ------------------------- |
| type="email"          | 限制必须输入 email 类型   |
| type="number"         | 数字类型                  |
| type="search"         | 搜索框                    |
| type="ranger"         | 范围滑块                  |
| type="color"          | 颜色                      |
| type="tel"            | 手机号码                  |
| type="date"           | 日期                      |
| type="month"          | 月                        |
| type="week"           | 周                        |
| type="time"           | 时间                      |
| type="datetime-local" | 日期时间                  |
| maxlength=""          | 允许输入最大长度,不会提示 |
| max                   | 允许输入最大长度          |
| min                   | 允许输入最大长度          |

#### 4.表单属性

| 属性         | 说明                                            |
| ------------ | ----------------------------------------------- |
| placeholder  | 提示文本                                        |
| multiple     | 可多选文件提交                                  |
| autofocus    | 自动聚焦                                        |
| autocomplete | 值为 on,off。只能用于 text 输入框. 记录输入历史 |
| required     | 不能为空，必填                                  |
| pattern      | 正则校验，不能用于 textarea,空输入不校验        |

### 5. canvas

使用 js 在网页上绘制图像
`<canvas id=''  width='' height=''></canvas>`

### 本地存储 localStorage

无时间限制，5M，只存储字符串

### 地理定位 Geolocation

### socket 通信：webscoket

### 后台 js：webworkers
