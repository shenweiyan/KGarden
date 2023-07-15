事情起因于在推特看到关于`ggVennDiagram`这个 R 包教程的一条推文，想着去复现一下，于是开始去安装，不料安装过程中出现了`sf`这个依赖包始终安装不成功的一堆错误，于是有了这一篇文章，特此记录一下，也希望给遇到类似问题的小伙伴一个参考。

# 第一个错误

首先说明一下，我用的 R 版本是 4.3.0，Linux 系统是 Red Hat 6.5。
最开始安装`sf`这个 R 包遇到的第一个 error 是 GDAL/GEOS/Proj.4 版本不符合要求的提示。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/126032/1686708933109-481e3593-1893-4643-9341-d4373652256b.png#averageHue=%23090705&clientId=u63ee707b-5cc1-4&from=paste&height=524&id=u535d55ca&originHeight=524&originWidth=848&originalType=binary&ratio=1&rotation=0&showTitle=false&size=52770&status=done&style=none&taskId=ua7209a3c-680d-4ff8-b753-533a540b5e9&title=&width=848)
于是，开始手动去安装 GDAL/GEOS/Proj.4，之所以选择基于普通用户手动去源码编译安装，主要一个原因是系统版本太老，第二出于安全考虑避免 root 带来的一系列麻烦。

