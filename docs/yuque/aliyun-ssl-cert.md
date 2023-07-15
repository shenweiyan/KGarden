## 一、阿里云证书资源包初体验

2021 年 1 月 13 左右，收到阿里云提示个人 SSL 证书即将过期，需要续费。于是登陆阿里云，在更新个人域名的 SSL 证书的时候发现，阿里云的 SSL 证书貌似都已经找不到了！取而代之的是出现了一个叫 "证书资源包" 的玩意。

> 自 2021 年 1 月 1 日起，每个经过实名认证的个人或企业主体可以在一个自然年内，通过 SSL 证书服务提供的免费证书扩容包，一次性领取 20 张免费 DV 证书。获取免费 DV 证书申请额度后，您需要通过 SSL 证书控制台提交证书申请，申请审核通过后，您将获得 CA 中心签发的证书。
>
> SSL 证书服务的 20 张免费证书资源包主要以公益目的，提供给阿里云用户使用，便于个人或企业用户在网站建设之初或业务需要使用 HTTPS 通信时进行测试。

第一次用，简直是不知道云里雾里，摸索半天，好不容易购买了全年 20 个免费证书后，然后去证书申请，居然还申请不了（“证书申请”的“确定”按钮没法使用）！一开始以为是要购买托管服务才能申请证书!!!
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612409640944-15993de6-3d48-45b0-85ab-96c7e061c6d9.png#averageHue=%23f5f7f6&height=506&id=nOJXE&originHeight=506&originWidth=623&originalType=binary&ratio=1&rotation=0&showTitle=false&size=144789&status=done&style=none&title=&width=623)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612409659199-46ae8f44-2a10-405b-bd88-eccc81e0a3aa.png#averageHue=%23f3f4f1&height=767&id=UGJ2U&originHeight=767&originWidth=937&originalType=binary&ratio=1&rotation=0&showTitle=false&size=223503&status=done&style=none&title=&width=937)

一年几百大洋的托管服务，真心表示不香！于是开始辗转腾讯云。

时隔将近一个月，再次回来看了一下阿里云的证书服务，发现免费证书居然可以不用选择托管服务去申请了！
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612410701531-ffbb3cc9-eebe-4a6f-8762-6b03b2def88c.png#averageHue=%23b5b5b5&height=663&id=kEhhR&originHeight=663&originWidth=1192&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72981&status=done&style=none&title=&width=1192)

我不知道我在 2021 年 1 月 13 登陆阿里云申请免费证书时，无法正常申请到底是一个 BUG，还是阿里云团队故意为之，经历这个事情后，对阿里云的好感的确不如以前。

- 阿里云有不少产品，新产品也好，老产品也罢，细分太深，用户很多时候不知道怎么去操作！
- 阿里云有不少优惠活动，但对于老用户，很不友好！

---

## 二、阿里云证书资源包申请免费 SSL 流程

2021 阿里云 SSL 免费证书购买地址又变了，为了解决免费证书近期存在的吊销、统计等问题，自 2021 年起，免费证书申请将切换到证书资源包下。阿里云 SSL 免费证书申请分为两个步骤，先支付 0 元购买免费证书扩容包，然后在证书资源包控制台申请，新手站长网来详细说下：

### 2.1 购买证书资源包中的免费证书扩容包

阿里云免费 SSL 证书需要支付 0 元，购买云盾证书资源包中的免费证书扩容包：

