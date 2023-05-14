---
title: 前端规范
date: 2020-11-11 20:33:33
categories:
  - web
---


## 一、命名规范

### 1.项目命名

全部采用`kebab-case`命名，字母小写，以短横分隔单词。

正例： `my-project-name`

反例：`my_project_name / myProjectName`

### 2.目录命名

- 全部采用`kebab-case`命名，字母小写，以短横分隔单词。
- 有复数结构时，要采用复数命名法，缩写不用复数。

正例：`scripts/styles/components/images/assets/views/utils/custom-themes/layout/img/doc/api/router`

反例：`script / style /util / imgs / docs / apis / routers`

### 3.文件命名

全部采用`kebab-case`命名，字母小写，以短横分隔单词。

正例：`render-dom.js / home.css / home-banner.png`
反例：`renderDom.js / Home.css / homeBanner.png`

### 4.命名严谨性

- 命名严禁拼音和英文混合方式，更不允许直接使用中文。
- 地名或者国际通用名称，可以视同英文。
- 使用有意义且规范的缩写，避免望文不知义。

正例：

`rmb / shenzhen / guangdong / henan / wuhan
logout / login / err / res`
反例：

`daZhePromotion 打折 / getPingFen 获得评分 / let 年龄 = 3
AbstractClass 缩写为 AbsClass / CalculateAmount 缩写为 CaAmount`

## 二、HTML规范

### 1.声明规范

#### 1.1声明文档类型

文档类型声明位于`HTML`文档的第一行，大小写必须遵守。

正例：`<!DOCTYPE html>`

反例：`<!doctype html>`

#### 1.2声明charset

必须在文档中声明编码集`charset` ，且文件本身编码需要保持一致，推荐都使用`utf-8`。制定字符编码的`meta`必须是`head`中的第一个元素。

```html
<head>
  <meta charset="utf-8" />
</head>
```

#### 1.3声明meta

在`head`中声明，跟随在`charset`之后，且自闭合标签。

```html
<head>
	<meta charset="utf-8" />
	<meta name="keywords" content="your keywords" />
	<meta name="description" content="this is a description" />
</head>
```

#### 1.4声明title

必须声明`title`，且紧随最后一个`meta`之后。

```html
<head>
  <meta charset="utf-8" />
  <meta name="keywords" content="your keywords" />
  <meta name="description" content="this is a description" />
  <title>页面标题</title>
</head>
```

#### 1.5声明link

引入了外部样式，紧随`title`之后，同时网站的标签也需要设置`icon`图标。

```html
<head>
  <meta charset="utf-8" />
  <meta name="keywords" content="your keywords" />
  <meta name="description" content="this is a description" />
  <title>页面标题</title>
  <link rel="icon" href="http://xxx.com/favicon.ico" />
  <link href="http://xxx.com/index.css" rel="stylesheet" />
</head>
```

#### 1.6声明script

由于 `js` 加载会阻塞页面渲染，`script` 元素需要声明在 `body` 标签的尾部，并且是需要闭合标签的。

```html
<body>
  <script src="http://xxx.com/index.js"></script>
</body>
```

#### 1.7模板示例

以下是一个整合后 `html` 代码规范示例：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="keywords" content="your keywords" />
    <meta name="description" content="this is a description" />
    <title>页面标题</title>
    <link href="http://xxx.com/index.css" rel="stylesheet" />
  </head>
  <body>
    <h1>这是一个标题</h1>
    <!-- 加载脚本 -->
    <script src="http://xxx.com/index.js"></script>
  </body>
</html>
```

### 2.代码规范

#### 2.1代码缩进

缩进使用soft tab（4个空格）

```html
<body>
    <div>代码为4个空格缩进</div>
</body>
```

#### 2.2标签命名

标签元素名统一小写，不可大小写混合，必须有结尾标签相呼应。

正例：`<div></div>`
反例：`<DIV></DIV>`

#### 2.3属性命名

标签属性和自定义属性采用`kebab-case`命名，字母小写，以短横分隔单词，并且必须是双引号。

正例：

```html
<div class="text-error"></div>
<div data-id="1" data-user-name="xiaoming"></div>
```

反例：

```html
<div class="text-error"></div>
<div dataId="1" dataUserName="xiaoming"></div>
```

#### 2.4属性顺序

标签的属性需要按照顺序来书写，以下规则是极力推荐使用的。

样式名
`class`

全局唯一标识
`id`
`name`

自定义属性（可复用的元素样式排首位）
`data-xx`

资源地址/类型（可复用的元素样式排首位）
`href`
`src`
`for`
`type`

提示性的内容
`placeholder`
`title`
`alt`

辅助性的内容
`disabled`
`readonly`
`required`
`checked`
`selected`

#### 2.5语义化标签

`html` 中包含了大量的语义化标签，推荐开发中合理的使用语义化的标签，便于页面结构化，易于阅读，更好的便于网页内容抓取。

```html
<body>
  <header>
    <!-- 头部内容 -->
  </header>
  <main>
    <!-- 主要内容 -->
  </main>
  <footer>
    <!-- 尾部内容 -->
  </footer>
</body>
```

以下列举出常见的语义化标签集合：

| 标签    | 描述       |
| ------- | ---------- |
| header  | 头部内容   |
| nav     | 导航       |
| main    | 主要内容   |
| footer  | 底部内容   |
| aside   | 侧边栏     |
| section | 文档中的节 |
| article | 文章       |
| time    | 时间       |
| mark    | 文本标记   |

#### 2.6结构顺序

除了使用语义化标签，一个网页还需要遵守一定的结构和顺序，自上而下，从左到右，保持一定的顺序。

```html
<div class="app-container">
  <!-- header -->
  <div class="header-container"></div>

  <!-- main -->
  <div class="main-container">
    <div class="left-box"></div>
    <div class="content-box"></div>
  </div>

  <!-- footer -->
  <div class="footer-container"></div>
</div>
```

#### 2.7图片alt不为空

`<img>`标签的 `alt `属性指定了替代文本，在图像无法显示时候替代的文案显示。

```html
<img src="http://www.xxx.com/dd.jpg" alt="加载中" />
```

### 3.注释规范

#### 3.1 单行注释

```html
<body>
  <!-- 单行注释 -->
  <div>内容</div>
</body>
```

#### 3.2 多行注释

```html
<body>
  <!-- 
    多行注释
    多行注释
  -->
  <div>内容</div>
</body>
```

#### 3.3 闭合注释

有时我们需要用注释来标记某段代码的开头和结束，就需要用到闭合注释（本规范中叫做闭合注释）。

```html
<body>
  <!-- 注释开头 -->
  <div>内容</div>
  <!-- /注释结束 -->
</body>
```

## 三、CSS规范

### 1.声明规范

#### 1.1 link 标签 引入 css

使用 link 标签引入外部css，写在 head 中。

```html
<head>
  <link href="//xxx.com/index.css" rel="stylesheet" />
</head>
```

#### 1.2 @import 导入 css 文件

导入 css 文件使用 @import，忽略使用 url 导入。

```
// 推荐
@import "./styles/index.css";

// 不推荐
@import url("./styles/index.css");
```

### 2.代码规范

#### 2.1 代码缩进

统一使用 4 空格缩进。

```css
.page-title {
  font-size: 18px;
}
```

#### 2.2 属性简写

样式中有很多属性是可以简写的，推荐这样的方式，易读且减少了复杂度。

正例：

```css
.page-title {
  padding: 10px 30px 10px 30px;
  border: 1px solid red;
}
```


反例：

```css
.page-title {
  padding-top: 10px;
  padding-right: 30px;
  padding-bottom: 10px;
  padding-left: 30px;
  border-width: 1px;
  border-style: solid;
  border-color: red;
}
```

#### 2.3 省略属性值 0 的单位

正例：

```css
.page-title {
  padding: 0;
  margin: 0 10px;
}
```


反例：

```css
.page-title {
  padding: 0px 0px 0px 0px;
  margin: 0px 10px 0px 10px;
}
```

#### 2.4 使用直接子选择器

在嵌套写选择器链的时候，应该减少后代选择器，改用直接子选择器，也就是多写个 >符号定位到元素，优点是 css 渲染效率更高。

正例：

```css
.tabs > .tabs-title {
  font-size: 18px;
}
```


反例：

```css
.tabs .tabs-title {
  font-size: 18px;
}
```

#### 2.5 避免使用 id 和标签选择器

应该避免使用这两个选择器，以防止污染全局样式。

正例：

```
<head>
  <style>
    .page-title {
      font-size: 18px;
    }
    .page-title .user-name {
      color: red;
    }
    .my-demo {
      font-weight: bold;
    }
  </style>
</head>
<body>
 <div class="page-title">
    <div class="user-name"></div>
  </div>
  <p class="my-demo"></p>
</body>
```

反例：

```css+html
<head>
	<style>
		#page-title {
          font-size: 18px;
        }
        p {
          font-weight: bold;
        }
	</style>
</head>
<body>
  <div id="page-title"></div>
  <p></p>
</body>
```

#### 2.6 避免使用行内样式

避免直接在标签写 style 行内样式，这样样式会很分散不易维护，最好是把样式提取到一个 class 中。

正例：

```css+html
<style>
.text-red {
    font-size: 18px;
    color: red;
  }
</style>
<body>
 <div class="text-red"></div>
</body>
```

反例：

```html
<body>
  <div style="font-size:18px; color:red;"></div>
