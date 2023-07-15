Shadowsocks （中文名称：影梭，下面简称 SS）是使用 Python 等语言开发的、基于 Apache 许可证开源的代理软件。SS 使用 socks5 代理，由于其流量特征不明显，不太容易用技术手段拦截，因此用于保护网络流量。

# 1. SS 特点

```
省电，在电量查看里几乎看不到它的身影；
支持开机自启动，且断网无影响，无需手动重连，方便网络不稳定或者 3G & Wi-Fi 频繁切换的小伙伴；
可使用自己的服务器，安全和速度的保证；
支持区分国内外流量，传统 VPN 在翻出墙外后访问国内站点会变慢；
可对应用设置单独代理，5.0 之后的系统无需 root。
```

# 2. SS 科学上网原理

Shadowsocks 客户端启动后会在本地开启一个代理，可以理解为一个数据的出入口。用户想通过 SS 访问墙外网站的请求都要经过这个本地代理。

通过 SS 科学上网的过程是这样的：

```
用户发起一个网络访问请求，比如用浏览器访问 google.com，请求被发送到本地代理。
客户端从本地代理拿到请求数据，然后发送至墙外的 SS 服务端。
服务端向 google.com 发起请求，然后收到 google 的响应数据，也就是 google 首页的数据。
服务端把响应数据发回客户端。
客户端再通过本地代理把响应数据交给浏览器，google 首页就显示出来了。
```

整个过程中的第 2 步和第 4 步都是通过 SS 自定义的协议隐蔽地进行，很难被过滤，所以我们才能一直用它顺畅地科学上网。

# 3. SS 服务搭建指南

## 3.1. 购买 VPS 服务器

SS 的正常使用需要服务器端，其实，所有的科学上网软件都是通过服务器端，而搭建服务器端，你就需要有自己的 VPS，所以第一步你就是需要购买一个自己的 VPS (或者你可以跟别人合租)，现在普遍使用的搭建服务器端的 vps 有很多，在这里我们主要介绍性价比比较高的 4 种，一个是 **Linode**，一个是 **DigitalOcean**，一个是 **BandwagonHOST (搬瓦工)**，还有就是 **vultr**，这是从价格，性能等方面做出的推荐，当然，一分价钱一分货，你可以根据自己的实际需求以及支付能力自由选择。

### 3.1.1. Linode

