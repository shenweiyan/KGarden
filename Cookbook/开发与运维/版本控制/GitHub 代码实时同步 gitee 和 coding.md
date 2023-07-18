GitHub 作为全世界最大的代码集中地，在上面，我们可以随意地下载或者参与各种著名开源项目和开源开发框架。

**玩 GitHub 至少有以下几个好处：**

1. 获取最新最热门最实用的开源组件，有助于开发公司项目；
2. 获取最流行的技术相关源代码，有助于参考学习借鉴；
3. 参与感兴趣的开源项目，增强与他人协作开发的能力；
4. 创建属于自己的开源项目，提升编程能力，打造个人名片。

但由于一些大家都知道的原因，国内访问 GitHub 有时候会不太稳定，这就可能导致你在安装 GitHub 上的一些软件（或者拉取代码）的时候，由于网络问题而失败。这时候你就想：

1. 把代码同步一份到 gitee，从 gitee 进行拉取下载；
2. 把代码同步一份到 coding，使用 coding 的网站托管/自动构建功能，实时部署你的站点应用。

虽然，gitee 和 coding 都提供了从 GitHub 导入项目和强制更新的选项（coding 还提供了可以**通过触发时间来自动同步**），但都比较繁琐，在这里介绍一种通过 GitHub Actions 的方法是一步实现 GitHub 代码实时同步 gitee 和 coding。

# 使用秘钥的方式

首先说明一下，这种方式有点繁琐，而且只能把 GitHub 的代码同步到 gitee 码云上，暂时无法同步到 coding 上（至少在本人的测试中，coding 是没办法的）。

基于秘钥的方式同步，主要参考 [Gitee Pages Action](https://github.com/yanglbme/gitee-pages-action) 项目的做法，具体步骤如下。

## 配置密钥

:::tips
**⚠️ 注意：Github 上的 SSH Keys 公钥和 Gitee 的个人设置页面配置的 SSH Keys 公钥内容必须是完全一致的！**
:::

密钥的配置步骤如下。

首先，在命令行终端或 Git Bash 使用命令 **ssh-keygen -t rsa -C "youremail@example.com**" 生成 SSH Key，注意替换为自己的邮箱。生成的 **id_rsa 是私钥**，**id_rsa.pub 是公钥**。(注意此处不要设置密码，生成的公私钥用于下面 GitHub / Gitee 的配置，以保证公私钥成对，否则从 GitHub -> Gitee 的同步将会失败。)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FnLtojw2-D77uJnv9XmlQRGFrXoZ.png)
第二，在 GitHub 项目的「**Settings** -> **Secrets** → **New repository secret**」路径下配置好命名为 **GITEE_RSA_PRIVATE_KEY** 和 **GITEE_PASSWORD** 的两个密钥。其中：

- **GITEE_RSA_PRIVATE_KEY** 存放 **id_rsa 私钥**；
- **GITEE_PASSWORD** 存放 Gitee **帐号的密码**；

私钥文件内容 **/home/shenweiyan/.ssh/id_rsa**（如下仅为示例）：