</body>
```

#### 2.7 避免使用 !important 优先级

非必须不要使用 !important 强制设置优先级最高，滥用出现问题会导致代码难以维护。

#### 2.8 避免样式过多层级嵌套

在使用 less 或者 scss 预编译语言时候，应该避免层级过多嵌套。过多的层级会显得很乱，难以阅读和维护，建议最多层级在 3 层最佳，同样层次多也可以拆分开。
正例：

```css
/* 减少层级 */
.main {
  .page-title {
    font-size: 18px;
    font-weight: bold;
  }
}
/* 拆分层级也可以 */
.main {
  .page {
    height: 100px;
  }
}
.page {
  .title {
    font-size: 18px;
    font-weight: bold;
  }
}
```

反例：

```css
.main {
  .page {
    height: 100px;
    .title {
      font-size: 18px;
      font-weight: bold;
    }
  }
}
```

#### 2.9 禁止使用“*”选择元素

不能使用*选择器来给元素设置样式，这样会使所有的元素都起作用。

反例：

```css
/* 不能这样写 */
{
	padding: 0;
	margin: 0;
}
```

#### 2.10 禁止将样式写为单行

不要把样式都放同一行，这样是不允许的。

反例：

```css
/* 不能这样写 */
.text-red { font-size:18px;color:red; }
```

非必须避免使用 !important ，因为会强制设置为优先级最高，滥用出现问题导致代码难以维护。

#### 2.11 css 的颜色值规则

- 颜色值默认使用十六进制表示，字母全部小写。
- 有透明度的要用 rgba 颜色值，最后一个透明度值需要写完整的小数。
- 颜色值全部一样，则缩减为 3 位数值。

正例：

```css
.demo {
  color: #abcdef;
  color: #333;
  color: #ccc;
  color: rgba(255, 255, 255, 0.8);
}
```

反例：

```css
.demo {
  color: #abcdef;
  color: #333333;
  color: #cccccc;
}
```

#### 2.12 样式属性书写顺序

按照一定的顺序书写样式，可以提升浏览器渲染 dom 的性能，本规范中该项非必须。

```css
.hotel-content {
  /* 1. 定位 */
  display: block;
  position: absolute;
  left: 0;
  top: 0;
  /* 2. 盒子模型 */
  width: 50px;
  height: 50px;
  margin: 10px;
  border: 1px solid black;
  /* 3. 文本 */
  font-size: 18px;
  color: red;
  text-align: center;
  /* 4. 背景 */
  background: #ccc;
  /* 5. 其他 */
  transition: all 0.2s;
}
```

#### 2.13 样式命名规则

第一种：

推荐采用 BEM 命名规范，BEM 命名就是通过 块(block)、元素(element)、修饰符(modifier)来连接组合元素之间的关系，例如： type-block__element--modifier。
该命名虽然能比较透明的展示出结构，但是也会有缺点，命名过于严格，类名太长，不易维护。

- 单中划线：连接某个块与下面的子模块。
- __ 双下划线：连接块和块的子元素。
- – 双中划线：描述该块或者子元素的一种状态。

```css+html
<head>
  <style>
    /* 原生CSS写法 */
    .tabs {
    }
    .tabs .tabs-inner .tabs-inner__button {
    }
    .tabs .tabs-inner .tabs-inner__button .tabs-inner__button--primary {
    }
    /* Less或者Scss写法 */
    .tabs {
      &-inner {
        &__button {
          &--primary {
          }
        }
      }
    }
  </style>
</head>
<body>
  <div class="tabs">
    <div class="tabs-inner">
      <button class="tabs-inner__button tabs-inner__button--primary">
        选项1
      </button>
      <button class="tabs-inner__button">选项2</button>
    </div>
  </div>
</body>
```

第二种（推荐）：

- 使用命名空间的方式来组合元素层级之间的关系，使用语义化的单词来当做命名空间区分不同模块。
- 该命名是把元素都放到某个块下面单独定义样式，这样不会影响到其它块下面的样式。如下示例：

```css+html
<head>
  <style>
    /* 原生CSS写法 */
    .message .title {
    }
    .message .content {
    }
    /* Less或者Scss写法 */
    .message {
      .title {
      }
      .content {
      }
    }
  </style>
</head>
<body>
  <!-- message -->
  <div class="message">
    <div class="title"></div>
    <div class="content"></div>
  </div>
</body>
```

### 3. 注释规范

#### 3.1 单行注释

单行注释以 /* 开头，以 */ 结束，注意前后各空一格。
在 less 或者 scss 中也可以使用双斜杠 // 作为单行注释*

```css
/* 单行注释 */
.tabs {
}
```

#### 3.2 多行注释

多行需要换行，每行前面没有 * 号，每行对齐前面空一格。

```css
/*
  多行注释1
  多行注释2
 */
 .tabs {
}
```

#### 3.3 块注释

- 块注释以 /* 开头，以 */ 结束，前后各空一格。

- 块注释用于划分某个模块的标记，需要写上块描述。

```
/* 用户模块样式 */
.user {
}
.user .user-name {
}
/* 弹框模块样式 */
.modal {
}
.modal .modal-title {
}
```

#### 3.4 特殊注释

用于标注修改、待办等信息，需要写上作者和时间等信息。

```
/* TODO: 标签页样式待补充 by 小红 2022-03-13 18:32 */
/* BUGFIX: 修复标签页的bug by 小红 2022-03-13 18:32 */
.tabs {
}
```

4.5 文件注释
文件注释以 /** 开头，以 */ 结束。

```
/**
 * css文件描述
 * @author: 小红
 * @update: 2021-04-13 18:32
   */

/* 标签页 */
.tabs {
}
```

### 4.样式名参考

在编写页面时候，class 样式起名是件很痛苦的事，多写几层就没词可用了，以下列举出不同场景常用的名称，帮你找到合适你页面的样式名称。

使用 `kebab-case `命名，字母一律小写，用短横分隔单词。
如果单词过长，需要使用言简意赅的缩写单词表示。

#### 4.1 样式文件命名

```
index.css    // 一般用于首页建立样式
head.css     // 头部样式，当多个页面头部设计风格相同时使用。
base.css     // 共用样式。
style.css    // 独立页面所使用的样式文件。
global.css   // 页面样式基础，全局公用样式，页面中必须包含。
layout.css   // 布局、版面样式，公用类型较多时使用，一般用在首页级页面和产品类页面中
module.css   // 模块，用于产品类页，也可与其它样式配合使用。
master.css   // 主要的样式表
columns.css  // 专栏样式
themes.css   // 主体样式
forms.css    // 表单样式
mend.css     // 补丁，基于以上样式进行的私有化修补。
print.css    // 打印
```

#### 4.2 页面结构命名

```
page         // 代表整个页面，用于最外层。
wrap         // 外套，将所有元素包在一起的一个外围包，用于最外层
wrapper      // 页面外围控制整体布局宽度，用于最外层
container    // 一个整体容器，用于最外层
head|header  // 页头区域，用于头部
nav          // 导航条
content      // 内容，网站中最重要的内容区域，用于网页中部主体
main         // 网站中的主要区域（表示最重要的一块位置），用于中部主体内容
column       // 栏目
sidebar      // 侧栏
foot|footer  // 页尾、页脚。网站一些附加信息放置区域，（或命名为 copyright）用于底部
left|right|center  // 左右中
```

#### 4.3 导航命名

```
nav|navbar|navigation|nav-wrapper  // 导航条或导航包，代表横向导航
topnav     // 顶部导航
mainbav    // 主导航
subnav     // 子导航
sidebar    // 边导航
left-sidebar|sidebar-l   // 左导航
right-sidebar|sidebar-r  // 右导航
title      // 标题
summary    // 摘要
menu       // 菜单，区域包含一般的链接和菜单
submenu    // 子菜单
drop       // 下拉
dorp-menu  // 下拉菜单
links      // 链接菜单
```

#### 4.4 功能命名

```
logo      // 标记网站logo标志
banner    // 标语、广告条、顶部广告条
login     // 登陆，（例如: 登录表单 form-login）
loginbar  // 登录条
register  // 注册
tool|toolbar    // 工具条
search          // 搜索
searchbar       // 搜索条
searchlnput     // 搜索输入框
shop            // 功能区，表示现在的
icon            // 小图标
label           // 商标
homepage        // 首页
subpage         // 二级页面子页面
hot             // 热门热点
list            // 文章列表，（例如: 新闻列表 list-news）
scroll          // 滚动
tab             // 标签
sitemap         // 网站地图
msg|message     // 提示信息
current         // 当前的
joinus          // 加入
status          // 状态
btn             // 按钮，（例如: 搜索按钮可写成  btn-search）
tips            // 小技巧
note            // 注释
guild           // 指南
arr|arrow       // 标记箭头
service         // 服务
breadcrumb      // (即页面所处位置导航提示）
download        // 下载
vote            // 投票
news            // 新闻
siteinfo        // 网站信息
partner         // 合作伙伴
link|friendlink // 友情链接
copyright       // 版权信息
siteinfoCredits // 信誉
siteinfoLegal   // 法律信息
```



## 四、JavaScript 规范

### 1. 命名规范

#### 1.1 引入资源

- 文件名不得包含空格。
- 采用`kebab-case` 命名，字母小写，以短横分隔单词。
- 引入资源推荐使用相对路径，不指定具体的协议(`http:`，`https:`)。

```js
<script src="//cdn.com/render-dom.min.js"></script>
```

#### 1.2文件命名

全部采用 `kebab-case` 命名，字母小写，以短横分隔单词。

正例：`render-dom.js / home.js / get-user-info.js`
反例：`renderDom.js / Home.js / getUserInfo.js`

#### 1.3声明变量

- 变量名使用 `camelCase` 驼峰命名。
- 不能以数字，特殊符号开头。
- 不能使用 JavaScript 关键字和保留字。
- 避免使用没有意义的命名。
- 变量名起名以 `类型+对象描述` 的规则。

```
let userId = 1;
var userName = "小红";
let tableTitle = "表格标题";
```

#### 1.4声明常量

常量名使用全部大写命名，多个单词中间用`_`下划线分隔。

正例：

```js
const AMOUNT = 10;
const MAX_COUNT = 10;
const APPROVE_TYPE = { DOING: 1, APPROVED: 2 };
```

反例：

```js
const amount = 10;
const MAXCOUNT = 10;
const APPROVETYPE = { DOING: 1, APPROVED: 2 };
```

#### 1.5 形参命名

形参本身是可以任意定义的，统一是为了更方便地复用/合并代码。

| 参数       | 描述        | 示例                          |
| ---------- | ----------- | ----------------------------- |
| i          | 索引        | switchTab(i){}                |
| i,v        | 值          | list.forEach((i,v) =>{})      |
| e          | 事件        | onInput(e){}                  |
| obj        | 对象        | for(let obj of userList){}    |
| arr        | 数组        | let arr = []                  |
| fun        | 函数        | callback(fun){}               |
| item       | 列表项      | list.map(item => {})          |
| item,index | vue遍历数组 | v-for="(item,index) in arr"   |
| key,value  | 键值对      | for(let [key,value] of arr){} |

#### 1.6 函数命名

函数名使用 camelCase 驼峰命名，命名为动词形式，或者动词+名词形式。

```
function getName() {}
function setName() {}
function canEdit() {}
function addUser() {}
function hasCode() {}
```

如果是内部使用的私有函数，需要用 `_`符号作为前缀，写到文件最下面，并用按照功能进行分组，写上块注释。

```
/* 格式化内容 */
function _formatText() {}
function _formatMoney() {}

