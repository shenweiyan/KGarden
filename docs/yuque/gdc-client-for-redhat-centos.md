今天，只聊一下 RedHat/CentOS 下 gdc-client 安装的那些事~

gdc-client（[https://gdc.cancer.gov/access-data/gdc-data-transfer-tool](https://gdc.cancer.gov/access-data/gdc-data-transfer-tool)）是由 GDC 官方提供的一个可以在命令行下批量下载 TCGA 数据的客户端工具。

在 gdc-client 官网可以看到 Mac、Windows 和 Ubuntu 的二进制版本下载，却唯独没看到 CentOS/RedHat 版本的！而且还给了我们一个提示说，如果你想要安装 RedHat Enterprise Release 6  版本的 gdc-client 需要跟 GDC 进行联系！！
![gdc.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597648591580-85e505fa-1b8d-48af-ac30-2737598a46c2.png#align=left&display=inline&height=653&originHeight=653&originWidth=700&size=55820&status=done&style=none&width=700)
如果你用 "gdc-client" + "centos6" 的关键字去谷歌，会发现大部分的答案都是教你用 Python2 的虚拟环境去安装 gdc-client。
![gdc-client-centos6-Google.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597649000539-5c5d4dd0-7bc0-4fd1-83a2-2063d7eaabb1.png#align=left&display=inline&height=687&originHeight=687&originWidth=669&size=78032&status=done&style=none&width=669)
其实，这些大部分都存在误导的成分，虽然 gdc-client 官网虽然没有提供 CentOS 6 的二进制程序包，但它托管在 GitHub 的源码我们是可以直接安装的，而且是只支持 Python 3！！

### 坑一：Python 2 引发 parse 模块异常

```bash
conda create -n Python2 python=2.7
source activate Python2
git clone https://github.com/NCI-GDC/gdc-client
cd gdc-client
python setup.py install 2>&1 | tee -a install.log
```

这种方法虽然看起来没什么问题，却会执行 `gdc-client -h`  提示 parse 模块异常。其原因是 **build/bdist.linux-x86_64/egg/gdc_client/download/parser.py**  的第三行 `from urllib import parse as urlparse`  是 py3 的语法：在 python 2.x 中的 `urlparse`  模块在 Python 3 中已经重命名为 `urllib.parse` 。

```python
# Python 2 正确语法
from urlparse import urlparse

# Python 3 正确语法
from urllib import parse as urlparse
```

![gdc-client-parser.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597650816075-5f920636-e182-4f97-b539-216f1eb051e5.png#align=left&display=inline&height=422&originHeight=422&originWidth=1095&size=35607&status=done&style=none&width=1095)

### 坑二：conda 安装无法响应

bioconda 虽然也提供了 gdc-client，但是本人 一直没法安装成功，可能是我的运气不太好！
![bioconda-gdc-client.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597651034760-e3879fb0-f225-4921-9d19-a15c230ee24a.png#align=left&display=inline&height=326&originHeight=326&originWidth=1097&size=22528&status=done&style=none&width=1097)

### 最后，CentOS 6 的正确解锁姿势

```bash
conda create -n gdc python=3.7
source activate gdc

git clone https://github.com/NCI-GDC/gdc-client
cd gdc-client
pip install -r requirements.txt
python setup.py install 2>&1 | tee -a install.log
```

![gdc-client-help.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597651834408-7083cc7f-46b6-43a4-a698-d1a98733c42a.png#align=left&display=inline&height=399&originHeight=399&originWidth=897&size=24277&status=done&style=none&width=897)

最后，打开 GDC 的官方《[Data Transfer Tool Command Line Documentation](https://docs.gdc.cancer.gov/Data_Transfer_Tool/Users_Guide/Data_Download_and_Upload/)》文档，查看在命令下怎么使用 gdc-client 下载你想要的 TCGA 数据吧！