```
-----BEGIN RSA PRIVATE KEY-----
MIIEoQIBAAKCAQEAu8Y+QEgYbQG2g01THpSqG3SAEt9JrH0TvFGZI3DS0HjbRz46
7qkiT+TXju7ilAEfX26JkstIxKDA9rOMlndsuhFuzT1kCB6+iA6Uoauu5/xNnRqf
+tchWhSroJeQLo7WUuaoKKBaU1OgViIYBf431vNee9vuJobve2lKCuN1I5yT86lt
2O/5fOCgwPd0p8S9v+xe4IADu0dgFQijZtc5VnvHQk0RRwORJsyaJIr+0z6AycoR
EY/ZBPsJ3gJ0naRS81eEwEv87fS/bq8iYBaaWNzpB/G4LLywKvD8Yuyv0q9zKSUV
jTgWE0/CgH14T3JTvyzFe5lYktj1GzAgX7Pu1wIBIwKCAQEAgMJz1E6xqdVJ86K8
p0Fes72Zpop7qXpWrQTAx9hWC0uPDEft5XtKunEI1wo098ZBZgKnepoGAyxnD5EP
8iYBaaWNzpB/G4LLywKvD8YuyvYml506gB59RRV9AGft1shYc1xWDTrBmDlYAIxo
Pp+xQvAGRk2qnhNiY0D8iYBaaWNzpB/G4LLywKvD8YuyvL/AunpbLhLtkeCsdbiI
q8IpWAS1rFRkq5jj6phHAoGAc2t6PYMLPAgKKbQL+nCXBpK7pOduaKsZrFpNGBF2
JmV+GOR0aFSkyHHvViXWF5QvYDy26/ROEVGEcHcAMF0iEvtsRa3WHv9otVf7tPJ6
FjzQYbYN0zNvRR+eJrE9NoelvKG4HIMWYLtQqap2H6p2oQlkXeIvkhFR/wUf3HVN
8o0CgYA2BIrBgMx5LtEcsbht7/dl/sj4oRmQhl7X+uA/ISjtk9hwCALLhCb+yevr
Y+N74xj4Q1hA1ro477z6axoS8E0fATb9s4rieG7KC61u8lII0epEzDdQQ348yA7Y
XLDII+15947YRnitPaI6J8E5Zpie6cdC1gtvZwws+9J+Qk5nBA==
-----END RSA PRIVATE KEY-----
```

公钥文件内容 **/home/shenweiyan/.ssh/id_rsa.pub**（如下仅为示例）：

```
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAu8Y+QEgYbQG2g01THpSqG3SAEt9JrH0TvFGZI3DS0HjbRz467qkiT+TXju7ilAEfX26JkstIxKDA9rOMlndsuhFpWAS1rFRkq5jj6phHAoGAc2t6PYMLPAgKKbQL+nCXBpK7pOduT3JTvyzFe5lYktj1GzAgX7Pu1w== ishenweiyan@foxmail.com
```

