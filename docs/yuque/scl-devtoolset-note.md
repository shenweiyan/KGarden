# 1. 背景

> CentOS/RHEL Linux 发行版以稳定性著称，所有的软件都要尽可能 stable，导致的一个结果就是基础软件的版本非常的低，比如 CentOS 6.7（15 年发布） 中 gcc 版本还是 4.4.7（12 年的版本）。这对开发来说就不是很友好，比如我们想用 C++ 11 中的某个特性，就必须自己编译一个高版本的 gcc 出来，但是这会有另外一个问题，开发环境不好维护，如果自己有多台电脑或者多个人合作的项目，每台机器上都要自己编一份，维护起来就比较麻烦。

## SCL

SCL(Software Collections)是一个 CentOS/RHEL Linux 平台的软件多版本共存解决方案，为 RHEL/CentOS Linux 用户提供一种方便、安全地安装和使用应用程序和运行时环境的多个版本的方式，同时避免把系统搞乱。

SCL 项目主页：[https://www.softwarecollections.org](https://www.softwarecollections.org/)

## Devtoolset

> 不同平台的编译环境不一样，所以 RedHat 就推出了 scl (Software Collections) ，它可以根据 devtoolset 一起配合来快速统一开发环境，不用一个个的去找各个官网再去编译源码安装。

使用 scl 可以暂时的改变当前用户的编译工具，例如你的系统版本 gcc 4.4.7 但是你可以使用 scl 工具它可以临时的把你的 gcc 版本提升到 4.8。

其实，简单的来说，devtoolset 就是 SCL 提供的一套专门用于 CentOS 或 Red Hat Enterprise Linux 平台编译开发的一套工具集。

> Developer Toolset is designed for developers working on CentOS or Red Hat Enterprise Linux platform. It provides current versions of the GNU Compiler Collection, GNU Debugger, and other development, debugging, and performance monitoring tools.

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1630465685244-4a033477-8754-4bd4-a7af-698164e733f4.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_27%2Ctext_5rKI57u054eVIHwg5YWs5LyX5Y-377yaQmlvSVTniLHlpb3ogIU%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10#clientId=u1e25c63c-5db2-4&from=paste&height=942&id=ub04f79c9&originHeight=942&originWidth=930&originalType=binary&ratio=1&rotation=0&showTitle=false&size=161469&status=done&style=none&taskId=uaca47208-9fcb-4269-a6ab-8db053e0b76&title=&width=930)
当然，除了 devtoolset 这些专门用于编译开发的工具集，SCL 上还有其他的很多工具集，如 [Ruby](https://www.softwarecollections.org/en/scls/rhscl/rh-ruby26/)，[Redis](https://www.softwarecollections.org/en/scls/rhscl/rh-redis5/)，[nginx](https://www.softwarecollections.org/en/scls/rhscl/rh-nginx114/) 等等。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1630465859594-d28cc896-2461-4be4-8720-f7986fb91916.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_27%2Ctext_5rKI57u054eVIHwg5YWs5LyX5Y-377yaQmlvSVTniLHlpb3ogIU%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10#clientId=u1e25c63c-5db2-4&from=paste&height=916&id=u35c87109&originHeight=916&originWidth=932&originalType=binary&ratio=1&rotation=0&showTitle=false&size=160700&status=done&style=none&taskId=u38445d4f-78a8-4333-af02-2b8f246f0f9&title=&width=932)

# 2. 安装

## 安装配置 SCL YUM 源

首先，要解决的第一个问题就是 yum 源的问题。尤其是在 CentOS 6 已经停止了维护（2020 年 11 月 30 日）的前提下，yum 源如果失效/错误，一切都将免谈。

### CenOS 7

CentOS 7 最晚在 2024 年 6 月 30 后停止更新维护，在此之前在 CentOS 7 可以通过 yum 直接安装 SCL 源基本都是可以正常使用的。

```bash
yum install centos-release-scl centos-release-scl-rh
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1631002223022-ca4d26f3-53b6-47f2-aff8-d70b5f8de052.png#clientId=ueb07ee64-2a08-4&from=paste&height=505&id=u1e65c977&originHeight=505&originWidth=1094&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42498&status=done&style=none&taskId=uc29319ea-dce7-439b-b2ef-4c71e52dadd&title=&width=1094)
安装完成后，会默认在 **/etc/yum.repos.d** 下生成 2 个 repo 源文件：

- **CentOS-SCLo-scl.repo**

```
# CentOS-SCLo-sclo.repo
#
# Please see http://wiki.centos.org/SpecialInterestGroup/SCLo for more
# information

[centos-sclo-sclo]
name=CentOS-7 - SCLo sclo
# baseurl=http://mirror.centos.org/centos/7/sclo/$basearch/sclo/
mirrorlist=http://mirrorlist.centos.org?arch=$basearch&release=7&repo=sclo-sclo
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-sclo-testing]
name=CentOS-7 - SCLo sclo Testing
baseurl=http://buildlogs.centos.org/centos/7/sclo/$basearch/sclo/
gpgcheck=0
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-sclo-source]
name=CentOS-7 - SCLo sclo Sources
baseurl=http://vault.centos.org/centos/7/sclo/Source/sclo/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-sclo-debuginfo]
name=CentOS-7 - SCLo sclo Debuginfo
baseurl=http://debuginfo.centos.org/centos/7/sclo/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo
```

- **CentOS-SCLo-scl-rh.repo**

```
# CentOS-SCLo-rh.repo
#
# Please see http://wiki.centos.org/SpecialInterestGroup/SCLo for more
# information

