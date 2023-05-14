---
title: hexo搭建博客
date: 2019-10-11 22:11:22
categories:
- blog
---

## 安装nodejs

* archlinux:`pacman -S nodejs`
* ubuntu

## 安装npm

* archlinux:
`pacman -S npm`
* ubuntu:
`apt-get install npm`

### 换源

`npm config set registry https://registry.npmmirror.com  //添加国内源`

使用nrm切换镜像源

`npm install nrm -g //全局安装`

`nrm ls  //查看所有镜像源`

`nrm use taobao  //切换为淘宝镜像源`

查看镜像源：`npm get registry`

安装hexo

`npm install -g hexo-cli`

## ssh

查看ssh

`cd ~/.ssh`

若无，则生成ssh

`ssh-keygen -t rsa -C 用户名`

一路回车默认安装,打开生成的`id_rsa.pub`复制

设置git用户名邮箱  

```bash
git config --global user.name "你的账号"
git config --global user.email "你邮箱地址"
```

## hexo

进入一个文件夹进行初始化：`hexo init`

编辑`_config.yml`
找到

```yml
deploy:
  type:
```

修改为：

```yml
deploy:
  type: git
  repository: ........... ///克隆地址
  branchL: master
```

执行`hexo g -d`上传代码（注意第一次提交需要输入yes，不要一路回车）

**常用命令：**

```bash
hexo generate //Generate static files
hexo server //Run server
hexo deploy //Deploy to remote sites

hexo new My_New_Post    //Create a new post
```

### hexo博客迁移

先新建一个文件夹，初始化后，从原来的文件夹中拷贝下列文件进行覆盖

```txt
themes/
_config.yml
------------------
package.json
scaffolds/
source/
```

## Fexo

* 安装

```bash
 cd my-blog
 git clone git@github.com:forsigner/fexo.git themes/fexo
git clone https://github.com/forsigner/fexo.git
```

* 配置主题
主题配置全部在theme/fexo里面完成，所里下面所有配置指的是配置theme/fexo/_config.yml。

设置基本信息

```yml
blog_name: Forsigner
slogan: Find the bug of the world
```

设置头像

```yml
# relative url
avatar: /images/avatar.jpg
# or absolute url
avatar: https://avatars0.githubusercontent.com/u/2668081?v=3&s=460
```

设置favicon

```yml
favicon: /favicon.ico
```

设置关键词
关键词主要作用是优化SEO

```yml
keywords: forsigner,前端,设计,Hexo主题,前端开发,用户体验,设计,frontend,design,nodejs,JavaScript
```

设置首页内容
你可以设置是否在首页直接显示文章

```yml
init_page_content: HOME_NAV  # HOME_NAV | POST
```

设置首页导航

```yml
home_nav:
  - name: Blog
    url: /archives
  - name: Github
    url: https://github.com/forsigner
    target: _blank
  - name: Douban
    url: http://www.douban.com/people/forsigner/
    target: _blank
  - name: Twitter
    url: https://twitter.com/forsigner
    target: _blank
```

设置页面导航

```yml
page_nav:
  - 博客: /archives/
  - 分类: /category/
  - 标签: /tag/
  - 友链: /link/
  - 关于: /about/
  - RSS: /atom.xml
```

设置页面导航样式

```yml
page_nav_style: CIRCLE  # CIRCLE|ROUND_RECT
```

设置面包屑

```yml
breadcrumb:
  isShow: true # true|fase
```

设置盒子
你可设置盒子是否显示和其显示的文字

```yml
toolbox:
  isShow: true # true|fase
  text: 盒子
搜索页面 Slogan
search_slogan:
  isShow: true # true|fase
  text: Can you find the bug of world ~
友链页面 Slogan
link_slogan:
  isShow: true # true|fase
  text: 交换友链可以邮件 forsigner@gmail.com
```

设置文章标题对齐方式

```yml
post:
  header_align: center # left|center

```

启用页面
你可以启用你想要的页面，在开启关于、友链、项目的页面后，你可以对这些设置这些页面的内容

新建分类、搜索、关于界面：

```bash
hexo new page categories
//进入source/categories/文件夹
//编辑index.md文件,添加type或layout属性
---
title: categories
layout: categories
或
type: categories
---
```

启用分类页面
在博客根目录执行 `hexo new page category`
修改my-blog/source/category/index.md里面的内容:

```txt
---
title: category
layout: category
comments: false
---
```

启用标签页面
在博客根目录执行`hexo new page tag`
修改my-blog/source/tag/index.md里面的内容:

```txt
---
title: tag
layout: tag
comments: false
---
```

启用友链页面

在博客根目录执行`hexo new page link`
修改my-blog/source/link/index.md里面的内容:

```txt
---
title: link
layout: link
comments: false
---
```

启用友链页面后，可以设置类似以下格式的内容

```yml
link:
  - name: 织网
    info: 身体和灵魂，总有一个在路上
    url: http://zheng-ji.info/
    avatar: https://avatars3.githubusercontent.com/u/1414745?v=3&s=460
  - name: Dongyado
    info: 生命不止，折腾不息
    url: http://dongyado.com/
    avatar: https://avatars0.githubusercontent.com/u/6274940?v=3&s=460
  - name: OrangeCoder
    info: android ffmpeg nodejs gradle
    url: http://orangecoder.com/
    avatar: https://avatars0.githubusercontent.com/u/2263785?v=3&s=460
  - name: EverET
    info: 好记性不如烂笔头
    url: http://everet.org/about-me/
    avatar: https://avatars1.githubusercontent.com/u/1559563?v=3&s=460
```

启用关于页面

在博客根目录执行 `hexo new page about`
修改my-blog/source/about/index.md里面的内容:

```txt
---
title: about
layout: about
comments: false
---
```