1. 打开[阿里云 SSL 证书选购页面](https://www.xinshouzhanzhang.com/url/cas/)，点击“选购证书”；
2. 商品类型选择“云盾证书资源包”；
3. 资源包规格选择“免费证书扩容包”；
4. 资源包个数选择“20”。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/126032/1612420454288-80e615ae-3b98-4076-9ca7-2eeaf7e8a46b.jpeg#averageHue=%23fdc995&height=644&id=Wx1nF&originHeight=644&originWidth=1000&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=1000)
如上图，选择完毕后总配置费用为 0 元，然后点“立即购买”支付 0 元即可。至此完成了阿里云 SSL 免费证书资源包的购买，之后还要在控制台输入域名等信息来申请 SSL 证书文件。

### 2.2 控制台证书申请

#### 1. [登录到 SSL 证书管理控制台](https://www.xinshouzhanzhang.com/url/console_ssl/)

#### 2. 选择左侧栏“证书资源包”

可以看刚刚 0 元购买的证书资源包，免费证书资源包可申请 20 个证书，如下图：
![ssl-a1.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612417940200-84cb5a69-9397-446b-a4eb-40785e8e911e.png#averageHue=%23f9f8f8&height=417&id=N0Vyq&originHeight=417&originWidth=897&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38019&status=done&style=none&title=&width=897)

#### 3. 点击“证书申请”

在"证书资源包"页面，点击“证书申请”，如下图：
![ssl-a2.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612418020988-6e5fbd87-f4bd-4f1a-afab-3cc775c064b1.png#averageHue=%23fafaf9&height=515&id=LtDJ4&originHeight=515&originWidth=636&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35235&status=done&style=none&title=&width=636)
目前阿里云免费证书是 DigiCert 品牌的，之前是赛门铁克（Symantec）

#### 4. 证书申请

规格选择“单域名”，然后点“确定”，可以看到证书状态为“待申请”。
![ssl-a3.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612418679406-79dfe141-1143-46ad-a00f-4e4a5ff9946d.png#averageHue=%23fcfbfb&height=230&id=uv2O5&originHeight=230&originWidth=955&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18124&status=done&style=none&title=&width=955)

#### 5. 填写“证书申请”申请表单

![ssl-ab4.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612419276729-373596e6-3f29-4187-9650-494445a9ce79.png#averageHue=%23fbfaf7&height=586&id=uGgmi&originHeight=586&originWidth=735&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38873&status=done&style=none&title=&width=735)

- 证书绑定域名：填写你要申请 SSL 的域名，如 www.bioitee.com
- 域名验证方式：如果你的域名在本账号下，可以选择自动 DNS 验证，阿里云系统会为你的域名添加\_dnsauth 的 TXT 解析记录；如果域名不在本账号下，可以选择手工 DNS 验证或文件验证。详细教程参考：[阿里云 SSL 证书申请域名验证选择及操作流程](https://www.xinshouzhanzhang.com/sslyumingyanzheng.html)
- 联系人：填写联系人信息，可以新建也可以选择之前保存的
- 所在地：根据实际情况选择即可
- CSR 生成方式：CSR 文件是您的公钥证书原始文件，包含了您的服务器信息和您的单位信息，需要提交给 CA 认证中心审核。建议默认即可，使用系统创建的 CSR，避免因内容不正确而导致的审核失败。

#### 6. 域名验证，提交审核

SSL 证书验证可选择域名 DNS 解析验证，也可以选择文件验证，bioitee 网以域名解析验证为例：
![ssl-a5.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612419965983-08129eec-c6a5-4d5f-bf24-f64b722c0fc7.png#averageHue=%23fbfbfa&height=429&id=AFrz3&originHeight=429&originWidth=717&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23296&status=done&style=none&title=&width=717)
bioitee.com 的域名在阿里云账号下，所以阿里云系统会自动添加 TXT 记录。如果你的域名不在当前阿里云账号下，可以根据提示手动添加 DNS 域名解析记录。SSL 证书域名验证记录类型为 TXT，主机记录\_dnsauth，记录值根据提示复制填写。

然后点“验证”，争取无误的话会提示“域名验证成功，域名验证记录在证书签发后再删除，否则会因没有解析记录导致证书签发失败。”
![ssl-a6.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612420036891-d06dda6c-e79e-41c6-b566-489fba75c8f4.png#averageHue=%23fafaf9&height=535&id=sYmDE&originHeight=535&originWidth=735&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32487&status=done&style=none&title=&width=735)
验证通过后，点“提交审核”等待。会提示“已经成功提交到 CA 公司，请您保持电话畅通，并及时查阅邮箱中来自 CA 公司的电子邮件。”如下图：
![ssl-a7.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612420090246-c3316155-a8af-44c9-99d0-91585c8b9710.png#averageHue=%23faf9f8&height=157&id=X9uFg&originHeight=157&originWidth=573&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6806&status=done&style=none&title=&width=573)
一般等待几秒钟即可颁发证书。

#### 7. SSL 证书下载

SSL 证书管理控制台的证书资源包中，点“下载”，如下图：
![ssl-a8.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612420151686-f6f66fc3-18ba-4c06-af7d-41768f14df67.png#averageHue=%23fcfbfb&height=228&id=PbYux&originHeight=228&originWidth=993&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19777&status=done&style=none&title=&width=993)
阿里云提供不同服务器类型证书，如 Tomcat、Apache、Nginx、IIS、JKS、其他及根证书下载，根据你的云服务器 Web 服务器环境来选择对应的证书下载：
![ssl-a9.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1612420227914-44a3c09c-3de4-49a3-a796-b3d38daca1e6.png#averageHue=%23fbfbfb&height=526&id=OLKvs&originHeight=526&originWidth=623&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27279&status=done&style=none&title=&width=623)
根据服务器类型选择证书下载

bioitee 选择的是 Nginx 证书，SSL 证书文件自动下载保存到本地电脑中，然后在相应的地方复制粘贴或上传使用证书文件。

> 以上内容来源于新手站长网分享的[2021 年最新的阿里云 SSL 免费证书申请教程](https://www.xinshouzhanzhang.com/aliyunssl2021.html)，部分截图根据实际情况以作调整。

最后，再强调一下，目前免费证书在云盾证书资源包的免费证书扩容包中获取，每个阿里云账号限制 20 个免费证书。

## 三、参考资料

1. [2021 阿里云 SSL 免费证书申请教程（云盾证书资源包） - 新手站长网](https://www.xinshouzhanzhang.com/aliyunssl2021.html)
2. [申请免费 DV 证书 - SSL 证书 - 阿里云](https://help.aliyun.com/document_detail/156645.html)
