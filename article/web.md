---
title: web前端开发技术
date: 2021-02-31 16:32:11
categories:
  - web
---

## web 前端开发技术的优化策略

网络页面的核心价值在于传递消费者所需要的信息并且以此来满足消费者相应的需求。而对于消费者和体验用户来说，网页加载的快慢直接影响了这个网页的访问量。如果用户在等待网站信息呈现的事件过长，很大程度上会对其消费体验造成一定影响，会造成用户无法将信息进行高效利用。基于这样的考虑，对 web 前端技术进行优化并体现信息的真正价值具有重要的意义。web 前端技术的优化对网络性能的改善、工作效率的提高都发挥着重要作用。可以从以下几个方面开展工作，对 web 前端技术进行优化：

1. 减少 HTTP 请求数
   减少不必要的消耗时间的 Http 请求数，是 web 前端开发技术的一个重要方面。一个完整的 Http 包括 DNS 寻址、发送双方的数据、建立浏览器和服务器间的连接并传输数据、等待服务器响应等等多个环节。在这个过程中，每一个 Http 请求都必须携带数据，因此每一个请求都不免占用宽带资源，导致用户等待时间增加。要减少网页中 Http 的请求，经常被采用的方法有：将多个 CSS 文件或者 JavaScript 文件合并成一个；优化图片地图，在一个图像上划分不同区域以及注入不通过映射的链接；保持图片和文本一起显示和下载；通过利用 CSS background 进行背景图的绝对定位，这种做法可以提高网页的载入速度，提高用户的体验。

2. 优化文件的规模
   文件规模能对网页加载造成一定的影响 ，这个过程包括压缩 JavaScript 和 CSS 文件以及对相应的代码进行优化。优化代码的工作主要是删除一些不必要的 Html 标签，避免内联式和优化 CSS 代码。

3. 减少 DNS 查找
   DNS 查找的时间开销很大，这是国内许多网站的通病。在一次 DNS 的解析过程中会耗费 20~120ms，若 DNS 解析请求过于频繁，就会导致用户等待时间的增加，同时也会使信息传输的质量有所下降。并且在 DNS 查找结束前，浏览器将无法下载该域名下的任何东西。对于国内的站点来说，过多使用站外的 Widget（微件），也易引起 DNS 查找过多的问题。适当减少 DNS 查找能有效地提高网页地加载速度，对于 web 前端开发技术来说，DNS 查找的优化也非常重要。

4. 杜绝无用响应
   在用户访问网站的时候，常常会遇到无用响应，如 404 错误或拒绝访问错误，这是没有找到文件引起的。Http 请求耗费时间过长，而较长的等待时间却得到一个无用的响应页面，大大降低了用户体验。对页面的链接进行充分的测试加上对 web 服务器 Errorr 日志的不断跟踪能有效地减少 404 错误。大多时候，这种错误由于定位稍难而容易被技术人员忽略。

5. 有效避免重定向
   网页的重定向会耗费一定的时间，这也是造成用户等待时间过长的一个因素。重定向问题的产生有很多，每增加一次重定向就必然会增加一次对 web 的请求，所以重定向问题应该尽量避免。这要求技术人员能对 web 站点的子目录后加一个"/"，这种做法能有效避免重定向。

6. 优化网页内容
   CSS 的全称是层叠样式表，层叠表明后面的 CSS 可以覆盖前面的，高级的可以覆盖低级的，因此浏览器会在完全加载完后才考虑开展渲染工作。根据 CSS 这样的特性，要实现 web 的优化，可以考虑将样式表放在顶部。另外，可以将 script 放在底部，该举措是为了防止 script 脚本阻塞当前页面，从而造成下载速度较慢，页面的加载时间过长等问题的产生。

## Web 实时推送技术

### 一、双向通信

HTTP 协议有一个缺陷：通信只能由客户端发起。举例来说，我们想了解今天的天气，只能是客户端向服务器发出请求，服务器返回查询结果。HTTP 协议做不到服务器主动向客户端推送信息。这种单向请求的特点，注定了如果服务器有连续的状态变化，客户端要获知就非常麻烦。在 WebSocket 协议之前，有三种实现双向通信的方式：轮询（polling）、长轮询（long-polling）和 iframe 流（streaming）。

1. 轮询（polling）
   轮询是客户端和服务器之间会一直进行连接，每隔一段时间就询问一次。其缺点也很明显：连接数会很多，一个接受，一个发送。而且每次发送请求都会有 Http 的 Header，会很耗流量，也会消耗 CPU 的利用率。

- 优点：实现简单，无需做过多的更改
- 缺点：轮询的间隔过长，会导致用户不能及时接收到更新的数据；轮询的间隔过短，会导致查询请求过多，增加服务器端的负担

2. 长轮询（long-polling）

长轮询是对轮询的改进版，客户端发送 HTTP 给服务器之后，看有没有新消息，如果没有新消息，就一直等待。当有新消息的时候，才会返回给客户端。在某种程度上减小了网络带宽和 CPU 利用率等问题。由于 http 数据包的头部数据量往往很大（通常有 400 多个字节），但是真正被服务器需要的数据却很少（有时只有 10 个字节左右），这样的数据包在网络上周期性的传输，难免对网络带宽是一种浪费。

- 优点：比 Polling 做了优化，有较好的时效性
- 缺点：保持连接会消耗资源; 服务器没有返回有效数据，程序超时。

3. iframe 流（streaming）
   iframe 流方式是在页面中插入一个隐藏的 iframe，利用其 src 属性在服务器和客户端之间创建一条长连接，服务器向 iframe 传输数据（通常是 HTML，内有负责插入信息的 javascript），来实时更新页面。

