---
title: wifiDog
date: 2019-11-12 22:23:33
categories:
  - web
---

# wifiDog

首先简介一下什么是 Portal 认证。Portal 认证。通常也会叫 Web 认证。未认证用户上网时，设备强制用户登录到特定站点，用户能够免费訪问当中的服务。当用户须要使用互联网中的其他信息时，必须在门户站点进行认证。仅仅有认证通过后才干够使用互联网资源。

现金非常多中国移动 CMCC、中国联通、中国电信 ChinaNet 的 WIFI 都使用这样的认证接入方式。

在 OpenWRT 上实现 Portal 认证，实际上早已有解决方式：

1. chillispot，但原维护作者停止更新。被 chillispot.info 接管继续开发；

2.coova-chilli，它是基于 chillispot 开发拓展的，功能最为强大；能够去官方看一下 Coova-chilli；

3.wifidog

前两个因为原维护作者停止更新。笔者也没有深入研究，重点钻研了 wifidog，Wifidog 也是 OpenWRT 和 DD-WRT 中实现 Portal 比較出名的。

可是。Wifidog 仅仅是实现 AP 认证网关，须要配合外部的 Portalserver 才干使用。Portal 主要是提供认证所需的 WEB 页面且实现认证计费等的功能。尽管这也有非常多商用解决方式，比如 wiwiz、wifiap 等，可是这些商业解决方式的目标都是盈利。即使能够免费使用。免费账号的功能和权限都受到了非常大的限制，比如不能自己定义页面。Web 认证页面有广告等等。有条件的人可能打算自己搭建 Portalserver。可是看看 Wifidog 的官方 Wiki。对搭建过程实在是难以理解。后来，笔者发现网络上另一个 authpuppy 方案，官方站点www.authpuppy.org。是一个已实现好的Wifidog认证server，里面包括各种插件供你使用，官方的安装过程也非常easy，假设你懂的HTML和面向对象编程的相关知识且拥有一个server，能够自行改动认证页面。使用authpuppy也是一个不错的方案。

可是。即便如此，这些方案还是不够灵活，经过笔者认真钻研。查阅大量资料并经过多次抓包分析，最终理解了 Wifidog 的工作原理。

接下来笔者将会跟你介绍怎样自行编写一个轻量级的 Web Portal 认证 server。当然。这须要你具有程序设计基础，HTML、CSS 当然是少不得的。后端开发语言能够使用 PHP 或 Python 或 Java 等。

首先，须要简介一下 Wifidog 的工作原理：

1.client 发出初始化请求，比方訪问 www.baidu.com。

2.网关的防火墙规则将这个请求重定向到本地网关的 port 上。这个 port 是 Wifidog 监听的 port。

3.Wfidog 提供一个 HTTP 重定向回复，重定向到 Web 认证页面。重定向的 Url 的 Querystring 中包括了 Gateway 的 ID，Gateway 的 FQDN 以及其它的信息。

4.用户向认证 server 发出认证请求

http://portal_server:port/login_script?

gw_id=[GatewayID, default: "default"]

gw_address=[GatewayAddress, internal IP of router]

gw_port=[GatewayPort, port that wifidog Gateway is listening on]

url=[user requested url]；

5.网关返回一个（能够是自己定义的）splash（也称作“登录”）页面。

6.用户提供他的凭据信息，比方 username 和 password。

7.成功认证的话，client 将会被重定向到网关的自己的 web 页面上，而且带有一个认证凭据（一个一次性的 token），内容比方：

http://GatewayIP:GatewayPort/wifidog/auth?token=[auth token]；

8.用户就是用获取到的凭据訪问网关。

9.网关去认证 server 询问 token 的有效性。

10.认证 server 确认 token 的有效性。

11.网关发送重定向给 client。以从认证服务器上获取 成功提示页面，重定向到 http://portal_server:port/portal_script 这个位置。

12.认证 server 通知客户请求成功，能够上网了。

图解:

