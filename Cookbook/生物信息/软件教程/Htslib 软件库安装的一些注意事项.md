Htslib 系列的软件主要包括：Samtools、BCFtools、HTSlib，其中 HTSlib 是前两个都需要用到的 C 语言依赖库。
[Samtools](https://www.htslib.org/)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FqQNhgz8QJz79Lb_H6gPBpGIu7pe.png)
其他的一些软件有时候也会用到 HTSlib，所以，有时候可以单独安装 HTSlib，主要步骤：

```bash
git clone https://github.com/samtools/htslib.git
git submodule update --init --recursive
autoconf -i  # Autoconf version 2.64 or higher is required
./configure --prefix=/PATH/TO/htslib
```

如果 configure 过程提示：error: cannot find input file: `config.h.in'`，参考 [htslib - issues:1422](https://github.com/samtools/htslib/issues/1422) 下载 [htslib-1.15.1.tar.bz2](https://github.com/samtools/htslib/releases/download/1.15.1/htslib-1.15.1.tar.bz2)，然后继续后面的操作。

```bash
wget --no-check-certificate --content-disposition https://github.com/samtools/htslib/releases/download/1.15.1/htslib-1.15.1.tar.bz2
tar -jxvf htslib-1.15.1.tar.bz2
cd htslib-1.15.1
autoconf -i  # Autoconf version 2.64 or higher is required
./configure --prefix=/PATH/TO/htslib
make
make install
```
