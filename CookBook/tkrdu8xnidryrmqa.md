由于 conda 的慢，现准备转移到 [micro]mamba，这是学习和使用过程中得一些笔记。

# 安装

官方的安装文档：[https://mamba.readthedocs.io/en/latest/installation.html](https://mamba.readthedocs.io/en/latest/installation.html)

## mamba

### Fresh install

We strongly recommend to start from Mambaforge, a community project of the conda-forge community.
You can download [Mambaforge](https://github.com/conda-forge/miniforge#mambaforge) for Windows, macOS and Linux.
Mambaforge comes with the popular conda-forge channel preconfigured, but you can modify the configuration to use any channel you like.

## micromamba

```bash
mkdir /ifs1/micromamba
cd /ifs1/micromamba
curl -Ls https://micro.mamba.pm/api/micromamba/linux-64/latest | tar -xvj bin/micromamba
sudo ln -s /ifs1/micromamba/bin/micromamba /usr/local/bin
```

# 使用

```bash
micromamba -r /ifs1/micromamba create -n singularity -c conda-forge singularity
```

![image.png](https://cdn.nlark.com/yuque/0/2023/png/126032/1681885072948-38e71d3b-293f-4e26-acd4-21dcf34429b1.png#averageHue=%23060403&clientId=u4270f7e8-9980-4&from=paste&height=786&id=ub6c44792&originHeight=786&originWidth=803&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57906&status=done&style=none&taskId=ubb9756f4-74e4-4313-9c4d-74ed0a57d1c&title=&width=803)

```bash
$ export MAMBA_ROOT_PREFIX=/ifs1/micromamba
$ micromamba run -n singularity singularity
```
