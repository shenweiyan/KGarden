昨天在语雀开了一个话题讨论《[CentOS 7 和 8 不维护停止更新之后，服务器选择使用什么系统好？](https://www.yuque.com/bioitee/topics/3)》，在[ V2EX](https://www.v2ex.com/t/805300) 收到不少回复。

在前几天正好入手了腾讯云/阿里云的一个 2 核 4G，80GB SSD 盘的轻量云服务器，首年才 74，作为尝鲜一开始装了个 Debian 10，不得不说，RH 系用久了，回到 Debian/Ubuntu 还真是一下子没适应过来。

也看到不少朋友都已经在生产环境用上了 Rocky Linux 和 CentOS 8 无缝对接，突发奇想也想体验一下，搜了一圈发现虽然 Rocky Linux 迁移和安装的教程不少，但唯独没找到在云服务上的迁移的，而且目前国内的阿里云/腾讯云/华为云都没有提供 Rocky Linux 的镜像，于是开始自己折腾。

## 安装前准备

步骤 1. 首先，把你的服务器变更成 CentOS 8.x 系统。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1632984423443-908f9b8a-1420-4845-ab14-54fbfbd6eec8.png#clientId=ued49435e-b7e0-4&from=paste&height=672&id=u8c8ad525&originHeight=672&originWidth=1352&originalType=binary&ratio=1&rotation=0&showTitle=false&size=101688&status=done&style=stroke&taskId=u28eb239d-cd11-418d-90d9-d7580b2061a&title=&width=1352)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1632984661792-aee2a6b1-8bd3-4af9-a1c3-b85e00807634.png#clientId=ued49435e-b7e0-4&from=paste&height=637&id=u8de6bb35&originHeight=637&originWidth=1083&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71131&status=done&style=stroke&taskId=u6238056b-4eef-4011-85d2-f2b76aa72a5&title=&width=1083)

如果你用的是阿里云的 ECS（或者轻量云服务，个人用的是轻量云服务器），可以先升级到 CentOS 8.2 的系统。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1637908631205-d419d8de-e902-443a-8731-cc0f36cd93b2.png#clientId=u788a10dd-df7c-4&from=paste&height=547&id=u006d1a58&originHeight=547&originWidth=834&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33736&status=done&style=stroke&taskId=u51c1d8e0-46e9-4624-a0cf-973b1535f15&title=&width=834)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1637908934651-7ab6f724-dfe7-4fd6-9c0c-3ddafdf2de15.png#clientId=u788a10dd-df7c-4&from=paste&height=625&id=u5e5268fc&originHeight=625&originWidth=1236&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47879&status=done&style=stroke&taskId=u9a793ada-ef71-40cb-9f4b-1c3f7bc9777&title=&width=1236)

步骤 2. 然后，让我们先确保您的系统是最新的（如果已经是 CentOS 8.0 及以上，可以不用执行这一步）。

```bash
# 这一步会把系统升级到最新的版本，如果你是 CentOS 8.0，可能会升级到 8.5
sudo dnf update
sudo dnf upgrade
```

步骤 3. 下载 Rocky Linux 8 到 CentOS 8 的迁移脚本。

Rocky Linux 提供了一个名为的工具 [migrate2rocky](https://docs.rockylinux.org/zh/guides/migrate2rocky/)，该工具已在许多 RHEL 变体（例如 CentOS、AlmaLinux 和 Oracle Linux）上成功测试：

```bash
curl https://raw.githubusercontent.com/rocky-linux/rocky-tools/main/migrate2rocky/migrate2rocky.sh -o migrate2rocky.sh
```

或者，可以通过 git 下载：

```bash
dnf install git
git clone https://github.com/rocky-linux/rocky-tools.git
```

## 安装

这可能是最简单的一点。 登录到您的服务器，然后使用命令放终端 cd 到包含 migrate2rocky.sh 文件的文件目录。

然后，确保文件是可执行的：

```bash
sudo chmod +x migrate2rocky.sh
```

接下来，执行脚本：

```
./migrate2rocky.sh -r
```

- -r 选项告诉脚本继续安装所有内容 (That "-r" option tells the script to just go ahead and install everything.)。

```bash
$ ./migrate2rocky.sh -r
Preparing to migrate CentOS Linux 8 to Rocky Linux 8.

Determining repository names for CentOS Linux 8.....

Found the following repositories which map from CentOS Linux 8 to Rocky Linux 8:
CentOS Linux 8  Rocky Linux 8
appstream       appstream
baseos          baseos
extras          extras

Getting system package names for CentOS Linux 8.......

Found the following system packages which map from CentOS Linux 8 to Rocky Linux 8:
CentOS Linux 8        Rocky Linux 8
centos-backgrounds    rocky-backgrounds
centos-gpg-keys       rocky-gpg-keys
centos-logos          rocky-logos
centos-indexhtml      rocky-indexhtml
centos-linux-release  rocky-release
centos-linux-repos    rocky-repos
[...]
```

- 如果出现以下的报错信息，则参考 [Invalid configuration value: failovermethod=priority in repo config files](https://bugzilla.redhat.com/show_bug.cgi?id=1961083)，把 failovermethod=priority 一行从 /etc/yum.repos.d/CentOS-epel.repo 中删除。

```shell
[root@iZ7xv4bbjwm8qgx8m72z68Z ~]# ./migrate2rocky.sh -r

Removing dnf cache
Preparing to migrate CentOS Linux 8 to Rocky Linux 8.

Determining repository names for CentOS Linux 8.Invalid configuration value: failovermethod=priority in /etc/yum.repos.d/CentOS-epel.repo; Configuration: OptionBinding with id "failovermethod" does not exist
```

成功迁移 Rocky Linux 后，您应该会看到以下输出：

```bash
...
Complete!

Done, please reboot your system.
A log of this installation can be found at /var/log/migrate2rocky.log
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1632986970721-42b9a22d-1085-4263-a205-0f515edec681.png#clientId=ued49435e-b7e0-4&from=paste&height=683&id=u891fcbae&originHeight=683&originWidth=1279&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105400&status=done&style=stroke&taskId=u1bafff0c-8749-4412-9360-c40004caa13&title=&width=1279)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1637914058343-2a41a7c5-9477-41ab-ac4e-e70a0623cf9f.png#clientId=u788a10dd-df7c-4&from=paste&height=783&id=uca272f6c&originHeight=783&originWidth=1038&originalType=binary&ratio=1&rotation=0&showTitle=false&size=98225&status=done&style=none&taskId=u4422f411-fa78-411b-a93e-c4959a7eccc&title=&width=1038)

然后，运行以下命令来同步已安装的软件包，然后只需重新启动系统：

```bash
sudo dnf distro-sync -y
sudo reboot
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1632987078177-18ac7668-e309-4e52-90ae-026ff1f7e326.png#clientId=ued49435e-b7e0-4&from=paste&height=677&id=u5a1c4b3e&originHeight=677&originWidth=1281&originalType=binary&ratio=1&rotation=0&showTitle=false&size=103161&status=done&style=stroke&taskId=udd97ee7b-1fd7-4e20-8ff5-5ca1ca98ae4&title=&width=1281)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1637914114298-13be048a-02ab-4a57-80da-85a0839fe731.png#clientId=u788a10dd-df7c-4&from=paste&height=659&id=ue9a891bd&originHeight=659&originWidth=1038&originalType=binary&ratio=1&rotation=0&showTitle=false&size=82903&status=done&style=stroke&taskId=u42dca16f-a0c5-417e-b1c4-892c00105ec&title=&width=1038)

## 检查操作系统版本

为了确认您已成功迁移到 Rocky Linux，请检查操作系统版本：

```bash
cat /etc/redhat-release
```

输出：

```bash
Rocky Linux release 8.4 (Green Obsidian)
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1632987311045-998d4ae6-3607-48c8-8d6c-c61e61750559.png#clientId=ued49435e-b7e0-4&from=paste&height=712&id=ue22d4494&originHeight=712&originWidth=1362&originalType=binary&ratio=1&rotation=0&showTitle=false&size=115589&status=done&style=stroke&taskId=u4aa7e8cf-c38e-480e-9542-9a0b02b92e0&title=&width=1362)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1637914314097-26d42a24-2cef-4d16-a9f6-46e2e1330ad7.png#clientId=u788a10dd-df7c-4&from=paste&height=725&id=u6d32e356&originHeight=725&originWidth=1151&originalType=binary&ratio=1&rotation=0&showTitle=false&size=68443&status=done&style=stroke&taskId=u8e368a08-5efd-4d50-b8b4-af3666249d0&title=&width=1151)

## 参考资料

1. [Migrating To Rocky Linux - Documentation](https://docs.rockylinux.org/guides/migrate2rocky/)，官方文档
2. [如何从 CentOS 8 迁移到 Rocky Linux 8](https://www.xtuos.com/2819.html) - 统信 UOS 之家