- 优点：消息能够实时到达；浏览器兼容好
- 缺点：服务器维护一个长连接会增加开销；IE、chrome、Firefox 会显示加载没有完成，图标会不停旋转。

### 二、WebSocket

1. 什么是 websocket
   WebSocket 是一种全新的协议，随着 HTML5 草案的不断完善，越来越多的现代浏览器开始全面支持 WebSocket 技术了，它将 TCP 的 Socket（套接字）应用在了 webpage 上，从而使通信双方建立起一个保持在活动状态连接通道。

一旦 Web 服务器与客户端之间建立起 WebSocket 协议的通信连接，之后所有的通信都依靠这个专用协议进行。通信过程中可互相发送 JSON、XML、HTML 或图片等任意格式的数据。由于是建立在 HTTP 基础上的协议，因此连接的发起方仍是客户端，而一旦确立 WebSocket 通信连接，不论服务器还是客户端，任意一方都可直接向对方发送报文。

初次接触 WebSocket 的人，都会问同样的问题：我们已经有了 HTTP 协议，为什么还需要另一个协议？

2. HTTP 的局限性
   HTTP 是半双工协议，也就是说，在同一时刻数据只能单向流动，客户端向服务器发送请求(单向的)，然后服务器响应请求(单向的)。
   服务器不能主动推送数据给浏览器。这就会导致一些高级功能难以实现，诸如聊天室场景就没法实现。
3. WebSocket 的特点
   支持双向通信，实时性更强
   可以发送文本，也可以发送二进制数据
   减少通信量：只要建立起 WebSocket 连接，就希望一直保持连接状态。和 HTTP 相比，不但每次连接时的总开销减少，而且由于 WebSocket 的首部信息很小，通信量也相应减少了相对于传统的 HTTP 每次请求-应答都需要客户端与服务端建立连接的模式，WebSocket 是类似 Socket 的 TCP 长连接的通讯模式，一旦 WebSocket 连接建立后，后续数据都以帧序列的形式传输。在客户端断开 WebSocket 连接或 Server 端断掉连接前，不需要客户端和服务端重新发起连接请求。在海量并发和客户端与服务器交互负载流量大的情况下，极大的节省了网络带宽资源的消耗，有明显的性能优势，且客户端发送和接受消息是在同一个持久连接上发起，实时性优势明显。

### 三、Web 实时推送技术的比较

方式类型技术实现优点缺点适用场景

- 轮询 Pollingclient→server 客户端循环请求
  1、实现简单
  2、 支持跨域
  1、浪费带宽和服务器资源
  2、 一次请求信息大半是无用（完整 http 头信息）
  3、有延迟
  4、大部分无效请求适于小型应用
- 长轮询 Long-Pollingclient→server 服务器 hold 住连接，一直到有数据或者超时才返回，减少重复请求次数
  1、实现简单
  2、不会频繁发请求
  3、节省流量
  4、延迟低
  1、服务器 hold 住连接，会消耗资源
  2、一次请求信息大半是无用 WebQQ、Hi 网页版、Facebook
- IM 长连接 iframeclient→server 在页面里嵌入一个隐蔵 iframe，将这个 iframe 的 src 属性设为对一个长连接的请求，服务器端就能源源不断地往客户端输入数据。
  1、数据实时送达
  2、不发无用请求，一次链接，多次“推送”
  1、服务器增加开销
  2、无法准确知道连接状态
  3、IE、chrome 等一直会处于 loading 状态 Gmail 聊天
- WebSocketserver⇌clientnew WebSocket()
  1、支持双向通信，实时性更强
  2、可发送二进制文件
  3、减少通信量
  1、浏览器支持程度不一致
  2、不支持断开重连网络游戏、银行交互和支付

## 缓存

> 首次请求后保存一份请求资源的响应副本，当用户再次发起相同请求后，如果判断缓存命中则拦截请求，将之前存储的响应副本返回给用户，避免重新向服务器发起资源请求。
> 减少冗余数据传输，节省网络流量，减轻服务器压力，加快访问速度

### 强制缓存

不会向服务器发请求，而是直接从缓存中读取资源，在控制台中返回`200`状态码

响应头参数

- `pragma`:`no-cache`;兼容 http 1.0
- `Expires`:兼容 http 1.0,响应头里的过期时间，若在这个时间内，则命中强缓存；（以分钟为单位，在服务端配置，当客户端和服务端时间不一致的时候会出现问题，优先级比`Cache-Control`低）
- `Cache-Control`:只兼容 http 1.1。是在服务器端配置的,
  - 当值为`max-age=300`时（以秒为单位），则代表这个请求正确返回时间的五分钟内再次加载资源，就会命中强缓存。等于 0 则不使用缓存
  - 当为-`no-cache`时，表示不使用强缓存，需要使用协商缓存，先与服务器确认返回的响应是否被更改，如果之前的响应中存在 Etag，那么请求时会与服务端验证，如果资源未被更改，则可避免重复下载。
  - 当为-`no-store`时，表示直接禁止浏览器缓存数据。
  - 当为-`public`时，表示被所有的用户缓存，包括终端用户和 cdn 等中间代理服务器。
  - 当为-`private`时，表示只能被终端用户缓存，不允许 cdn 等中继服务器对其缓存。

### 协商缓存

在使用本地缓存前，需向服务器发送请求，服务器根据请求头的参数判断是否命中协商缓存，若命中，则返回`304`状态码并带上新的响应头通知浏览器从缓存中读取数据。协商缓存可以解决本地资源不更新的问题。

