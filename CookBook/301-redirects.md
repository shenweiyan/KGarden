> 📢 **域名 **[**https://weiyan.cc**](https://weiyan.cc)** 就是基于本文章中的 "**[**无服务器**](#pG3U6)**" 步骤实现的个人域名跳转至语雀个人主页！**
>
>      因此，本篇文档你也可以通过以下的链接访问：[**https://weiyan.cc/cookbook/301-redirects**](https://weiyan.cc/cookbook/301-redirects)**！**

语雀的个人使用目前是不支持自定义域名的，虽然空间的使用可以自定义二级域名，如：[weiyan.yuque.com](https://weiyan.yuque.com/)，但是空间知识库必须要先登录，不方便其他人查看，尤其是对于没有注册语雀的用户。

现在的情况是，我有一个已经备案的个人域名 www.example.com，现在我想：

- 让所有 www.example.com 的访问地址都跳转到 [https://www.yuque.com/shenweiyan](https://www.yuque.com/shenweiyan)，比如 https://www.example.com/cookbook 跳转到 [https://www.yuque.com/shenweiyan/cookbook](https://www.yuque.com/shenweiyan/cookbook)。
- www.example.com 的访问地址跳转同时支持 http/https。
- example.com/www.example.com 同时实现以上跳转。

反正就一句话，让下面的链接都跳转到 [https://www.yuque.com/shenweiyan](https://www.yuque.com/shenweiyan)：

- http://example.com
- http://www.example.com
- https://example.com
- https://www.example.com

下面简单记录一下具体的实现过程。

# 背景知识

**显性 URL 转发：**用的是 301 重定向技术，效果为浏览器地址栏输入 [http://a.com](http://a.com/) 回车，打开网站内容是目标地址 [http://cloud.baidu.com/](http://cloud.baidu.com/) 的网站内容，且**地址栏显示目标地址 **[**http://cloud.baidu.com/**](http://cloud.baidu.com/) 。

**隐性 URL 转发：**用的是 iframe 框架技术、非重定向技术，效果为浏览器地址栏输入 [http://a.com](http://a.com/) 回车，打开网站内容是目标地址 [http://cloud.baidu.com/](http://cloud.baidu.com/) 的网站内容，**但地址栏显示当前地址 **[**http://a.com**](http://a.com/) 。

**301 重定向是什么？**

301 重定向表示网页由一个地址永久地移动到了另外一个地址。这里中的 301 是被重定向网页的 HTTP 状态代码。

**例如：**[blog.ahrefs.com](https://blog.ahrefs.com/) 重定向到了 [ahrefs.com/blog](https://ahrefs.com/blog)。

简单来说，301 重定向是在告诉浏览器：“这个页面已经永久迁移了。这个是新的地址，我们不打算把它移回去啦。”这时，浏览器会回复：“没问题！我现在（开始）就把用户引向这里！”

这就是为什么访问 blog.ahrefs.com 已经不可能了。你最后会去到的网页是 ahrefs.com/blog。

# 前提条件

前提条件可以分为**有服务器**和**无服务器**两种情况，下面具体说一下。

1. 有服务器（可以考虑腾讯云或者阿里云的轻量云服务器，双十一优惠价一年也就几十块）；

   - 阿里云轻量云服务器：[购买链接](https://www.aliyun.com/activity/1111?userCode=mx65q35j)
   - 腾讯云轻量云服务器：[购买链接](https://curl.qcloud.com/0Sy0R0AX)
   - 域名（域名需要已经完成备案）；
   - SSL 证书（可以使用阿里云或者腾讯云的免费域名证书）；

2. 无服务器
   - 可以考虑使用 [Cloudflare Page Rules](https://support.cloudflare.com/hc/zh-cn/articles/218411427)（页面规则）；当然，其他的平台也可以；
   - 域名（有些域名可以不用备案）；
   - SSL 证书（如果你用的是 [Cloudflare Page Rules](https://support.cloudflare.com/hc/zh-cn/articles/218411427)，可以不用 SSL 证书 ）。

# 操作步骤：有服务器

本操作以 **ncbix.com** 域名为示例。

## 1. 域名解析

在你的域名供应商后台点击“添加记录”，分别输入 www 和 @，记录类型“A”，记录值就是你虚拟主机或 VPS 服务器的 IP 地址，最后保存。以 DNSPOD 为例。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FqvWUx6cyUz-O8d1avOdFZ1aDIWF.png)

## 2. SSL 证书

申请免费证书，具体操作可以自行百度。以腾讯云为例：[https://console.cloud.tencent.com/ssl](https://console.cloud.tencent.com/ssl)。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/Fo4XBIRStXxx27kvjKULIjHazdJ9.png)
根据截图，一步步点击操作。申请完成后，把证书下载并上传到你的服务器。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FqamrO4EBSQuO6wsJ28y8g-AH63E.png)

## 3. 安装 Nginx

可以直接使用 **yum/apt** 的方式直接安装；源码方式的安装，参考：《[CentOS 7 下编译安装 Nginx · 语雀](https://www.yuque.com/shenweiyan/cookbook/centos-install-nginx)》。

```bash
# Debian/Ubuntu
apt update
apt install nginx

# CentOS/RHEL
yum install nginx
```

## 4. 配置 Nginx

通过 **yum/apt** 安装的 Nginx 默认的置文件在 **/etc/nginx/nginx.conf**，编辑该文件。

```nginx
http {
    ##
    # Basic Settings
    ##
    ......

    ##
    # Virtual Host Configs
    ##
server {
    listen 80;
    listen 443 ssl;
    server_name ncbix.com www.ncbix.com;
    ssl_certificate /etc/nginx/ssl/nginx/www.ncbix.com_bundle.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx/www.ncbix.com.key;
    index  index.php index.html index.htm;

    if ( $scheme = "http" ) {
        return 301 https://www.yuque.com/shenweiyan$request_uri; #确保跳转到新域名HTTPS如果没有HTTPS可以去掉
    }
    location / {
        rewrite /.* https://www.yuque.com/shenweiyan$uri redirect; #跳转到新域名并重写为新域名
    }
  }

include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

## 5. 重启 Nginx

最后，通过下面的命令重启 Nginx 服务即可。

```bash
service nginx restart
```

# 操作步骤：无服务器

我们以 [Cloudflare Page Rules](https://support.cloudflare.com/hc/zh-cn/articles/218411427) 为例，说明一下具体怎么操作。

## 1. Cloudflare 中添加站点

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/Fjd2wS9yVs0ZiBBalZQVOuPQHbU1.png)
添加完站点后，可以选择 **Free 计划**，然后点击继续：
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FumaUELUaIJnv9s0Q4paKUDIAWTL.png)
点击继续后，Cloudflare 会自动扫描你对应域名的一些解析记录：
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FtQZKAyQd_6dd5d7A_ZLCVp4NUEg.png)
我们可以直接选择 **"继续"**。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/Fvkt0gHVP7lmiqapWf3pM7Zx4YwN.png)

## 2. 修改域名 DNS

首先，我的域名是在腾讯云注册的，可以去腾讯云控制台 **"我的域名"** 中直接修改 DNS：

```python
# 添加 Cloudflare 名称服务器
imani.ns.cloudflare.com
caroline.dnspod.net
```

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FlKyYHHLGHwD9IPKYvLtPorghBpr.png)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FmRwodrlbkBt6SkX7RsG2ec1iruc.png)

## 3. 完成 Cloudflare 添加站点

可以把后面快速指南的这几个配置都勾选。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FtULJ4uM_TuPjPkkMtdXAhxpyDes.png)
等待几分钟就可以看到你的域名站点已经添加到 Cloudflare 上了！
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/Fk8Hv5dkW8kHd1IorhNfT427RliQ.png)

## 4. 设置 DNS 记录

> The first thing you will need is a DNS record for **@**, **www** and any other subdomains you want to redirect, set to ![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FjjhP041Zj-aZVPAHpQ-YnHIzMA6.png). This can point to any IP address as the redirection page rule will execute first. I would recommend pointing them to 192.0.2.1 , a dummy IP.
>
> From：[https://community.cloudflare.com/t/redirecting-one-domain-to-another/81960](https://community.cloudflare.com/t/redirecting-one-domain-to-another/81960)

在配置 Cloudflare 站点的页面规则前，你需要把该域名的 **@**，**www** 或者其他你想要进行重定向的子域名添加到 DNS 记录中，这个记录的值可以指向任何 IP 地址，因为重定向页面规则将首先执行。我建议将它们指向 192.0.2.1 ，一个虚拟 IP。

在这里，我们以 **@** 和 **note** 子域名为例，添加 DNS 记录，先让它们指向一个虚拟 IP。
![以 ncbix.com 和 note.ncbix.com 为例，均重定向到 https://www.yuque.com/shenweiyan 页面](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/Fk08ydFuPukTv88AAa0V27T4xTBh.png "以 ncbix.com 和 note.ncbix.com 为例，均重定向到 https://www.yuque.com/shenweiyan 页面")

## 5. 配置 Cloudflare 站点页面规则

首先，在 Cloudflare 的主页上点击对应的站点，选择 **"页面规则"**，点击。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FqolXaSzUdkgtKo66yRC3xRkV7mv.png)
点击**"创建页面规则"**：
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FnF2bk6hzYvulCtXdLSQSUiV1Z44.png)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/Fp5DSLxKInn25a591hjYjDNpUeRn.png)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FiFuRSizKlmkmJ5LxiBfkIGreBdm.png)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FtNPR2rLIFDKMYz3N8gqkLBcQFnG.png)

>

### 什么是页面规则？

> 页面规则为 Cloudflare 设置提供基于 URL 的粒度控制。关于页面规则需要了解的最重要事情是，针对一个 URL 仅触发一个页面规则，因此一定要按照优先级顺序对页面规则进行排序并将最具体的页面规则放在顶部。

#### 页面规则中允许哪些模式？

> 如果使用的是转发页面规则，则可以将这些通配符映射到变量。在转发 URL 中，可以按照从左到右的顺序指定与原始 URL 中的通配符相匹配的 $1、$2，以此类推。
>
> 例如，可以将 http://_.example.com/_ 转发到 http://$2.example.com/$1.jpg。此规则将与 http://cloud.example.com/flare 相匹配，这最终将转发到 http://flare.example.com/cloud.jpg。

#### 一些有用的提示：

> 1. 如果要同时匹配 http 和 https，只需编写 example.com 即可。无需编写 example.com。
> 2. 如果要匹配域中的每个页面，则需要编写 example.com/，仅编写 example.com 是不够的。
> 3. 请参阅 [了解和配置 Cloudflare 页面规则](https://support.cloudflare.com/hc/articles/218411427) 了解有关页面规则模式的更多详细信息。

## 6. 配置 SSL(不必要)

:::info
**📢 Update 2022.09.05：这一步不是必要的，这里仅供参考！**
:::

1. 申请 www.example.com 域名的 SSL 证书；
2. 把 DNS 验证域名的记录添加到 Cloudfare 的 DNS 中；

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FiEmXujpQKK0j0p2GxzaOgTC3xwN.png)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FnOHAmNIAcEmwBD_fv3eIUBzxmiB.png)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FtxcA4iXJhT029vkw35EVkDN-2QG.png)
**注意：**
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FgrIb03h7Inot6k7fa1BHGH4G3HO.png)

# 参考资料

1. [nginx 实现两个域名之间跳转配置 - SegmentFault 思否](https://segmentfault.com/q/1010000015157572)
2. [智能云解析 DNS - 通过 Nginx 实现 URL 转发 | 百度智能云文档](https://cloud.baidu.com/doc/DNS/s/ukq4w1pji)
3. [SEO 的 301 重定向：你需要知道的一切](https://ahrefs.com/blog/zh/301-redirects/)
