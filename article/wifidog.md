---
title: wifiDog
date: 2021-11-12 22:23:33
categories: 
- web
---

# wifiDog


首先简介一下什么是Portal认证。Portal认证。通常也会叫Web认证。未认证用户上网时，设备强制用户登录到特定站点，用户能够免费訪问当中的服务。当用户须要使用互联网中的其他信息时，必须在门户站点进行认证。仅仅有认证通过后才干够使用互联网资源。

现金非常多中国移动CMCC、中国联通、中国电信ChinaNet的WIFI都使用这样的认证接入方式。


在OpenWRT上实现Portal认证，实际上早已有解决方式：

1. chillispot，但原维护作者停止更新。被chillispot.info接管继续开发；

2.coova-chilli，它是基于chillispot开发拓展的，功能最为强大；能够去官方看一下Coova-chilli；

3.wifidog

前两个因为原维护作者停止更新。笔者也没有深入研究，重点钻研了wifidog，Wifidog也是OpenWRT和DD-WRT中实现Portal比較出名的。

可是。Wifidog仅仅是实现AP认证网关，须要配合外部的Portalserver才干使用。Portal主要是提供认证所需的WEB页面且实现认证计费等的功能。尽管这也有非常多商用解决方式，比如wiwiz、wifiap等，可是这些商业解决方式的目标都是盈利。即使能够免费使用。免费账号的功能和权限都受到了非常大的限制，比如不能自己定义页面。Web认证页面有广告等等。有条件的人可能打算自己搭建Portalserver。可是看看Wifidog的官方Wiki。对搭建过程实在是难以理解。后来，笔者发现网络上另一个authpuppy方案，官方站点www.authpuppy.org。是一个已实现好的Wifidog认证server，里面包括各种插件供你使用，官方的安装过程也非常easy，假设你懂的HTML和面向对象编程的相关知识且拥有一个server，能够自行改动认证页面。使用authpuppy也是一个不错的方案。

可是。即便如此，这些方案还是不够灵活，经过笔者认真钻研。查阅大量资料并经过多次抓包分析，最终理解了Wifidog的工作原理。

接下来笔者将会跟你介绍怎样自行编写一个轻量级的Web Portal认证server。当然。这须要你具有程序设计基础，HTML、CSS当然是少不得的。后端开发语言能够使用PHP或Python或Java等。

首先，须要简介一下Wifidog的工作原理：

1.client发出初始化请求，比方訪问 www.baidu.com。

2.网关的防火墙规则将这个请求重定向到本地网关的port上。这个port是Wifidog监听的port。

3.Wfidog提供一个HTTP重定向回复，重定向到Web认证页面。重定向的Url的Querystring中包括了Gateway的ID，Gateway的FQDN以及其它的信息。

4.用户向认证server发出认证请求

http://portal_server:port/login_script?

gw_id=[GatewayID, default: "default"]

gw_address=[GatewayAddress, internal IP of router]

gw_port=[GatewayPort, port that wifidog Gateway is listening on]

url=[user requested url]；

5.网关返回一个（能够是自己定义的）splash（也称作“登录”）页面。

6.用户提供他的凭据信息，比方username和password。

7.成功认证的话，client将会被重定向到网关的自己的web页面上，而且带有一个认证凭据（一个一次性的token），内容比方：

http://GatewayIP:GatewayPort/wifidog/auth?token=[auth token]；

8.用户就是用获取到的凭据訪问网关。

9.网关去认证server询问token的有效性。

10.认证server确认token的有效性。

11.网关发送重定向给client。以从认证服务器上获取 成功提示页面，重定向到 http://portal_server:port/portal_script 这个位置。

12.认证server通知客户请求成功，能够上网了。

图解:

图解Wifidog工作原理

然后考察一下Wifidog的配置文件/etc/wifidog.conf，关键的配置项是：

AuthServer {

Hostname             (Mandatory; Default: NONE)

SSLAvailable           (Optional; Default: no; Possible values: yes, no)

SSLPort               (Optional; Default: 443)

HTTPPort             (Optional; Default: 80)

Path                  (Optional; Default: /wifidog/ Note:  The path must be both prefixed and suffixed by /.  Use a single / for server root.)

LoginScriptPathFragment  (Optional; Default: login/?

Note:  This is the script the user will be sent to for login.)

PortalScriptPathFragment (Optional; Default: portal/? Note:  This is the script the user will be sent to after a successfull login.)

MsgScriptPathFragment    (Optional; Default: gw_message.php? Note:  This is the script the user will be sent to upon error to read a readable message.)

PingScriptPathFragment    (Optional; Default: ping/?

Note:  This is the script the user will be sent to upon error to read a readable message.)

AuthScriptPathFragment    (Optional; Default: auth/?

Note:  This is the script the user will be sent to upon error to read a readable message.)

}

 

# Listen on this port

GatewayPort 2060

 

# Parameter: CheckInterval