需设置`Cache-Control`为`no-cache`

- `Last-Modify`:浏览器第一次请求一个资源时，服务器返回的响应头会加上`Last-Modify`，其为一个时间标识，记录该资源最后修改的时间.
- `If-Modify-Since`:当浏览器再次请求同一个资源时，请求头会包含`If-Modify-Sine`，值为缓存之前返回的`Last—Modify`。服务器收到后根据当前资源修改的 时间判断是否命中缓存

- `Etag`:浏览器第一次请求一个资源时，服务器返回当前资源在服务器的唯一标识
- `If-None-Match`:再次请求时，若发现资源有 Etag 声明，则请求头会带上`If-None-Match`,值为`Etag`的值，服务器收到后进行比较来判断是否命中缓存。

区别

- 优先级上，`etag/if-none-match`更高
- 精度上`etag`要优于`last-modify`，`last-modify`单位为 s，使用它无法区分一秒内修改多次的情况。
- 性能上`etag`要逊于`last-modify`，`etag`需要服务器额外性能计算 hash 值。

> ctrl+r/f5/地址栏刷新：强缓存和协商缓存都生效
> ctrl+f5/ctrl+shift+r:全部失效，会重新请求资源并缓存
> post 请求无法被缓存

静态页面可通过配置 meta 标签设置缓存属性

```html
<meta http-equiv="Cache-Control" content="max-age=3000" />
```

## 从输入 url 到页面完成加载发生了什么

- DNS 解析：将域名解析成 IP 地址
  DNS(domain name system，域名系统)：因特网上域名和 IP 地址相互映射的分布式数据库；简单理解就是域名与 IP 地址的对照表
  URL(Uniform Resource Locator)，统一资源定位符，用于定位互联网上资源，俗称网址。
  遵守以下的语法规则：
  `scheme://host.domain:port/path/filename`

- TCP 连接：TCP 三次握手
  第一次握手，由浏览器发起，告诉服务器我要发送请求了；
  第二次握手，由服务器发起，告诉浏览器我准备接受了，你赶紧发送吧。
  第三次握手，由浏览器发送，告诉服务器，我马上就发了，准备接受吧。
  “三次握手”的目的是“为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误”。
- 发送 HTTP 请求

HTTP 请求报文格式：`请求行`+`请求头`+`空行`+`消息体`，请求行包括请求方式(GET/POST/DELETE/PUT)、请求资源路径(URL)、HTTP 版本号；

- 服务器处理请求并返回 HTTP 报文

服务器收到请求后会发出应答，即响应数据。HTTP 响应与 HTTP 请求相似， HTTP 响应报文格式：`状态行`+`响应头`+`空行`+`消息体`，状态行包括 HTTP 版本号、状态码、状态说明。

- 浏览器解析渲染页面
  浏览器拿到响应文本后，解析 HTML 代码，请求 js，css 等资源，最后进行页面渲染，呈现给用户。页面渲染一般分为以下几个步骤：

(1)根据 HTML 文件解析出 DOM Tree

(2)根据 CSS 解析出 CSSOM Tree(CSS 规则树)

(3)将 DOM Tree 和 CSSOM Tree 合并，构建 Render tree(渲染树)

(4)reflow(重排)：根据 Render tree 进行节点信息计算(Layout)

(5)repaint(重绘)：根据计算好的信息绘制整个页面(Painting)

- 连接结束：TCP 四次挥手

```txt
1、A——>B ：A告诉B：“我发完了”；

2、B——>A：B告诉A：“好的，我知道你发完了”

3、B——>A：B告诉A：“我收完了”；

4、A——>B：A告诉B：“好的，我知道你发收完了”
```

## 重绘 和 重排（回流）

> 重绘不一定导致重排，但重排一定会导致重绘
> 重排（Reflow） && 重绘（Redraw）会付出高昂的性能代价

### 重绘

重绘 （Redraw）：某些元素的外观被改变所触发的浏览器行为（重新计算节点在屏幕中的绝对位置并渲染的过程）； 例如：修改元素的填充颜色，会触发重绘；

下面情况会发生重绘：

- color
- border-style
- border-radius
- text-decoration
- box-shadow
- outline
- background
- ...

### 重排（回流）

重排 （Reflow）：重新生成布局，重新排列元素（重新计算各节点和 css 具体的大小和位置：渲染树需要重新计算所有受影响的节点）；例如：改元素的宽高，会触发重排；
通过两者概念区别明显得知，重排要比重绘的成本大得多，我们应该尽量减少重排操作，减少页面性能消耗

- 页面初始渲染，这是开销最大的一次重排;
- 添加/删除可见的 DOM 元素;
- 改变`元素位置`;
- 改变`元素尺寸`，比如边距、填充、边框、宽度和高度等;
- 改变`元素内容`，比如文字数量，图片大小等;
- 改变元素`字体大小`;
- 改变浏览器窗口尺寸，比如 resize 事件发生时;
- 激活`CSS伪类`（例如：:hover）;
- 设置 `style 属性`的值，因为通过设置 style 属性改变结点样式的话，每一次设置都会触发一次 reflow;
- 查询某些属性或调用某些计算方法：`offsetWidth`、`offsetHeigh`t 等，除此之外，当我们调用 getComputedStyl 方法，或者 IE 里的 currentStyle 时，也会触发重排，原理是一样的，都为求一个“即时性”和“准确性”;

