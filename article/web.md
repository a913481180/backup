---
title: web前端开发技术
date: 2021-02-31 16:32:11
categories:
- web
---
# web前端开发技术
## web前端开发技术的优化策略
网络页面的核心价值在于传递消费者所需要的信息并且以此来满足消费者相应的需求。而对于消费者和体验用户来说，网页加载的快慢直接影响了这个网页的访问量。如果用户在等待网站信息呈现的事件过长，很大程度上会对其消费体验造成一定影响，会造成用户无法将信息进行高效利用。基于这样的考虑，对web前端技术进行优化并体现信息的真正价值具有重要的意义。web前端技术的优化对网络性能的改善、工作效率的提高都发挥着重要作用。可以从以下几个方面开展工作，对web前端技术进行优化：

1. 减少HTTP请求数
减少不必要的消耗时间的Http请求数，是web前端开发技术的一个重要方面。一个完整的Http包括DNS寻址、发送双方的数据、建立浏览器和服务器间的连接并传输数据、等待服务器响应等等多个环节。在这个过程中，每一个Http请求都必须携带数据，因此每一个请求都不免占用宽带资源，导致用户等待时间增加。要减少网页中Http的请求，经常被采用的方法有：将多个CSS文件或者JavaScript文件合并成一个；优化图片地图，在一个图像上划分不同区域以及注入不通过映射的链接；保持图片和文本一起显示和下载；通过利用CSS background进行背景图的绝对定位，这种做法可以提高网页的载入速度，提高用户的体验。

2. 优化文件的规模
文件规模能对网页加载造成一定的影响 ，这个过程包括压缩JavaScript和CSS文件以及对相应的代码进行优化。优化代码的工作主要是删除一些不必要的Html标签，避免内联式和优化CSS代码。

3. 减少DNS查找
DNS查找的时间开销很大，这是国内许多网站的通病。在一次DNS的解析过程中会耗费20~120ms，若DNS解析请求过于频繁，就会导致用户等待时间的增加，同时也会使信息传输的质量有所下降。并且在DNS查找结束前，浏览器将无法下载该域名下的任何东西。对于国内的站点来说，过多使用站外的Widget（微件），也易引起DNS查找过多的问题。适当减少DNS查找能有效地提高网页地加载速度，对于web前端开发技术来说，DNS查找的优化也非常重要。

4. 杜绝无用响应
在用户访问网站的时候，常常会遇到无用响应，如404错误或拒绝访问错误，这是没有找到文件引起的。Http请求耗费时间过长，而较长的等待时间却得到一个无用的响应页面，大大降低了用户体验。对页面的链接进行充分的测试加上对web服务器Errorr日志的不断跟踪能有效地减少404错误。大多时候，这种错误由于定位稍难而容易被技术人员忽略。

5. 有效避免重定向
网页的重定向会耗费一定的时间，这也是造成用户等待时间过长的一个因素。重定向问题的产生有很多，每增加一次重定向就必然会增加一次对web的请求，所以重定向问题应该尽量避免。这要求技术人员能对web站点的子目录后加一个"/"，这种做法能有效避免重定向。

6. 优化网页内容
CSS的全称是层叠样式表，层叠表明后面的CSS可以覆盖前面的，高级的可以覆盖低级的，因此浏览器会在完全加载完后才考虑开展渲染工作。根据CSS这样的特性，要实现web的优化，可以考虑将样式表放在顶部。另外，可以将script放在底部，该举措是为了防止script脚本阻塞当前页面，从而造成下载速度较慢，页面的加载时间过长等问题的产生。

## Web 实时推送技术

### 一、双向通信
HTTP 协议有一个缺陷：通信只能由客户端发起。举例来说，我们想了解今天的天气，只能是客户端向服务器发出请求，服务器返回查询结果。HTTP 协议做不到服务器主动向客户端推送信息。这种单向请求的特点，注定了如果服务器有连续的状态变化，客户端要获知就非常麻烦。在WebSocket协议之前，有三种实现双向通信的方式：轮询（polling）、长轮询（long-polling）和iframe流（streaming）。

1. 轮询（polling）
轮询是客户端和服务器之间会一直进行连接，每隔一段时间就询问一次。其缺点也很明显：连接数会很多，一个接受，一个发送。而且每次发送请求都会有Http的Header，会很耗流量，也会消耗CPU的利用率。

- 优点：实现简单，无需做过多的更改
- 缺点：轮询的间隔过长，会导致用户不能及时接收到更新的数据；轮询的间隔过短，会导致查询请求过多，增加服务器端的负担

