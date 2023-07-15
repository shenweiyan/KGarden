# GCC

默认的 Ubuntu 软件源包含了一个软件包组，名称为 "[build-essentia](https://pkgs.org/download/build-essential)l"，它包含了 GNU 编辑器集合，GNU 调试器，和其他编译软件所必需的开发库和工具。

想要安装开发工具软件包，以 拥有 sudo 权限用户身份或者 root 身份运行下面的命令：

```bash
sudo apt update
sudo apt install build-essential
```

这个命令将会安装一系列软件包，包括 gcc，g++ 和 make。

你可能还想安装关于如何使用 GNU/Linux 开发的手册。

```bash
sudo apt install manpages-dev
```

通过运行下面的命令，打印 GCC 版本，来验证 GCC 编译器是否被成功地安装。

```bash
gcc --version
```

在 Ubuntu 20.04 软件源中 GCC 的默认可用版本号为 **9.3.0**：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640056216355-9f80e74b-dc6e-48da-8921-88f20f8b390e.png#clientId=ued197642-78ba-4&from=paste&height=146&id=ucda0a1ee&originHeight=146&originWidth=740&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12127&status=done&style=none&taskId=ub6cdcb4a-2d31-44a3-8327-3cf8ed7096d&title=&width=740)
就这些。GCC 已经在你的 Ubuntu 系统上安装好了，你可以开始使用它了。

# 参考资料

1. [如何在 Ubuntu 20.04 上安装 GCC (build-essential)](https://developer.aliyun.com/article/766146) - 阿里云开发者社区
