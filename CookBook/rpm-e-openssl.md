最开始是想安装 Bioconductor 中的 **minfi**（[https://github.com/hansenlab/minfi](https://github.com/hansenlab/minfi)） 这个 R 包，但是发现不管是 **conda** 还是 **BiocManager::install("minfi")** 总不能成功，依赖包贼多不说，有些依赖包还特别坑爹。

首先要说的是 **sparseMatrixStats**（[https://github.com/const-ae/sparseMatrixStats](https://github.com/const-ae/sparseMatrixStats)），虽然这个包比较坑，但作者起码在 GitHub 上给出了说明和解决方法。

最坑的应该是一个叫 **rhdf5filters**（[https://github.com/grimbough/rhdf5filters](https://github.com/grimbough/rhdf5filters)）的 R 包，费劲了九牛二虎之力都搞不定，尤其是在 GCC-4.8.5 编译器下有人说 "assume it is just obsolete compiler"，让我感觉到我的 CentOS-6.5 +gcc-4.8.5 想要安装好它基本是没戏了！
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1630575974537-fda4cd5e-c9e0-4402-88c6-60c7b6e6a79d.png#clientId=u0d47deb1-57c7-4&from=paste&height=578&id=u5f5514d6&originHeight=578&originWidth=932&originalType=binary&ratio=1&size=52039&status=done&style=none&taskId=udc746aac-0a3e-408f-a50c-737db84f697&width=932)

几经折腾，虽然最终在 R-3.4 中倒腾成功了，但是对于 R>=3.5 总是耿耿于怀想再尝试一下（不见棺材不落泪 😭😭😭），结合到前几天看到的 **devtoolset**，于是想着去折腾一下。

有时候现实就是这么残酷，摩拳擦掌蠢蠢欲试，有时候结果会让你直接吐血——在安装 devtoolset 的时候总发现有这么一条重复出现且让人讨厌的 openssl 错误信息：

:::warning \***\* Found 1 pre-existing rpmdb problem(s), 'yum check' output follows:**
**openssl-1.0.1e-57.el6.x86_64 is a duplicate with openssl-1.0.1e-15.el6.x86_64**
:::
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1630572326482-f051260f-17dc-46d0-9220-e2a5914cb848.png#clientId=u0d47deb1-57c7-4&from=paste&height=439&id=uc7e97251&originHeight=439&originWidth=1089&originalType=binary&ratio=1&size=66081&status=done&style=none&taskId=ua98a277d-51b5-41e8-ab0a-5b9582aea05&width=1089)
初生牛犊不怕虎（其实当时应该先谷歌一下能不能卸载 😳😳😳），也没想太多于是就把重复的 openssl-1.0.1e-57.el6.x86_64 卸载了！！！
:::warning
$ **rpm -e openssl-1.0.1e-57.el6.x86_64**
:::
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1630572645594-4b803c70-7bba-468a-af7c-38e63b21f21b.png#clientId=u0d47deb1-57c7-4&from=paste&height=861&id=udb5b7f0c&originHeight=861&originWidth=920&originalType=binary&ratio=1&size=92074&status=done&style=none&taskId=u29039e60-583b-4e49-9800-b06dccc064b&width=920)
接下来，惊魂的酸爽时刻来了：

1. **yum** 不能用了，开始提示 libssl.so.10 不存在！
   :::warning
   **libssl.so.10: cannot open shared object file: No such file or directory**
   :::

2. 新的 Terminal 想要通过 SSH 再次登陆 log01，提示网络被关闭，登陆不上去了！！

开始意识到出问题了，于是去谷歌，搜索完差点一口老血喷出来！但万幸的是 root 还没有退出来！！！

于是开始自救！

首先，下载对应版本的 rpm（这时候 wget/scp 都罢工了！），上传到服务器，然后 rpm 安装。

最先下载的是 openssl-1.0.0-27.el6.src.rpm，但 rpm 安装的时候又出现了新的问题。

```shell
[root@log01 lib64]# rpm -ivh /home/bioadmin/openssl-1.0.0-27.el6.src.rpm
warning: /home/bioadmin/openssl-1.0.0-27.el6.src.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
   1:openssl                warning: user mockbuild does not exist - using root
warning: group mockbuild does not exist - using root
warning: user mockbuild does not exist - using root
warning: group mockbuild does not exist - using root
......
warning: group mockbuild does not exist - using root
########################################### [100%]
warning: user mockbuild does not exist - using root
warning: group mockbuild does not exist - using root
warning: user mockbuild does not exist - using root
warning: group mockbuild does not exist - using root

[root@log01 lib64]# sudo useradd -s /sbin/nologin mockbuild
[root@log01 lib64]# rpm -ivh /RiboBio/home/bi.chengnan/R/openssl-1.0.0-27.el6.src.rpm
warning: /RiboBio/home/bi.chengnan/R/openssl-1.0.0-27.el6.src.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
   1:openssl                ########################################### [100%]
```

安装完，发现不起作用，/usr/lib64 下面缺失的 **libssl.so.10** 和 **libcrypto.so.10** 没有回来，SSH 依然无法登陆。

接着，下载卸载的 openssl-1.0.1e-57.el6.x86_64.rpm，不出意外 rpm 安装又遇到新的问题（心底一万个草泥马飘过）！

```shell
[root@log01 usr]# rpm -ivh /home/bioadmin/openssl-1.0.1e-57.el6.x86_64.rpm
warning: /home/bioadmin/openssl-1.0.1e-57.el6.x86_64.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
        file /usr/bin/openssl from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/.libcrypto.so.1.0.1e.hmac from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/.libssl.so.1.0.1e.hmac from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/libcrypto.so.1.0.1e from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/libssl.so.1.0.1e from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/lib4758cca.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libaep.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libatalla.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libcapi.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libchil.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libcswift.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libgmp.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libnuron.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libpadlock.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libsureware.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libubsec.so from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/doc/openssl-1.0.1e/README.FIPS from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/ca.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/ciphers.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/cms.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/ec.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/ocsp.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/openssl.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/req.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/s_client.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/s_server.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/s_time.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/smime.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/speed.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/ts.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/verify.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/x509.1ssl.gz from install of openssl-1.0.1e-57.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
```

第三，尝试 openssl-1.0.1e-15.el6.x86_64.rpm，居然还是这个问题（。。。。。）！

```shell
[root@log01 usr]# rpm -ivh /home/bioadmin/openssl-1.0.1e-15.el6.x86_64.rpm
warning: /home/bioadmin/R/openssl-1.0.1e-15.el6.x86_64.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
        package openssl-1.0.1e-15.el6.x86_64 is already installed
        file /usr/bin/openssl from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/.libcrypto.so.1.0.1e.hmac from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/.libssl.so.1.0.1e.hmac from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/libcrypto.so.1.0.1e from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/libssl.so.1.0.1e from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/lib4758cca.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libaep.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libatalla.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libcapi.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libchil.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libcswift.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libgmp.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libnuron.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libpadlock.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libsureware.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/lib64/openssl/engines/libubsec.so from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/ca.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/cms.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/ec.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/ocsp.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/openssl.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/req.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/s_client.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/s_server.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/s_time.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/smime.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/ts.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/verify.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64
        file /usr/share/man/man1/x509.1ssl.gz from install of openssl-1.0.1e-15.el6.x86_64 conflicts with file from package openssl-1.0.1e-15.el6.x86_64

```

第四，尝试下载 openssl-1.0.1e.tar.gz 源码包安装，但是 make 的时候却出现了 Perl 有关的 Error（注意这里是一个很关键的步骤），最开始也以为要凉凉了！！

虽然在《[Centos6 卸载 OpenSSL 后重装提示 libssl.so.10: cannot open shared object file: No such file or directory](https://www.cxyzjd.com/article/baidu_33864675/93332571)》这篇文章中提到："**Make：通过 make 来生成生成 libssl.so.1.0.0 和 libcrypto.so.1.0.0**"，对这句话一直不理解以为是要安装完成后，在目标目录才会生成 **libssl.so.1.0.0 **和** libcrypto.so.1.0.0**！其实并非如此！

```shell
$ tar zvxf openssl-1.0.1e.tar.gz
$ cd openssl-1.0.1e
$ ./config shared zlib-dynamic
$ make
......
installing man1/asn1parse.1
installing man1/CA.pl.1
installing man1/ca.1
installing man1/ciphers.1
installing man1/cms.1
cms.pod around line 457: Expected text after =item, not a number
cms.pod around line 461: Expected text after =item, not a number
cms.pod around line 465: Expected text after =item, not a number
cms.pod around line 470: Expected text after =item, not a number
cms.pod around line 474: Expected text after =item, not a number
POD document had syntax errors at /RiboBio/Bioinfo/APPS/perl/bin/pod2man line 71.
make: *** [install_docs] Error 1
```

其实，**make 这一步已经在当前目录（注意是当前执行编译的目录）生成我们想要的 libssl.so.1.0.0 和 libcrypto.so.1.0.0 了**！！！
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1630575037203-98188a89-2583-4f12-87ba-156832829f48.png#clientId=u0d47deb1-57c7-4&from=paste&height=366&id=u0eb802ce&originHeight=366&originWidth=865&originalType=binary&ratio=1&size=41933&status=done&style=none&taskId=u2511958f-52ae-464a-8110-9ecc9909a41&width=865)
第五，把这两个文件 **ln -s** 到 **/usr/lib64**，神奇的发现 yum 居然恢复正常了，虽然 SSH 还是不能登录。

```shell
[root@log01 lib64]# ln -s /home/bioadmin/src/openssl-1.0.1e/libssl.so.1.0.0 /usr/lib64/libssl.so.10
[root@log01 lib64]# ln -s /home/bioadmin/src/openssl-1.0.1e/libcrypto.so.1.0.0 /usr/lib64/libcrypto.so.10
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1630575388511-351fca77-a935-4c6e-9ffb-5ae162cdd2b9.png#clientId=u0d47deb1-57c7-4&from=paste&height=818&id=u28f37161&originHeight=818&originWidth=1599&originalType=binary&ratio=1&size=159120&status=done&style=none&taskId=u3db1fc14-1c42-4e95-8444-f412cbc21e2&width=1599)

最后，尝试着使用 **yum** 把 **openssl** 重新安装一下。这一次发现，一切都恢复正常了！
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1630575467981-a5483ede-1f34-4b8c-b5bc-d8163985a7fc.png#clientId=u0d47deb1-57c7-4&from=paste&height=879&id=uea390129&originHeight=879&originWidth=1599&originalType=binary&ratio=1&size=90229&status=done&style=none&taskId=u0782363d-0217-45d8-8e12-64b4453ad1e&width=1599)

**参考资料：**

1. 程序员宅基地，《[Centos6 卸载 OpenSSL 后重装提示 libssl.so.10: cannot open shared object file: No such file or directory](https://www.cxyzjd.com/article/baidu_33864675/93332571)》，[程序员宝宝](https://www.cxybb.com/)
2. [augusite](https://home.cnblogs.com/u/augusite/)，《[libssl.so.10: cannot open shared object file: No such file or directory - augusite - 博客园](https://www.cnblogs.com/augusite/p/10593222.html)》
3. OpenSSL tar.gz 源码包下载地址：[Index of /source/old/1.0.1](https://ftp.openssl.org/source/old/1.0.1/)
4. OpenSSL rpm 源码下载地址：[https://mirrors.aliyun.com/centos-vault/6.5/updates/x86_64/Packages/](https://mirrors.aliyun.com/centos-vault/6.5/updates/x86_64/Packages/)