2. 长轮询（long-polling）

长轮询是对轮询的改进版，客户端发送HTTP给服务器之后，看有没有新消息，如果没有新消息，就一直等待。当有新消息的时候，才会返回给客户端。在某种程度上减小了网络带宽和CPU利用率等问题。由于http数据包的头部数据量往往很大（通常有400多个字节），但是真正被服务器需要的数据却很少（有时只有10个字节左右），这样的数据包在网络上周期性的传输，难免对网络带宽是一种浪费。

- 优点：比 Polling 做了优化，有较好的时效性
- 缺点：保持连接会消耗资源; 服务器没有返回有效数据，程序超时。

3. iframe流（streaming）
iframe流方式是在页面中插入一个隐藏的iframe，利用其src属性在服务器和客户端之间创建一条长连接，服务器向iframe传输数据（通常是HTML，内有负责插入信息的javascript），来实时更新页面。

- 优点：消息能够实时到达；浏览器兼容好
- 缺点：服务器维护一个长连接会增加开销；IE、chrome、Firefox会显示加载没有完成，图标会不停旋转。

## 二、WebSocket
1. 什么是websocket
WebSocket是一种全新的协议，随着HTML5草案的不断完善，越来越多的现代浏览器开始全面支持WebSocket技术了，它将TCP的Socket（套接字）应用在了webpage上，从而使通信双方建立起一个保持在活动状态连接通道。

一旦Web服务器与客户端之间建立起WebSocket协议的通信连接，之后所有的通信都依靠这个专用协议进行。通信过程中可互相发送JSON、XML、HTML或图片等任意格式的数据。由于是建立在HTTP基础上的协议，因此连接的发起方仍是客户端，而一旦确立WebSocket通信连接，不论服务器还是客户端，任意一方都可直接向对方发送报文。

初次接触 WebSocket 的人，都会问同样的问题：我们已经有了 HTTP 协议，为什么还需要另一个协议？

2. HTTP的局限性
HTTP是半双工协议，也就是说，在同一时刻数据只能单向流动，客户端向服务器发送请求(单向的)，然后服务器响应请求(单向的)。
服务器不能主动推送数据给浏览器。这就会导致一些高级功能难以实现，诸如聊天室场景就没法实现。
3. WebSocket的特点
支持双向通信，实时性更强
可以发送文本，也可以发送二进制数据
减少通信量：只要建立起WebSocket连接，就希望一直保持连接状态。和HTTP相比，不但每次连接时的总开销减少，而且由于WebSocket的首部信息很小，通信量也相应减少了相对于传统的HTTP每次请求-应答都需要客户端与服务端建立连接的模式，WebSocket是类似Socket的TCP长连接的通讯模式，一旦WebSocket连接建立后，后续数据都以帧序列的形式传输。在客户端断开WebSocket连接或Server端断掉连接前，不需要客户端和服务端重新发起连接请求。在海量并发和客户端与服务器交互负载流量大的情况下，极大的节省了网络带宽资源的消耗，有明显的性能优势，且客户端发送和接受消息是在同一个持久连接上发起，实时性优势明显。

## 三、Web 实时推送技术的比较
方式类型技术实现优点缺点适用场景
- 轮询Pollingclient→server客户端循环请求
1、实现简单 
2、 支持跨域
1、浪费带宽和服务器资源 
2、 一次请求信息大半是无用（完整http头信息） 
3、有延迟 
4、大部分无效请求适于小型应用
- 长轮询Long-Pollingclient→server服务器hold住连接，一直到有数据或者超时才返回，减少重复请求次数
1、实现简单 
2、不会频繁发请求 
3、节省流量 
4、延迟低
1、服务器hold住连接，会消耗资源 
2、一次请求信息大半是无用WebQQ、Hi网页版、Facebook 
- IM长连接iframeclient→server在页面里嵌入一个隐蔵iframe，将这个 iframe 的 src 属性设为对一个长连接的请求，服务器端就能源源不断地往客户端输入数据。
1、数据实时送达 
2、不发无用请求，一次链接，多次“推送”
1、服务器增加开销 
2、无法准确知道连接状态 
3、IE、chrome等一直会处于loading状态Gmail聊天
- WebSocketserver⇌clientnew WebSocket()
1、支持双向通信，实时性更强 
2、可发送二进制文件
3、减少通信量
1、浏览器支持程度不一致 
2、不支持断开重连网络游戏、银行交互和支付