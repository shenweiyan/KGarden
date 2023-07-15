事情起源于源码编译安装 PyTorch 的时候一直提示`sys/auxv.h: No such file or directory`。

```bash
# Clone 源码
$ git clone --recursive https://github.com/pytorch/pytorch
$ cd pytorch/
$ git submodule update --init --recursive	 # 第三方依赖很多，这一步非常耗时

# 开始源码安装
$ NO_CUDA=1 python3 setup.py install
....
/Bioinfo/SRC/github/build/pytorch/third_party/fbgemm/third_party/asmjit/src/asmjit/core/cpuinfo.cpp:18:24: fatal error: sys/auxv.h: No such file or directory
compilation terminated.
[3849/6269] Building CXX object third_party/fbgemm/CMakeFiles/fbgemm_generic.dir/src/EmbeddingSpMDM.cc.o
[3851/6269] Building CXX object third_party/fbgemm/CMakeFiles/fbgemm_avx2.dir/src/FbgemmI8DepthwiseAvx2.cc.o
ninja: build stopped: subcommand failed.
```

# 手动编译安装 Python 3.9

最开始在 [https://github.com/python/cpython/issues/91124](https://github.com/python/cpython/issues/91124) 这里看到了 Python 3.9 有对这个问题进行了修复，于是考虑着从 Cpython 的源码入手，手动编译安装一下 Python 3.9，然后重新源码编译安装一下 PyTorch 试试。

## 下载源码

```bash
$ git clone -b 3.9 https://github.com/python/cpython.git
```

## 编译安装

具体步骤如下。对于一些遇到的依赖问题，参考：[生物信息学 Python 入门之源码安装](https://www.yuque.com/shenweiyan/cookbook/install-python-from-source?view=doc_embed)

```bash
$ cd cpython
$ ./configure
$ make
$ make test
$ make install

# If you wish, you can create a subdirectory and invoke configure from there. For example:
mkdir debug
cd debug
../configure --with-pydebug
make
make test
# This will fail if you also built at the top-level directory. You should do a make clean at the top-level first.
```

一番折腾后，发现安装了最新的 Python 3.9.16 然后重新源码编译 PyTorch 依然会出现 **sys/auxv.h: No such file or directory**，重新审查报错信息，发现从 log 信息中，可以看到是`asmjit`模块引发的这一问题。

# 安装 asmjit 模块

于是，顺其自然去`pip3 install asmjit`，却得到了一个错误导致无法成功安装：

```bash
RuntimeError: Could not find a `llvm-config` binary. There are a number of reasons this could occur, please see: https://llvmlite.readthedocs.io/en/latest/admin-guide/install.html#using-pip for help.
```

接着，开始安装 llvm-config。

## 手动编译安装 LLVM11

参考：[第 1 章 编译和安装 LLVM — Getting Started with LLVM Core Libraries 文档](https://getting-started-with-llvm-core-libraries-zh-cn.readthedocs.io/zh_CN/latest/ch01.html#id6)

> pip3 install asmjit --use-pep517
> Building llvmlite requires LLVM 11, 12, 13, or 14, got '3.4'. Be sure to set LLVM_CONFIG to the right executable path.

```bash
# 设置环境变量
export PATH=/RiboBio/Bioinfo/Pipeline/SoftWare/cmake-3.21.1/bin:$PATH
$enabledevtoolset4  # 编译llvm，需要gcc至少为5.1版本，centos默认安装的是gcc 4.8.5
export PATH=/RiboBio/Bioinfo/Pipeline/SoftWare/Python-3.9.7/bin:$PATH

# 下载解压
curl -LjO https://github.com/llvm/llvm-project/archive/refs/tags/llvmorg-11.0.0.tar.gz
tar zvxf llvmorg-11.0.0.tar.gz
cd llvm-project-llvmorg-11.0.0
mkdir build
```

编译命令一，会导致以下截图错误，因此放弃该编译命令：

```bash
cmake -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_RTTI=ON -DLLVM_LIBC_ENABLE_LINTING=OFF -DCMAKE_INSTALL_PREFIX=/RiboBio/Bioinfo/Pipeline/SoftWare/llvm-11.0.0 -DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra;lld;lldb" -DLLVM_ENABLE_RUNTIMES="libc;compiler-rt;libcxx;libcxxabi;libunwind" -G "Unix Makefiles" ../llvm
```

### ![](https://cdn.nlark.com/yuque/0/2023/png/126032/1685347699800-12bf72bc-4b7d-4ae9-aae7-3112baf36156.png#averageHue=%23060302&from=url&id=C1nk2&originHeight=765&originWidth=1371&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

编译命令二，可以正常编译安装成功：

```bash
# 实际编译命令
cmake -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_RTTI=ON -DLLVM_ENABLE_PROJECTS="clang;libcxx;libcxxabi" -DCMAKE_INSTALL_PREFIX="/RiboBio/Bioinfo/Pipeline/SoftWare/llvm-11.0.0" -G "Unix Makefiles" ../llvm

# 后续编译安装
make -j4
make install
```

## 安装 asmjit

```bash
# 设置环境变量
export PATH=/RiboBio/Bioinfo/Pipeline/SoftWare/llvm-11.0.0/bin:$PATH
export PATH=/RiboBio/Bioinfo/Pipeline/SoftWare/Python-3.9.7/bin:$PATH
$enabledevtoolset4	# 必须设置改env，否则 asmjit 安装会报一堆 c++ 错误

# 执行安装
pip3 install asmjit
```

![image.png](https://cdn.nlark.com/yuque/0/2023/png/126032/1685416566063-28461b40-1145-41d5-a8da-c495c550a867.png#averageHue=%23060403&clientId=ufc83d32a-eb48-4&from=paste&height=345&id=u289610b9&originHeight=345&originWidth=1562&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43905&status=done&style=none&taskId=u0514d966-ded5-4c0b-bf0b-33029eb8c3a&title=&width=1562)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/126032/1685495525200-1fc16184-ea5b-4be4-b54c-c365daa66915.png#averageHue=%23070503&clientId=uf4cb9347-803f-4&from=paste&height=162&id=u448b8d07&originHeight=162&originWidth=748&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13362&status=done&style=none&taskId=u98e2e37c-f64b-4100-9c92-c0d7f3f360e&title=&width=748)

# 安装 PyTorch

经过上面的各种处理，依然没法解决源码编译 PyTorch 时候出现的 **sys/auxv.h: No such file or directory** 的问题。

## PIP 安装

试试用 pip 安装一下：[https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/126032/1685425603669-b57b87a7-c87b-4719-bc41-43e17d958f38.png#averageHue=%23fdf5f3&clientId=u551d4bdf-9fbc-4&from=paste&height=766&id=u8961c50e&originHeight=766&originWidth=1286&originalType=binary&ratio=1&rotation=0&showTitle=false&size=113803&status=done&style=none&taskId=uf8a53c37-6955-4d82-b1e1-8310ac49889&title=&width=1286)

```bash
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
```

这种安装方法会导致 GLIBC 的问题：

```bash
$ python3
Python 3.9.7 (default, Sep 15 2021, 17:22:54)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-4)] on linux
Type "help", "copyright", "credits" or "license" for more information.
    >>> import torch
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "/RiboBio/Bioinfo/Pipeline/SoftWare/Python-3.9.7/lib/python3.9/site-packages/torch/__init__.py", line 217, in <module>
_load_global_deps()
File "/RiboBio/Bioinfo/Pipeline/SoftWare/Python-3.9.7/lib/python3.9/site-packages/torch/__init__.py", line 177, in _load_global_deps
raise err
File "/RiboBio/Bioinfo/Pipeline/SoftWare/Python-3.9.7/lib/python3.9/site-packages/torch/__init__.py", line 172, in _load_global_deps
ctypes.CDLL(lib_path, mode=ctypes.RTLD_GLOBAL)
File "/RiboBio/Bioinfo/Pipeline/SoftWare/Python-3.9.7/lib/python3.9/ctypes/__init__.py", line 374, in __init__
self._handle = _dlopen(self._name, mode)
OSError: /RiboBio/Bioinfo/Pipeline/SoftWare/glibc-2.14/lib/libc.so.6: version `GLIBC_2.17' not found (required by /RiboBio/Bioinfo/Pipeline/SoftWare/Python-3.9.7/lib/python3.9/site-packages/torch/lib/libgomp-a34b3233.so.1)
>>>
```

## 源码编译

> 📢 温馨提示：
> 清空当前编译命令：**python3 setup.py clean**

源码编译前的环境变量设置：

```bash
export PATH=/RiboBio/Bioinfo/Pipeline/SoftWare/cmake-3.21.1/bin:$PATH
export PATH=/RiboBio/Bioinfo/Pipeline/SoftWare/Python-3.9.7/bin:$PATH
export PATH=/RiboBio/Bioinfo/Pipeline/SoftWare/llvm-11.0.0/bin:$PATH
export LD_LIBRARY_PATH=/RiboBio/Bioinfo/Pipeline/SoftWare/glibc-2.14/lib:$LD_LIBRARY_PATH

export CC=/RiboBio/Bioinfo/Pipeline/SoftWare/llvm-11.0.0/bin/clang
export CXX=/RiboBio/Bioinfo/Pipeline/SoftWare/llvm-11.0.0/bin/clang++
export LDFLAGS="-L/RiboBio/Bioinfo/Pipeline/SoftWare/llvm-11.0.0/lib"
export CPPFLAGS="-I/RiboBio/Bioinfo/Pipeline/SoftWare/llvm-11.0.0/include"
```

执行源码编译命令：**NO_CUDA=1 python3 setup.py install**，出现无法找到 **OpenMP/OpenMP_CXX** 错误。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/126032/1685501624949-433c3c3f-cc8f-4399-b385-aea49955c3b3.png#averageHue=%23060301&clientId=uf4cb9347-803f-4&from=paste&height=705&id=u589bbfbc&originHeight=705&originWidth=1386&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89778&status=done&style=none&taskId=u6e4d0c20-0f6a-446d-80fe-4e78423760f&title=&width=1386)
执行编译命令：**USE_CUDA=0 USE_CUDNN=0 USE_MKLDNN=0 python3 setup.py develop**，虽然能解决 **OpenMP/OpenMP_CXX** 错误的问题，但依然没法绕过 **sys/auxv.h: No such file or directory** 的问题！

# 参考资料

- [Pytorch 源码编译简明指南 - Oldpan 的个人博客](https://oldpan.me/archives/pytorch-build-simple-instruction)
- [从零开始编译 PyTorch 软件包](https://zhuanlan.zhihu.com/p/347084475)
- [llvm clang 源码编译安装 - nanmi - 博客园](https://www.cnblogs.com/nanmi/p/16044444.html)
- [Centos7 上源码编译安装 llvm 11.0.0](https://zhuanlan.zhihu.com/p/350595463)
