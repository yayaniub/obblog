---
{"dg-publish":true,"permalink":"/digital-garden/mac-mini/","dgPassFrontmatter":true,"noteIcon":""}
---

#### 1.前言

最近看Mac mini价格基本位于最低点，三千出头的样子就入手了一台Mac mini，一是体验下macos，二是因为有公网ipv6准备折腾把它当作一台服务器，把常用的服务都部署在上面，说到这里就得吐槽下国内服务器不仅贵，带宽还小还得域名备案，于是萌生了将自己的服务部署在自己家里的服务器上，买Mac mini也为了此，下面就详细叨叨Mac mini作为服务器的折腾过程。
#### 2.安装ddns-go

现在应该是全面IPv6公网了，但是有些路由器默认是不开启的，需要自己到路由器后台手动开启。如果没有ipv6致电运营商要一个，不行就投诉到工信部，现在是全面建设IPv6网络，投诉直接说一些纯IPv6站点打不开。ipv6是动态的一般是两天和随着路由器的重启发生变化，这就需要通过ddns将最新IP解析到自己域名上，我用的是ddns-go，以下是项目地址，具体操作方法看项目文档[ddns-go](https://github.com/jeessy2/ddns-go)
- 登陆后台内网ip:9876填入域名解析API-token

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/14222032.png)

- 填入你需要解析的域名，并点保存，这样的ip解析好了

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/14222110.png)

#### 3.安装orbstack

下面简单介绍下orbstack

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/14222840.jpeg)

🛰️ [OrbStack：Docker 桌面版的替代品](https://orbstack.dev/)

✨ **Features
**
- 苹果原生 UI 设计

- 轻量化

- 功能简洁

- 一键启动 Linux 容器，支持多个 Linux 发行版

- 命令行 Cheatsheet

- 命令行工具

OrbStack 在实现了 Docker Desktop 绝大部份的基础上，还增加了一键启动 Linux 虚拟机的功能。这个虚拟机的架构类似 WSL，共享内核。Orbstack 用起来比 Docker Desktop 快上很多。启动，功能都要快上很多。设计也更加简洁，并且契合 Mac 的原生设计。基础的功能 Orbstack 也都支持了，完成度也很高。如果不需要一些复杂的功能，Orbstack 完全可以帮助管理一些简单的容器。

#### 4.安装Nginx Proxy Manager

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/14223154.webp)

- 在第二步中安装了docker，Nginx Proxy Manager[安装教程参考](https://blog.laoda.de/archives/nginxproxymanager/?highlight=ning)
- 配置转发，通过第一步解析你的域名可以利用域名来访问你的服务，按照如下图填写域名和你的部署的服务的内网ip和端口，点击保存

<img src="https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/14224020.png" alt="照片1" width="335
	  px">    <img src="https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/14224056.png" alt="照片2" width="335px">

#### 5.orbstack Linux 虚拟机

- 安装Linux虚拟机，点击左侧边栏Machines-右上角的+号选择你喜欢的linux操作系统点击创建，创建的速度很快。

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/14224611.png)

- 右键点击进入操作终端然后就可以部署你想要的服务了

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/14224945.png)

- 将部署的服务通过域名访问，虚拟的内网地址可以看上面图片，copy address或者进入终端输入ifconfig查看
- 将你部署的服务的内网IP和端口填入Nginx Proxy Manager中，然后你的服务就可以通过域名访问
- 如果你是在安装了宝塔部署网站，可以在域名管理哪里填写内网ip加端口，端口随意设置只要是没有占用就可以，然后将IP和端口到填入Nginx Proxy Manager，并设置好你的域名。

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/14225832.png)

#### 6.最后

- 将Mac mini作为服务器使用最终要的是安装好ddns-go和Nginx Proxy Manager这样就可以通过域名的形式访问到我们在mac上部署的服务，其实window等操作系统步骤都是一样的。
- 将 Mac mini设置为永不休眠和来电自动开机自动登录，还可以开启远程ssh和远程连接具体操作就不写了具体参考以下视频[【「黑貓」把 Mac mini 用作家用服务器 | 入门教程 + 体验感受-哔哩哔哩】 ](https://b23.tv/hkogezC)
- 还可以将Mac mini当作软路由来使用具体可以搜索相关教程