[centos-sclo-rh]
name=CentOS-7 - SCLo rh
#baseurl=http://mirror.centos.org/centos/7/sclo/$basearch/rh/
mirrorlist=http://mirrorlist.centos.org?arch=$basearch&release=7&repo=sclo-rh
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-rh-testing]
name=CentOS-7 - SCLo rh Testing
baseurl=http://buildlogs.centos.org/centos/7/sclo/$basearch/rh/
gpgcheck=0
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-rh-source]
name=CentOS-7 - SCLo rh Sources
baseurl=http://vault.centos.org/centos/7/sclo/Source/rh/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-rh-debuginfo]
name=CentOS-7 - SCLo rh Debuginfo
baseurl=http://debuginfo.centos.org/centos/7/sclo/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo
```

### CentOS 6

如果您使用 yum 安装 centos-release-SCL/centos-release-scl 时遇到 Error: Nothing to do 异常（尤其是在已经停止维护的 CentOS 6 的系统中）：

```bash
[root@log01 ~]# yum install centos-release-SCL
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Setting up Install Process
No package centos-release-SCL available.
Error: Nothing to do
```

这种情况下，您可以参考 [https://github.com/sclorg/centos-release-scl](https://github.com/sclorg/centos-release-scl) 提供的方法进行安装（或者参考本文下面提到的手动创建 CentOS-SCLo-scl.repo 和 CentOS-SCLo-scl-rh.repo 的方式进行安装）：

```bash
# CentOS 6
sudo yum-config-manager --add-repo=https://copr.fedoraproject.org/coprs/rhscl/centos-release-scl/repo/epel-6/rhscl-centos-release-scl-epel-6.repo
sudo yum install centos-release-scl

# CentOS 7
sudo yum-config-manager --add-repo=https://copr.fedoraproject.org/coprs/rhscl/centos-release-scl/repo/epel-7/rhscl-centos-release-scl-epel-7.repo
sudo yum install centos-release-scl
```

> **Important: **Please, mind, that packages build by SCLo SIG are not supported and are not part of the supported Red Hat portfolio. For installing supported Software Collections packages, install packages from official RHSCL repository only.

### 手动调整 SCL YUM 源

如果您通过 rpm（或者其他的方式）成功安装了 centos-release-scl，但是安装 devtoolset（或者其他工具集时）提示 404 异常。

```shell
[root@log01 ~]# yum install devtoolset-3
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
http://mirror.centos.org/centos/6/sclo/x86_64/sclo/repodata/repomd.xml: [Errno 14] PYCURL ERROR 7 - "Failed to connect to mirror.centos.org port 80: Connection refused"
Trying other mirror.
Error: Cannot retrieve repository metadata (repomd.xml) for repository: centos-sclo-sclo. Please verify its path and try again

