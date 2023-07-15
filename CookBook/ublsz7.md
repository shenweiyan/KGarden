参考 Anaconda 官方文档《[Using R language with Anaconda](https://docs.anaconda.com/anaconda/user-guide/tasks/using-r-language/)》安装 R-4.0.2：

```r
conda create -n r-4.0.2 r-essentials r-base==4.0.2
```

## 1. unable to open connection to X11 display

```r
> plot(1:10)
Error in .External2(C_X11, d$display, d$width, d$height, d$pointsize,  :
  unable to start device X11cairo
In addition: Warning message:
In (function (display = "", width, height, pointsize, gamma, bg,  :
  unable to open connection to X11 display ''
> capabilities()
       jpeg         png        tiff       tcltk         X11        aqua
       TRUE        TRUE        TRUE        TRUE       FALSE       FALSE
   http/ftp     sockets      libxml        fifo      cledit       iconv
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
        NLS     profmem       cairo         ICU long.double     libcurl
       TRUE        TRUE        TRUE        TRUE        TRUE        TRUE
> Sys.getenv(c("DISPLAY"))
[1] "localhost:22.0"
>
> options(bitmapType='cairo')
> png(file="test.png", width = 480, height = 480)
> plot(1:10)
> dev.off()
png
  2
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1578988915325-bfbf1c72-f5a2-488a-a023-4006145b0c3c.png#align=left&display=inline&height=518&originHeight=518&originWidth=496&size=38389&status=done&style=none&width=496)

## 2. 命令行下 R 画图无法弹出图形界面结果

正常情况下，capabilities() 如果现实 X11 为 TRUE，执行 plot(1:10) 时会在 windows 下弹出一个绘图的结果图形界面。
![xshell-4-x11.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1600147909687-f14cbdb3-49cd-413f-89cd-c1a3ae92d210.png#align=left&display=inline&height=728&originHeight=728&originWidth=1175&size=97242&status=done&style=none&width=1175)

如果你的 capabilities() 如果现实 X11 为 FALSE，需要进行如下操作。

首先，使用 root 安装下面一些 X11 依赖：

```bash
yum install xorg-x11-* libX11-* libXt-*
```

其次，在你的 XShell 中配置 X11 转发功能。如果你用的是 MobaXterm，则跳过这一步设置。
![xshell-x11.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1600141383523-1b3ae8cf-6fee-48ae-b1dc-5fc6a2d31cb7.png#align=left&display=inline&height=686&originHeight=686&originWidth=681&size=51910&status=done&style=none&width=681)
