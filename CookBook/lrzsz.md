# 简介

**rz**，**sz** 是 GNU 免费工具 **lrzsz** 的两个子命令行程序，它们是 Linux/Unix 同 Windows 进行 ZModem 文件传输的命令行工具，但是 Windows 端需要支持 ZModem 的 telnet/ssh 客户端，比如 Xshell 或者 SecureCRT 下可以使用。

官网：[https://www.gnu.org/software/lrzsz/](https://www.gnu.org/software/lrzsz/)

网络有些文章说 Putty 和 MobaXterm 无法使用 lrzsz，其实通过安装插件 MobaXterm 是可以使用的，但体验没有 Xshell 或者 SecureCRT 好。

> lrzsz is a unix communication package providing the [XMODEM, YMODEM](ftp://ftp.std.com/obi/Standards/FileTransfer/YMODEM8.DOC.1.Z) [ZMODEM](http://www.easysw.com/~mike/serial/zmodem.html) file transfer protocols. lrzsz is a heavily rehacked version of the last public domain release of [Omen Technologies](http://www.omen.com/) rzsz package, and is now [free software](http://www.gnu.ai.mit.edu/philosophy/free-sw.html) and released under the [GNU General Public Licence](http://www.gnu.ai.mit.edu/copyleft/gpl.html).

> lrzsz 是一个提供 XMODEM、YMODEM、ZMODEM 文件传输协议的 unix communication package。 lrzsz 是 Omen Technologies rzsz 软件包最后一个公共领域版本的重整版，现在是免费软件，并在 GNU 通用公共许可证下发布。

这两个命令也很好区分：

- sz：将选定的文件发送（send）到本地机器，s 作为 send 的简写；
- rz：运行该命令会弹出一个文件选择窗口，从本地选择文件上传到服务器(receive)，r 作为 receive 的简写。

# 特点

Features of lrzsz：

- 非常便携，使用 GNU autoconf 自动配置。
- 崩溃恢复（crash recovery）。
- 高达 8KB 的块大小 (ZMODEM8K)。
- 国际化（使用 GNU gettext）。德语翻译的程序输出（German translation of the programs output exists）。
- 远比原始来源更安全。
- 高性能。`make vcheck-z`可以查看 BPS 速率 - 我最近看到每秒 1.4 MB 通过管道传输大文件 (on a I586/133 system. Beat that!)。
- 良好的块大小计算（尝试根据发生的错误数量计算最佳块大小）。
- 它是免费软件。

# 安装

安装也比较容易，这里以 openEuler 系统为例：

```bash
yum install lrzsz
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1636337094040-cac833f2-89da-49f5-83fc-ab69b35f5044.png#clientId=ua5c3dec7-70c2-4&from=paste&height=346&id=u7baaf5e0&originHeight=346&originWidth=790&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30656&status=done&style=none&taskId=uc250383b-0d97-48aa-9906-75ef3da0195&title=&width=790)

也可以选择从源码安装：

```bash
$ wget https://ohse.de/uwe/releases/lrzsz-0.12.20.tar.gz
$ tar zvxf lrzsz-0.12.20.tar.gz
$ cd lrzsz-0.12.20
$ $ ./configure --prefix=/path/to/lrzsz-0.12.20
$ make
$ make install
```

# 使用

首先利用 Xshell 登录服务器，然后就可以直接使用 rz 与 sz 了。sz 与 rz 有很多选项参数，但是基本不用设置也可以。

```bash
$ sz --help
$ rz --help
-+, --append			#将文件内容追加到已存在的同名文件
-a, --ascii				#以文本方式传输
-b, --binary			#以二进制方式传输
--delay-startup N	#等待 N 秒
-e, --escape			#对字符转义
-E, --rename			#已存在同名文件则重命名新上传的文件，以点和数字作为后缀
-p, --protect			#对ZMODEM协议有效，如果目标文件已存在则跳过
-q, --quiet				#安静执行，不输出提示信息
-v, --verbose			#输出传输过程中的提示信息
-y, --overwrite		#存在同名文件则替换
-X, --xmodem			#使用XMODEM协议
-y, --overwrite		#Yes, clobber existing file if any
 	  --ymodem			#使用YMODEM协议
-Z, --zmodem			#使用ZMODEM协议
```

## **sz 下载案例**

将两个文件传输到本地，直接 sz 发送，后面接文件名，回车之后就会弹出 Windows 对话框，选择要保存的位置即可，然后就开始传输了（一般传输小文件比较好）。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1636350136899-deb2ae62-214e-4285-907d-dcdba39d266a.png#clientId=ua5c3dec7-70c2-4&from=paste&height=773&id=u0026c6f2&originHeight=773&originWidth=1000&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99911&status=done&style=none&taskId=u1154b5e8-0d6e-453b-9bc2-13d5f91fd18&title=&width=1000)

## rz 上传案例

直接在命令行输入 rz 命令，稍后就会弹出 Windows 对话框，选择要上传的文件，确认之后就开始传输了。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1636350194344-2e79333a-6edf-4f85-91f8-45b8b5e25cb4.png#clientId=ua5c3dec7-70c2-4&from=paste&height=773&id=u3652c63c&originHeight=773&originWidth=1000&originalType=binary&ratio=1&rotation=0&showTitle=false&size=128706&status=done&style=none&taskId=u8e2f6058-4cfb-4a20-98ab-85790de9747&title=&width=1000)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1636350394892-e61ebe7b-e83d-484b-bca7-39c81731f052.png#clientId=ua5c3dec7-70c2-4&from=paste&height=773&id=u6250ec0e&originHeight=773&originWidth=1000&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83091&status=done&style=none&taskId=uc8ec0713-90d1-4c21-aab7-7ab46a14329&title=&width=1000)

# MobaXterm 上使用

不是所有工具都支持 rz 与 sz，必须支持 ZModem 协议才行，例如 putty 不能使用 rz 与 sz。MobaXterm 默认也不支持 rz 与 sz，但可以通过安装插件实现。

首先，下载 CygUtils.plugin 和 lrzsz 插件，放到 mobaxterm.exe 目录。

- 网址：[https://mobaxterm.mobatek.net/plugins.html](https://mobaxterm.mobatek.net/plugins.html)
- 插件：CygUtils.plugin，Lrzsz

然后，在 Linux 下输入 rz/sz 命令，此时会出现一行类似乱码的东西，不要紧，我们操作第三步：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1636350764876-40828cfb-d600-4127-bccd-16d0d47e6df5.png#clientId=ua5c3dec7-70c2-4&from=paste&id=ue2af2e58&originHeight=47&originWidth=686&originalType=url&ratio=1&rotation=0&showTitle=false&size=3896&status=done&style=none&taskId=u16633094-ac33-4604-85f3-dcffe4ae3f5&title=)

第三步，在界面当中使用 Ctrl+右键，打开选择框如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1636351012228-0c94b6d8-2dab-4664-b20b-0cfa0171fb58.png#clientId=ua5c3dec7-70c2-4&from=paste&height=844&id=u7452f9b6&originHeight=844&originWidth=1000&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91344&status=done&style=none&taskId=ubb3b50c7-fdc7-42fe-a2ac-83e6b82c142&title=&width=1000)
然后点击相应的按钮就可以了，现在就会弹出文件选择框了。