```txt
width height margin padding
display border-width border position
overflow font-size vertical-align min-height
clientWidth clientHeight clientTop clientLeft
offsetWidth offsetHeight offsetTop offsetLeft
scrollWidth scrollHeight scrollTop scrollLeft
scrollIntoView() scrollTo() getComputedStyle()
getBoundingClientRect() scrollIntoViewIfNeeded()
```

### 减少重排次数

- 样式集中改变
  不要一条一条地修改 DOM 的样式。可以先定义好 css 的 class，然后修改 DOM 的 className。
- 分离读写操作
  DOM 的多个读操作（或多个写操作），应该放在一起。不要两个读操作之间，加入一个写操作
- 将 DOM 离线
  使用 display:none

- 使用 absolute 或 fixed 脱离文档流
  为动画的 HTML 元件使用 fixed 或 absoult 的 position，那么修改他们的 CSS 是不会 reflow 的。

## script 标签中 defer 和 async 的区别

多个带 async 属性的标签，不能保证加载的顺序；多个带 defer 属性的标签，按照加载顺序执行;

- async 属性，表示后续文档的加载和执行与 js 脚本的加载和执行是并行进行的，即异步执行；
- defer 属性，加载后续文档的过程和 js 脚本的加载(此时仅加载不执行)是并行进行的(异步)，js 脚本需要等到文档所有元素解析完成之后才执行，DOMContentLoaded 事件触发执行之前。

## 前端路由

前端路由是指在浏览器端控制页面内容切换显示的机制。在没有服务器端参与的情况下，前端路由可以根据 URL 的变化，对应展现不同的内容，实现页面的“伪”跳转。在现代前端开发中，单页面应用（SPA）已成为一种常见的架构风格。与传统的多页面应用（MPA）相比，SPA 能够提供更加流畅的用户体验，因为它无需重新加载整个页面即可更新部分页面内容。

### 路由的基本模式

- Hash 模式原理：
  - 浏览器原生支持通过 window.location.hash 读写 URL 中的 hash 值，并且当 hash 值变化时，页面不会触发重新加载。
  - SPA 可以监听 hashchange 事件，在 URL 的 hash 部分变化时根据定义好的路由映射关系来动态渲染内容。

```js
// Hash模式的简易实现
window.addEventListener("hashchange", routeChange);
function routeChange() {
  const hash = window.location.hash.slice(1); // Remove the '#' symbol
  // 基于hash值显示不同内容
  routerView.innerHTML = routes[hash] ? routes[hash] : routes["404"];
}
```

- History 模式原理：
  - History API 允许 SPA 在浏览历史记录中添加、修改记录而不会触发页面加载。
  - 通过 history.pushState 和 history.replaceState 可以改变 URL 且不重新加载页面。
  - SPA 可以监听 popstate 事件来响应浏览器前进、后退操作。

```js
// History模式的简易实现
window.addEventListener("popstate", routeChange);
function navigate(path) {
  history.pushState({}, "", path);
  routeChange();
}
function routeChange() {
  const path = window.location.pathname;
  // 根据pathname来渲染不同的页面组件
  routerView.innerHTML = routes[path] ? routes[path] : routes["404"];
}

// navigate('/user'); // 导航至用户页面
```

### 前端路由的实现

在实际项目中，前端路由通常借助专业的路由库（如 react-router、vue-router 等）来实现。而且，需要考虑到如下两个常见问题的解决方案：

- 路由懒加载：

为了加快首次页面加载速度，可以将不同路由对应的组件分割成独立的代码块后懒加载，仅当路由被访问时才加载对应组件。

- 路由的保护与权限控制：

前端路由守卫提供了在路由跳转执行前后插入逻辑的能力，可用于权限验证、数据预加载、动画过渡等。

1. 使用路由导航钩子控制跳转流程，如进行权限校验。
2. 结合状态管理和路由，在状态改变时同步 URL 变化，确保用户随时刷新页面或分享链接都能获得一致的界面状态。
3. 利用`<link rel="prefetch">`或`import(/* webpackPrefetch: true */ './path/to/Component')`进行资源预获取，提高路由跳转的性能。

## 零宽度字符

### 什么是零宽度字符

1、零宽度字符是隐藏不显示的，也是不可打印的，也就是说这种字符用大多数程序或编辑器是看不到的。

最常见的是零宽度空格，它是 Unicode 字符空格，就像如果在两个字母间加一个零宽度空格，该空格是不可见的，表面上两个字母还是挨在一起的。比如这两个 (​​​​​) 括号中间我放了 5 个零宽字符，你们能看见吗?

这种字符的出现是为了文字控制排版作用的，但是由于它拥有肉眼无法观察到的特性，零宽度字符可作为识别某些用户身份的“指纹”数据，也可非常方便地追溯到某些秘密数据的泄露源。

2、下面介绍三种零宽字符

（1）不换行空格，全称 No-Break Space，它是最常见和我们使用最多的空格，大多数的人可能这个字符叫做 Zero Width Space，中文可称为“零宽空白”，这个字符在主流文本编辑器中均没有任何显示效果，就像一只看不见、摸不着的幽灵。拷贝也会带上零宽空白，HTML 字符值引用为： `&#8203;`

（2）零宽不连字：不换行空格，全称 No-Break Space，它是最常见和我们使用最多的空格，大多数的人可能它叫零宽不连字，全称是 Zero Width Non Joiner，简称“ZWNJ”，是一个不打印字符，放在电子文本的两个字符之间，抑制本来会发生的连字，而是以这两个字符原本的字形来绘制。Unicode 中的零宽不连字字符映射为（zero width non-joiner，U+200C），HTML 字符值引用为： `&zwj;`或`&#8204;`

