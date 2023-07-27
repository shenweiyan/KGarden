---
title: WebStack-Hugo | 一个静态响应式导航主题
urlname: 2021-07-16-webstack-hugo
author: 章鱼猫先生
date: "2021-07-16 06:29:20"
updated: "2023-07-27 00:43:09"
---

:::tips
**📢 让更多人的人制作自己的导航网站。如果你觉得本主题对你有所帮助，欢迎请作者**[**喝杯咖啡**](#OhWDI)** >.<**
:::

[我给自己做了一个导航网站](https://mp.weixin.qq.com/s/gVWGjxG9qd7qSyX3N8Zgag)

**主题开源地址：**
🔗 Gitee - [**https://gitee.com/shenweiyan/WebStack-Hugo**](https://gitee.com/shenweiyan/WebStack-Hugo)
🔗 GitHub -\*\* **[**https://github.com/shenweiyan/WebStack-Hugo**](https://github.com/shenweiyan/WebStack-Hugo)

**主题展示地址：**

- [~~**https://nav.bioitee.com**~~](https://nav.bioitee.com/)~~\*\* \*\*~~**- Bio & IT 网址导航**（**该链接后续将不可用**）
- [**https://bioit.top/**](https://bioit.top/)** - WebStack-Hugo 网址导航**
- [**https://hao.bioitee.com/**](https://hao.bioitee.com/)** - 生信网址导航**
- [**https://so.gd.cn**](https://so.gd.cn/)** - 搜搜化州 | 信息导航**

# 为什么做这个网站

之所以想着要给自己倒腾一个导航网站，主要有几个原因：

- 购买了一个域名，且也备案成功了，总想折腾点跟它有关的事情；
- 经常在公司、家里（有时候还有其他的临时场所）更换电脑，每次同步书签（或者登陆一些导航网站）需要各种登陆，麻烦。

说干就干，从 [WebStack 的开源项目](https://github.com/WebStackPage/WebStackPage.github.io)开始，断断续续的折腾了好几天，终于把轮子造起来了。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FqvW72CTWIjrwsYjCWwnFqM-gD5E.png)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FoLShxuILDlQMHhB-RWREUm6uel0.png)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/Fv9ENtsjR8H7BUsQ0wNacZ2MvpmX.png)
![webstack-apple.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FljK_pCY88abSO0FnF-l_fDsF4Us.png)

# 跟其他导航网站有什么区别

这是 Hugo 版 WebStack 主题。可以借助 Github/Gitee Pages 或者云平台直接托管部署，无需服务器。

本项目是基于纯静态的网址导航网站 [webstack.cc](https://github.com/WebStackPage/WebStackPage.github.io) 制作的 [Hugo](https://gohugo.io/) 主题，其中部分代码参考了以下几个开源项目：

- [https://github.com/WebStackPage/WebStackPage.github.io](https://github.com/WebStackPage/WebStackPage.github.io)
- [https://github.com/liutongxu/liutongxu.github.io](https://github.com/WebStackPage/WebStackPage.github.io)
- [https://github.com/iplaycode/webstack-hugo](https://github.com/iplaycode/webstack-hugo)

总体说一下特点：

- 采用了一直以来最喜欢的 hugo 部署方式，方便高效。
- 主要的配置信息都集成到了 **config.toml**，一键完成各种自定义的配置。
- 导航的各个信息都集成在 **data/webstack.yml** 文件中，方便后续增删改动。

```yaml
- taxonomy: 科研办公
  icon: fas fa-flask fa-lg
  list:
    - term: 生物信息
      links:
        - title: NCBI
          logo: ncbi.jpg
          url: https://www.ncbi.nlm.nih.gov/
          description: National Center for Biotechnology Information.
        - title: Bioconda
          logo: bioconda.jpg
          url: https://anaconda.org/bioconda/
          description: "Bioconda :: Anaconda.org."
    - term: 云服务器
      links:
        - title: 阿里云
          logo: 阿里云.jpg
          url: https://www.aliyun.com/
          description: 上云就上阿里云。
        - title: 腾讯云
          logo: 腾讯云.jpg
          url: https://cloud.tencent.com/
          description: 产业智变，云启未来。
```

- 做了手机电脑自适应以及夜间模式。
- 增加了搜索功能，以及下拉的热词选项（基于百度 API）。
- 增加了一言、和风天气的 API。

# Windows 下安装部署

本安装部署在 Windows 7 x64 上测试没问题，相关操作同样适用于 Windows 10，如有任何问题，欢迎留言或者微信与我联系。

## 第一，下载 Windows 版本的 hugo

下载链接：[https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases)，在这里我们下载 [hugo_0.89.4_Windows-64bit.zip](https://github.com/gohugoio/hugo/releases/download/v0.89.4/hugo_0.89.4_Windows-64bit.zip)。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/Fkm43Bi3mi71qf16MaRdUJKjBWWx.png)

## 第二，解压

我们把 [hugo_0.89.4_Windows-64bit.zip](https://github.com/gohugoio/hugo/releases/download/v0.89.4/hugo_0.89.4_Windows-64bit.zip) 下载到 **F:\WebStack** 目录下，然后解压到当前文件夹。
![解压完成后，在该目录会多出 hugo.exe、LICENSE、README.md 三个文件](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FqBKVo5JoTzgOY1QsAczSYRmgv0k.png "解压完成后，在该目录会多出 hugo.exe、LICENSE、README.md 三个文件")

## 第三，看 hugo 安装是否安装成功

:::info
**🏷️ 温馨提示：**

Windows 命令运行窗口中可以使用 Tab 进行命令行补全，例如你当前目录下有一个 WebStack-Hugo 目录，你在命令行窗口中输入一个 w 后按下 Tab 键，命令行就会自动出现 WebStack-Hugo！

使用命令行补全，可以减少代码（或者文件名）的输入，方便快捷，又能减少错误！
:::

1. 在 Windows 中使用 **Win+R** 打开“**运行**”对话框，在对话框中输入“**cmd**”，点击确认。

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FmIW4HA2hWImM7IrtsdCtWwWqcdi.png)

2. 在 Windows 运行窗口，先切换盘符到 **F** 盘，然后进入 hugo 的解压缩目录（**F:\WebStack**），具体操作如下。

- 在光标处输入**F:**，然后按回车；

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/Fsp03gEdM-de5enU1ivNJInfUtHv.png)

- 我们就将盘符切换为 F 盘；

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FnwKctq7i0hX_iWQgymrJge8BeSX.png)

- 接着输入 **cd WebStack**，回车，就进入了 **F:\WebStack** 目录；使用 **ls** 可以看到当前目录下的文件。

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FqzY-gqYycHnCN43FlNkfWHYOvmc.png)

- 最后，输入 **hugo.exe version**，回车，如图所示，则代表安装成功。

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FvT_Dc50_SaiA8SmT6Y62ENOhE0A.png)

## 第四，下载 WebStack-Hugo

浏览器打开 [https://github.com/shenweiyan/WebStack-Hugo](https://github.com/shenweiyan/WebStack-Hugo)，点击 Code 下的 **"Download ZIP"**，把 WebStack-hugo-main.zip 下载到刚才 hugo 解压缩的目录（**F:\WebStack**）。
![2022.09.26-11.47.12.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FgBB_DzuGlOUFs1Nt4iseGF6yhN0.png)

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/Fqyg5wJJpttIrC7LVXg9bnCf6Wgv.png)

## 第五，解压，重命名

把 **WebStack-Hugo-main.zip** 解压到当前目录。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FpWA394pXnu7TxElb-XP4StAHp3T.png)
![webstack.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FtlBhjh4sOB7bHiz0eZU6zNVtfmX.png)

## 第六，安装主题

首先，进入 **F:\WebStack** 目录；

然后，创建一个 **themes** 的文件夹；
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/Fo3KJHf8MPeoCWDadu_eJcLWa6Er.png)
接着，把解压后的 WebStack-Hugo 整个文件夹移动到 themes 中。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FvzuDBR38TFwp7O094GSMOm1BPGs.png)
第四，将 themes/WebStack-Hugo/exampleSite 目录下的所有文件复制到 hugo 站点根目录（即 F:\WebStack）。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FpgjrR-xAq7fT8DRgT2iMyqGrPyZ.png)

## 第七，启动 Hugo 预览服务器

在刚才已经打开的 Windows 命令运行窗口中，使用下面的命令执行 **hugo server**，启动站点——Hugo 可以启动一个 Web 服务器，同时构建站点内容到内存中并在检测到文件更改后重新渲染，方便我们在开发环境实时预览对站点所做的更改。

```shell
hugo.exe server
```

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FjE4vkK_X_EIF9Via0K8rbuhHBFr.png)
最后，在浏览器中打开 [**http://127.0.0.1:1313/**](http://127.0.0.1:1313/)，即可看到生成的站点。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FisfN1Ak2zU2xiyjXkjE-qkyHri3.png)

# Linux 下安装部署

安装完本 WebStack-Hugo 主题后，将 exampleSite 目录下的文件复制到 hugo 站点根目录，根据需要把 config.toml 的一些信息改成自己的，导航的网址信息可通过 data 目录下 webstack.yml 修改。

具体执行步骤如下：

```bash
mkdir /home/shenweiyan/mysite
cd /home/shenweiyan/mysite

# 安装 WebStack-Hugo 主题
git clone https://github.com/shenweiyan/WebStack-Hugo.git themes/WebStack-Hugo

# 将 exampleSite 目录下的文件复制到 hugo 站点根目录
cd /home/shenweiyan/mysite
cp -r themes/WebStack-Hugo/exampleSite/* ./

# 启动 hugo 站点
hugo server
# 如果你知道你的公网 ip, 如下面的 132.76.230.31, 可以使用下面的方式执行 hugo server
hugo server --baseUrl=132.76.230.31 --bind=0.0.0.0
```

也可以参考 [@jetsung](https://github.com/jetsung) 在 [pull 15](https://github.com/shenweiyan/WebStack-Hugo/pull/15) 所用的方法安装部署：

```bash
# 创建项目
mkdir navsites
cd $_

# 初始化项目
git init

# 将 WebStack-Hugo 源下载到 themes/WebStack-Hugo 文件夹
git submodule add https://github.com/shenweiyan/WebStack-Hugo.git themes/WebStack-Hugo
cp -r themes/WebStack-Hugo/exampleSite/* ./

# 安装 hugo
go install github.com/gohugoio/hugo@latest

# 本地测试
hugo server

# 生成 docs 文件夹，将并静态内容生成至此处
hugo -D
```

# 导出 HTML 静态网页至 publishDir

Windows/Linux 下执行的 **hugo server** 命令将会通过热加载的方式临时启动一个 Hugo 服务器（Hugo 可以启动一个 Web 服务器，同时构建站点内容到内存中并在检测到文件更改后重新渲染，方便我们在开发环境实时预览对站点所做的更改），这个时候我们打开浏览器 [http://127.0.0.1:1313/](http://127.0.0.1:1313/)，就可以看到我们站点的样子了。

如果我们想要把我们的站点部署到 GitHub/Gitee Pages（或者本地的服务器），我们可以：

## 1. 生成静态页面内容

可以通过下面的命令，生成(构建)静态页面内容。

```python
hugo -D 或者 hugo --minify
```

这个命令会默认在`**public/**`目录中生成您的网站，当然您可以通过改变站点配置中的`**publishDir**`选项来配置这个输出目录。

> **🏷️Hugo 小知识 - 草案、未来和过期内容**

> Hugo 允许您在网站内容的前言设定中设置文档的`draft`，`publishdate`甚至`expirydate`字段。默认情况下，Hugo 不会发布下面内容：
>
> 1. `publishdate` 发布日期值设定在未来的内容；
> 2. `draft:true` 草案状态设置为真的内容；
> 3. `expirydate` 过期日期值设置为过去某事件的内容。
>
> 这三个可以在本地开发和部署编译时通过对`hugo`和`hugo server`分别添加如下参数来重新设定，或者在配置文件中设定对应(不包含`--`)域的 boolean 值：
>
> 1. -F, --buildFuture include content with publishdate in the future
> 2. -D, --buildDrafts include content marked as draft
> 3. -E, --buildExpired include expired content

## 2. 部署站点

把生成的 `**public/**`静态内容目录上传到 GitHub，开启 GitHub/Gitee Pages，并且绑定 cname 域名即可。

# 使用说明与技巧

这是一个开源的公益项目，你可以拿来制作自己的网址导航，也可以做与导航无关的网站。

## 左导航栏图标

左侧、顶部导航栏图标用的都是 **Font Awesome** 图标库 **v5** 版本 **Free** 的图标。链接如下：

🔗 [https://fontawesome.com/v5/search?o=r&m=free](https://fontawesome.com/v5/search?o=r&m=free)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FoHd7w72AykJGSrbzttHgGi4VqlY.png)

## 调整头部搜索栏

头部搜索栏的默认位置可以通过下面的方法进行修改。

1. 直接修改 **layouts/partials/content_search.html**，调整对应部分的位置。
2. 调整默认的搜索（即点击"常用/搜索/工具 ...." 时下指箭头的指向），把对应的 id 添加到对应的 label 里面。

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FoPMASsgrgPyFSsJy8HvPrgq8ELN.png)
![2022.09.21-10.48.55.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/Fp0fJmbfJVdg_hVP1ULdh_MmjCcX.png)

## 自定义头部导航

[WebStack-Hugo](https://github.com/shenweiyan/WebStack-Hugo) 把头部的导航菜单的各个信息集成在了 **data/header.yml** 文件中，每个人可以根据自己的需要调整。

```yaml
- item: 首页
  icon: fa fa-home
  link: "./"

- item: 作者
  icon: fa fa-book
  link: https://www.yuque.com/shenweiyan

- item: 配置
  icon: fa fa-cog
  link: ""
  list:
    - name: 源码
      url: "#"
    - name: 图标
      url: "#"
```

## 获取网站图标

[Bio & IT 网址导航](https://nav.bioitee.com/)默认使用的是个人收集的网站图标，主要是查看网站源码、百度、谷歌等途径把对应导航的图标下载下来，这个方法比较原始繁琐，适合导航不是很多的情况。

你也可以使用一为提供的的 [Favicon](https://www.iowen.cn/tag/favicon/) 图标 [api](https://www.iowen.cn/tag/api/)：[https://api.iowen.cn/doc/favicon.html](https://api.iowen.cn/doc/favicon.html)。

**接口地址：**[https://api.iowen.cn/favicon](https://api.iowen.cn/favicon)
**返回格式：**图片
**请求方式：**get
**请求示例：**

      - [https://api.iowen.cn/favicon/www.iowen.cn.png](https://api.iowen.cn/favicon/www.iowen.cn.png)
      - [https://api.iowen.cn/favicon/www.baidu.com.png](https://api.iowen.cn/favicon/www.baidu.com.png)

**请求参数说明：**

| 名称                                      | 必填 | 类型 | 说明   |                                                                |
| ----------------------------------------- | ---- | ---- | ------ | -------------------------------------------------------------- |
|                                           | url  | 是   | string | 需要获取图标的 URL 地址，如：www.iowen.cn，确保URL能够正常打开 |
| **不需要 http(s):// ，且结尾必须填 .png** |

**返回参数说明：**

| 名称 | 类型 | 说明 |
| ---- | ---- | ---- |
| 无   | 无   | 无   |

**返回示例：**返回网址图标

# 已知问题

1. 日间模式与夜间模式切换时候，头部搜索栏的背景图片切换不够流畅（个人的 js 知识有限，在 footer.html 做了一些简单的调整来实现），如果你有更好的想法，欢迎 PR 或者交流。

# 感谢墙

本主题的部分代码参考了以下几个开源项目，特此感谢。

- [WebStackPage/WebStackPage.github.io](https://github.com/WebStackPage/WebStackPage.github.io)
- [liutongxu/liutongxu.github.io](https://github.com/liutongxu/liutongxu.github.io)
- [iplaycode/webstack-hugo](https://github.com/iplaycode/webstack-hugo)

感谢 [WebStack](https://github.com/WebStackPage/WebStackPage.github.io) 的作者 [Viggo](https://twitter.com/decohack) 的肯定和[推广宣传](https://twitter.com/decohack/status/1569188705478516738)。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FuJQKMnwajVc-di8fIJO6Mu9w6a7.png)

感谢以下所有朋友对本主题所做出的贡献。
[**@yanbeiyinhanghang**](https://github.com/yinhanghang)\*\* **
[**@jetsung\*\*](https://github.com/jetsung)

# 赞赏

如果你觉得本项目对你有所帮助，欢迎请作者喝杯热咖啡 >.<
![donate-wecaht-aliapy.jpg](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/Fh453wf6DbZoquA_G6wBMWS3uYGC.jpeg)

# 反馈与交流

最后，最重要的，秉承 WebStack 的宗旨，这是一个开源的公益项目，你可以拿来制作自己的网址导航，也可以做与导航无关的网站。

WebStack 有非常多的魔改版本，这是其中一个。

**如果你对本主题进行了一些个性化调整，欢迎在本项目中 **[**issue**](https://github.com/shenweiyan/WebStack-Hugo/issues)** 中一起分享交流！**

**如果参考本主题构建了属于你自己的网址导航，欢迎在本评论区（或源码 **[**issue**](https://github.com/shenweiyan/WebStack-Hugo/issues)** 区）留下你网站的访问链接！**

**最后，欢迎你来信（或点击**[**这里**](https://wj.qq.com/s2/11644593/a6db/)**）和本站点交换友链 >.<**