/* 设置日期 */
function _setDate() {}
function _setMonth() {}
```

增删改查和查看详情，统一使用下面的单词。

`add /delete / update / get / detail`

附常用的动词列表：

```
get 获取 | set 设置
add 增加 | delete 删除
start 启动 | launch 启动 | run 运行 | ready 准备 | stop 停止
open 打开 | close 关闭
read 读取 | write 写入 | save 保存
load 载入 | unload 不加载
create 创建 | destroy 销毁
begin 开始 | start 开始 | end 结束
from 从.. | to 到..
backup 备份 | restore 恢复
import 导入 | export 导出
print 打印 | afterprint 打印机关闭
split 分割 | merge 合并
touch 轻按 | click 点击  | dblclick 双击 | drag 拖拽
inject 注入 | extract 提取
attach 附着 | detach 脱离
bind 绑定 | separate 分离
view 查看 | browse 浏览
edit 编辑 | modify 修改 | update 更新 | upgrade 升级
select 选取 | change 改变 | mark 标记
copy 复制 | paste 粘贴
do 做 | undo 撤销 | redo 重做
insert 插入 | remove 移除 | revoke 撤销 |  revert 复原 | reset 重置
clean 清理 | clear 清除
index 索引 | sort 排序
find 查找 | search 搜索
increase 增加 | decrease 减少
rise 上涨 | fall 下降
play 播放 | pause 暂停
forward 前进 | rewind 后退
compile 编译 | execute 执行
suspend 挂起 | complete 完成
debug 调试 | trace 跟踪
observe 观察 | listen 监听
build 构建 | publish 发布
input 输入 | output 输出
encode 编码 | decode 解码
encrypt 加密 | decrypt 解密
compress 压缩 | decompress 解压缩
pack 打包 | unpack 解包
parse 解析 | emit 生成
connect 连接 | disconnect 断开
send 发送 | receive 接收
upload 上传 | download 下载
refresh 刷新 | synchronize 同步
lock 锁定 | unlock 解锁
check out 签出 | check in 签入
submit 提交 | commit 交付
push 推 | pull 拉
expand 展开 | collapse 折叠
active 激活 | deactive 未激活
wait 等待 | process 进程中 | finish 完成
enter 进入 | exit 退出
abort 中止 | quit 离开
obsolete 废弃 | depreciate 废旧
collect 收集 | aggregate 聚集
```

### 2.代码规范

#### 2.1代码缩进

统一使用 `4` 个缩进


#### 2.2 代码行最大字符数

统一每行最大 `80` 个字符，超出自动换行，`VsCode编辑器`默认是 `80` 个字符。

#### 2.3 使用 let 代替 var

声明变量推荐使用 `let` 代替 `var` 声明。
`var` 会有变量提升问题，容易造成变量污染。

```
let userId = 2;
let userName = "小红";	
```

#### 2.4 多个变量声明简写

同时声明多个变量，不需要每个都用 `let` 起一行声明，可以合并到一起。

正例：

```
let id = 2,
  name = "小红",
  sex = "女",
  age = 18;
```

反例：

```
let id = 2;
let name = "小红";
let sex = "女";
let age = 18;
```

#### 2.5 使用模板字符串代替 “+” 拼接

在有变量拼接的场景下，尽量使用模板字符串来代替普通的“+”拼接，这样会使得代码结构更清晰。

正例：

```
let id = 2,
  name = "小红",
  sex = "女",
  age = 18;
let str = `我叫${name}，性别${sex}，我今年${age}岁了。`;
```


反例：

```
let id = 2,
  name = "小红",
  sex = "女",
  age = 18;
let str = "我叫" + name + "，性别" + sex + "，我今年" + age + "岁了。";
```

#### 2.6 this 自身引用的命名

对上下文this的引用只能使用'_this', 'that', 'self'其中一个来命名；

```
let self = this;
setTimeout(function () {
  self.say();
}, 1000);
```

#### 2.7 避免 console.log 滥用

避免大量使用 console.log，这样很影响代码执行效率。打印日志只作用看调试内容输出，不能代码里随意到处都写，在代码调试完成之后必须要删除。

#### 2.8 三元条件简单判断

在简单的判断条件下，使用三元运算符能更加的精简代码，增加可读性。

正例：

```
let flag = 1;
console.log(flag == 1 ? "是" : "否");
// 甚至再多一层判断也可以
let status = 0;
console.log(status == 0 ? "代办" : status == 1 ? "已办" : "无");
```

反例：

```
// 这里 6 行代码，用三元运算符 2 行搞定
let flag = 1;
if (flag == 1) {
  console.log("是");
} else {
  console.log("否");
}
```

#### 2.9 使用字面量对象

使用字面量创建对象，更加简洁。
正例：

```
let user = {
  id: 1,
  name: "小红",
  sex: "女",
  age: 18,
};
```


反例：

```
let user = new Object();
user.id = 1;
user.name = "小红";
user.sex = "女";
user.age = 18;
```

#### 2.10 对象结构简写

变量名是对象的属性值，推荐使用简写方式，并且按照归类分组将简写属性统一放到开头，对象的结构更加的清晰明了。

```
let name = "小红",
  age = 18;

let user = {
  name,
  age,
  sex: "女",
  mobile: "1300000000",
};
```

#### 2.11 使用 “…” 扩展运算符

浅拷贝对象

```
let age = 18;
let user = {
  id: 1,
  name: "小红",
};
let copy = { ...user, age };
```


浅拷贝数组

```
let arr1 = [1, 2];
let arr2 = [3, 4];
let arr3 = [...arr1, ...arr2];
```

#### 2.12 使用可选链 “?.” 操作符

```
let user = null;
// 如果直接取 user.sex 是会报错的，用了可选链后可以不用判断直接取了
console.log(user?.sex);
```

#### 2.13 函数参数设置默认值

```
function getTimestamp(date = new Date()) {
  return date.getTime();
}
console.log(getTimestamp());
```

#### 2.14 函数参数太多，封装为对象传参

正例：

```
let user = {
  id: 1,
  name: "小红",
  sex: "女",
  age: 18,
};
function addUser(user) {}
```


反例：

```
function addUser(id, name, sex, age) {}
```

#### 2.15 函数内避免过多处理逻辑

在一个函数中，如果处理逻辑很多，需要拆分多个方法来处理。

正例：

```
function main() {
  // 逻辑拆分为不同的方法来处理
  foo1();
  foo2();
  foo3();
}
// 逻辑1
function foo1() {}
// 逻辑2
function foo2() {}
// 逻辑3
function foo3() {}
```


反例：

```
function main() {
  // 逻辑1
  // 逻辑2
  // 逻辑3
}
```

#### 2.16 简单判断并行函数执行

如果条件比较少，条件通过则执行函数，这种可以在单行用 && 符号判断并执行代码。

正例：

```
let div = document.getElementById("div");
div && div.append("child");
```


反例：

```
let div = document.getElementById("div");
if (div) {
  div.append("child");
}
```

#### 2.17 使用箭头函数

推荐使用箭头函数，箭头函数能避免 this 作用域不确定的问题，而且使代码更加简洁。

```
const getUserInfo = () => {};
```

#### 2.18 提前 return，使判断逻辑简单化

当有比较多判断的情况下，把判断前置，使我们的代码结构更加清晰分明。

正例：

```
function submit() {
  // 校验
  if (!this.listQuery.name) return;
  if (!this.listQuery.status) return;
  if (!this.listQuery.type) return;
  // 校验通过，正常执行代码
  addUser();
}
```

反例：

```
function submit() {
  // 校验 name
  if (this.listQuery.name) {
    // 校验 status
    if (this.listQuery.status) {
      // 校验 type
      if (this.listQuery.type) {
        // 校验通过，正常执行代码
        addUser();
      }
    }
  }
}
```

#### 2.19 使用 Promise，async/await 代替回调函数

```
// 使用Promise
function getMsg(res) {
  return new Promise((resolve, reject) => {
    if (res.code == 200) {
      resolve(res);
    } else {
      reject(res);
    }
  });
}
getMsg({ code: 200, result: [] }).then((res) => {});

// 使用ssync/await
async function login() {
  // 先执行获取用户信息
  let user = await getUserInfo();
  // 再执行获取菜单，传入用户的id
  let menu = await getMenuList(user.id);
  console.log(user, menu);
}
```
#### 2.20  undefined 判断
永远不要直接使用 undefined 进行变量判断；使用 typeof 和字符串’undefined’对变量进行判断。
正例：
```
if (typeof person === 'undefined') {
... }
```

反例：
```
if (person === undefined) {
... }
```
#### 2.22 null
适用场景：

初始化一个将来可能被赋值为对象的变量
与已经初始化的变量做比较
作为一个参数为对象的函数的调用传参
作为一个返回对象的函数的返回值
不适用场景：

不要用null来判断函数调用时有无传参
不要与未初始化的变量做比较

#### 2.21 eslint 和 Prettier 代码格式化

使用 `eslint` 做代码检查，格式化统一用`Prettier Code` 插件。

### 3. 注释规范

#### 3.1 单行注释

- 单行注释以 `//` 开头，可以在代码末尾，也可以单独一行。
- 单行注释和后面内容空一格。