Linode 是国外非常著名的 VPS 商之一，目前在国内站长圈中备受推崇，被许多使用用户评为 "高富帅" 主机产品。Linode 目前最低的 1G 内存方案，1T 流量/月，25G 固态 SSD 硬盘仅需 5 美金一月，折合 RMB 也就在 35 元左右。截止 2018.07.01，Linode VPS   主机报价，详情参考：[https://www.linode.com/pricing](https://www.linode.com/pricing)。
![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339036295-a31faedf-c21d-4b74-828a-c4bbfdd76237.png#align=left&display=inline&height=339&originHeight=339&originWidth=660&size=0&status=done&style=none&width=660)

### 3.1.2. DigitalOcean

digitalocean 是一家成立于 2012 年的总部设置在纽约的云主机商家，采用 KVM 虚拟，配置高性能的 SSD 做储存。截止 2018.07.01，DigitalOcean VPS 主机报价，其中最低 1G 内存方案，1T 流量/月，25G 固态 SSD 硬盘需 5 美金一月（与 Linode 一样），详情参考：[https://www.digitalocean.com/pricing/](https://www.digitalocean.com/pricing/)。
![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339035988-8b986c77-596c-48dc-8e96-e45de2c6b7ed.png#align=left&display=inline&height=434&originHeight=434&originWidth=567&size=0&status=done&style=none&width=567)

### 3.1.3. Vultr

Vultr 是全球最大的游戏主机提供商之一，上线之后以高质的性价比、15 个数据中心，以及新注册账户赠送 5 美金的账户使用金优惠促销，吸引广大的用户。作为 Vultr 的用户，日本、洛杉矶等数据中心速度较好，如果有需要海外其他机房也可以在其 12 个数据中心中选择到适合自己的。

截止 2018.07.01，Vultr VPS 主机报价，目前最低的 1G 内存方案，500G 流量/月，512M 硬盘仅需 2.5 美金一月，性价比相当不错，详情参考：[https://www.vultr.com/pricing/](https://www.vultr.com/pricing/)。
![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339035974-499e4d15-22bb-4a25-9b21-56fd63a99dc7.png#align=left&display=inline&height=448&originHeight=448&originWidth=807&size=0&status=done&style=none&width=807)

### 3.1.4. BandwagonHOST

搬瓦工 VPS 是一款性价比较高的便宜 VPS 主机，且适合入门级网友学习 Linux 和建站用途。

截止 2018.07.01，BanwagonHOST VPS 主机报价（搬瓦工最新 10G VPS、20G VPS 虚拟主机现在都是按年服务，以前按月服务的已经找不到），详情参考：[https://bwh1.net/vps-hosting.php](https://bwh1.net/vps-hosting.php)。
![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339036013-be335ba9-f0fb-4def-9ed0-cec5ee2bc4ca.png#align=left&display=inline&height=412&originHeight=412&originWidth=704&size=0&status=done&style=none&width=704)

---

以下 VPS 的购买以 2017 年按月付费的 BanwagonHOST VPS 为例。2018 最新按年服务的 BanwagonHOST VPS 购买和部署使用步骤也一样。

2017 年按月付费的 BanwagonHOST VPS：
![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339036046-146712cf-1347-4b66-9002-cf2b23f22cdb.png#align=left&display=inline&height=416&originHeight=416&originWidth=704&size=0&status=done&style=none&width=704)

1. 首先选择 $4.99 的 KVM 搬瓦工 VPS 主机，点击 "Order KVM" 进入购买配置页：![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339036008-ef83f620-196b-424c-ac8d-fda4f046d6a0.png#align=left&display=inline&height=581&originHeight=581&originWidth=685&size=0&status=done&style=none&width=685)

2. 在上一步的 Product Configuration 页面点击鼠标右键，选择"查看页面源代码"，获取优惠码：![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339620331-22e4e1e3-492d-4e98-935a-fff0d8ac0a01.png#align=left&display=inline&height=148&originHeight=148&originWidth=485&size=0&status=done&style=none&width=485)

3. 在 Product Configuration 页面，点击"Add to Cart"。在出现的 Order Summary 页面输入 Promotional Code：**BWH1ZBPVK**，点击 "Validate Code" 验证优惠码：
   ![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339640470-a12e0803-0449-4b54-93dc-9bd6657427b9.png#align=left&display=inline&height=349&originHeight=340&originWidth=782&size=0&status=done&style=none&width=782)

4. 点击 "Checkout" 填写注册信息，选择使用支付宝支付。![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339036131-0be73e11-031c-42ad-a70c-d7b3cb95db00.png#align=left&display=inline&height=575&originHeight=575&originWidth=711&size=0&status=done&style=none&width=711)

5. 最后点击 "Complete Order" 完成订单，KiwiVM 会把所有买 VPS 主机的 IP、root 密码、SSH 端口信息发送到你的注册邮箱，当然你也可以登陆 BanwagonHOST (https://bwh1.net/clientarea.php)去查看。![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339580020-4c65d0aa-4a7e-4b94-bebc-e557f2da678e.png#align=left&display=inline&height=323&originHeight=323&originWidth=810&size=0&status=done&style=none&width=810)

## 3.2. SS 服务端搭建

1. SecureCRT 或 putty 或者 xshell 连接服务器。

2. 环境安装与更新。搬瓦工 $4.99/mon VPS 主机默认为 centos-6 操作系统，Shadowsocks 服务端搭建我们推荐使用一键安装脚本，使用 root 用户登录，运行以下命令：

```bash
$ wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
$ chmod +x shadowsocks-all.sh
$ ./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
Which Shadowsocks server you'd select:
1) Shadowsocks-Python
2) ShadowsocksR
3) Shadowsocks-Go
4) Shadowsocks-libev
Please enter a number (Default Shadowsocks-Python):1   # 选择安装 Python 版 shadowsocks

You choose = Shadowsocks-Python

Please enter password for Shadowsocks-Python
(Default password: teddysun.com): xxxx   # 设置密码

password = xxxx

Please enter a port for Shadowsocks-Python [1-65535]
(Default port: 8989): 18989   # 设置默认端口

port = 18989

Please select stream cipher for Shadowsocks-Python:
1) aes-256-gcm
2) aes-192-gcm
3) aes-128-gcm
4) aes-256-ctr
5) aes-192-ctr
6) aes-128-ctr
7) aes-256-cfb
8) aes-192-cfb
9) aes-128-cfb
10) camellia-128-cfb
11) camellia-192-cfb
12) camellia-256-cfb
13) xchacha20-ietf-poly1305
14) chacha20-ietf-poly1305
15) chacha20-ietf
16) chacha20
17) salsa20
18) rc4-md5
Which cipher you'd select(Default: aes-256-gcm):9   # 选择加密方式；如不设定，Python 和 libev 版默认为 aes-256-gcm，R 和 Go 版默认为 aes-256-cfb

cipher = aes-128-cfb

Press any key to start...or Press Ctrl+C to cancel
[Info] Checking the EPEL repository...
[Info] Checking the EPEL repository complete...
....

Starting Shadowsocks success

Congratulations, Shadowsocks-Python server install completed!
Your Server IP        :  xxx.xxx.xxx.xxx
Your Server Port      :  18989
Your Password         :  xxxxxx
Your Encryption Method:  aes-128-cfb

Your QR Code: (For Shadowsocks Windows, OSX, Android and iOS clients)
 ss://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxk=
Your QR Code has been saved as a PNG file path:
 /data/shadowsocks_python_qr.png

Welcome to visit: https://teddysun.com/486.html
Enjoy it!
```

3. 安装完成后，从脚本提示获取服务器 IP、密码、端口、加密方式、二维码等客户端登陆必须的信息。

4. SS 卸载，若已安装多个版本，则卸载时也需多次运行（每次卸载一种）。使用 root 用户登录，运行以下命令：

```bash
./shadowsocks-all.sh uninstall
```

5. 启动脚本。启动脚本后面的参数含义，从左至右依次为：启动，停止，重启，查看状态。

```bash
# Shadowsocks-Python 版
/etc/init.d/shadowsocks-python start | stop | restart | status

# ShadowsocksR 版
/etc/init.d/shadowsocks-r start | stop | restart | status

# Shadowsocks-Go 版
/etc/init.d/shadowsocks-go start | stop | restart | status

# Shadowsocks-libev 版
/etc/init.d/shadowsocks-libev start | stop | restart | status
```

6. 各版本默认配置文件

```bash
# Shadowsocks-Python 版
/etc/shadowsocks-python/config.json

# ShadowsocksR 版
/etc/shadowsocks-r/config.json

# Shadowsocks-Go 版
/etc/shadowsocks-go/config.json

# Shadowsocks-libev 版
/etc/shadowsocks-libev/config.json
```

## 3.3. SS 客户端使用

各个平台下 SS 客户端列表：

| 平台    | 软件名          | 链接                                                                                                                       |                                             |
| :------ | :-------------- | :------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------ |
| Windows | 影梭            | [https://github.com/shadowsocks/shadowsocks-windows/releases](https://github.com/shadowsocks/shadowsocks-windows/releases) | GitHub 官方下载                             |
| Android | -               | [https://github.com/shadowsocks/shadowsocks-android/releases](https://github.com/shadowsocks/shadowsocks-android/releases) | 下载 APK，安装后完成设置即可                |
| iOS     | OpenWingy       | APP Store 搜索 OpenWingy 进行安装                                                                                          | AppStore 搜索 OpenWingy，安装后完成设置即可 |
| linux   | shadowsocks-qt5 | deepin linux 应用商店                                                                                                      |                                             |

### 3.3.1. windows 下使用

SS 常规版 windowns 客户端下载地址：[https://github.com/shadowsocks/shadowsocks-windows/releases](https://github.com/shadowsocks/shadowsocks-windows/releases)。客户端下载解压后，只需要按照服务器的配置填写服务器 IP 地址、服务器端口、本地端口（如果没有本地端口选项，就是默认的 1080）、密码、加密方式等参数就可以愉快地科学上网了。

### 3.3.2. Linux 下使用

这里主要说一下 deepin linux 中 SS 的配置使用。

① 进入"深度商店"，搜索 "shadowsocks-qt5"，点击完成安装。
② 在系统启动器中找到 SS，点击启动，设置连接。
③ **安装完后连接代理服务器，发现 google 依然无法访问，这里需要浏览器插件支持才能访问。**以 chrome 为例，安装 **SwitchyOmega** 插件。安装教程：[https://www.switchyomega.com/download.html](https://www.switchyomega.com/download.html)。
⑤ 设置代理服务器，点击 "应用选项" 保存设置。

![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339036007-08922170-e14c-4062-a014-a56a63a7fb41.png#align=left&display=inline&height=373&originHeight=373&originWidth=800&size=0&status=done&style=none&width=800)

⑥ 连接上网，登陆 YouTube 测试。
![](https://cdn.nlark.com/yuque/0/2020/png/126032/1591339036026-8ed5fba4-9634-414d-8196-60f8c6790ce9.png#align=left&display=inline&height=507&originHeight=507&originWidth=785&size=0&status=done&style=none&width=785)

# 5. 参考资料

- Shadowsocks 一键安装脚本（四合一）：[https://teddysun.com/486.html](https://teddysun.com/486.html)
- GiuHub：shadowsocks/shadowsocks-windows 客户端：[https://github.com/shadowsocks/shadowsocks-windows/releases](https://github.com/shadowsocks/shadowsocks-windows/releases)
- 从零开始：史上最详尽 Shadowsocks 搭建教程：[https://www.iwwenbo.com/0-1-shadowsocks-start/](https://www.iwwenbo.com/0-1-shadowsocks-start/)
- 使用 VPS 搭建 shadowsocks 服务：[http://blog.csdn.net/asliulue/article/details/54949475](http://blog.csdn.net/asliulue/article/details/54949475)
- Deepin（基于 Debian 的 Linux 系统）安装 Shadowsocks：[http://blog.csdn.net/wan_yanyan528/article/details/52572786](http://blog.csdn.net/wan_yanyan528/article/details/52572786)
- 科学上网利器(1) – Shadowsocks vps 设置方法：[https://www.1-17.cn/shadowsocks-online/](https://www.1-17.cn/shadowsocks-online/)