# Default: 60

# Optional

#

# How many seconds should we wait between timeout checks.  This is also

# how often the gateway will ping the auth server and how often it will

# update the traffic counters on the auth server.  Setting this too low

# wastes bandwidth, setting this too high will cause the gateway to take

# a long time to switch to it’s backup auth server(s).

CheckInterval 60

 

# Parameter: ClientTimeout

# Default: 5

# Optional

#

# Set this to the desired of number of CheckInterval of inactivity before a client is logged out

# The timeout will be INTERVAL * TIMEOUT

ClientTimeout 5

 

AuthServer是Portalserver的配置项。GatewayPort是Wifidog监听的地址。默认是2060。一般保持默认就可以；CheckInterval是心跳时长。单位是秒。什么是心跳呢，client认证成功之后，假设有网络訪问动作，Wifidog getway就会每隔一段时间訪问Portalserver的一个脚本。用于认证计费。当然。假设客户使用超时或超流量，也能够通过心跳强制client下线。ClientTimeout是用户一次认证成功后的网络訪问时长，超过这个时间须要又一次认证，这个时长并不是由ClientTimeout单独决定。取决于INTERVAL * TIMEOUT。

具体的配置信息能够訪问：http://dev.wifidog.org/browser/trunk/wifidog/wifidog.conf。