第三，在 GitHub 的个人设置页面「[Settings -> SSH and GPG keys](https://github.com/settings/keys)」配置 SSH Keys 公钥（即：**id_rsa.pub**），命名随意。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FtcUhF3OCjBv7i0AZpOHWVpUoGkr.png)
第四，在 Gitee 的个人设置页面「[安全设置 -> SSH 公钥](https://gitee.com/profile/sshkeys)」配置 SSH 公钥（即：**id_rsa.pub**），命名随意。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/Fsey-rTV7Z0rZMAvirujPTPobsL5.png)

## 创建 workflow

在你的 GitHub 项目 **.github/workflows/** 文件夹下创建一个 .yml 文件，如 **sync.yml**，内容如下：

```yaml
name: SyncToGitee

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Github Sync to Gitee
        uses: wearerequired/git-mirror-action@master
        env:
          # 注意在 Settings->Secrets 配置 GITEE_RSA_PRIVATE_KEY
          SSH_PRIVATE_KEY: ${{ secrets.GITEE_RSA_PRIVATE_KEY }}
        with:
          # 注意替换为你的 GitHub 源仓库地址
          source-repo: git@github.com:shenweiyan/WebStack-Hugo.git
          # 注意替换为你的 Gitee 目标仓库地址
          destination-repo: git@gitee.com:shenweiyan/WebStack-Hugo.git
```

## 执行同步

最后，修改代码（如修改 README），提交，成功触发同步！
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FhNciqJ2JAce216HOdJhqNMLNylJ.png)

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/Fr8zbEMkhW6KnY1RqwtOwMX0ezP_.png)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FhC-7z6OHx5gKOtXGRdFbh3JL60z.png)

# 使用账号密码的方式

这个方法参考的是 [@abersheeran](https://github.com/abersheeran) 的 [push-to-mirror ](https://github.com/abersheeran/index.py/blob/a9ef1e2dca0c975108b942657679ec47908c7bcc/.github/workflows/setup.yml#L55-L82)方法，可以同步到 gitee 以及任何一个支持 git 的平台，其原理很简单，就拉取然后推送。

## 配置账号密码

首先，在 GitHub 项目的「**Settings** -> **Secrets** → **New repository secret**」路径下配置好你需要同步的 coding 和 gitee 账号密码（命名可以随便，只要求跟下面 **sync.yml **的变量名称一致即可）。其中：

- **GITEE_USERNAME** 存放 **Gitee 的账号**；
- **GITEE_PASSWORD** 存放 **Gitee** **帐号的密码**；
- **CODING_USERNAME** 存放 **Coding 的账号**；
- **CODING_PASSWORD** 存放 **Coding** **帐号的密码**；

![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FsW8HjkaxCtwI0YVC4DHrFPceXmD.png)
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FrllDsG6dnVD9N553JIP4a1GVOZA.png)

## 创建 workflow

在你的 GitHub 项目 **.github/workflows/** 文件夹下创建一个 .yml 文件，如 **sync.yml**，内容如下：

```yaml
name: Sync-To-Gitee-and-Coding

on:
  push:
    branches:
      - main

jobs:
  push-to-mirror:
    runs-on: ubuntu-latest
    steps:
      - name: Clone
        run: |
          git init
          git remote add origin https://github.com/${GITHUB_REPOSITORY}.git
          git fetch --all
          for branch in `git branch -a | grep remotes | grep -v HEAD`; do
            git branch --track ${branch##*/} $branch
          done
        env:
          GITHUB_REPOSITORY: shenweiyan/GitHub-SYNC

      - name: Push to Coding
        run: |
          remote_repo="https://${CODING_USERNAME}:${CODING_PASSWORD}@e.coding.net/${CODING_REPOSITORY}.git"

          git remote add coding "${remote_repo}"
          git show-ref # useful for debugging
          git branch --verbose

          # publish all
          git push --all --force coding
          git push --tags --force coding
        env:
          CODING_REPOSITORY: shenweiyan/devs/GitHub-SYNC
          CODING_USERNAME: ${{ secrets.CODING_USERNAME }}
          CODING_PASSWORD: ${{ secrets.CODING_PASSWORD }}

      - name: Push to Gitee
        run: |
          remote_repo="https://${GITEE_USERNAME}:${GITEE_PASSWORD}@gitee.com/${GITEE_REPOSITORY}.git"

          git remote add gitee "${remote_repo}"
          git show-ref # useful for debugging
          git branch --verbose

          # publish all
          git push --all --force gitee
          git push --tags --force gitee
        env:
          GITEE_REPOSITORY: shenweiyan/GitHub-SYNC
          GITEE_USERNAME: ${{ secrets.GITEE_USERNAME }}
          GITEE_PASSWORD: ${{ secrets.GITEE_PASSWORD }}
```

## 执行同步

最后，修改代码（如修改 README），提交，成功触发同步！
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FnPI_f183sRTBRgB6Gh5bVbzJE7b.png)
![同步 workflows 执行成功！](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/Fn7B-rFuN_RYsSuuraS0H7YpBx-f.png "同步 workflows 执行成功！")
![GitHub 代码同步 Coding 成功！](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FqGnfZCiqR_Jd1EiWghqmztpcfe2.png "GitHub 代码同步 Coding 成功！")
![GitHub 代码同步 Gitee 成功！](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-cookbook-img/FlfUfgGFbwojp08rLox4EwsFVP4d.png "GitHub 代码同步 Gitee 成功！")

# 总结

文章中参考前人的一些成果成功把 GitHub 的代码同步到了 gitee 和 coding，可能还有可以改进的地方，但不管怎么说，基本满足了个人的需求。

最后，GitHub 是一个非常棒的平台，希望大家拥抱开源，多多分享。