```
function setId(id) {
  // 可以在代码上面注释
  this.id = id;
}

let name = "小红"; // 如果是声明变量的注释，推荐写在行尾
let birthday = "2000-10-02"; // 生日
```

#### 3.2 多行注释

- 多行注释以 `/*` 开头，以 `*/` 结束。
- `/*` 开头与后面内容空一格。
- 如果注释内容较多，需要换行注释。
- 若以 `/* xx */` 注释内容都在同一行，推荐使用 `//` 单行注释。

```
/*
  注释内容1
  注释内容2
 */
setTitle();
```

#### 3.3 块注释

- 块注释以 `/*` 开头，以 `*/` 结束，前后各空一格。
- 块注释用于划分某个块的标记，需要写上块描述。

```
/* 获取列表 */
function getList() {}

/* 新增和修改用户 */
function addUser() {}
function updateUser() {}
```

#### 3.4方法注释

方法注释以 `/**` 开头，以 `*/` 结束，方法注释要使用`注释标签`说明方法的参数，返回结果等等内容。

```
/**
 * 获取用户信息
 * @param {Number} id 用户id
 * @returns {Object} 返回用户对象
 */
function getUserInfo(id) {}
```

方法的`注释标签`必须要写，参考[JSDoc 文档](https://jsdoc.zcopy.site/)，以下是常用的标签说明。

| 标签         | 语法                           |
| ------------ | ------------------------------ |
| @description | @description描述说明           |
| @param       | @param{参数类型}参数说明       |
| @returns     | @returns{返回类型}返回结果说明 |
| @author      | @author作者                    |
| @version     | @version 版本号                |
| @example     | @example 示例代码              |
| @todo        | @todo 待办的内容描述           |

#### 3.5文件注释

```
/**
 * 文件说明
 * @author 作者名
 * @date 2022-03-23
 */
```

#### 3.6版权注释

```
/*!
 * @lime-util/core v3.0.10
 * Copyright 2021-2022, GaoXXXXX <575122332@qq.com>
 * Licensed under the MIT License.
 */
```

## 五、Vue项目规范

### 1.项目目录

项目基于 `vue-cli4` 开发，项目目录结构如下：

```
├── .vscode                    # vscode 的项目配置
├── build                      # 构建相关
├── dist                       # 打包文件
├── mock                       # 项目mock 模拟数据
├── public                     # 静态资源
│   │── favicon.ico            # favicon 图标
│   └── index.html             # html模板
├── src                        # 源代码目录
│   ├── api                    # 所有 api接口
│   ├── assets                 # 图片，字体静态资源
│   ├── components             # 全局公用组件
│   ├── constants              # 全局 常量
│   ├── directive              # 全局指令
│   ├── filters                # 全局过滤器
│   ├── mixins                 # 全局混入
│   ├── plugins                # 全局插件
│   ├── icons                  # 全局 svg icons
│   ├── lang                   # 国际化 language
│   ├── layout                 # 全局 layout
│   ├── router                 # 路由
│   ├── store                  # 全局 store管理
│   ├── styles                 # 全局样式
│       │── themes             # 主题
│       └── index.less         # 样式主入口
│   ├── utils                  # 全局公用方法
│   ├── vendor                 # 公用 vendor
│   ├── views                  # views 所有页面
│   ├── App.vue                # 入口页面
│   ├── main.js                # 入口文件 加载组件 初始化等
│   ├── permission.js          # 权限管理
│   └── settings.js            # 项目设置文件
├── test                       # 测试
├── .env.xxx                   # 环境变量配置
├── .eslintignore              # eslint 忽略配置
├── .eslintrc.js               # eslint 配置项
├── .gitignore                 # git 忽略配置
├── .prettierrc                # prettier 配置项
├── .babelrc                   # babel-loader 配置
├── .travis.yml                # 自动化 CI 配置
├── LICENSE                    # license证书
├── postcss.config.js          # postcss 配置
├── vue.config.js              # vue config 配置
├── package.json               # package.json
├── package-lock.json          # package-lock.json
└── README.md                  # README 文件
```

### 2.组件规范

#### 2.1 组件命名

##### 2.1.1 单文件组件名

- 在 views 目录下单文件组件统一采用`kebab-case`命名，字母小写，以短横分隔单词。
- 组件名不能和`html`标签元素冲突。
- 组件名应该以描述性单词开头，以修饰性单词结尾。
- 使用有意义的单词组合，2~3 个单词最佳，单词不能过长。
- 只有一个单词的组件全部都要小写。

正例：

```
login.vue
home.vue
user-info.vue
base-config.vue
search-button-clear.vue
```


反例：

**以下命名是不规范的，快看有没有中招**

```
Login.vue
Home.vue
span.vue
userInfo.vue
base_Config.vue
search_button_clear.vue
```

##### 2.1.2 基础组件名

基础组件文件名为 base 开头，使用完整单词而不是缩写，表示基础展示类、无逻辑或无状态的组件。

正例：

```
components/
├─ base-button.vue
└─ base-table.vue
```


反例：

```
components/
├─ my-button.vue
└─ my-table.vue
```

##### 2.1.3 单例组件名

单例组件文件名为 the 开头，作用是在每个页面只用一次，比如网页顶部的导航栏，底部的版权地址信息，这些组件不会接受 prop 传参。

正例：

```
components/
├─ the-header.vue
└─ the-footer.vue
```


反例：

```
components/
├─ header.vue
└─ footer.vue
```

##### 2.1.4 紧密耦合的组件名

如果一个组件只在某个父组件的场景下有意义，这层关系应该体现在其名字上，因为编辑器通常会按字母顺序组织文件，所以这样做可以把相关联的文件排在一起。

正例：

```
components/
├─ todo-list.vue
├─ todo-list-item.vue
└─ todo-list-item-button.vue
```


反例：

```
components/
├─ todo-list.vue
├─ todo-item.vue
└─ todo-button.vue
```

#### 2.2 公用组件

##### 2.2.1 局部公用

在当前功能的目录新建 components 文件夹，所有声明的组件放在里面，采用 PascalCase 帕斯卡命名，并且功能目录需要有 index.vue 入口文件。

```
src/views/user-info
├─ index.vue
└─ components/
  ├─ BaseInfo.vue
  └─ UserInfo.vue
```

##### 2.2.2 全局公用

统一放到工程目录的 src/components 下面，目录名称采用 PascalCase 帕斯卡命名，并且每个公用组件目录必须有 index.vue 入口文件和 README.md 文件 Api 说明文档。
在编写 README.md 文档时也需要注意，Api 文档中Attribute属性 和Event事件的命名都是要用`kebab-case`命名，不能用驼峰命名。

```
src/components/
├─ PageHeader
   ├─ index.vue
   └─ README.md
├─ UploadData
   ├─ index.vue
   └─ README.md
```

#### 2.3 组件使用

自定义组件的使用按照以下规范，除了在Dom模板中用`kebab-case`命名，其他都是用PascalCase 帕斯卡命名。

如果是用 viewUI，element-ui，ant-vue等 UI 框架中的组件则按照官方示例的命名规则使用。

##### 2.3.1 单文件组件中使用

在单文件组件中用 PascalCase 帕斯卡命名，如果组件中没有内容则需要自闭合标签。

```
<template>
  <div class="app-container">
    <UserInfo :user-detail="userDetail" />
  </div>
</template>
```

##### 2.3.2 字符串模板中使用

在字符串模板template 中使用组件，采用 PascalCase 帕斯卡命名，如果组件中没有内容则需要自闭合标签。

```
Vue Vue({
  template:`<UserInfo :user-detail="userDetail" />`
});
```

##### 2.3.3 JS/JSX 中使用

在 JS/JSX 的` render `函数中使用组件，采用 PascalCase 帕斯卡命名，如果组件中没有内容则需要自闭合标签。

```
new Vue({
  el: '#app',
  render: function (h) {
    return (
      <UserInfo :user-detail="userDetail" />
    )
  }
})
```

##### 2.3.4 DOM 模板中使用

在 `DOM `模板中需要用`kebab-case`命名，因为是直接在 html 页面中使用，html 是会忽略大小写，并且不能自闭合标签。

```
 <html>
   <body>
   	  <div class="app-container">
        <user-info :user-detail="userDetail"></user-info>
      </div>
   </body>
 </html>
```

### 3.命名规则

#### 3.1 布尔值命名

布尔值是两种逻辑状态的变量`真`和`假`，对应` true` 和` false`，也可以用数字 1 和 0 表示真假。
推荐命名方式为 is + 动词（现在进行时）/ 形容词，但是有些场景下也可以不用写 is 开头。

- 场景：可见性 / 进行中

```
{
  "isShow": "是否显示",
  "isVisible": "是否可见",
  "isLoading": "是否处于加载中",
  "isConnecting": "是否处于连接中",
  "isValidating": "正在验证中",
  "isDoing": "正在进行中",
  "isRunning": "正在运行中",
  "isListening": "正在监听中"
}
```

- 场景：属性 / 状态

```
{
  "isDisabled": "是否禁用",
  "isEdit": "是否可编辑",
  "isAdd": "是否可增加",
  "isUpdate": "是否可修改",
  "isRemove": "是否可删除",
  "isClearable": "是否可清空",
  "isReadonly": "是否只读",
  "isExpand": "是否可展开",
  "isShrink": "是否可收缩",
  "isChecked": "是否选中",
  "isClick": "是否可点击",
  "isDrag": "是否可拖拽",
  "isOpen": "是否打开",
  "isClose": "是否打开",
  "isChoice": "是否选择（复选框，单选框）",
  "isSelect": "是否选择（下拉框，列表）",
  "isConfig": "是否可配置"
}
```

- 场景：弹框显示 / 隐藏

```
{
  // 组合1-基础表单
  "addModal": "新增弹框",
  "editModal": "编辑弹框",
  "detailModal": "详情弹框",

  // 组合2-业务相关
  "syncDataModal": "同步数据弹框",
  "addUserModal": "新增用户弹框",
  "userInfoModal": "用户信息弹框",
  "assignPermissionModal": "分配权限弹框",
  "departTreeModal": "部门树弹框"
}
```

#### 3.2 时间变量命名

和时间相关的变量，必须带有`date / month / year / time / hour / minutes / seconds`等等和时间相关的。

- 场景：快捷日期

```
{
  "now": "此刻",
  "today": "今天",
  "yesterday": "昨天",
  "tomorrow": "明天",
  "workday": "工作日",
  "weekend": "周末（周六和周日）"
}
```

- 场景：开始 / 结束

```
{
  "startDate": "是否禁用",
  "endDate": "是否可编辑",
  "startTime": "是否可增加",
  "endTime": "是否可修改"
}
```

- 场景：上下 / 前后

```
{
  "prevMonth": "上一月",
  "nextMonth": "下一月",
  "prevYear": "上一年",
  "nextYear": "下一年",
  "beforeCurrentDate": "当前日期之前",
  "afterCurrentDate": "当前日期之后"
}
```

#### 3.3 数组变量命名

数组主要有 复数形式 和 xxxList 形式，简单的区别是 复数形式 的适合前端本身声明的数组，xxxList 形式的适合后台接口返回的数组。

```
{
  // 前端声明变量
  "users": "用户列表",
  "menus": "菜单列表",
  "selectNodes": "选中节点列表",

  // 后台返回数据接收变量
  "userList": "用户列表",
  "menuList": "菜单列表",
  "statusList": "状态列表"
}
```

#### 3.4 选项变量命名

主要针对的是单选框，复选框，下拉框，选项卡等元素数据。
常见的词汇有：title， name，label，field，text，id，key，value，children，index，nodes等。
其中 title，name，label，text，field作为选项显示名，id，key，value 用于唯一标识，children，nodes用于包含子节点内容。

- 场景：复选 / 单选 / 下拉

```
{
  // 常用组合
  "label": "标题",
  "value": "值",

  // 组合1
  "title": "标题",
  "value": "值",

  // 组合2
  "text": "文本",
  "value": "值",

  // 组合3
  "id": "标题",
  "name": "值",

  // 组合4
  "field": "标题",
  "value": "值",
  "index": "标题"
}
```

- 场景：选择项 / 激活项

```
{
  "activeName": "激活的项名称",
  "activeTab": "激活的选项卡",
  "currentPage": "当前页",
  "currentIndex": "当前选项的下标"
}
```

#### 3.5 Api 接口命名

主要用作 axios 请求后台接口封装，接口方法使用 camelCase 驼峰命名，命名为动词+名词形式。
后台一般采用 `Restful Api` 规范，如下示例：

```
/api/user/list
/api/user/detail/${id}
/api/user/add
/api/user/update
/api/user/delete/${id}
```


前端对应的接口方法命名用 `getXxx，addXxx，updateXxx，deleteXxx `这几种命名形式，并且一定要写注释，注释格式为`模块名-方法`描述，如下示例：

```
/**
用户管理-分页列表
*/
export const getUserList = (params) => {
  return axios.get("/api/user/list", params);
};

/**
用户管理-详情
*/
export const getUserDetail = (params) => {
  return axios.get(`/api/user/detail/${params.id}`);
};

/**
用户管理-新增
*/
export const addUser = (data) => {
  return axios.post("/api/user/add", data);
};

/**
用户管理-修改
*/
export const updateUser = (params) => {
  return axios.post("/api/user/update", data);
};

/**
用户管理-删除
*/
export const deleteUser = (params) => {
  return axios.get(`/api/user/delete/${params.id}`);
};
```

#### 3.6传参属性命名

- 场景：普通 attribute
  属性类型是 String，Number，Boolean，Object，Array，Date 这几种数据类型，采用 `名词` 或者 `名词+修饰词` 形式。

```
{
  "class-name": "类名",
  "z-index": "层级",
  "fullscreen": "全屏显示",
  "show-upload-list": "是否展示上传文件列表"
}
```

- 场景：传参 function
  组件除了能传普通的数据类型，还能传递 `function`，采用`介词+动词/形容词`形式，注意这里事件名是允许用 `on-xx` 形式的，用来区分该 `props` 传递的是个函数。

```
{
  "on-success": "上传成功",
  "on-progress": "上传进度",
  "on-error": "上传失败",
  "on-preview": "预览",
  "before-upload": "上传之前",
  "before-close": "关闭之前",
  "after-close": "关闭之后"
}
```

#### 3.7 函数命名

- 场景：绑定事件监听
  绑定的事件名采用`kebab-case`命名，字母小写，以短横分隔单词。因为在 `Dom 模板`中的事件监听，由于 `HTML `中是不区分大小写的，`Vue `会自动转换事件监听为全小写，所以推荐 始终使用` kebab-case` 的命名。
  事件的命名采用 `动词形式`，事件绑定本身就是 `v-on`指令，不能写成`v-on:on-close`，所以绑定的事件统一都不用` on-xx `的形式。

```
{
  // 原生事件监听
  "input": "输入",
  "change": "改变",
  "blur": "失去焦点",
  "focus": "聚焦",
  "submit": "提交",
  "keyup": "提交",
  "keydown": "提交",
  "enter": "提交",
  "submit": "提交",

  // 自定义事件监听
  "preview": "预览",
  "clear": "清空",
  "cancel": "关闭",
  "open": "打开",
  "select-change": "选择",
  "select-all": "选择所有",
  "selection-change": "选项改变",
  "sort-change": "排序",
  "row-click": "单击行",
  "row-dblclick": "双击行"
}
```

在声明 `事件监听函数` 或者声明 `props 绑定函数` 的时候有些是需要用 `onXxx` 的形式，有些则可以不带 `on`，具体要看当前环境中的作用和语义。

```
// 组合1
function onInput() {}
function onPreview() {}
function onSelectAll() {}
function onRowClick() {}
function onSelectionChange() {}

// 组合2
function beforeUpload() {}
function beforeOpen() {}
function beforeClose() {}
```

- 场景：自定义事件
  自定义事件名使用 `camelCase` 驼峰命名，命名为`动词+名词`形式。
  名称不能过长，要明确表达此事件处理的作用，避免望文不知义。
  可以参考 `JavaScript 规范`中 `函数命名` 章节，使用常用的动词来命名。

一般由页面直接或者间接发起的操作，用 `handleXxx` 的形式。

```
function handleSearch() {}
function handleReset() {}
function handleResize() {}
```

由方法发起调用的操作，用`动词`开头的形式，命名看方法的用途决定。

```
function resetQuery() {}
function resetForm() {}
function resetSelect() {}
```

改变数据结果，切换状态的显示的操作，也以`动词+形容词/名词`形式，比如 Modal 的展示隐藏，分页条的改变。

在其他场景下，很多命名方式也都可以用`动词+形容词/名词`形式，例如`select+范围`，`元素+click`，`change+元素`，`clear+元素`，这些动词开头，用在改变元素，改变分页，改变业务状态，切换 Modal 弹框显示等等。

```
// 组合1-元素操作
function selectOne() {}
function selectAll() {}
function tabClick() {}
function switchTab() {}
function clearName() {}

// 组合2-分页
function changePageNum() {}
function changePageSize() {}
function jumpPage() {}

// 组合3-业务弹框
function showAddModal() {}
function showEditModal() {}
function showDetailModal() {}
function showSyncDataModal() {}
function showUserInfoModal() {}
function showDepartTreeModal() {}

// 组合4-业务状态
function changeUserStatus() {}
function changeUserStatus() {}
function changeUserPermission() {}
```

- 场景：异步处理方法
  在了解过接口命名规则之后，前端页面中使用 `Api 接口`的异步事件，这些一般比较固定，以下组合列举出了一些场景。

```
// 组合1
// 查询的是后台返回的列表，推荐是用 queryXxxList 这种形式
function queryList() {}
function queryMenuList() {}
function queryUserList() {}
function queryDetail() {}

// 组合2
// 查询的是后台返回的数据信息，详情等，推荐用 queryXxx+Data/Info/Detail 这种形式
function queryProcessData() {}
function queryStepInfo() {}
function queryTaskDetail() {}

// 组合3
// 页面增删查改的操作，用 handleXxx 的形式
function handleAdd() {}
function handleEdit() {}
function handleDelete() {}
function handleSubmit() {}
```

- 场景：路由页面跳转
  路由页面的跳转一般只有 `goXxx` 和 `toXxx` 两种，两者使用差别不大，只在少部分页面跳转上有语义的差别。
  比如 去首页，返回上级 这种表示只跳到这些页面就结束行为的用 `goXxx` 比较合适。
  比如 跳转到详情页面，跳转到审批页面 这种表示跳到有数据信息的页面，并且可能带传参的用 `toXxx` 比较合适。

```
{
  // 组合1
  "goHome": "去首页",
  "goBack": "返回页面",
  "goMailBox": "去邮箱",

  // 组合2
  "toUserDetail": "跳转到用户详情",
  "toApprove": "跳转到审批页面",

  // 组合3
  "navigateToHome": "导航到首页",
  "redirectToLogin": "重定向到登录页",
  "switchHome": "切换到首页",
  "switchOrder": "切换到订单页"
}
```

- 场景：数据的加工
  针对数据在不同场景下的转换处理操作，比如格式化，排序，过滤，转换，切换，比较，去除空格，添加，区间，相差等等。

```
{
  // 组合1
  "formatThousand": "格式化数字千分位",
  "formatRmbChinese": "格式化人民币金额大写",
  "formatStartOfName": "格式化姓名中间为星号",

  // 组合2
  "sortUserList": "用户列表排序",
  "sortOrder": "订单排序",
  "sortByName": "根据名称排序",

  // 组合3
  "filterUser": "过滤用户",
  "filterByName": "通过名称过滤",

  // 组合4
  "convertCurrency": "转换货币单位",
  "convertAmount": "转换金额",

  // 组合5
  "toggleClass": "切换演示",

  // 组合6
  "compareDate": "比较日期",
  "compareAge": "比较年龄大小",

  // 组合7
  "trimStart": "去除字符串开始位置的空格",
  "trimEnd": "去除字符串结束位置的空格",
  "trimAll": "去除字符串中全部的空格",

  // 组合8
  "addDate": "日期加天",
  "addMonth": "日期加月",
  "addYear": "日期加年",

  // 组合8
  "diffDay": "相差的天数",
  "diffWeek": "相差的周数",
  "diffMonth": "相差的月数"
}
```

#### 3.8 方法参数命名

方法中参数名称和数量是根据具体使用来决定，但是还是有一些固定的模式可以参考。比如监听属性的新值和旧值，reduce 方法的上个值和当前值，回调函数的参数值等等。

- 场景：新值 / 旧值
  用于 `Vue` 的 `watch` 属性监听的回调参数。

```
{
  "newVal": "新值",
  "oldVal": "旧值"
}
```

- 场景：上个值 / 下个值 / 当前值

```
{
  // 组合1
  "from": "从...",
  "to": "到...",

  // 组合2
  "prev": "上一个...",
  "next": "下一个...",
  "cur": "当前",

  // 组合3
  "source": "源",
  "target": "目标",

  // 组合4
  "start": "开始",
  "end": "结束",

  // 组合5
  "before": "在之前",
  "after": "在之后",

  // 组合6
  "prefix": "前缀",
  "suffix": "后缀"
}
```

- 异步数据回调
  用于 `Promise` 的 `then` 方法参数，`await` 返回的值等，可选择的词汇有：`res / data / json`，推荐使用 `response` 简写的 `res`。

```
// 示例1
getUserInfo().then((res) => {});
// 示例2
const res = await getUserInfo();
```

- 场景：其他参数命名
  除了以上这些，实际中还有其他众多类型的应用场景，以下列举出开发中常用的一些。

```
{
  // 组合1
  "str": "字符串",
  "char": "单个字符",
  "num": "数字",
  "arr": "数组值",
  "obj": "对象值",

  // 组合2
  "len": "数组长度",
  "index": "下标",
  "size": "大小",
  "unit": "单位",
  "count": "数量",
  "amount": "金额",
  "timer": "定时器",

  // 组合2
  "n": "次数",
  "p": "数值的精度",
  "x": "x轴坐标",
  "y": "y轴坐标",
  "radix": "基数",
  "decimals": "小数位数",

  // 组合3
  "rate": "比例",
  "status": "状态",
  "bool": "布尔值",
  "asc": "升序",
  "desc": "降序",
  "pid": "父id",
  "cid": "子id",

  // 组合4
  "reg": "正则",
  "func": "函数",
  "callback": "回调函数",

  // 组合5
  "source": "数据源",
  "target": "目标源",
  "root": "根",
  "elem": "元素",
  "children": "子级",
  "parent": "父级"
}
```

#### 3.9 接口数据响应

接口一般是后台来给，前端如果用 `nodejs` 做后台服务器，接口响应报文体也需要制定一套规则。

- Restful Api 命名
  在`接口命名`章节中已经列举了 api 的命名和调用方法规范，前端一般用 `nodejs+express/koa2` 就可以实现 `Restful Api `方式。

```
/api/user/list
/api/user/detail/${id}
/api/user/add
/api/user/update
/api/user/delete/${id}
```

- code 码
  前端用 `nodejs` 做后台，业务场景不会太过复杂。 简单的场景可以用以下的定义 `code`，复杂的业务需要指定单独的`业务 code`。

```
200 # 成功
400 # 请求参数错误
403 # 无权限访问
404 # 请求路径无效
500 # 服务器内部异常
502 # 网关访问错误
504 # 网关访问超时
```

- 响应数据结构
  分页列表

```
{
  "code": 200,
  "message": "查询成功",
  "data": {
    "result": [
      { "id": 1, "name": "小红" },
      { "id": 1, "name": "小红" }
    ],
    "total": 0,
    "page": 1,
    "size": 10
  }
}
```


增删改，详情等返回非数组结果

```
{
  "code": 200,
  "message": "操作成功",
  "data": { "id": 1, "name": "小红" }
}
```

3.10 模板结构顺序
在 `HTML 规范` 的 `结构顺序` 这一章节，已经大致描述了编写页面的结构和顺序，在 `Vue 模板`中也一样，自上而下，从左到右，保持一定的顺序。
`Vue 模板`中，采用了` 容器 `和 `盒子 `结合的语义化结构，先划分不同的容器空间，每个容器里再划分具体的盒子内容。

```
<template>
  <div class="app-container">
    <!-- header -->
    <div class="header-container"></div>

    <!-- main -->
    <div class="main-container">
      <div class="left-box"></div>
      <div class="content-box"></div>
    </div>

    <!-- footer -->
    <div class="footer-container"></div>

    <!-- modal 弹框 -->
    <!-- 新增，编辑，详情等等弹框都放在这里 -->
    <el-modal></el-modal>

    <!-- components 组件 -->
    <!-- 引入的外部组件放在这里，按照作用顺序排列：自定义组件》第三方的排在最后 -->
    <UserInfo />
    <BaseTable />
    <UploadData />
  </div>
</template>
```

#### 3.11 CRUD 模板示例

通过上面的 vue 项目中的不同种类的命名规则，以下是一段代码示例模板。

```
<template>
  <div class="app-container">
    <!-- header -->
    <div class="header-container">
      <!-- 查询条件 -->
      <div class="search-box">
        <el-input
          v-model="listQuery.name"
          clearable
          placeholder="姓名"
          style="width:300px"
        ></el-input>
        <el-button type="primary" @click="handleSearch">查询</el-button>
        <el-button type="primary" @click="handleReset">重置</el-button>
      </div>
      <!-- 按钮操作 -->
      <div class="operate-box">
        <el-button
          icon="el-icon-plus"
          type="primary"
          @click="handleShowAddModal"
        >
          添加
        </el-button>
      </div>
    </div>

    <!-- main -->
    <div class="main-container">
      <!-- 表格列表 -->
      <div class="table-box">
        <el-table v-loading="listLoading" :data="list">
          <el-table-column align="center" label="序号" width="70">
            <template slot-scope="{ row, $index }">
              {{ listQuery.pageSize * (listQuery.pageNo - 1) + ($index + 1) }}
            </template>
          </el-table-column>
          <el-table-column align="center" label="姓名">
            <template slot-scope="{ row, $index }">
              <el-link :underline="false" @click="toUserDetail">
                {{ row.name }}
              </el-link>
            </template>
          </el-table-column>
          <el-table-column align="center" label="创建时间">
            <template slot-scope="{ row, $index }">
              {{ row.createTime }}
            </template>
          </el-table-column>
          <el-table-column align="center" label="操作">
            <template slot-scope="{ row, $index }">
              <el-link
                type="primary"
                :underline="false"
                @click="handleShowEditModal(row)"
              >
                编辑
              </el-link>
              <el-link
                type="danger"
                :underline="false"
                @click="handleDelete(row)"
              >
                删除
              </el-link>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <!-- 分页 -->
      <div class="pagination-container">
        <el-pagination
          background
          :current-page.sync="listQuery.pageNo"
          :page-sizes="[10, 20, 30, 50]"
          :page-size="listQuery.pageSize"
          :total="listTotal"
          layout="total, sizes, prev, pager, next, jumper"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
        ></el-pagination>
      </div>
    </div>

    <!-- modal 弹框 -->
    <!-- 新增或编辑 -->
    <el-dialog
      :title="editForm.id ? '编辑' : '添加'"
      :visible.sync="editModal"
      width="960px"
    >
      <el-form
        :model="editForm"
        :rules="editFormRules"
        ref="editFormRef"
        label-suffix="："
        label-width="100px"
      >
        <el-form-item label="姓名" prop="name">
          <el-input
            v-model="editForm.name"
            placeholder="请输入姓名"
            style="width: 600px"
            clearable
          ></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" style="text-align: center">
        <el-button @click="editModal = false">取消</el-button>
        <el-button type="primary" @click="handleSubmit">确定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
// 引入公用工具类
import util from "@/utils";
// 引入api
import { getUserList, addUser, updateUser, deleteUser } from "@/api/user";

export default {
  data() {
    return {
      // 分页查询
      list: [],
      listTotal: 0,
      listLoading: false,
      listQuery: {
        name: "",
        pageNo: 1,
        pageSize: 10,
      },

      // 新增或编辑
      editModal: false,
      editForm: {
        id: "",
        name: "",
      },
      editFormRules: {
        name: [
          {
            required: true,
            message: "请输入姓名",
            trigger: "blur",
          },
        ],
      },

      // 当前行数据
      selectRow: {},
      // 当前多选行数据
      selectRows: [],
    };
  },
  created() {
    this.queryList();
  },
  methods: {
    /**
     * 查询分页列表
     */
    queryList() {
      this.listLoading = true;
      getUserList({ name: this.listQuery.name })
        .then((res) => {
          this.listLoading = false;
          if (res.code == "200") {
            this.list = res.data.result;
            this.listTotal = res.data.total;
          } else {
            this.$message.error(res.message);
          }
        })
        .catch((err) => {
          this.listLoading = false;
        });
    },
    // 分页
    handleSizeChange(val) {
      this.listQuery.pageSize = val;
      this.listQuery();
    },
    handleCurrentChange(val) {
      this.listQuery.pageNo = val;
      this.listQuery();
    },

    /**
     * 重置分页
     */
    handleReset() {
      this.listQuery = {
        name: "",
        pageNo: "",
        pageSize: 10,
      };
    },

    /**
     * 添加操作
     */
    handleShowAddModal() {
      this.editForm = { id: "", name: "" };
      if (this.$refs.editFormRef) {
        this.$refs.editFormRef.resetFields();
      }
      this.editModal = true;
    },

    /**
     * 修改操作
     * @param {Object} row 选择当前行数据
     */
    handleShowEditModal(row) {
      this.selectRow = row;
      this.editForm = {
        id: row.id,
        name: row.name,
      };
      this.editModal = true;
    },

    /**
     * 提交操作
     */
    handleSubmit() {
      this.$refs.editFormRef.validate((valid) => {
        if (valid) {
          // id不为空，则是修改
          if (this.editForm.id) {
            this.handleUpdate();
          } else {
            this.handleAdd();
          }
        } else {
          console.log("submit fail");
          return false;
        }
      });
    },
    // 添加
    handleAdd() {
      addUser({
        name: this.editForm.name,
      }).then((res) => {
        if ((res.code = "200")) {
          this.$message.success("添加成功");
          this.editModal = false;
        } else {
          this.$message.error(res.message);
        }
      });
    },
    // 修改
    handleUpdate() {
      updateUser({
        id: this.editForm.id,
        name: this.editForm.name,
      }).then((res) => {
        if ((res.code = "200")) {
          this.$message.success("修改成功");
          this.editModal = false;
        } else {
          this.$message.error(res.message);
        }
      });
    },

    /**
     * 删除操作
     * @param {Object} row 选择当前行数据
     */
    handleDelete(row) {
      this.$confirm("确定删除该条记录吗？", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      }).then(() => {
        deleteUser({
          id: row.id,
        }).then((res) => {
          if ((res.code = "200")) {
            this.$message.success("删除成功");
            this.queryList();
          } else {
            this.$message.error(res.message);
          }
        });
      });
    },

    /**
     * 跳转详情操作
     * @param {Object} row 选择当前行数据
     */
    toUserDetail(row) {
      this.$router.push({
        path: "/user/detail",
        query: { id: row.id },
      });
    },
  },
};
</script>
<style lang="scss" scoped></style>
```

### 4.代码规范

##### 4.1 代码缩进

统一使用 2 个缩进，不允许使用 Tab 制表符，全部替换为空格缩进。

##### 4.2 bind/on/v-slot 指令缩写

指令推荐都是用缩写形式

- `:` 表示 `v-bind`
- `@` 表示 `v-on`
- `#`表示 `v-slot`

正例：

```
<template>
  <el-input :maxlength="20" @input="onInput"></el-input>
  <div #header>
    <p>this is header!</p>
  </div>
</template>
```

反例：

```
<template>
  <el-input v-bind:maxlength="20" v-on:input="onInput"></el-input>
  <div v-slot:header>
    <p>this is header!</p>
  </div>
</template>
```

##### 4.3 data 必须是函数

当在组件中使用 data 属性的时候，它的值必须是返回一个对象的函数，因为如果直接是一个对象的话，子组件之间的属性值会互相影响。
正例：

```
<script>
export default {
  data () {
    return {
      age: 20
    }
  }
}
</script>
```

反例：

```
<script>
export default {
   data: {
    age: 20
  }
}
</script>
```

##### 4.4 v-for 设置 key

在组件上总是必须用 key 配合 v-for，以便维护内部组件及其子树的状态。

```
<template>
  <div v-for="(item, index) in list" :key="index">{{ item.name }}</div>
</template>
```

##### 4.5 v-if 和 v-show 选择

`v-if `和` v-show `都可以用于显示隐藏效果，但是本质上还是有区别的。
`v-if` 是惰性的，只有条件成立时候才渲染，比较适合根据条件显示隐藏的；
`v-show` 是不管条件真假，直接就会被渲染上，比较适合频繁切换的场景。

##### 4.6避免` v-if` 和 `v-for` 一起使用

永远不要把 `v-if `和 `v-for` 同时用在同一个元素上，`v-for` 的优先级比 `v-if` 高，所以会导致先循环遍历，然后依次根据` v-if `的判断显示隐藏，会比较浪费性能。

```
<template>
  <!-- 可以列表提前通过计算属性过滤一遍 -->
  <div v-for="item in userListComputed"></div>

  <!-- 也可以在最外层先包括一层，再判断 -->
  <template v-if="showUser">
    <div v-for="item in userList"></div>
  </template>
</template>
```

##### 4.7 组件 attribute 传参

在组件中属性传参使用`kebab-case`命名，字母小写，以短横分隔单词。

正例：

```
<template>
  <UserInfo :user-detail="userDetail" />
</template>
```

反例：

```
<template>
  <UserInfo :userDetail="userDetail" />
</template>
```

##### 4.8 多个 attribute 封装对象

多个 attribute 的元素应该分多行，如果属性比较多，推荐封装个到对象中传参。
正例：

```
<template v-if="showUser">
  <!-- 多个元素分行 -->
  <UserInfo
    :id="1"
    :name="name"
    :sex="sex"
    :age="10"
    :birthday="birthday"
  ></UserInfo>

  <!-- 元素太多，推荐封装对象 -->
  <UserInfo :user-info="user"></UserInfo>
</template>
```

##### 4.9 带引号的 attribute 值

模板中 attribute 的值需要带引号，并且需要与前面空一格，另外模板中也是可以使用模板字符串。

```
<template>
  <div>
    <el-input :style="{ width: myWidth + 'px' }" />
    <el-input :style="`width: ${myWidth}px`" />
    <el-select>
      <el-option
        v-for="item in userList"
        :key="item.id"
        :label="`${item.name}（${item.age}岁）`"
        :value="item.id"
      ></el-option>
    </el-select>
  </div>
</template>
```

##### 4.10 组件 attribute 值的简单逻辑

模板中 `attribute` 的值是需要逻辑判断结果的时候，只允许用 `等于`，`大于`，`小于`这些`一元运算`，或者简单的`三元运算`判断，复杂的需要拆分为单独的计算属性或者方法。

正例：

```
<template>
  <div>
    <!-- 简单的 一元运算 -->
    <el-input v-model="name" :disabled="editForm.id" />
    <el-input v-model="age" :disabled="editForm.age > 20" />
    <!-- 简单的 三元运算 -->
    <el-input
      v-model="departName"
      :disabled="editForm.status == 1 ? true : false"
    />
    <!-- 需要判断多种状态的逻辑，把同类的抽取出来放到数组中也可以 -->
    <el-input
      v-model="departName"
      :disabled="[1,2,3].includes(editForm.status])"
    />
    <!-- 更多一点的逻辑判断，需要封装为计算属性，或者方法 -->
    <el-input v-model="departName" :disabled="canEditDepartName()" />
  </div>
</template>
<script>
export default {
  methods: {
    canEditDepartName() {
      return this.editForm.age > 20 || [1, 2, 3].includes(editForm.status);
    },
  },
};
</script>
```

反例：

```
<template>
  <div>
    <!-- 过于复杂 三元运算 -->
    <el-input
      v-model="departName"
      :disabled="
        editForm.status == 1
          ? true
          : editForm.status == 2
          ? true
          : editForm.status == 3
          ? true
          : false
      "
    />
    <!-- 以下这种判断很长的也禁止这样写 -->
    <el-input
      v-model="departName"
      :disabled="
        editForm.status == 1 ||
        editForm.status == 2 ||
        editForm.status == 3 ||
        editForm.status == 4 ||
        (editForm.status == 5 && editForm.age > 20)
      "
    />
    <!-- 更不能图省事直接在模板中写实际的操作代码 -->
    <el-input
      v-model="departName"
      :disabled="
        findStatus.some((item) => {
          return item.status == editForm.status;
        })
      "
    />
  </div>
</template>
```

##### 4.11 组件 attribute 属性顺序

元素的 `attribute` 应该有统一的顺序，如果有使用 eslint 时候也会按照一定规则校验。

- 样式名（可复用的元素样式排首位）
  `class`

- 列表渲染 (创建多个变化的相同元素)
  `v-for`

- 条件渲染 (元素是否渲染/显示)
  `v-if`
  `v-else-if`
  `v-else`
  `v-show`
  `v-cloak`

- 渲染方式 (改变元素的渲染方式)
  `v-pre`
  `v-once`

- 自定义指令 (改变元素的属性和效果)
  `v-loading`
  `v-input-number`

- 全局唯一 (唯一性的标识)
  `id`

- 唯一的 attribute (需要唯一值的 attribute)
  `ref`
  `key`

- 双向绑定 (把绑定和事件结合起来)
  `v-model`

- 自定义属性 (所有自定义绑定的 attribute)
  `:user-name`
  `user-age`

- 事件 (组件事件监听器)
  `v-on`

- 自定义事件 (所有自定义绑定的 event)
  `@row-click`
  `@row-dblclick`

- 元素内容 (覆写元素的内容)
  `v-html`
  `v-text`

##### 4.12 组件 prop 定义

- 必须使用 `camelCase `驼峰命名。
- 必须指定参数类型。
- 必须加上注释，表明参数的含义。
- 必须加上 `default` 默认值。
- 推荐加上 `required`，如果有业务校验也需要加上 `validator`。

正例：

```
// 简洁的
export default{
  props: {
    userDetail: Object,
  }
}

// 符合eslint推荐的
export default{
  props: {
    userDetail: {
      type: Object,
      default: () => {}
    },
  }
}

// 更加详情，带业务校验的
export default{
  props: {
    userDetail: {
      type: Object,
      required: true,
      validator: function (value) {
        // 校验
      }
    }
  }
}
```

反例：

```
props: ["userDetail"];
```

##### 4.13 模板中简单的表达式

组件模板中应该只包含简单的表达式，比较复杂的处理需要用计算属性或者方法。

正例：

```
<template>
  <div>{{ normalizedFullName }}</div>
</template>
<script>
export default {
  // 复杂表达式已经移入一个计算属性
  computed: {
    normalizedFullName() {
      return this.fullName
        .split(" ")
        .map(function (word) {
          return word[0].toUpperCase() + word.slice(1);
        })
        .join(" ");
    },
  },
};
</script>
```

反例：

```
<template>
  <div>
    {{
      fullName
        .split(" ")
        .map(function (word) {
          return word[0].toUpperCase() + word.slice(1);
        })
        .join(" ")
    }}
  </div>
</template>
```

##### 4.14 简单的计算属性

计算属性应该保持单一性，处理多个逻辑的需要拆分为多个计算属性。

正例：

```
export default {
  computed: {
    basePrice: function () {
      return this.manufactureCost / (1 - this.profitMargin);
    },
    discount: function () {
      return this.basePrice * (this.discountPercent || 0);
    },
    finalPrice: function () {
      return this.basePrice - this.discount;
    },
  },
};
```

反例：

```
export default {
  computed: {
    price: function () {
      var basePrice = this.manufactureCost / (1 - this.profitMargin);
      return basePrice - basePrice * (this.discountPercent || 0);
    },
  },
};
```

##### 4.15 组件/实例的选项顺序

```
<template>
  <div class="app-container">
    <div class="header-container"></div>
    <div class="main-container"></div>
    <div class="footer-container"></div>
  </div>
</template>
<script>
export default {
  name: "MyVue",
  components: {},
  directives: {},
  filters: {},
  extends: {},
  mixins: {},
  props: {},
  data() {
    return {};
  },
  computed: {},
  watch: {},
  beforeCreate() {},
  created() {
    // 请求数据的方法放这里
  },
  beforeMount() {},
  mounted() {
    // 更改页面结构的方法放这里
  },
  beforeUpdate() {},
  updated() {},
  activated() {},
  deactivated() {},
  beforeDestroy() {},
  destroyed() {},
  methods: {},
};
</script>
<style lang="less" scoped></style>
```

##### 4.16 模板中引入资源和组件的顺序

在模板中引入资源是有顺序的，一般是先`公用`后`局部`，资源引入顺序为`js资源 > 组件 > css资源`，如果引入资源比较多，需要合理的分组并写上注释。

```
// 引入第三方工具类
import util from "@lime-util/util";
// 引入第三方组件
import VueScroll from "vuescroll";

// 引入公用方法
import { uploadFile } from "@/common";
// 引入公用组件
import PageHeader from "@/components/PageHeader";

// 引入api
import {
  getUserList,
  getUserDetail,
  addUser,
  updateUser,
  deleteUser,
} from "@/api/user";
// 引入局部组件
import UserInfo from "./components/UserInfo";

export default {};
```

##### 4.17 组件的 name 必须大写

声明组件的 `name`使用PascalCase 帕斯卡命名，尽量每个组件页面都写，方便 `vue` 识别，或者在使用` keep-alive` 时候做缓存。

正例：

```
export default {
  name: "UserInfo",
};
```


反例：

```
export default {
  name: "userInfo",
};
export default {
  name: "user-info",
};
```

##### 4.18自闭合组件

在`单文件组件`、`字符串模板`和 `JSX `中没有内容的组件应该是自闭合的，但在` DOM 模板`里不要这样做。

在单文件组件中

```
<template v-if="showUser">
  <UserInfo :user-detail="userDetail" />
</template>
```

在字符串模板中

```
Vue Vue({
  template:`<UserInfo :user-detail="userDetail" />`
});
```


在 JS/JSX 中

```
new Vue({
  el: '#app',
  render: function (h) {
    return (
      <UserInfo :user-detail="userDetail" />
    )
  }
})
```

在 `Dom 模板`中，需要有结束标签

```
<body>
  <div class="app-container">
    <user-info :user-detail="userDetail"></user-info>
  </div>
</body>
```

##### 4.19组件选项中的空行

有时候 props 或者 computed 中有很多选项，中间间隔挨着就会不容易区分，这时候就需要添加空行来让代码更加可观一些。添加合理的空行也适用于在 filters，data，methods 中。

```
<script>
export default {
  filters: {
    // 格式化数字
    formatAmount(val) {},

    // 格式化文字
    formatText(val) {},

    // 格式件时间
    formatTime(val) {},
  },
  props: {
    // id
    id: {
      type: Number,
      required: true,
      default: undefined,
    },

    // 名称
    name: {
      type: String,
      required: true,
      default: "",
    },

    // 性别
    sex: {
      type: String,
      required: true,
      default: "未知",
    },

    // 年龄
    age: {
      type: Number,
      required: true,
      default: undefined,
    },

    // 生日
    birthday: String,
  },
  computed: {
    // 计算总数
    totalAmount() {},

    // 求和
    sumAmount() {},
  },
  methods: {
    /**
     * 获得用户列表
     */
    getList() {},

    /**
     * 新增用户
     */
    addUser() {},

    /**
     * 修改用户
     */
    updateUser() {},
  },
};
</script>
```

##### 4.20 组件标签的顺序

单文件组件应该使`<template>`，`<script>`和`<style>`标签保持顺序，并且`<style>`标签要放在最后，因为另外两个标签至少都是有的。

正例：

```
<template></template>
<script></script>
<style></style>
```

反例：

```
<template></template>
<style></style>
<script></script>
```

```
<style></style>
<template></template>
<script></script>
```

##### 4.21 组件样式的 scoped 作用域

编写组件页面的样式，一定要加上 `scoped`，使用独立的作用域，避免污染全局样式。

```
<style lang="less" scoped></style>
```

##### 4.22 避免使用 $parent 操作父组件

不能直接使用$parent 访问父组件的上下文，这种做法是不被允许的。可以改为 $emit ，.sync同步 ，props 传参，vue bus` 方式操作父组件的属性和方法。

##### 4.23 尽量不要手动操作 Dom

在项目开发中尽量使用 Vue 的数据驱动更新 Dom，尽量不要手动操作 DOM，包括：增删改 Dom 元素，更改样式，添加事件等等。

##### 4.24 模板中监听事件的简写

在模板中，如果事件监听的方法不需要传参，则需要省略括号。

正例：

```
<template>
  <el-button type="primary" @click="handleAdd" />
  <el-button type="primary" @click="handleDelete(1)" />
</template>
```

反例：

```
<template>
  <el-button type="primary" @click="handleAdd()" />
  <el-button type="primary" @click="handleOpen()" />
</template>
```

##### 4.25 模板中 Modal 弹框的规范

Modal 弹框组件是特别常见的，为了规范开发和方便阅读，使用时候不能随意写在模板的任意位置，必须统一声明放在模板结尾标签位置，并加上注释区分弹框模块的用途。

```
<template>
  <div class="app-container">
    <!-- 表格列表 -->
    <div class="table-box"></div>

    <!-- Modal 弹框 -->
    <!-- 新增和编辑 -->
    <el-dialog
      title="新增/编辑"
      :visible.sync="editModal"
      width="900px"
    ></el-dialog>
    <!-- 详情 -->
    <el-dialog
      title="详情"
      :visible.sync="detailModal"
      width="900px"
    ></el-dialog>
  </div>
</template>
```

##### 4.26 less 和 sass 样式穿透

在某些场景下，需要修改第三方组件样式，由于 scoped 属性的样式隔离，需要用到样式穿透，统一使用 ::v-deep。

```
<style lang="less" scoped>
::v-deep .el-button {
  height: 60px;
}
</style>
```

##### 4.27 vue-router 路由懒加载

- 路由的组件推荐使用懒加载方式。
- 推荐页面都有一个 `index.vue`入口文件。
- 省略掉所有文件后缀名，`vue-cli` 会自动识别。

正例：

```
const routes = [
  {
    path: "/home",
    meta: {
      title: "首页",
    },
    component: () => import("@/views/home"),
  },
  {
    path: "/user-info",
    meta: {
      title: "用户信息",
    },
    component: () => import("@/views/user-info"),
  },
];
```

反例：

```
const routes = [
  {
    path: "/home",
    meta: {
      title: "首页",
    },
    component: () => import("@/views/home.vue"),
  },
  {
    path: "/user-info",
    meta: {
      title: "用户信息",
    },
    component: () => import("@/views/user-info/index.vue"),
  },
];
```

##### 4.28 vue-router 路由传参

使用 `$router` 的 `query `传参，不使用 `params`，这样参数会在地址栏中不会刷新消失。

```
// 跳转路由
this.$router.push({
  path: "home",
  query: {
    id: 1,
  }
});

// 在 home/index.vue 中接收参数
let id = this.$route.query.id;
```

##### 4.29 vue-router 路由规则

- `path` 路径采用`kebab-case`命名，字母小写，以短横分隔单词。
- 路由 `name` 采用 `PascalCase 帕斯卡`命名。
- 子路由 `path` 可以不用拼父路由的地址，`vue-router` 会自动识别。

```
{
  path: "/base",
  name: "Base",
  component: () => import("@/layout"),
  meta: {
    title: "基础管理"
  },
  children: [
    {
      path: "user-manage",
      name: "UserManage",
      component: () => import("@/views/base/user-manage"),
      meta: {
        title: "用户列表"
      }
    }
  ]
}
```

##### 4.30 vuex 书写和命名规范

- mutations 中的方法必须全部大写，并且用 `SET / ADD / DEL / UPDATE / CHANGE / RESET` 动词开头。
- 接收和处理 `state` 必须在 `actions` 的方法中完成。

```
/* index.js store模板 */
// state
const state = {
  token: "",
};

// mutations
const mutations = {
  SET_TOKEN: (state, token) => {
    state.token = token;
  },
};

// actions
const actions = {
  saveToken({ commit }, token) {
    commit("SET_TOKEN", token);
  },
};

// export
export default {
  namespaced: true,
  state,
  mutations,
  actions,
};
```


#### 5. 注释说明
整理必须加注释的地方
- 公共组件使用说明
- api 目录的接口 js 文件必须加注释
- store 中的 state, mutation, action 等必须加注释
- vue 文件中的 template 必须加注释，若文件较大添加 start end 注释
- vue 文件的 methods，每个 method 必须添加注释
- vue 文件的 data, 非常见单词要加注释

#### 6 其他
1) 尽量不要手动操作 DOM
因使用 vue 框架，所以在项目开发中尽量使用 vue 的数据驱动更新 DOM，尽量（不到万不得
已）不要手动操作 DOM，包括：增删改 dom 元素、以及更改样式、添加事件等。
2) 删除无用代码
因使用了 git/svn 等代码版本工具，对于无用代码必须及时删除，例如：一些调试的 console 语
句、无用的弃用功能代码。