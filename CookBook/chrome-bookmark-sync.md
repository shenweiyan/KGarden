由于东方的神秘力量，国内正常情况下是连不上 Google 账号的，所以平时使用 Chrome 经常会头疼书签同步问题。  由于魔法力量的不稳定，有时候不同步，有时还会同步错乱导致书签丢失。

针对这个问题，这两天尝试了一下微软最新版本的 Edge，不得不说 Edge 很多地方的确很符合国人的使用习惯，尤其无需梯子即可进行书签同步，真心香！但唯一有点坑的地方是对 Windows 7 的支持还不够友好。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615866522437-624fec49-9a65-4d99-b6fe-490e4ec7e859.png#height=458&id=ieFXU&originHeight=458&originWidth=1018&originalType=binary&ratio=1&size=64520&status=done&style=none&width=1018)

虽然现在的 Edge 提供了 Windows 7 版本，但是安装过程中需要把 IE 升级到最新的 IE11，就算你好不容易把 IE11 升级好的，Edge 在获取更新说不定还会遇到其他更加难搞的事情。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615872365724-c518eb80-e0c3-4364-821b-4e2324cc77f6.png#height=528&id=FYPhW&originHeight=528&originWidth=997&originalType=binary&ratio=1&size=61194&status=done&style=none&width=997)

回到 Chrome，介绍一下这个偶然发现的插件：书签同步码云。这个工具可以把谷歌浏览器书签同步至码云
在国内码云平台是访问速度比较快的，平时用着也比较方便。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615872609499-226f1bd0-8c41-4328-894d-9568a5710080.png#height=807&id=xRj63&originHeight=807&originWidth=1020&originalType=binary&ratio=1&size=138536&status=done&style=none&width=1020)

## 1. 安装插件

如果有条件用谷歌商店的可以直接去谷歌商店中搜索安装，当然也有同步在 Github 中的插件，也是类似，应用商店也可以找到。