（3）零宽连字，全称是 Zero Width Joiner，简称“ZWJ”，是一个不打印字符，放在某些需要复杂排版语言（如阿拉伯语、印地语）的两个字符之间，使得这两个本不会发生连字的字符产生了连字效果。

零宽连字符的 Unicode 码位是 U+200D，HTML 字符值引用为： `&zwnj;`或`&#8205;`

### 零宽度字符能做什么？

1、数据防爬

将零宽度字符插入文本中，干扰关键字匹配。爬虫得到的带有零宽度字符的数据会影响他们的分析，但不会影响用户的阅读数据。

2、信息传递

将自定义组合的零宽度字符插入文本中，用户复制后会携带不可见信息，达到传递作用。

3、传递隐密信息

利用零宽度字符不可见的特性，我们可以用零宽度字符在任何未对零宽度字符做过滤的网页内插入不可见的隐形文本。下面是一个简单的利用零宽度字符对文本进行加密/解密的例子：

```js
// 使用零宽度字符加密解密
// str -> 零宽字符
function strToZeroWidth(str) {
  return str
    .split("")
    .map((char) => char.charCodeAt(0).toString(2)) // 1 0 空格
    .join(" ")
    .split("")
    .map((binaryNum) => {
      if (binaryNum === "1") {
        return "​"; // ​
      } else if (binaryNum === "0") {
        return "‌"; // ‌
      } else {
        return "‍"; // ‍
      }
    })
    .join(""); // ‎
}

// 零宽字符 -> str
function zeroWidthToStr(zeroWidthStr) {
  return zeroWidthStr
    .split("") // ‎
    .map((char) => {
      if (char === "​") {
        // ​
        return "1";
      } else if (char === "‌") {
        // ‌
        return "0";
      } else {
        // ‍
        return " ";
      }
    })
    .join("")
    .split(" ")
    .map((binaryNum) => String.fromCharCode(parseInt(binaryNum, 2)))
    .join("");
}
```

```js
//1、加密
// 为了代码的简洁与易读性，以下代码会忽略性能方面考量
const text = "123😀";

// Array.from 能让我们正确读取宽度为2的Unicode字符，例：😀
const textArray = Array.from(text);

// 用codePointAt读取所有字符的十进制Unicode码
// 用toString将十进制Unicode码转化成二进制（除了二进制，我们也可以使用更大的进制来缩短加密后的信息长度，以此提升效率）
const binarify = textArray.map((c) => c.codePointAt(0).toString(2));

// 此时binarify中的值是 ["110001", "110010", "110011", "11111011000000000"]，下一步我们需要将"1"，"0"和分隔符映射到响应的零宽度字符上去

// 我们用零宽度连字符来代表1，零宽度断字符来代表0，零宽度空格符来代表分隔符
// 下面的''看上去像是空字符串，但其实都是长度为1，包含零宽度字符的字符串
const encoded = binarify
  .map((c) =>
    Array.from(c)
      .map((b) => (b === "1" ? "‍" : "‌"))
      .join("")
  )
  .join("​");

// 此时encoded中包含的就是一串不可见的加密文本了
```

```js
2、解密
// 接着上面的encoded
// 用分隔符（零宽度空格符）提取加密文本中的字符
const split = encoded.split('​');

// 将文本转回成二进制数组
const binary = split.map(c => Array.from(c).map(z => z === '‍' ? '1' : '0').join(''));

// 此时binary中的值再次回到开始的 ["110001", "110010", "110011", "11111011000000000"]

// 最后一部只需要将二进制文本转回十进制，再使用 String.fromCodePoint 就可以得到原文本了
const decoded = binary.map(b => String.fromCodePoint(parseInt(b, 2))).join('');

// 此时decoded中的值即是 "123😀"

```

4、隐形水印

通过零宽度字符我们可以对内部文件添加隐形水印。在浏览者登录页面对内部文件进行浏览时，我们可以在文件的各处插入使用零宽度字符加密的浏览者信息，如果浏览者又恰好使用复制粘贴的方式在公共媒体上匿名分享了这个文件，我们就能通过嵌入在文件中的隐形水印轻松找到分享者了。

5、加密信息分享

通过零宽度字符我们可以在任何网站上分享任何信息。敏感信息的审核与过滤在当今的互联网社区中扮演着至关重要的角色，但是零宽度字符却能如入无人之境一般轻松地穿透这两层信息分享的屏障。对比明文哈希表加密信息的方式，零宽度字符加密在网上的隐蔽性可以说是达到了一个新的高度。仅仅需要一个简单的识别/解密零宽度字符的浏览器插件，任何网站都可以成为信息分享的游乐场。

6、逃脱敏感词过滤

通过零宽度字符我们可以轻松逃脱敏感词过滤。敏感词自动过滤是维持互联网社区秩序的一项重要工具，只需倒入敏感词库和匹配相应敏感词，即可将大量的非法词汇拒之门外。使用谐音与拼音来逃脱敏感词过滤会让语言传递信息的效率降低，而使用零宽度字符可以在逃脱敏感词过滤的同时将词义原封不动地传达给接受者，大大提高信息传播者与接受者之间交流的效率。

### Emoji

#### 合成行为

- 可以额外在人物的 Emoji 上修饰肤色。语法格式是<emoji>\ud83c[\udffb-\udfff]，即 U+D83C 后面跟不同的几个值表示不同的肤色控制。肤色总共有 5 中，用\udffb-\udfff 表示。

