Htslib 系列的软件主要包括：Samtools、BCFtools、HTSlib，其中 HTSlib 是前两个都需要用到的 C 语言依赖库。
[Samtools](https://www.htslib.org/)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1660183289774-e67ff3c2-c347-405c-bd58-44715d3f7c09.png#clientId=ud8a5c5b0-af0e-4&from=paste&height=234&id=u576cc097&originHeight=234&originWidth=1148&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25957&status=done&style=none&taskId=uaf531e0d-5b3e-4289-a680-a5e56ebd779&title=&width=1148)
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