启用关于页面后，可以设置类似以下格式的内容:

```yml
about:
  - type: me
    icon: icon-user
    text_value:
    - "Scut，1991，Spring."
    - "喜欢设计，擅长编程，喜欢睡懒觉."
    - "前端开发工程师，常用 HTML / CSS / JavaScript."
  - type: Github
    icon: icon-github
    text_key: Github
    text_value: "@forsigner"
    text_value_url: https://github.com/forsigner
  - type: weibo
    icon: icon-weibo
    text_key: 微博
    text_value: "@forsigner"
    text_value_url: http://weibo.com/u/1847075964
  - type: mail
    icon: icon-mail
    text_key: Gmail
    text_value: "forsigner@gmail.com"
  - type: location
    icon: icon-location
    text_value: 珠海
```

启用项目页面

在博客根目录执行 `hexo new page project`
修改my-blog/source/project/index.md里面的内容:

```txt
---
title: project
layout: project
comments: false
---
```

启用项目页面后，可以设置类似以下格式的内容

```yml
project:
  - type: personal
    name: fexo
    url: https://github.com/forsigner/fexo
    intro: A minimalist design theme for hexo
  # - type: company
  #   name: Fexo
  #   url: https://github.com/forsigner/fexo
  #   intro: A minimalist design theme for hexo
  - type: personal
    name: beside
    url: https://github.com/forsigner/beside
    intro: I need you beside me
  - type: personal
    name: web-fontmin
    url: https://github.com/forsigner/web-fontmin
    intro: 字体子集化，在线提取你需要的字体
  - type: personal
    name: magic-check
    url: https://github.com/forsigner/magic-check
    intro: Beautify Radio and Checkbox with pure CSS
  - type: personal
    name: nice-bar
    url: https://github.com/forsigner/nice-bar
    intro: A nice and lightweight scrollbar
  - type: personal
    name: angular-nice-bar
    url: https://github.com/forsigner/angular-nice-bar
    intro: A nice and lightweight scrollbar in Angular
```

启用搜索页面

在博客根目录执行 `hexo new page search`
修改my-blog/source/search/index.md里面的内容:

```txt
---
title: search
layout: search
comments: false
---
```

然后安装 Hexo 插件 `hexo-search` (重要)
先进入 blog 的根目录

```bash
 cd my-blog
 npm install hexo-search --save
```

个性化设置

自定义CSS

也许 Fexo 默认的样式满足不了你个性化的需求，使用此配置你可以在不修改 Fexo 源码的情况下，任意的自定义 Fexo 的样式，方法如下：

在 blog 根目录新建文件夹 my-blog/source/css
然后在此目录新建一个 CSS，名字随意，如 personal-style.css
修改fexo/_config.yml下面配置，然后你就可以写你想要的样式了
personal_style: /css/personal-style.css

```css
# 如果不想启用自定义样式，注释这行就可以了
比如我的个人自定义样式如下：

@font-face {
  font-family: "Meiryo";
  src: url("/fonts/Meiryo.eot");
  /* IE9 */
  src: url("/fonts/Meiryo.eot?#iefix") format("embedded-opentype"), /* IE6-IE8 */
  url("/fonts/Meiryo.woff") format("woff"), /* chrome, firefox */
  url("/fonts/Meiryo.ttf") format("truetype"), /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
  url("/fonts/Meiryo.svg#Meiryo") format("svg");
  /* iOS 4.1- */
  font-style: normal;
  font-weight: normal;
}
html.page-home {
  /*background-image: url('/images/bg.jpg')*/

  /*background: linear-gradient( #1abc9c, transparent), linear-gradient( 90deg, skyblue, transparent), linear-gradient( -90deg, coral, transparent);*/
  /*background-blend-mode: screen;*/

  /*background: linear-gradient(to left, #5f2c82, #49a09d);*/
}
```

自定义博客名的字体

由于中文字体文件太大，有的快10M，所以 Fexo 没有引入中文字体，导致博客名有点难看。
但是可以通过提取字体来减小字体文件大小，让字体只有几KB。
一下步骤可以让你实现自定义博客名字体，包括英文和中文：

```txt
下载免费可用的ttf格式字体
利用 Web-Fontmin 提取字体，然后下载 Web 字体和样式
在博客根目录的source文件夹新建目录 fonts
把下载的 web-fontmin 里的 CSS 内容 copy 到你的 personal-style.css 里去
修改fexo/_config.yml下面配置，设置字体名称：
blog_name_font_familiy: myFontName

# 注意: 这是css文件里的font-familiy的值 ,例如里面是 font-familiy: "myfontName"
PS：自定义博客名字体前请先自定义CSS
```

为首页设置背景

如果你不喜欢首页简洁的白色，想个性化一点，你可以自定义首页的背景颜色或者图片

修改personal-style.css:

```css
html.page-home {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: url('/images/bg.jpg');
  background-color: transparent;
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;

  /*background: linear-gradient( #1abc9c, transparent), linear-gradient( 90deg, skyblue, transparent), linear-gradient( -90deg, coral, transparent);*/
  /*background-blend-mode: screen;*/

  /*background: linear-gradient(to left, #5f2c82, #49a09d);*/
}
```

第三方服务

启用统计

```yml
google_analytics:
baidu_analytics: 57e94d016sfsf1fba3xxxx8a2b0263af0
```

启用评论

```yml
disqus_shortname: forsigner
# duoshuo_shortname: forsigner
```

使用 Mathjax

要使用 Mathjax，可以通过 Hexo 插件 hexo-renderer-mathjax支持

查看 hexo-renderer-mathjax 文档

### 常见问题

```bash
$ hexo d
ERROR Deployer not found: git
```

解决办法：
`npm install --save hexo-deployer-git`