```js
let colors = ["\udffb", "\udffc", "\udffd", "\udffe", "\udfff"];
let defaultBoy = " ";
colors.map((color) => defaultBoy + "\ud83c" + color);
// ["  ", "  ", "  ", "  ", "  "]
```

- 可以将多个 Emoji 合成为一个 Emoji, 如 ‍ ‍ 实际上是由一个 ，一个 ，一个 合成的。具体语法为`<Emoji> U+200D <Emoji> U+200D <Emoji> ` 。U+200D 表示将前后的 Emoji 合成的意思。

```js
const man = ' ',women = ' ', daughter=' ', son = ' '
[man, women, son].join('\u200d') === " ‍ ‍ "
[man, women, daughter, son].join('\u200d')
" ‍ ‍ ‍ "

' ‍ ‍ '.length // 8, 每个基础Emoji长度为2，每个连接符长度为1，所以总长为 8
[...' ‍ ‍ '] // [" ", "‍", " ", "‍", " "] // 中间的空字符串其实就是 U+200D
" \u200D \u200D " // " ‍ ‍ "
```

- 键帽 Emoji: 0-9\*#, 是由 0-9 的 unicode 加上一个 Emoji 修饰符，加一个键帽修饰符表示的。

```js
[0, 1, 2, 3, 4, 5].map((num) => num + "\ufe0f\u20e3");
```

### 正确地统计用户输入长度

- 使用字符串的 Iterator 统计长度。
  字符串的迭代器能正确识别代理对的情况,但对于长度超过 2 的 Emoji,还是无法识别，但这是最简单，能解决大部分场景的方案。

```js
const testStr = "123 ";

for (let c of testStr) {
  console.log(c);
}
// 1
// 2
// 3
//
console.log([...testStr].length);
// 4
```

- 增加对合成 Emoji 情况的识别。

合成 Emoji 只有以下几种类型:肤色，键帽，使用\u200D 拼接多个 Emoji 为一个。

方案 1：先解构再截取，最后再拼接：

```js
const article = "123456789 ";
const subject = [...article].slice(0, 10).join(""); // '123456789 '
```

方案 2：考虑 emoji 的情况，根据合成 Emoji 的特定语法一点点识别

## lottie

> Lottie 是一个面向 Android、iOS、Web 和 Windows 的库，它解析导出为带有 bodymovin 的 json 的 AE（Adobe After Effects）动画，并在移动和 Web 上以本地方式呈现这些动画。

### web 使用

#### 安装

npm:`https://www.npmjs.com/package/lottie-web`
首先是根据需求引用 lottie 库，官方给出的下载地址：https://cdnjs.com/libraries/bodymovin

```html
<script>
  <div id="app"><!-- 动画容器 --></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.5.6/lottie.min.js"></script>
var animation = bodymovin.loadAnimation({
  container: document.getElementById('app'), 	// 渲染动画的容器元素，必须填
  // animationData	动画数据（和path参数二选一）
  path: 'assets/lottie-1.json', // 动画数据json文件的路径（和animationData参数二选一），必须填
  renderer: 'canvas', 	// 渲染方式，'svg'/'canvas'/'html'（轻量版仅svg渲染），必须填
  loop: true, // 一直循环？，选填
  autoplay: true, // 自动播放？，选填
  name: "lottie-1", // 命名，选填
})
</script>
```

#### 方法

配置完动画后，我们可以拿到动画实例并通过实例方法进行控制：

- `play()`：播放动画，从当前停止帧开始
- `stop()`：终止动画，回到第 0 帧
- `pause()`：暂停动画，停止在当前帧
- `setLocationHref(href)`：href 作为 location.href，当你在 Safari 中遇到不带符号的掩码问题时，它非常有用。
- `setSpeed(speed)`：设置动画速度（1 是正常速度）。speed 参数为速度，number。
- `goToAndStop(value, isFrame)`：动画跨越到某进度并停止。value 为进度数值,number；isFrame 定义第一个参数（value）是基于时间的值还是基于帧的（默认为 false）。
- `setDirection(direction)`：设置动画播放顺序（顺序播放/倒叙播放）。direction 参数表明顺序，1 为顺序，-1 为倒叙。
- `playSegments(segments, forceFlag)`：播放动画片段。segments 参数可以包含两个将用作动画第一帧和最后一帧的数值，或者可以包含一系列数组，每个数组都有 2 个数值，array。forceFlag 表示是否立即强制播放该片段，如果设置为 false，它将等待当前段完成。如果为 true，它将立即播放此片段，boolean。如 `animation.playSegments([10,20], false);` // 播放完之前的片段，播放 10-20 帧、`animation.playSegments([[0,5],[10,18]], true);` // 直接播放 0-5 帧和 10-18 帧
- `setSubframe(useSubFrames)`：useSubFrames 参数如果为 false，则将尊重原始的 AE fps。如果为 true，它将使用中间值在每个 RequestAnimationFrame 上更新。默认值为 true, boolean。
- `destory()`：删除该动画，移除相应的元素标签等。在 unmount 的时候，需要调用该方法
- `getDuration(inFrames)`，获取动画持续时间。inFrames 参数为 true，则返回以帧为单位的持续时间；如果为 false，则返回以秒为单位的持续时间。

#### 事件

在使用中可能也需要监听一些事件：

`complete`: 播放完成（循环播放下不会触发）
`loopComplete`: 当前循环下播放（循环播放/非循环播放）结束时触发
`enterFrame`: 每进入一帧就会触发，播放时每一帧都会触发一次，stop 方法也会触发
`segmentStart`: 播放指定片段时触发，playSegments、resetSegments 等方法刚开始播放指定片段时会发出，如果 playSegments 播放多个片段，多个片段最开始都会触发。
`data_ready`: 动画数据 json 文件加载完毕触发
`data_fail`：动画数据 json 文件加载失败触发
`loaded_images`：当所有图片加载成功/失败时触发
`DOMLoaded`: 动画相关的 dom 已经被添加到 html 后触发
`destroy`: 将在动画删除时触发
使用如：