[root@log01 ~]# yum install devtoolset-7-gcc*
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
http://mirror.centos.org/centos/6/sclo/x86_64/sclo/repodata/repomd.xml: [Errno 14] PYCURL ERROR 22 - "The requested URL returned error: 404 Not Found"
Trying other mirror.
Error: Cannot retrieve repository metadata (repomd.xml) for repository: centos-sclo-sclo. Please verify its path and try again
```

主要原因可能在于：
![](https://cdn.nlark.com/yuque/0/2021/png/126032/1630994486421-1ddbab6a-2aa5-4122-a21e-20a70857ca2d.png?date=1631062232709#from=url&id=OC1Rm&originHeight=129&originWidth=948&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
由于 CentOS 6 已经停止了维护，但 CentOS 6 对应的 centos-release-scl repo 中 baseurl 指向的链接可能已经弃用失效（**deprecated**），尤其是 baseur 指向 [http://mirror.centos.org/centos/6/](http://mirror.centos.org/centos/6/) 的链接。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1630551979941-2d719451-54aa-4db2-81d0-adf13d53a3e5.png#clientId=ufbac06d1-374d-4&from=paste&height=791&id=u0bb15816&originHeight=791&originWidth=1119&originalType=binary&ratio=1&rotation=0&showTitle=false&size=111645&status=done&style=none&taskId=u32ffe194-400b-4acd-846c-be204fa279e&title=&width=1119)
针对以上的情况，可以按照以下步骤，手动进行处理（本文章中使用的阿里云的 YUM 源）。

> **提示：**
>
> 通过 [https://github.com/sclorg/centos-release-scl](https://github.com/sclorg/centos-release-scl)（或者其他方式）安装了 centos-release-scl 的童鞋，如果想要获取更好的下载体验，也可以根据需要手动调整对应 repo 的 baseurl 地址。

> 目前，可以的 CentOS 6 源地址（CentOS 7 还在维护周期内，大部分源均可用）：
>
> - [https://mirrors.aliyun.com/centos-vault/](https://mirrors.aliyun.com/centos-vault/)
> - [http://vault.centos.org/centos/](http://vault.centos.org/centos/)
> - [https://download.copr.fedorainfracloud.org/results/rhscl/centos-release-scl/](https://download.copr.fedorainfracloud.org/results/rhscl/centos-release-scl/)

第一，在 /etc/yum.repos.d 手动创建 CentOS-SCLo-scl.repo 和 CentOS-SCLo-scl-rh.repo 文件。

- **CentOS-SCLo-scl.repo**

```
# CentOS-SCLo-sclo.repo
#
# Please see http://wiki.centos.org/SpecialInterestGroup/SCLo for more
# information

[centos-sclo-sclo]
name=CentOS-6 - SCLo sclo
#baseurl=http://vault.centos.org/centos/6/sclo/$basearch/sclo/
baseurl=https://mirrors.aliyun.com/centos-vault/6.9/sclo/x86_64/sclo/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-sclo-testing]
name=CentOS-6 - SCLo sclo Testing
baseurl=http://buildlogs.centos.org/centos/6/sclo/$basearch/sclo/
gpgcheck=0
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-sclo-source]
name=CentOS-6 - SCLo sclo Sources
baseurl=http://vault.centos.org/centos/6/sclo/Source/sclo/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-sclo-debuginfo]
name=CentOS-6 - SCLo sclo Debuginfo
baseurl=http://debuginfo.centos.org/centos/6/sclo/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo
```

- **CentOS-SCLo-scl-rh.repo**

```
# CentOS-SCLo-rh.repo
#
# Please see http://wiki.centos.org/SpecialInterestGroup/SCLo for more
# information

[centos-sclo-rh]
name=CentOS-6 - SCLo rh
#baseurl=http://vault.centos.org/centos/6/sclo/$basearch/rh/
baseurl=https://mirrors.aliyun.com/centos-vault/6.9/sclo/x86_64/rh/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-rh-testing]
name=CentOS-6 - SCLo rh Testing
baseurl=http://buildlogs.centos.org/centos/6/sclo/$basearch/rh/
gpgcheck=0
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-rh-source]
name=CentOS-6 - SCLo rh Sources
baseurl=http://vault.centos.org/centos/6/sclo/Source/rh/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo

[centos-sclo-rh-debuginfo]
name=CentOS-6 - SCLo rh Debuginfo
baseurl=http://debuginfo.centos.org/centos/6/sclo/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo
```

第二，更新 yum 源的缓存。

```bash
$ cd /etc/yum.repos.d
$ yum clean all
$ yum makecache
```

第三，yum 源更新完后，就可以使用以下的命令查看对应源的软件包信息。

```bash
# 查看 centos-sclo-rh 源所有可用的软件包
$ yum list all --enablerepo='entos-sclo-rh'

# 查看 centos-sclo-rh 源中名为 scl-utils 的软件包
$ yum search scl-utils --enablerepo='centos-sclo-rh'
```

## 安装 scl-utils

scl-utils 是管理 SCL (Software Collection) 环境设置和运行软件的一套软件工具。

```bash
yum install scl-utils
```

个人在 CentOS 6.5 中安装 devtoolset-4 就遇到 scl-utils 版本太低，要求 scl-utils >= 20120927-11 的报错。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1631006072717-0d274974-6934-426e-ba93-b22dc89423b2.png#clientId=ueb07ee64-2a08-4&from=paste&height=514&id=u74c265bb&originHeight=514&originWidth=1196&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83537&status=done&style=none&taskId=u2d0f97f8-3ac5-4a34-9f51-4a6aa518dc5&title=&width=1196)
解决方法如下：

```bash
# centos-scl 参考 Devtoolset 一节中的 /etc/yum.repos.d/centos-scl.repo 文件
yum install scl-utils --enablerepo=centos-scl
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1631006173405-1b2437ac-ddf8-42aa-9af9-487c3c565dd8.png#clientId=ueb07ee64-2a08-4&from=paste&height=819&id=u4f6c194b&originHeight=819&originWidth=1567&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84845&status=done&style=none&taskId=uf41fdf8c-b6df-4c60-9c79-7ee3addaeab&title=&width=1567)

