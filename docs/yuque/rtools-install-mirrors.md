## Rtools 是什么

在 windows 使用 R，尤其是安装 R 包的时候，经常会遇到一些 Rtools 的问题。Rtools 作用很大，但我们一般不怎么会直接使用。

> Rtools provides a toolchain for Windows platform that work well with R. It mainly includes GNU make, GNU gcc, and other utilities commonly used on UNIX-ish platform. R statistical language & environment is that it is open source and cross-platform.

## Rtools 安装

在 RStudio 中安装 shiny 包的时候，就出现了要安装 Rtools 的 warning，提示信息中还给出了下载的链接地址。但问题是  [https://cran.rstudio.com/bin/windows/Rtools/](https://cran.rstudio.com/bin/windows/Rtools/)  是位于国外的服务器，下载速度慢的令人发指。
![Rtools.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1590377162275-c77ae6a3-91c4-4e5d-8605-2aa2d0bf68f1.png#align=left&display=inline&height=684&originHeight=684&originWidth=775&size=78376&status=done&style=none&width=775)

### 方法一

使用清华大学的 CRAN 镜像下载 Rtools，镜像地址：[https://mirrors.tuna.tsinghua.edu.cn/CRAN/](https://mirrors.tuna.tsinghua.edu.cn/CRAN/)，如果你记不住这一串常常地址，可以从 CRAN 官网点击进去。
![cran-mirrors.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1590377963014-7ba95159-9f98-40ef-951b-7dfbc1a745e3.png#align=left&display=inline&height=550&originHeight=550&originWidth=1265&size=99115&status=done&style=none&width=1265)

在清华大学的 CRAN 页面选择  [Download R for Windows](https://mirrors.tuna.tsinghua.edu.cn/CRAN/bin/windows/)，在出现的 R for Windows 页面选择 Rtools：
![r-for-win.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1590378374652-a951ae74-cf74-4f37-932c-5ceb5e16b403.png#align=left&display=inline&height=571&originHeight=571&originWidth=1244&size=100897&status=done&style=none&width=1244)
![rtootls.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1590378321910-6c3ad2ed-b8d7-4c29-8d24-22228b62bc49.png#align=left&display=inline&height=571&originHeight=571&originWidth=1244&size=105077&status=done&style=none&width=1244)

在 Rtools 选择下载最新版本的 Rtools，或者下载 R 版本对应 Rtools：
![rtools-main.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1590378749751-7383d259-d5bc-4bff-9e2d-5c2911333bff.png#align=left&display=inline&height=587&originHeight=587&originWidth=1244&size=114440&status=done&style=none&width=1244)
![rtools-download.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1590378630208-b49ef88f-6e13-41fd-a52c-7b4d9fdc3f29.png#align=left&display=inline&height=611&originHeight=611&originWidth=1244&size=119245&status=done&style=none&width=1244)\*\*

:::tips
**注意！！！**
\*\*
**当我们通过下面\*\***的链接直接访问清华大学 CRAN 的 Rtools 时：\***\* **[**https://mirrors.tuna.tsinghua.edu.cn/CRAN/bin/windows/Rtools/**](https://mirrors.tuna.tsinghua.edu.cn/CRAN/bin/windows/Rtools/)\*\* \*\*
\*\*
**这里面有一个问题需要注意，即点击 "this page" 访问 R 其他版本对应的 Rtools 时会默认跳转到 CRAN 官网默认的\*\***页面！**
[**https://cran.r-project.org/bin/windows/Rtools/history.html**](https://cran.r-project.org/bin/windows/Rtools/history.html)

**正常情况下应该是跳转到清华大学的 Rtools history 页面\*\***才对！！！**
[**https://mirrors.tuna.tsinghua.edu.cn/CRAN/bin/windows/Rtools/history.html**](https://mirrors.tuna.tsinghua.edu.cn/CRAN/bin/windows/Rtools/history.html)
:::

[Building R for Windows](https://mirrors.tuna.tsinghua.edu.cn/CRAN/bin/windows/Rtools/history.html)

![bug-cran-rtools.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1590384999723-97b57d62-49f8-4a82-8696-42cd8827bccf.png#align=left&display=inline&height=402&originHeight=402&originWidth=949&size=61596&status=done&style=none&width=949)

最后，安装 Rtools，这没什么好说的，按照默认设置一路 Next 下去，最后就安装成功，这里只强调一点是把勾选添加 rtools 到环境变量中。
![add-rtools-to-path.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1590385563974-955f6bbb-36f3-4768-bfbf-cb7632c015dc.png#align=left&display=inline&height=398&originHeight=398&originWidth=513&size=22319&status=done&style=none&width=513)

### 方法二

第二种方法，我是在网络上看到，大家也可以参考一下。

```r
options("repos"=c(CRAN="https://mirrors.tuna.tsinghua.edu.cn/CRAN/"))
install.packages('installr')
library(installr)
install.Rtools()
```

这是通过设置清华大学 CRAN 后，先安装  installr 包，再通过这个包直接下载安装 Rtools，速度上同样也是杠杠的。\*\*
