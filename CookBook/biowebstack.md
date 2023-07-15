前不久在 [@王诗翔(shixiangwang)](/shixiangwang) GitHub 的讨论组上看到了类似的这个讨论 ["将重点资源整理成一个生信导航网页"](https://github.com/ShixiangWang/self-study/issues/65)，加上 [@郑宝童(btzheng)](/btzheng) 曾经也做了一个[生信极客部落网址导航](https://zhengbaotong.gitee.io/biogeekgps/)（旨在搭建一个生信专属网址导航），好像也有一段时间没更新过了。
![@王诗翔 Bioinformatics Guide Site](https://cdn.nlark.com/yuque/0/2022/png/126032/1653529838247-f378efa5-31e1-47d0-9afd-599bffc482fd.png#clientId=uf9c1706d-68c8-4&from=paste&height=820&id=ua9ed607b&originHeight=820&originWidth=1293&originalType=binary&ratio=1&rotation=0&showTitle=true&size=428765&status=done&style=none&taskId=u7fbd0adb-64de-4090-bb10-a11d3bc26ef&title=%40%E7%8E%8B%E8%AF%97%E7%BF%94%20Bioinformatics%20Guide%20Site&width=1293 "@王诗翔 Bioinformatics Guide Site")
![@郑宝童 生信极客部落网址导航](https://cdn.nlark.com/yuque/0/2022/png/126032/1653529945324-62b18667-cca9-4277-90e0-f52fea03f9bf.png#clientId=uf9c1706d-68c8-4&from=paste&height=820&id=ubced8ddc&originHeight=820&originWidth=1293&originalType=binary&ratio=1&rotation=0&showTitle=true&size=128999&status=done&style=none&taskId=ua32a0b97-1310-4126-bfa7-1e6c6e9870b&title=%40%E9%83%91%E5%AE%9D%E7%AB%A5%20%E7%94%9F%E4%BF%A1%E6%9E%81%E5%AE%A2%E9%83%A8%E8%90%BD%E7%BD%91%E5%9D%80%E5%AF%BC%E8%88%AA&width=1293 "@郑宝童 生信极客部落网址导航")
于是有了 **生信重点资源+WebStack-Hugo=生信导航** 的初步的想法。[WebStackPage](https://github.com/WebStackPage) 本身是一个由 [viggo](https://www.viggoz.com/) 开发的一个网址导航开源项目，有许多的魔改版本，而 [WebStack-Hugo](https://github.com/shenweiyan/webstack-hugo) 则是本人基于 Hugo 进行修改调整的其中一个主题。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1666926036064-2ab1fcf4-f685-4a71-ae2e-02f3b78944aa.png#clientId=u5d268128-f9a7-4&from=paste&height=862&id=u9e67b82d&originHeight=862&originWidth=1219&originalType=binary&ratio=1&rotation=0&showTitle=false&size=134400&status=done&style=none&taskId=uefec2256-ce20-48a5-9e59-b7c665069db&title=&width=1219)
有了现成的主题，又有了前人的一些资源整理（轮子都已经有了，就缺组装），于是在 GitHub 上开始了 [BioWebStack](https://github.com/bioitee/BioWebStack) 这一个将重点资源整理成一个生信导航网页的仓库（目前已初步把站点搭建了起来，资源内容还在更新中）。

:::success

- **GitHub 开源地址：**[https://github.com/bioitee/BioWebStack](https://github.com/bioitee/BioWebStack)
- **BioWebStack 站点地址：**[https://bioit.top/](https://bioit.top/)（备用链接：[https://hao.bioitee.com/](https://hao.bioitee.com/)）
  :::

![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1666925981245-faaa9ae2-40fa-4185-8beb-75f26a4b9e8d.png#clientId=u5d268128-f9a7-4&from=paste&height=1039&id=u566db78a&originHeight=1039&originWidth=1736&originalType=binary&ratio=1&rotation=0&showTitle=false&size=571121&status=done&style=none&taskId=u2d253176-5544-41ec-a09f-a297f7343e6&title=&width=1736)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1666925927114-5639f8df-80e5-4a9f-a32e-3767cf5f095c.png#clientId=u5d268128-f9a7-4&from=paste&height=1039&id=u75d44476&originHeight=1039&originWidth=1736&originalType=binary&ratio=1&rotation=0&showTitle=false&size=556964&status=done&style=none&taskId=ud836c23f-9e33-47d8-bbf9-da57647fdf8&title=&width=1736)

**关于 BioWebStack 的一些特点和说明：**

- 致力于将一些重点资源整理成一个生信导航网页。
- 该网站用于个人学习和科研。
- 长期开源，欢迎大家提交错误报告以及宝贵的资源。
- 采用了一直以来最喜欢的 Hugo 部署方式，方便高效。
- 主要的配置信息都集成到了 **config.toml**，一键完成各种自定义的配置。
- 导航的各个信息都集成在 **data/webstack.yml** 文件中，方便后续增删改动。
- 无需服务器，GitHub Pages/Webify Pages/Cloudfare Pages 均可部署。

如果你也是做生信研究，如果你也正好喜欢学习生信，如果你一直苦于收藏夹越来越多，很难找到某个不常用的网站，那希望这个网站能给你带来一些作用。