```js
// 上例的animation实例
animation.addEventListener("data_ready", function () {
  console.log("animation data file has loaded");
});
```

### lottie.json 关键字段

- ip：起始帧
- op：结束帧
- w：宽
- h：高
- v：版本
- fr：频率/帧率
- ddd
- nm
- tiny
- fonts：字体
  - list
    - fontConfig
- asset：资源,图片资源信息集合，这里放置的是 制作动画时引用的图片资源。
  - assetConfig
    - id
    - w：宽
    - h：高
    - u：路径
    - p：名称
    - e
- layers：图层，这里可以获取到多少图层，每个图层的开始帧 结束帧等。
  - layerConfig
    - st：开始帧
    - ip：起始帧
    - op：结束帧
    - nm：名字文本
    - ind：图层 id
    - ty：图层类型
    - ks：动画
    - shapes：元素集合，可以获取到每个图层都包含多个动画元素。

### 动态修改 Lottie 中的文本

- 修改 json 数据
  lottie.json 内部描述了动画的所有细节，自然也就包含了动画中的那段文本，如果我们能找到相应的字段进行修改，也就可以实现文本替换了

  ```js
  fetch("xxx.json")
    .then((resp) => resp.text())
    .then((text) => {
      // 简单演示替换
      const newJSON = text.replace("${days}", "3");
      lottie.loadAnimation({
        animationData: JSON.parse(newJSON),
        container: document.getElementById("app"),
        loop: true,
      });
    });
  ```

  缺点：需要设计师在 AE 中写入 ${days} 这样明确的占位符.无法做到「运行时」修改，也就是 lottie 解析播放后就无法再修改文本了，

- 修改 JS 对象

通过修改 lottie 解析后的运行时 JS 对象，理论上一样可以修改文本，官方其实提供了相应的 API，

第一个参数表示如何替换文本，t 表示文本内容，这里还可以传入 s 修改大小，fc 修改颜色
第二个参数表示替换指定关键帧的文本，文本可以有关键帧，在动画过程中展示不同的文本，如果没有关键帧可以省略这个参数

```js
anim.addEventListener("DOMLoaded", () => {
  anim.renderer.elements[0].elements[0].updateDocumentData({ t: "拾亿" }, 0);
});
```

如果设计师在 AE 图层命名时尾部加入 #xxx ，那么生成的 svg 元素就会有一个 id 属性为 xxx，
“getKeyPath”是 Lottie-web 库中的一个函数，其作用是返回符合指定名称的所有属性路径。动画文件中的属性路径是指导出文件中存在的层次结构路径，每一层级都有一个名称或标记。
渲染的 dom 节点有个 id 为 "amount" 的唯一标识，那么我们就可以通用 getkeyPath 通过这个唯一 id 获取到对象，最终通过 js 修改的方式如下：

```js
anim.addEventListener("DOMLoaded", () => {
  const api = lottie_api.createAnimationApi(anim);
  const elements = api.getKeyPath("#amount"); // 查找对象
  //  const elements = api.getKeyPath("comp1,textnode");  // 其实就是图层名称的拼接，这个 lottie 内部有两层结构，第一层是「合成」，名称叫 comp1，文本位于 comp1 的内部，名称叫做 textnode
  elements.getElements()[0].setText("1.02");
  // setText() 方法可以传入第二个入参替换指定关键帧中的文案，这与 updateDocumentData 一致，因为底层调用的就是 updateDocumentData
});
```

getkeyPath 不仅可以通过 id 标识查找到节点，还可以通过".calssName" 查找。elements.getElements() 可以获取到所有满足条件的 JS 对象，因为 lottie 中图层的名称并不唯一，有时候可能会找到多个对象就需要有一定的约定去规范这种图层命名的方式。

缺点是图层命名需要有一定的规范，同时需要了解如何找到这个图层的名称。

- 修改 svg 节点

如果设计师在 AE 图层命名时尾部加入 `#xxx`，那么生成的 svg 元素就会有一个 id 属性为 `xxx`，

```js
anim.addEventListener("DOMLoaded", () => {
  const element = document.getElementById("J_txt");
  element.querySelector("tspan").innerHTML = "拾亿";
});
```

> canvas 模式的问题
>
> > 最上面提到先忽略 canvas 模式，那么这个模式有什么问题吗？这里又涉及到 lottie 的底层实现了。由于在 canvas 模式下绘制文本是很慢的，作者考虑后放弃了 drawText 的实现，转而在导出 json 时抽取字体库中的字形（Glyphs）路径放入文件，canvas 模式下通过路径绘图来画出文本.所以上述方案中的 方案三 肯定无法支持 canvas 模式，而 方案一/二 要支持则依赖于导出 JSON 时，要替换的文本字形是否已经存在于 JSON 中，因此从实际出发，一般 canvas 模式下就比较难实现替换文本了。从 AE 导出 lottie 时都记得去掉「Glyphs」这个选项，并且指定具体的 font family，记得把这件事告诉你的设计师小伙伴

### 动态修改 Lottie 中的图片

