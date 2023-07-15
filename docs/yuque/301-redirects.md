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
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1637040961966-ff66c403-303d-42ad-a4ed-6d20f71a5466.png#averageHue=%23fefefd&clientId=uaf222485-97ae-4&from=paste&height=238&id=ub5f56ed4&originHeight=238&originWidth=1348&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28718&status=done&style=none&taskId=u9daa7931-ddc7-4c6e-ba55-23fbf8ab02e&title=&width=1348)

## 2. SSL 证书

申请免费证书，具体操作可以自行百度。以腾讯云为例：[https://console.cloud.tencent.com/ssl](https://console.cloud.tencent.com/ssl)。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1637041374522-15215315-4190-4845-8a40-ef422179e264.png#averageHue=%23fcfcfc&clientId=uaf222485-97ae-4&from=paste&height=583&id=u85985aec&originHeight=583&originWidth=1380&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87121&status=done&style=none&taskId=u6d80be60-8bf3-4473-bae6-065a5b93ca7&title=&width=1380)
根据截图，一步步点击操作。申请完成后，把证书下载并上传到你的服务器。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1637041621817-c9b376fb-5bfb-426d-bd00-6452e743f466.png#averageHue=%23ade1f0&clientId=uaf222485-97ae-4&from=paste&height=185&id=u65a43a05&originHeight=185&originWidth=1373&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25423&status=done&style=none&taskId=u2ec94c1e-c87c-4ce7-86a8-7531f96ac03&title=&width=1373)

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

![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1643181795250-a5687a6b-44a9-48d7-94e2-0ff6c48a7a07.png#averageHue=%23e7bb89&clientId=uf85dfbeb-99c4-4&from=paste&height=333&id=uaea79516&originHeight=333&originWidth=1028&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47662&status=done&style=none&taskId=u47af25e5-b74d-498a-904a-c5ec1547b2b&title=&width=1028)
添加完站点后，可以选择 **Free 计划**，然后点击继续：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1643181876312-e391cdd1-bb8d-4081-9a4e-842ad8874566.png#averageHue=%23faf9f8&clientId=uf85dfbeb-99c4-4&from=paste&height=555&id=u04e42c28&originHeight=555&originWidth=1028&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65900&status=done&style=none&taskId=u839e6d87-1938-4137-87f2-cceaa1ed0e6&title=&width=1028)
点击继续后，Cloudflare 会自动扫描你对应域名的一些解析记录：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1649400235588-ee9172ac-d069-44e7-b66d-600a6af0b1ea.png#averageHue=%23fafaf9&clientId=u0fc2d6df-8811-4&from=paste&height=668&id=u1b4409ce&originHeight=668&originWidth=1107&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64019&status=done&style=none&taskId=u7bc07247-2db7-4a45-ac18-ace06bd61a4&title=&width=1107)
我们可以直接选择 **"继续"**。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1643182025314-2bc49d26-ade5-4aaf-97f5-1e1301c09408.png#averageHue=%23faf9f9&clientId=uf85dfbeb-99c4-4&from=paste&height=825&id=uffb95351&originHeight=825&originWidth=1028&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95866&status=done&style=none&taskId=u60e0b392-b4f3-424c-a324-31266d3ca0f&title=&width=1028)

## 2. 修改域名 DNS

首先，我的域名是在腾讯云注册的，可以去腾讯云控制台 **"我的域名"** 中直接修改 DNS：

```python
# 添加 Cloudflare 名称服务器
imani.ns.cloudflare.com
caroline.dnspod.net
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1643181320734-08faff0b-6302-4aa0-a5a9-d0a8847868fc.png#averageHue=%23e0c9a2&clientId=uf85dfbeb-99c4-4&from=paste&height=811&id=u81dd9c54&originHeight=811&originWidth=910&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57201&status=done&style=none&taskId=u7723e3b3-fe37-41f5-ba76-34416fca1e6&title=&width=910)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1643182127916-818ff37a-2994-4ca6-b650-5d295eee965c.png#averageHue=%23d4c9b9&clientId=uf85dfbeb-99c4-4&from=paste&height=506&id=u019b6048&originHeight=506&originWidth=597&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32581&status=done&style=none&taskId=u1da97065-31aa-4a8c-83c9-a42ace0863e&title=&width=597)

## 3. 完成 Cloudflare 添加站点

可以把后面快速指南的这几个配置都勾选。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1643182399735-ff607e75-0040-497f-b5ec-4275f8bbe2e5.png#averageHue=%23fcfbfa&clientId=uf85dfbeb-99c4-4&from=paste&height=607&id=uf8d3cf7a&originHeight=607&originWidth=638&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37214&status=done&style=none&taskId=u360c3d43-3c19-44da-9142-cb2d0629670&title=&width=638)
等待几分钟就可以看到你的域名站点已经添加到 Cloudflare 上了！
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1643182518799-c126a319-776f-426d-ae4a-8c18b1be5073.png#averageHue=%23fcfcfc&clientId=uf85dfbeb-99c4-4&from=paste&height=463&id=u4f2a0821&originHeight=463&originWidth=1004&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26074&status=done&style=none&taskId=u6018c27f-a37a-4851-9cfa-9f6acfd9fe8&title=&width=1004)

## 4. 设置 DNS 记录

> The first thing you will need is a DNS record for **@**, **www** and any other subdomains you want to redirect, set to ![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1662348295530-d1cb3b58-35bb-4783-b1c3-45d0cb5fc11d.png#averageHue=%237a470c&clientId=u3676b463-d195-4&from=paste&height=16&id=u0c517c2a&originHeight=100&originWidth=100&originalType=url&ratio=1&rotation=0&showTitle=false&size=4518&status=done&style=none&taskId=u8d360cb9-3931-4b92-816a-6273fe78722&title=&width=16). This can point to any IP address as the redirection page rule will execute first. I would recommend pointing them to 192.0.2.1 , a dummy IP.
>
> From：[https://community.cloudflare.com/t/redirecting-one-domain-to-another/81960](https://community.cloudflare.com/t/redirecting-one-domain-to-another/81960)

在配置 Cloudflare 站点的页面规则前，你需要把该域名的 **@**，**www** 或者其他你想要进行重定向的子域名添加到 DNS 记录中，这个记录的值可以指向任何 IP 地址，因为重定向页面规则将首先执行。我建议将它们指向 192.0.2.1 ，一个虚拟 IP。

在这里，我们以 **@** 和 **note** 子域名为例，添加 DNS 记录，先让它们指向一个虚拟 IP。
![以 ncbix.com 和 note.ncbix.com 为例，均重定向到 https://www.yuque.com/shenweiyan 页面](https://cdn.nlark.com/yuque/0/2022/png/126032/1662349252367-99b78bf7-aa47-4ec8-816a-bc0b1b4d8719.png#averageHue=%23f9f9f8&clientId=u3676b463-d195-4&from=paste&height=278&id=uebf1ccd0&originHeight=278&originWidth=1041&originalType=binary&ratio=1&rotation=0&showTitle=true&size=21887&status=done&style=none&taskId=ub971a6db-7119-44ef-a535-5e8a3ff4467&title=%E4%BB%A5%20ncbix.com%20%E5%92%8C%20note.ncbix.com%20%E4%B8%BA%E4%BE%8B%EF%BC%8C%E5%9D%87%E9%87%8D%E5%AE%9A%E5%90%91%E5%88%B0%20https%3A%2F%2Fwww.yuque.com%2Fshenweiyan%20%E9%A1%B5%E9%9D%A2&width=1041 "以 ncbix.com 和 note.ncbix.com 为例，均重定向到 https://www.yuque.com/shenweiyan 页面")

## 5. 配置 Cloudflare 站点页面规则

首先，在 Cloudflare 的主页上点击对应的站点，选择 **"页面规则"**，点击。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1643182796686-98f83df4-fba6-4c3a-8a84-5a8ba5498408.png#averageHue=%23fafaf9&clientId=uf85dfbeb-99c4-4&from=paste&height=730&id=u82a5ccd7&originHeight=730&originWidth=1028&originalType=binary&ratio=1&rotation=0&showTitle=false&size=115085&status=done&style=none&taskId=ud03bca0d-393e-4c84-a684-005838f33e1&title=&width=1028)
点击**"创建页面规则"**：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1643182875178-8c3408c7-1841-4378-96fc-b52990fff40f.png#averageHue=%23f7f6f6&clientId=uf85dfbeb-99c4-4&from=paste&height=486&id=u7e361c20&originHeight=486&originWidth=743&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36975&status=done&style=none&taskId=u2e8738d7-2e4c-4956-97d8-9575925f07e&title=&width=743)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1643182958149-bfa0363d-f34c-4fa0-995c-aa7e0db4622b.png#averageHue=%23f9f9f8&clientId=uf85dfbeb-99c4-4&from=paste&height=508&id=u58818595&originHeight=508&originWidth=734&originalType=binary&ratio=1&rotation=0&showTitle=false&size=31928&status=done&style=none&taskId=u6d68c9c3-e126-4b78-b79b-1cafce1b788&title=&width=734)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1662349011274-c9ffd263-54fa-4eb3-bc54-9899732b7150.png#averageHue=%23faf9f8&clientId=u3676b463-d195-4&from=paste&height=546&id=u092369ec&originHeight=546&originWidth=877&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42044&status=done&style=none&taskId=u8ff18b46-1f96-41b6-b801-69c21661763&title=&width=877)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1662349183068-9ed6e857-6b78-4be0-bc7f-a69663bf8276.png#averageHue=%23f8f8f7&clientId=u3676b463-d195-4&from=paste&height=614&id=u284a7548&originHeight=614&originWidth=1077&originalType=binary&ratio=1&rotation=0&showTitle=false&size=61499&status=done&style=none&taskId=u5d5e1d09-2093-4dcc-8f46-936a2a31521&title=&width=1077)

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

![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1649396905153-c2950201-1029-48cb-b7b8-c97c31f85a9d.png#averageHue=%23e1e8cb&clientId=u5ab78f5d-c256-4&from=paste&height=605&id=ua5eca405&originHeight=605&originWidth=1117&originalType=binary&ratio=1&rotation=0&showTitle=false&size=61056&status=done&style=none&taskId=u6886d9d8-e90d-47be-bcdf-d311e3dc150&title=&width=1117)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1649396993249-003fd4d3-11b1-41a7-ac02-389af190353c.png#averageHue=%23e8e1af&clientId=u5ab78f5d-c256-4&from=paste&height=738&id=u953fe6ed&originHeight=738&originWidth=1491&originalType=binary&ratio=1&rotation=0&showTitle=false&size=93151&status=done&style=none&taskId=ub5e78dca-2d78-4ebe-8509-a58040fcd97&title=&width=1491)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1649397071292-110075df-b035-4030-9479-7a2cbec4eaaa.png#averageHue=%23f9f9f8&clientId=u5ab78f5d-c256-4&from=paste&height=376&id=u68431020&originHeight=376&originWidth=1069&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25848&status=done&style=none&taskId=u1736235a-217f-4b11-a212-c6540c9ef25&title=&width=1069)
**注意：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1649397852133-b2b43018-a517-43f5-8eb7-167316f178fc.png#averageHue=%23f7f6f6&clientId=u5ab78f5d-c256-4&from=paste&height=215&id=uebf3a76f&originHeight=215&originWidth=745&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24412&status=done&style=stroke&taskId=u9d1c0e09-6cf9-4b1a-89c5-bee869c8660&title=&width=745)

# 参考资料

1. [nginx 实现两个域名之间跳转配置 - SegmentFault 思否](https://segmentfault.com/q/1010000015157572)
2. [智能云解析 DNS - 通过 Nginx 实现 URL 转发 | 百度智能云文档](https://cloud.baidu.com/doc/DNS/s/ukq4w1pji)
3. [SEO 的 301 重定向：你需要知道的一切](https://ahrefs.com/blog/zh/301-redirects/)