图解 Wifidog 工作原理

然后考察一下 Wifidog 的配置文件/etc/wifidog.conf，关键的配置项是：

AuthServer {

Hostname (Mandatory; Default: NONE)

SSLAvailable (Optional; Default: no; Possible values: yes, no)

SSLPort (Optional; Default: 443)

HTTPPort (Optional; Default: 80)

Path (Optional; Default: /wifidog/ Note: The path must be both prefixed and suffixed by /. Use a single / for server root.)

LoginScriptPathFragment (Optional; Default: login/?

Note: This is the script the user will be sent to for login.)

PortalScriptPathFragment (Optional; Default: portal/? Note: This is the script the user will be sent to after a successfull login.)

MsgScriptPathFragment (Optional; Default: gw_message.php? Note: This is the script the user will be sent to upon error to read a readable message.)

PingScriptPathFragment (Optional; Default: ping/?

Note: This is the script the user will be sent to upon error to read a readable message.)

AuthScriptPathFragment (Optional; Default: auth/?

Note: This is the script the user will be sent to upon error to read a readable message.)

}

# Listen on this port

GatewayPort 2060

# Parameter: CheckInterval

# Default: 60

# Optional

#

# How many seconds should we wait between timeout checks. This is also

# how often the gateway will ping the auth server and how often it will

# update the traffic counters on the auth server. Setting this too low

# wastes bandwidth, setting this too high will cause the gateway to take

# a long time to switch to it’s backup auth server(s).

CheckInterval 60

# Parameter: ClientTimeout

# Default: 5

# Optional

# Set this to the desired of number of CheckInterval of inactivity before a client is logged out

# The timeout will be INTERVAL \* TIMEOUT

ClientTimeout 5

AuthServer 是 Portalserver 的配置项。GatewayPort 是 Wifidog 监听的地址。默认是 2060。一般保持默认就可以；CheckInterval 是心跳时长。单位是秒。什么是心跳呢，client 认证成功之后，假设有网络訪问动作，Wifidog getway 就会每隔一段时间訪问 Portalserver 的一个脚本。用于认证计费。当然。假设客户使用超时或超流量，也能够通过心跳强制 client 下线。ClientTimeout 是用户一次认证成功后的网络訪问时长，超过这个时间须要又一次认证，这个时长并不是由 ClientTimeout 单独决定。取决于 INTERVAL \* TIMEOUT。

具体的配置信息能够訪问：http://dev.wifidog.org/browser/trunk/wifidog/wifidog.conf。