## 安装 Devtoolset

不同的 devtoolset 对应了不同的 gcc 版本，如：

- devtoolset-1 是 gcc 4.7
- devtoolset-2 是 gcc 4.8
- devtoolset-3 是 gcc 4.9
- [devtoolset-4 ](https://access.redhat.com/documentation/en-us/red_hat_developer_toolset/4/html-single/user_guide/index)是 gcc 5.2/5.3
- [devtoolset-6](https://access.redhat.com/documentation/en-us/red_hat_developer_toolset/6) 是 gcc 6.2/6.3
- [devtoolset-7](https://access.redhat.com/documentation/en-us/red_hat_developer_toolset/7) 是 gcc 7.2/7.3

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1631004057058-b0b72dd6-12e4-48dc-b01c-c06805c9b308.png#clientId=ueb07ee64-2a08-4&from=paste&height=744&id=uc23859de&originHeight=744&originWidth=1097&originalType=binary&ratio=1&rotation=0&showTitle=false&size=131827&status=done&style=none&taskId=u4bc59bcf-6482-44ce-b287-ce9c6d7db21&title=&width=1097)
CentOS 7 的 centos-sclo-rh/centos-sclo 默认支持 devtoolset-7 及以上，如果想要使用 devtoolset-3 到 7 之间的版本，可以参考下面的做法：

1. 创建一个 /etc/yum.repos.d/centos-scl.repo 文件，内容如下：

```bash
[centos-scl]

name=centos-scl
baseurl=http://vault.centos.org/6.5/SCL/$basearch/
gpgcheck=0
enabled=0
```

2. 创建完了以后，安装 scl-utils，如果你前面已经安装这一步可以跳过：

```bash
$ yum install scl-utils --enablerepo=centos-scl
```

3. 创建一个 /etc/yum.repos.d/centos-devtools.repo，内容如下：

```bash
[centos-devtools]
name=centos-devtools
#baseurl=http://vault.centos.org/centos/6/sclo/$basearch/rh/
baseurl=https://mirrors.aliyun.com/centos-vault/6.9/sclo/x86_64/rh/
gpgcheck=1
enabled=1
```

4. 安装 devtools：

```bash
yum install devtoolset-4 --enablerepo='centos-devtools'
```

# 3. 使用

## 激活与切换

可以使用下面的命令查看通过 scl 安装了哪些软件：

```bash
$ scl -l
devtoolset-3
devtoolset-4
```

激活 scl 安装的软件：

```bash
$  scl enable devtoolset-4 bash

# 如果 scl enable 不起作用，可使用 source 激活
$ source /opt/rh/devtoolset-4/enable

$ gcc --version
gcc (GCC) 5.3.1 20160406 (Red Hat 5.3.1-6)
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1631006375569-7b3c0764-1a11-42a0-ba11-110590fa16b9.png#clientId=ueb07ee64-2a08-4&from=paste&height=467&id=u773da7e2&originHeight=467&originWidth=798&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53467&status=done&style=none&taskId=u2e1a9973-c567-4ab7-a000-9e5ea3c011b&title=&width=798)

## 卸载

可能大家用完开发工具集后就会想要删除它，其实很简单，输入以下命令：

```bash
yum remove devtoolset-3\*
```

然后也可以删除 SCL 管理工具：

```bash
yum remove scl-utils\*
```

# 3. 参考资料

1. [Installing node.js v8.11.1 to centos 6.5 server. - kakts-log](https://kakts-tec.hatenablog.com/entry/2018/04/11/000613)
2. [RedHat6 系列 Devtool-Set - SegmentFault 思否](https://segmentfault.com/a/1190000004193587)
3. [Software Collections — Software Collections](https://www.softwarecollections.org/en/)，SCL 官网
4. [Product Documentation for Red Hat Developer Toolset 4 | Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_developer_toolset/4)
5. [Fedora copr Rhscl's Projects Project List](https://copr.fedorainfracloud.org/coprs/rhscl/)，Fedora copr Rhscl's Projects 官网
6. [Index - Developer Toolset](https://linux.web.cern.ch/devtoolset/)，devtoolset1-3，Linux @ CERN