我们重点讨论Portalserver的配置项，Hostname是Portalserver的ip或者是域名。SSLAvailable和SSLPort是SSL加密配置，假设你的Portalserver有配置HTTPS加密，则须要配置这两项。Path是指你的脚本路径(举例。http://a.com/to/，则a.com是域名。/to/是路径)，注意路径必须以“/”开头和结尾，假设是根路径。则填一个“/”就可以；接下来的5个配置指明你的脚本名，这说明了我们须要写五个脚本，我会具体说明。

(下面文中涉及的“第几步”均是指Wifidog认证过程的步骤)

LoginScriptPathFragment配置项配置的是登陆脚本，它通过GET方式接受传入參数gw_address、gw_port、gw_id、mac和url。gw_address是AP Getway的ip地址；gw_port是Wifidog监听的端口。即上面介绍的wifidog.conf中的GatewayPort配置；gw_id是AP Getway的id。配置文件wifidog.conf中能够配置，默认值是default，这个值的作用是当存在多个AP是，server或管理员能够依据不同的id确定用户的接入点；mac是客户计算机的网卡物理地址。注意不是AP网关的mac。这个mac是用来识别客户计算机的；url是客户初始訪问的Url。这些Querystring都是AP Getway向client发出重定向请求自己主动生成的。

这个脚本同一时候须要提供登陆页面，假设登陆成功，须要向客户。端返回302重定向，重定向到：http://gw_address:gw_port/wifidog/auth?

token=[token]；即实现第7步。当中[token]是你自己自己主动生成的token字符串，随机生成一个字符串就可以，可是长度最好长些，安全性更高，另外，token须要依据不同用户保存，最好保存于数据库中，之后的AP Getway询问token有效性(第9步)还须要用到。这里最好使用cookie或session，使之后的登陆成功页面能够推断用户已经成功，阻止未登录成功的人訪问认证成功页面。

PortalScriptPathFragment配置项配置的是登陆成功后server展示的脚本(第11步)，它通过GET方式接受1个传入參数，gw_id。这个脚本比較简单。告知用户登陆成功就可以，当然，最好重定向到用户之前想要方位的url。即第1步用户输入的URL。

MsgScriptPathFragment配置项配置的是错误信息展示脚本，它通过GET方式接受一个传入參数message，这个脚本也非常easy，展示message的内容就可以。目的是当认证过程出现错误，AP Getway会重定向到这个脚本，URL中含有错误的信息。

PingScriptPathFragment配置项配置的是心跳脚本。这个脚本它通过GET方式接受5个传入參数，gw_id，sys.uptime，sys.memfree，sys.load，wifidog.uptime，当中，sys.uptime指的是AP Getway的启动时间。sys.memfree指的是AP Getway的空暇内存，sys.load指的是AP Getway的CPU负载。wifidog.uptime指的是wifidog的启动时间，这个脚本每隔一段时间(Wifidog.conf里配置的CheckInterval)，Wifidog会自己主动訪问，可是其目的不是用户验证，而是帮助管理员管理AP节点，了解AP节点的负载情况，适时添加节点等，Wifidog訪问这个脚本时，须要这个脚本返回Pong。假设你没有统计AP节点负载数据的需求，能够丢弃这些数据。直接回应Pong，注意，这个回应仅仅包括“Pong”字符串。无需包括其它html标签。

AuthScriptPathFragment是用户认证脚本。实现的是第10步的功能。这个脚本它通过GET方式接受7个传入參数：stage、ip、mac、token、incoming、outcoming和gw_id。当中stage的值是login，ip是client的ip，注意不是AP Getwap的ip；mac是client的网卡物理地址。token就是你在认证脚本生成并返回给client的。incoming和outcoming用于流量控制，默认值为0；gw_id同上。

怎样识别用户登录成功，通过mac和token吧，LoginScriptPathFragment登陆脚本在用户登陆成功后须要记录用户的mac和token。然后在此处验证，假设匹配。回复Auth: 1，否则。回复Auth: 0。

另外，这个脚本也是心跳脚本。每隔一段时间Wifidog会自己主动訪问，假设用户使用时间超过限制或流量超过额度。server能够及时回应Auth: 0结束用户的訪问。

另外须要注意的是。回应相同无需包括html标签，另外，在Auth后的冒号和0/1之间，有一个空格，缺少这个空格也会导致出错。

在配置Wifidog的配置文件wifidog.conf是。配置脚本的配置项都必须以“?

”结尾，否则以GET方式传递的QueryString会因Url缺少问号訪问错误的脚本。

看到了吧。只5个简单脚本。就能够实现利用Wifidog的Portal认证，当然。这过中还能够有非常多应用尚未发掘，比方流量控制、带宽控制、结合Radiusserver实现认证等。你的开发也能够更上一层楼。实现很多其它功能。只是笔者另一个建议。在登录页面除了username和password意外，最好加个验证码，防止不怀好意之人暴力破解。

这样，你仅仅须要一个免费的空间，甚至是简单的百度云、新浪SAE等。就能够实现一个认证server。有的人可能还会问，能不能把这些脚本集成到路由器其中，我的回答是能，仅仅要你的脚本的功能不多。问题应该不大，可是这么做的风险比較大，路由的负载比較高，导致路由的执行会非常不稳定，甚至常常死机，这也是笔者亲身实践的结果，所以笔者不建议这么做。

最后啰嗦提醒的是，WiFidog是使用iptables基于三层协议工作的，所以使用Wifidog的结果是。不仅是Wifi接入须要Portal认证，有线接入相同须要认证。

避免这样的情况最简单的做法是设立mac白名单。可能有的人又会问。能不能做到仅是Wifi接入须要认证，有线接入的无需认证。有的人可能想更上一层楼，能不能开两个Wifi，仅当中一个Wifi须要认证，还有一个Wifi和有线网络不须要Portal认证，我的回答是能。至于详细做法，以后再介绍。




---

1. 在这个配置文件里面第一格不能是空格，否则就会出错。

2. GatewayAddress  192.168.1.1 //路由器地址

3. GatewayID 123456789 //与服务器authpuppy中对应

4. ExternalInterface eth0.2  //外网接口，这个是wan口，连接到Internet的接口

5. GatewayInterface  br-lan  //内网接口，这个是lan口，连接到局域网的接口

6. AuthServer {                        //认证服务器配置项                             

Hostname xx.xx.xx.xx //认证服务器IP地址或者名                                                 

SSLAvailable no //认证服务器有HTTPS加密则配置该项，此处无

SSLPort //认证服务器有HTTPS加密则配置该项，此处无

Path /authpuppy/web/        (认证路径)

/****************此处为wifidog认证服务器的五个脚本文件，用于认证********************/

LoginScriptPathFragment //登录展示的脚本，以get的方式传入gw_address、gw_port、gw_id、mac、url(AP的IP地址，wifidog监听端口，AP的ID，客户端mac地址，客户初始访问的url)

PortalScriptPathFragment //客户端登陆成功后展示的脚本，之传入gw_id一个参数，用于告知用户登陆成功

MsgScriptPathFragment //错误信息展示脚本，通过get方式传入一个参数message，用于展示认证中出现错误的页面

PingScriptPathFragment //心跳脚本，通过get方式传入5个参数，gw_id，sys.uptime，sys.memfree，sys.load，wifidog.uptime；wifidog每隔一段时间访问一次这个脚本(时间间隔由CheckInterval决定)。注意，此脚本需要返回“Pong"字符串

AuthScriptPathFragment //用户认证脚本，通过get的方式接受7各参数，stage,ip,mac,token,incoming,outcoming和gw_id；stage的值为login,ip为客户端IP，mac为客户端的mac地址，                  }还有这个Path，一开始我是设置成  /   根目录，结果不行，出现了这个问题：HTTP Response from Server: [HTTP/1.1 404 Not Found后来设置成这个目录才可以。还有一点要注意的是Path的目录两边都要加/，不然也会出错。

7. GatewayPort 2060 //wifidog的监听地址，通常保持默认

8. CheckInterval 60 //心跳时长，单位是秒，

心跳：客户端认证成功，如果有网络访问动作，Wifidog getway就会每隔一段时间访问Portal服务器的一个脚本，用于认证计费，当然，如果客户使用超时或超流量，也可以通过心跳强制客户端下线

9. ClientTimeout 5 //ClientTimeout是用户一次认证成功后的网络访问时长，超过这个时间需要重新认证