我们重点讨论 Portalserver 的配置项，Hostname 是 Portalserver 的 ip 或者是域名。SSLAvailable 和 SSLPort 是 SSL 加密配置，假设你的 Portalserver 有配置 HTTPS 加密，则须要配置这两项。Path 是指你的脚本路径(举例。http://a.com/to/，则a.com是域名。/to/是路径)，注意路径必须以“/”开头和结尾，假设是根路径。则填一个“/”就可以；接下来的5个配置指明你的脚本名，这说明了我们须要写五个脚本，我会具体说明。

(下面文中涉及的“第几步”均是指 Wifidog 认证过程的步骤)

LoginScriptPathFragment 配置项配置的是登陆脚本，它通过 GET 方式接受传入參数 gw_address、gw_port、gw_id、mac 和 url。gw_address 是 AP Getway 的 ip 地址；gw_port 是 Wifidog 监听的端口。即上面介绍的 wifidog.conf 中的 GatewayPort 配置；gw_id 是 AP Getway 的 id。配置文件 wifidog.conf 中能够配置，默认值是 default，这个值的作用是当存在多个 AP 是，server 或管理员能够依据不同的 id 确定用户的接入点；mac 是客户计算机的网卡物理地址。注意不是 AP 网关的 mac。这个 mac 是用来识别客户计算机的；url 是客户初始訪问的 Url。这些 Querystring 都是 AP Getway 向 client 发出重定向请求自己主动生成的。

这个脚本同一时候须要提供登陆页面，假设登陆成功，须要向客户。端返回 302 重定向，重定向到：http://gw_address:gw_port/wifidog/auth?

token=[token]；即实现第 7 步。当中[token]是你自己自己主动生成的 token 字符串，随机生成一个字符串就可以，可是长度最好长些，安全性更高，另外，token 须要依据不同用户保存，最好保存于数据库中，之后的 AP Getway 询问 token 有效性(第 9 步)还须要用到。这里最好使用 cookie 或 session，使之后的登陆成功页面能够推断用户已经成功，阻止未登录成功的人訪问认证成功页面。

PortalScriptPathFragment 配置项配置的是登陆成功后 server 展示的脚本(第 11 步)，它通过 GET 方式接受 1 个传入參数，gw_id。这个脚本比較简单。告知用户登陆成功就可以，当然，最好重定向到用户之前想要方位的 url。即第 1 步用户输入的 URL。

MsgScriptPathFragment 配置项配置的是错误信息展示脚本，它通过 GET 方式接受一个传入參数 message，这个脚本也非常 easy，展示 message 的内容就可以。目的是当认证过程出现错误，AP Getway 会重定向到这个脚本，URL 中含有错误的信息。

PingScriptPathFragment 配置项配置的是心跳脚本。这个脚本它通过 GET 方式接受 5 个传入參数，gw_id，sys.uptime，sys.memfree，sys.load，wifidog.uptime，当中，sys.uptime 指的是 AP Getway 的启动时间。sys.memfree 指的是 AP Getway 的空暇内存，sys.load 指的是 AP Getway 的 CPU 负载。wifidog.uptime 指的是 wifidog 的启动时间，这个脚本每隔一段时间(Wifidog.conf 里配置的 CheckInterval)，Wifidog 会自己主动訪问，可是其目的不是用户验证，而是帮助管理员管理 AP 节点，了解 AP 节点的负载情况，适时添加节点等，Wifidog 訪问这个脚本时，须要这个脚本返回 Pong。假设你没有统计 AP 节点负载数据的需求，能够丢弃这些数据。直接回应 Pong，注意，这个回应仅仅包括“Pong”字符串。无需包括其它 html 标签。

AuthScriptPathFragment 是用户认证脚本。实现的是第 10 步的功能。这个脚本它通过 GET 方式接受 7 个传入參数：stage、ip、mac、token、incoming、outcoming 和 gw_id。当中 stage 的值是 login，ip 是 client 的 ip，注意不是 AP Getwap 的 ip；mac 是 client 的网卡物理地址。token 就是你在认证脚本生成并返回给 client 的。incoming 和 outcoming 用于流量控制，默认值为 0；gw_id 同上。

怎样识别用户登录成功，通过 mac 和 token 吧，LoginScriptPathFragment 登陆脚本在用户登陆成功后须要记录用户的 mac 和 token。然后在此处验证，假设匹配。回复 Auth: 1，否则。回复 Auth: 0。

另外，这个脚本也是心跳脚本。每隔一段时间 Wifidog 会自己主动訪问，假设用户使用时间超过限制或流量超过额度。server 能够及时回应 Auth: 0 结束用户的訪问。

另外须要注意的是。回应相同无需包括 html 标签，另外，在 Auth 后的冒号和 0/1 之间，有一个空格，缺少这个空格也会导致出错。

在配置 Wifidog 的配置文件 wifidog.conf 是。配置脚本的配置项都必须以“?

”结尾，否则以 GET 方式传递的 QueryString 会因 Url 缺少问号訪问错误的脚本。

看到了吧。只 5 个简单脚本。就能够实现利用 Wifidog 的 Portal 认证，当然。这过中还能够有非常多应用尚未发掘，比方流量控制、带宽控制、结合 Radiusserver 实现认证等。你的开发也能够更上一层楼。实现很多其它功能。只是笔者另一个建议。在登录页面除了 username 和 password 意外，最好加个验证码，防止不怀好意之人暴力破解。

这样，你仅仅须要一个免费的空间，甚至是简单的百度云、新浪 SAE 等。就能够实现一个认证 server。有的人可能还会问，能不能把这些脚本集成到路由器其中，我的回答是能，仅仅要你的脚本的功能不多。问题应该不大，可是这么做的风险比較大，路由的负载比較高，导致路由的执行会非常不稳定，甚至常常死机，这也是笔者亲身实践的结果，所以笔者不建议这么做。

最后啰嗦提醒的是，WiFidog 是使用 iptables 基于三层协议工作的，所以使用 Wifidog 的结果是。不仅是 Wifi 接入须要 Portal 认证，有线接入相同须要认证。

避免这样的情况最简单的做法是设立 mac 白名单。可能有的人又会问。能不能做到仅是 Wifi 接入须要认证，有线接入的无需认证。有的人可能想更上一层楼，能不能开两个 Wifi，仅当中一个 Wifi 须要认证，还有一个 Wifi 和有线网络不须要 Portal 认证，我的回答是能。至于详细做法，以后再介绍。

---

1. 在这个配置文件里面第一格不能是空格，否则就会出错。

2. GatewayAddress 192.168.1.1 //路由器地址

3. GatewayID 123456789 //与服务器 authpuppy 中对应

4. ExternalInterface eth0.2 //外网接口，这个是 wan 口，连接到 Internet 的接口

5. GatewayInterface br-lan //内网接口，这个是 lan 口，连接到局域网的接口

6. AuthServer { //认证服务器配置项

Hostname xx.xx.xx.xx //认证服务器 IP 地址或者名

SSLAvailable no //认证服务器有 HTTPS 加密则配置该项，此处无

SSLPort //认证服务器有 HTTPS 加密则配置该项，此处无

Path /authpuppy/web/ (认证路径)

/**\*\***\*\*\*\***\*\***此处为 wifidog 认证服务器的五个脚本文件，用于认证**\*\*\*\***\*\*\*\***\*\*\*\***/

LoginScriptPathFragment //登录展示的脚本，以 get 的方式传入 gw_address、gw_port、gw_id、mac、url(AP 的 IP 地址，wifidog 监听端口，AP 的 ID，客户端 mac 地址，客户初始访问的 url)

PortalScriptPathFragment //客户端登陆成功后展示的脚本，之传入 gw_id 一个参数，用于告知用户登陆成功

MsgScriptPathFragment //错误信息展示脚本，通过 get 方式传入一个参数 message，用于展示认证中出现错误的页面

PingScriptPathFragment //心跳脚本，通过 get 方式传入 5 个参数，gw_id，sys.uptime，sys.memfree，sys.load，wifidog.uptime；wifidog 每隔一段时间访问一次这个脚本(时间间隔由 CheckInterval 决定)。注意，此脚本需要返回“Pong"字符串

AuthScriptPathFragment //用户认证脚本，通过 get 的方式接受 7 各参数，stage,ip,mac,token,incoming,outcoming 和 gw_id；stage 的值为 login,ip 为客户端 IP，mac 为客户端的 mac 地址， }还有这个 Path，一开始我是设置成 / 根目录，结果不行，出现了这个问题：HTTP Response from Server: [HTTP/1.1 404 Not Found 后来设置成这个目录才可以。还有一点要注意的是 Path 的目录两边都要加/，不然也会出错。

7. GatewayPort 2060 //wifidog 的监听地址，通常保持默认

8. CheckInterval 60 //心跳时长，单位是秒，

心跳：客户端认证成功，如果有网络访问动作，Wifidog getway 就会每隔一段时间访问 Portal 服务器的一个脚本，用于认证计费，当然，如果客户使用超时或超流量，也可以通过心跳强制客户端下线

9. ClientTimeout 5 //ClientTimeout 是用户一次认证成功后的网络访问时长，超过这个时间需要重新认证