如果无法使用谷歌商店，我上传到天翼云盘，有需要的可以关注** "BioIT 爱好者**" 公众号后台回复  **"码云书签"**  关键字，即可获取下载链接。
![BioIT爱好者-关注我们.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1616722230840-c1d2cbc8-a431-46c3-89ca-d72cb59aa2fc.png#height=454&id=oUvQ3&originHeight=600&originWidth=600&originalType=binary&ratio=1&size=64809&status=done&style=none&width=454)
下载解压后，把里面 .crx 文件直接拖到浏览器，应该就可以加载，如果提示无效或者错误的话，可以把后缀名改成 .zip 或者 .rar，然后找个目录解压了，在打开开发者模式：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615872923958-ef09d4c2-43fd-4f7f-bcb1-bcf424fb7ceb.png#height=578&id=C0Nw3&originHeight=578&originWidth=1132&originalType=binary&ratio=1&size=75806&status=done&style=none&width=1132)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1616031262621-f4080565-6145-4e36-851e-f6d5ca7e0622.png#height=234&id=DB04N&originHeight=234&originWidth=942&originalType=binary&ratio=1&size=37991&status=done&style=none&width=942)
然后点击加载已解压的扩展程序，选择你解压的目录，注意是目录，不是具体的文件，点击确定就可以，应该就可以见扩展程序页面的插件了，如下：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615873108101-599088c2-fab3-4ff4-ac7b-12049b05fcac.png#height=220&id=XMb8u&originHeight=220&originWidth=417&originalType=binary&ratio=1&size=11041&status=done&style=none&width=417)

插件打开长这样子：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615873300868-dbfad7ba-63a1-41cc-bf9d-febb049e8ca7.png#height=584&id=htNik&originHeight=584&originWidth=1075&originalType=binary&ratio=1&size=77870&status=done&style=none&width=1075)

## 2. 添加码云仓库使用

### 2.1 新建仓库

打开码云，新建仓库。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615873583223-c927d0ff-a34f-4de2-bc23-a526ea3a5138.png#height=1006&id=EtTTu&originHeight=1006&originWidth=719&originalType=binary&ratio=1&size=105971&status=done&style=none&width=719)

### 2.2 填写插件信息

#### access_token

首先，在码云中点击设置：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615874035936-1e0663d5-6f84-4bd4-b026-6d25515812ec.png#height=351&id=tlLbK&originHeight=351&originWidth=1341&originalType=binary&ratio=1&size=64147&status=done&style=none&width=1341)
第二，进去之后，点击  **私人令牌 -> 生成新令牌  **：![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615874269912-b8bceee3-34d3-43e9-a918-f1bf7f884bab.png#height=749&id=nWGMD&originHeight=749&originWidth=1117&originalType=binary&ratio=1&size=72058&status=done&style=none&width=1117)
点击生成令牌之后，在页面中填写 **私人令牌描述**，下面权限要全选：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615874392117-d9e4c243-b620-47a2-ab0d-fe96702b9533.png#height=751&id=IQnqQ&originHeight=751&originWidth=1114&originalType=binary&ratio=1&size=101998&status=done&style=none&width=1114)
然后点击提交，会验证当前账户密码，验证之后会弹出令牌页面。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615874468360-8263af55-9399-48d6-9a45-1a0ad513a0b6.png#height=743&id=UXorT&originHeight=743&originWidth=1130&originalType=binary&ratio=1&size=81438&status=done&style=none&width=1130)

**注意：这个令牌只显示一次，建议复制保存到本地记事本或者其他地方之后再确认关闭！！！**
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615874521305-614ab02b-b356-430f-b60c-27e23a375bfb.png#height=740&id=dzRyp&originHeight=740&originWidth=1136&originalType=binary&ratio=1&size=85348&status=done&style=none&width=1136)
然后再将该该令牌复制到 插件的 access_token 位置就好。

#### owner

回到我们刚才创建的仓库，例如 [https://gitee.com/shenweiyan/bookmarks](https://gitee.com/shenweiyan/bookmarks)，owner 就是 **shenweiyan**，把这个信息复制到插件的 owner 位置就好。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615874714449-b3088741-e810-4b31-9b7f-ca14d6a24b78.png#height=445&id=m68rT&originHeight=445&originWidth=1152&originalType=binary&ratio=1&size=84878&status=done&style=none&width=1152)

#### repo

以我们刚才创建的 [https://gitee.com/shenweiyan/bookmarks](https://gitee.com/shenweiyan/bookmarks) 仓库为例，repo 就是 **bookmarks**，把这个信息复制到插件的 repo 位置就好。

#### path

注意，这里写的是相对 repo 仓库的 path 信息，如果你想直接把文件保存的仓库根目录，path 就可以写 chrome.html 或者 chrome.json，名称可以随便写，以 html/json 作为格式后缀即可。

#### branch

分支(通常是写 master)。

以 [https://gitee.com/shenweiyan/bookmarks](https://gitee.com/shenweiyan/bookmarks) 仓库为例的最终插件信息如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615876647844-01b78fed-2c09-4cd4-99b4-61064b746627.png#height=354&id=NznFn&originHeight=354&originWidth=354&originalType=binary&ratio=1&size=16301&status=done&style=none&width=354)

## 3. 使用事项

注意，如果是第一次添加使用，在填写完信息之后，需要先在仓库中创建一个 path 的文件（例如，这里的 chrome.json，需要先创建）。

然后，直接点 Upload 把当前浏览器的书签上传到 gitee 仓库中。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1615875558341-63a1be95-6ccd-4db7-81b2-0a2ecb6391bb.png#height=445&id=AHGBv&originHeight=445&originWidth=1480&originalType=binary&ratio=1&size=97081&status=done&style=none&width=1480)
最后，就可以通过点击 Download 把云端（即仓库）的书签信息同步到其他电脑的当前浏览器。

:::success
**注意，注意，注意！！！**
如果是两个电脑用这个同步，建议先把当前浏览器的书签线导出到本地，因为这个 Download 会用云端（即仓库）的书签把当前浏览器（即本地）的书签**覆盖。如果直接把当前浏览器（即本地）的书签 Upload，则会把云端（即仓库）的书签覆盖。**
:::

我就是这么操作之后才知道，不过还好本地覆盖云端还有救，因为码云可以看历史版本，恢复一下就好了。

正常操作应该是：

1）先将当前浏览器书签导出到本地电脑。
2）然后点击 Download 把云端仓库的书签信息同步到当前浏览器。
3）然后再将本地书签导入到当前浏览器，再自己将书签整理下，把当前浏览器的书签和云端仓库的书签整合。
4）整理完毕再上传（Upload）就 OK。

## 3. 参考资料

1. [谷歌浏览器书签同步工具 - 知乎](https://zhuanlan.zhihu.com/p/158026344)