- 修改 json 数据
  所有的图片都会被放置到 json 的 assets 数组中，并用 p 字段标识相应地址（http 或 base64）

  ```js
  const resp = await fetch(
    "https://gw.alipayobjects.com/os/sage/9d72da30-ce87-4ba7-8951-1907759320b5/data.json"
  );
  const json = await resp.json();
  // 找到对应 json 节点直接替换 p 属性
  const asset = json.assets.find((a) => a.id === "7");
  asset.p =
    "https://gw.alipayobjects.com/mdn/rms_91e1e4/afts/img/A*2mfsTo-gbDgAAAAAAAAAAABkARQnAQ";
  lottie.loadAnimation({
    container: mountNode,
    animationData: json,
  });
  ```

  - 修改 JS 对象
    相比修改文本，要修改图片略麻烦一些，主要是 lottie-web 本身并没有直接提供类似 updateDocumentData 这样的方法，不过只要知道实现原理，找到解法并不难

通过阅读 lottie-web 源码，可以发现 svg 和 canvas 模式下，图片的实现不一样（svg, canvas），所以我们的修改方案也需要判断先判断 renderer 模式

```js
anim.addEventListener('DOMLoaded', () => {
  if (anim.renderer.rendererType === 'canvas') {
    // canvas 模式下的图片替换
    anim.renderer.elements[0].elements[8].img.src = 「'https://gw.alipayobjects.com/mdn/rms_91e1e4/afts/img/A*2mfsTo-gbDgAAAAAAAAAAABkARQnAQ';
  } else {
    // svg 模式下的图片替换，前两个参数为固定值
    anim.renderer.elements[0].elements[8].innerElem.setAttributeNS(
      'http://www.w3.org/1999/xlink',
      'href',
      'https://gw.alipayobjects.com/mdn/rms_91e1e4/afts/img/A*2mfsTo-gbDgAAAAAAAAAAABkARQnAQ'
    );
  }
});
```

注意，在 canvas 模式下替换的图片需要保持与原图片尺寸一致，否则实际动画效果会有问题；svg 模式下则受益于 svg image 元素的特性不需要如此强的约束

- 修改 svg 节点

与文本同理，仅限于 svg 渲染模式，只需要设计师在图层命名时，尾部加入 #xxx 即可，这样生成的 svg 元素就会有一个 id 属性为 xxx
那么通过 DOM API 找到元素修改属性即可

```js
anim.addEventListener("DOMLoaded", () => {
  const element = document.getElementById("xxx");
  element.querySelector("image").href =
    "https://gw.alipayobjects.com/mdn/rms_91e1e4/afts/img/A*2mfsTo-gbDgAAAAAAAAAAABkARQnAQ";
});
```

动态修改 lottie 中的图片，与动态修改文本大同小异，只是 JSON 结构的属性、JS 对象的 API 有所不同，另外图片不像文本，在 canvas 模式下可以正常使用，lottie 导出时也没有特殊的要求.设计师导出是不是要选择把图片资源包含在 json 文件里，就导出一个 json 文件，而不是导出的一个 json 文件+图片资源文件夹

### 注意事项

#### 下面这些效果，lottie 暂不支持：

- 描边动效目前不支持，（会导致 lottie 性能问题，所以后来 lottie 去掉了对该属性的支持）。
- merge Path(合并路径)Android 只支持 4.4 以上，iOS 都不支持.

#### 遵循下面的方案，会使 json 文件减小：

- 尽量减少图层个数。每个图层都会导出成相应的 json 数据，图层减少能从很大程度上减小 json 大小。
- 尽可能所有的图层都是在 AE 里面画出来的，而不是从其他软件引入的。如果是其他软件引入的，很可能导致描述这个图形的 json 部分变得很大。

#### 坑点

- 记得在 lottie 动画 DOMLoaded 之后再调用 playSegments，否则会播放完整段动画后才播放片段，这可以说是 lottie 实现的一个小 bug，但也是新手最常遇到的问题
- 动画是否循环由初始化动画时的 loop 属性决定，如果初始化时设为 false，那么 playSegements 的片段只会播放一遍。可以在播放前通过 animation.loop=true 再设置，不过这个属性不见于官方文档
- addEventListener 的返回值是一个函数，用于移除事件监听
- Lottie 文档中有提到过 setSubFrame ,设置为 true 时会在每次 requestAnimationFrame 时更新界面（可以做到 60fps），关闭时按照 AE 原始帧率来播放，那么自然低 fps 时性能会好很多，但是也可能出现卡顿
  但是在动效幅度不那么大的情况下，其实未必需要 60fps，下面就是 AE 设计时 24fps 的 Lottie 在是否开启 subFrame 下的对比，如果不是很仔细看的话，差别并不大,而相应的 Performance 却有较大的差别.通常情况下，可以默认将 setSubframe 置为 false，以 AE 设计阶段的帧率为准；毕竟设计师自己在 AE 内预览时就是按照设定帧率，「小幅度」的动作其实 20~30 帧是够用的，不会有明显视觉上的卡顿，而页面性能则会好很多。
  甚至在某些情况下，是一定要关闭这个选项的，一些动画在设计时精确地指定了每一个帧的属性，但是如果前端播放时使用了 60fps，反而可能会出现一些不正确的补间动画而导致闪烁之类的问题

- Lottie 动画播放的基本原理

先将每一个动画元素实例化为[animationItem](http://link.zhihu.com/?target=https%3A//github.com/airbnb/lottie-web/blob/master/player/js/animation/AnimationItem.js)

requestAnimationFrame 每次触发时，调用每一个元素的 advanceTime() -> setCurrentRawFrameValue() -> gotoFrame() 计算出要更新的属性值
最终调用 renderer 的 renderFrame() 来更新界面