一番折腾，把 **gdal-2.2.3+geos-3.4.0+proj-4.9.1** 都装好了，设置完 **PATH+LD_LIBRARY_PATH** 后，却悲催的出现类似这个的报错：[https://github.com/r-spatial/sf/issues/678](https://github.com/r-spatial/sf/issues/678) —— 这是`sf`包的一个错误！

于是，问题变成了如何成功去安装`sf`包，或者说如何解决`sf`的 **proj_conf_test.c:4:28: error: expected ')' before 'const'** 安装错误。

# sf 包依赖与解决

在`sf`包的 [https://github.com/r-spatial/sf#linux](https://github.com/r-spatial/sf#linux) 中明确提到了这个包需要依赖 GDAL/GEOS/Proj.4，具体版本要求如下。

> 📢 **For Unix-alikes, GDAL (>= 2.0.1), GEOS (>= 3.4.0) and Proj.4 (>= 4.8.0) are required.**

后来经过一番的折腾尝试，才发现：

1. **gdal-2.2.3+geos-3.4.0+proj-4.9.1 **出现类似[这个](https://github.com/r-spatial/sf/issues/678)的报错 —— **proj_conf_test.c:4:28: error: expected ')' before 'const'**！
2. **gdal-2.2.0+geos-3.4.0+proj-4.8.0 **的组合可以解决以上遇到的问题！
3. **手动源码安装的话，Proj.4 要先于 GDAL 安装，因为 GDAL 安装的时候需要指定 Proj.4 进行编译。**

## GEOS

1. 要求 GEOS version >= 3.4.0；

```bash
export PATH=/Bioinfo/Pipeline/SoftWare/gcc-4.8.5/bin:$PATH
wget https://download.osgeo.org/geos/geos-3.4.0.tar.bz2
tar xvjf geos-3.4.0.tar.bz2 -C ../build/
cd ../build/geos-3.4.0
./configure --prefix=/Bioinfo/Pipeline/SoftWare/geos-3.4.0
make -j4 && make install
```

## Proj.4

1. 要求 Proj.4 (>= 4.8.0) ；
2. **Proj.4 要先于 GDAL 安装！**

```bash
export PATH=/Bioinfo/Pipeline/SoftWare/gcc-4.8.5/bin:$PATH
wget https://download.osgeo.org/proj/proj-4.8.0.tar.gz
tar zvxf proj-4.8.0.tar.gz -C ../build/
cd ../build/proj-4.8.0/
./configure --prefix=/Bioinfo/Pipeline/SoftWare/proj-4.8.0
make -j4 && make install
```

## GDAL

1. 要求 GDAL version >= 2.0.1；记住一定要加 **--with-static-proj4** 进行编译，否则 [https://github.com/r-spatial/sf/issues/678](https://github.com/r-spatial/sf/issues/678) 这个问题没法解决！！
2. 尝试了一下 **gdal-2.2.3+proj-4.9.1** 组合，好像有问题（**configure: error: GDALAllRegister not found in libgdal**）；

![image.png](https://cdn.nlark.com/yuque/0/2023/png/126032/1686707981701-d82448a7-95de-4571-b1ef-a5713e883cf6.png#averageHue=%23060403&clientId=u63ee707b-5cc1-4&from=paste&height=955&id=u4f240ffd&originHeight=955&originWidth=1297&originalType=binary&ratio=1&rotation=0&showTitle=false&size=140879&status=done&style=none&taskId=u4d298e07-54d1-4b98-ac52-c0e822ff6a4&title=&width=1297)

3. 但是 GDAL-2.2.0+proj-4.8.0 是可以的！

```bash
$enabledevtoolset4
wget http://download.osgeo.org/gdal/2.2.0/gdal-2.2.0.tar.gz
tar zvxf gdal-2.2.0.tar.gz -C ../build/
cd ../build/gdal-2.2.0
./configure --prefix=/Bioinfo/Pipeline/SoftWare/gdal-2.2.0 --with-static-proj4=/Bioinfo/Pipeline/SoftWare/proj-4.8.0/
make -j4 && make install
```

## 最终安装命令

```bash
export PATH=/Bioinfo/Pipeline/SoftWare/gdal-2.2.0/bin:/Bioinfo/Pipeline/SoftWare/geos-3.4.0/bin:/Bioinfo/Pipeline/SoftWare/proj-4.8.0/bin:$PATH
export LD_LIBRARY_PATH=/Bioinfo/Pipeline/SoftWare/gdal-2.2.0/lib:/Bioinfo/Pipeline/SoftWare/geos-3.4.0/lib:/Bioinfo/Pipeline/SoftWare/proj-4.8.0/lib:$LD_LIBRARY_PATH
```

出现 **configure: error: libproj not found in standard or given locations.** 异常，参考：[https://github.com/r-spatial/sf/issues/1471](https://github.com/r-spatial/sf/issues/1471) 得到解决：

```bash
# configure: error: libproj not found in standard or given locations.
options("repos"=c(CRAN="https://mirrors.tuna.tsinghua.edu.cn/CRAN/"))
install.packages('sf', configure.args='--with-gdal-config=/Bioinfo/Pipeline/SoftWare/gdal-2.2.0/bin/gdal-config --with-geos-config=/Bioinfo/Pipeline/SoftWare/geos-3.4.0/bin/geos-config --with-proj-include=/Bioinfo/Pipeline/SoftWare/proj-4.8.0/include --with-proj-lib=/Bioinfo/Pipeline/SoftWare/proj-4.8.0/lib', configure.vars='GDAL_DATA=/Bioinfo/Pipeline/SoftWare/gdal-2.2.0/share/gdal')
```

# 使用说明

安装完`sf`和`ggVennDiagram` 后，如果要使用这两个包，则需要：

```bash
export LD_LIBRARY_PATH=/Bioinfo/Pipeline/SoftWare/gdal-2.2.0/lib:/Bioinfo/Pipeline/SoftWare/geos-3.4.0/lib:/Bioinfo/Pipeline/SoftWare/proj-4.8.0/lib:$LD_LIBRARY_PATH
```

![image.png](https://cdn.nlark.com/yuque/0/2023/png/126032/1686711600565-d48881bd-57bb-44ff-9521-a7351a42e0cd.png#averageHue=%23070504&clientId=u63ee707b-5cc1-4&from=paste&height=449&id=uca02cf10&originHeight=449&originWidth=727&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28844&status=done&style=none&taskId=u7bcdddaa-21f9-4410-839c-ae034bf3662&title=&width=727)
