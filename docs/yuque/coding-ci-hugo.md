一直以来，个人的很多应用站点，包括博客、[MDX](https://mdx.ncbix.com/)、[BioIT 网址导航](https://nav.bioitee.com/)等，都托管和部署在了 Coding 上，而且用的基本都是网站托管服务中的**"静态网站"**（coding 新建网站没有 hugo 可以勾选）!
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640314317910-e581d5eb-4908-47da-ac89-0af69fed3fa7.png#clientId=u1fcd185c-2a9d-4&from=paste&height=753&id=u1423683f&originHeight=753&originWidth=715&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49986&status=done&style=none&taskId=ue944d82a-1b46-4575-9df6-3eb5e5e281c&title=&width=715)

自己平时懒得去管主机的各种服务之类的，主机（尤其是云主机）在部署 Hugo 用的最多的是：

1. 使用 hugo 来生成站点的静态网站文件；
2. 把静态网站文件 push 到 coding，通过 coding 的静态网站服务进行自动部署。

今天，来解锁 coding 上的一个 **"持续集成"** 功能，通过它自动进行托管并部署 Hugo 网站（从此再也不用依赖主机了）。

# CODING 和腾讯云关联

这边要打通两边，做一个关联，详细就不多说了，折腾一下都 OK 的。

# 创建项目

做了上面的事情后，你应该会有一个团队主体，然后用这个团队主体创建一个项目。
![随便选一个都行，我是选的左边那个](https://cdn.nlark.com/yuque/0/2021/png/126032/1640248375785-954741d9-5f23-4c47-86df-ea914a290902.png#clientId=ud4798a05-3a10-4&from=paste&height=460&id=ud386de17&originHeight=460&originWidth=1222&originalType=binary&ratio=1&rotation=0&showTitle=true&size=51736&status=done&style=none&taskId=ude140ddf-a935-45aa-947c-818373e77ce&title=%E9%9A%8F%E4%BE%BF%E9%80%89%E4%B8%80%E4%B8%AA%E9%83%BD%E8%A1%8C%EF%BC%8C%E6%88%91%E6%98%AF%E9%80%89%E7%9A%84%E5%B7%A6%E8%BE%B9%E9%82%A3%E4%B8%AA&width=1222 "随便选一个都行，我是选的左边那个")
![填写项目信息](https://cdn.nlark.com/yuque/0/2021/png/126032/1640248494081-92366495-00df-4079-9255-40e879069c71.png#clientId=ud4798a05-3a10-4&from=paste&height=562&id=u7143884e&originHeight=562&originWidth=1087&originalType=binary&ratio=1&rotation=0&showTitle=true&size=49559&status=done&style=none&taskId=u6d28e1c1-7549-4dd3-a6d9-ce6b14a46cf&title=%E5%A1%AB%E5%86%99%E9%A1%B9%E7%9B%AE%E4%BF%A1%E6%81%AF&width=1087 "填写项目信息")

# 创建两个仓库

创建项目成功后进入项目。创建两个仓库，一个是存储源代码的文件，一个是放置生成的静态网站文件。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640248631157-70fb8f5e-ddea-464d-a51c-0c27fbe06452.png#clientId=ud4798a05-3a10-4&from=paste&height=275&id=u2cee33f8&originHeight=275&originWidth=685&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16492&status=done&style=none&taskId=u0dbd1b68-f354-4719-aa58-d5b1e88bb60&title=&width=685)

# 流程配置

创建完网站后，在左边导航栏选择"持续集成"→"构建计划"，选择自定义构建过程。
![创建构建计划](https://cdn.nlark.com/yuque/0/2021/png/126032/1640248885874-dd1cae1b-ec08-45fc-8e8a-cc6cf0dbb4cd.png#clientId=ud4798a05-3a10-4&from=paste&height=546&id=u43ada6f2&originHeight=546&originWidth=1172&originalType=binary&ratio=1&rotation=0&showTitle=true&size=66641&status=done&style=none&taskId=u6732fc1c-5fbc-493b-8d56-082322bf4f3&title=%E5%88%9B%E5%BB%BA%E6%9E%84%E5%BB%BA%E8%AE%A1%E5%88%92&width=1172 "创建构建计划")
![选择自定义构建过程](https://cdn.nlark.com/yuque/0/2021/png/126032/1640249032289-e3a97b91-7406-4a4c-a159-6ab0bc8ffaa6.png#clientId=ud4798a05-3a10-4&from=paste&height=526&id=u829bfe5a&originHeight=526&originWidth=1140&originalType=binary&ratio=1&rotation=0&showTitle=true&size=69586&status=done&style=none&taskId=u64fcc75d-ef2e-42a2-a6ff-bcfa2a6e1de&title=%E9%80%89%E6%8B%A9%E8%87%AA%E5%AE%9A%E4%B9%89%E6%9E%84%E5%BB%BA%E8%BF%87%E7%A8%8B&width=1140 "选择自定义构建过程")
![设置自定义构建过程的代码源：CODING](https://cdn.nlark.com/yuque/0/2021/png/126032/1640249387618-20001071-3a8a-411d-b2ca-d6f7334ffc81.png#clientId=ud4798a05-3a10-4&from=paste&height=844&id=u506f537a&originHeight=844&originWidth=1124&originalType=binary&ratio=1&rotation=0&showTitle=true&size=64469&status=done&style=none&taskId=uee968d21-a401-467d-9cf6-2ec29c890c7&title=%E8%AE%BE%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89%E6%9E%84%E5%BB%BA%E8%BF%87%E7%A8%8B%E7%9A%84%E4%BB%A3%E7%A0%81%E6%BA%90%EF%BC%9ACODING&width=1124 "设置自定义构建过程的代码源：CODING")

在自定义构建过程中点击确定后，进入创建构建计划设置——流程配置。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640249546389-474cb214-8af9-4789-97a0-888621ebff76.png#clientId=ud4798a05-3a10-4&from=paste&height=543&id=u0dbd8af8&originHeight=543&originWidth=1396&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65032&status=done&style=none&taskId=u29e775c2-668c-4f47-8c72-f11efc2b898&title=&width=1396)

## 代码检出

代码检出保持原有的就可以，不用去设置什么。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640249911868-7481b863-4a1d-48d4-8ba5-49dd06b658ff.png#clientId=ud4798a05-3a10-4&from=paste&height=221&id=u5d87087a&originHeight=221&originWidth=256&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6857&status=done&style=none&taskId=u997709c7-d4a2-4024-bd82-5da9c81c49b&title=&width=256)

## 执行 Hugo 构建

定义 2-1 步骤名称（我这里命名为：执行 Hugo 构建）。
![定义 2-1 步骤名称](https://cdn.nlark.com/yuque/0/2021/png/126032/1640249989600-bf26bad2-571a-41f5-bf43-7e5c5c2a24cc.png#clientId=ud4798a05-3a10-4&from=paste&height=498&id=u1efc81bf&originHeight=498&originWidth=765&originalType=binary&ratio=1&rotation=0&showTitle=true&size=48366&status=done&style=none&taskId=u62a9125d-4183-4ecf-85d7-94c9fc94386&title=%E5%AE%9A%E4%B9%89%202-1%20%E6%AD%A5%E9%AA%A4%E5%90%8D%E7%A7%B0&width=765 "定义 2-1 步骤名称")
![设置 2-1 步骤日志消息](https://cdn.nlark.com/yuque/0/2021/png/126032/1640250076832-71638fc3-f20a-480b-b308-46710d6d8943.png#clientId=ud4798a05-3a10-4&from=paste&height=247&id=ua3917fea&originHeight=247&originWidth=780&originalType=binary&ratio=1&rotation=0&showTitle=true&size=16105&status=done&style=none&taskId=ud8576cb5-d1c2-4234-a11f-17f73f1297c&title=%E8%AE%BE%E7%BD%AE%202-1%20%E6%AD%A5%E9%AA%A4%E6%97%A5%E5%BF%97%E6%B6%88%E6%81%AF&width=780 "设置 2-1 步骤日志消息")
![打印消息后，添加执行 Shell 脚本命令](https://cdn.nlark.com/yuque/0/2021/png/126032/1640250200440-86bb962b-aab9-405d-b1fd-7c7304c9b0e2.png#clientId=ud4798a05-3a10-4&from=paste&height=380&id=ubefe26fd&originHeight=380&originWidth=451&originalType=binary&ratio=1&rotation=0&showTitle=true&size=35811&status=done&style=none&taskId=u42717c8e-9814-4a47-a89e-8fea05995b3&title=%E6%89%93%E5%8D%B0%E6%B6%88%E6%81%AF%E5%90%8E%EF%BC%8C%E6%B7%BB%E5%8A%A0%E6%89%A7%E8%A1%8C%20Shell%20%E8%84%9A%E6%9C%AC%E5%91%BD%E4%BB%A4&width=451 "打印消息后，添加执行 Shell 脚本命令")
![配置具体的 Shell 脚本命令](https://cdn.nlark.com/yuque/0/2021/png/126032/1640250873501-38554855-78a8-4aff-8d8c-4a13b5650ffa.png#clientId=ud4798a05-3a10-4&from=paste&height=523&id=u65a2fb04&originHeight=523&originWidth=798&originalType=binary&ratio=1&rotation=0&showTitle=true&size=79970&status=done&style=none&taskId=u4f161515-81b1-48a4-abca-705ab1ff3ea&title=%E9%85%8D%E7%BD%AE%E5%85%B7%E4%BD%93%E7%9A%84%20Shell%20%E8%84%9A%E6%9C%AC%E5%91%BD%E4%BB%A4&width=798 "配置具体的 Shell 脚本命令")

```bash
# 这一步是下载 hugo 的执行程序，我是放在了 coding 的制品里面了；
# 外网下载好像速度有点慢，可以修改成其他版本或下载地址，下面需要对应修改文件名。
curl -fL "https://shenweiyan-generic.pkg.coding.net/btscl/tools/hugo_Linux-64bit.tar.gz?version=0.91.0" -o hugo_Linux-64bit-0.91.0.tar.gz

# 解压
tar -zvxf hugo_Linux-64bit-0.91.0.tar.gz
# 删除压缩包
rm -rf hugo_Linux-64bit-0.91.0.tar.gz

# 列出当前目录的文件
ls -hal

# 移动 hugo 到执行目录
mv hugo /usr/bin/hugo

# 由于前一步我们把代码检出了，所以我们目前是处于源代码目录下的；
# 执行 hugo 命令便可生成静态网站到 public 目录。
hugo -D
```

## 执行代码同步

点击"增加阶段"，增加一个"3-1 执行代码同步阶段"。

![执行代码同步，选择命令→执行 Shell 脚本](https://cdn.nlark.com/yuque/0/2021/png/126032/1640251260235-36417508-dd98-451a-abed-a341d4b6556f.png#clientId=ud4798a05-3a10-4&from=paste&height=380&id=u788cb95d&originHeight=380&originWidth=1013&originalType=binary&ratio=1&rotation=0&showTitle=true&size=52565&status=done&style=none&taskId=uf4f6be51-5c9b-4650-9b1e-3acabbcd16e&title=%E6%89%A7%E8%A1%8C%E4%BB%A3%E7%A0%81%E5%90%8C%E6%AD%A5%EF%BC%8C%E9%80%89%E6%8B%A9%E5%91%BD%E4%BB%A4%E2%86%92%E6%89%A7%E8%A1%8C%20Shell%20%E8%84%9A%E6%9C%AC&width=1013 "执行代码同步，选择命令→执行 Shell 脚本")

![执行代码同步，编辑执行的 Shell 脚本](https://cdn.nlark.com/yuque/0/2021/png/126032/1640307274107-d2b46cdd-3ff0-4ce3-96d2-f5dfb3a5bee9.png#clientId=u1fcd185c-2a9d-4&from=paste&height=362&id=u0edca748&originHeight=362&originWidth=772&originalType=binary&ratio=1&rotation=0&showTitle=true&size=47748&status=done&style=none&taskId=uaf129678-5468-4104-9f09-6ca6558a27b&title=%E6%89%A7%E8%A1%8C%E4%BB%A3%E7%A0%81%E5%90%8C%E6%AD%A5%EF%BC%8C%E7%BC%96%E8%BE%91%E6%89%A7%E8%A1%8C%E7%9A%84%20Shell%20%E8%84%9A%E6%9C%AC&width=772 "执行代码同步，编辑执行的 Shell 脚本")

```bash
# 进入到网站目录
cd ./public
# 初始化仓库
git init
# 绑定public仓库，这边是可以用默认变量token来代替认证
git remote add origin https://${CODING_USER}:${CODING_TOKEN}@e.coding.net/shenweiyan/webstack/nav.bioitee.pub.git
git add --all
git commit -m "执行自动更新"
# 强制推送内容
git push --force origin master
```

## 添加环境变量

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640252255354-f6c4c2e3-44ab-412e-bc7b-64fd58c55f9c.png#clientId=ud4798a05-3a10-4&from=paste&height=340&id=u24db8356&originHeight=340&originWidth=717&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33569&status=done&style=none&taskId=u39d21796-1f7e-486b-bb96-15f54e7412f&title=&width=717)
**CODING_USER** 和 **CODING_TOKEN** 的环境变量可以通过下面的方法获取（如下截图），也可以直接使用你个人的用户名和密码。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640251805673-30356b90-44ca-405f-9f87-6973e83355bb.png#clientId=ud4798a05-3a10-4&from=paste&height=611&id=uf99ed6a9&originHeight=611&originWidth=1062&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44011&status=done&style=none&taskId=ua9f2c12c-ad7c-48b7-88e7-cf71f4d7064&title=&width=1062)
勾选以下权限：

- **project:depot**，读/写，完整的仓库控制权限；
- **project:file**，读/写，授权读取与操作文件；
- **project:deployment**，读/写，授权用户持续部署；
- **project:ci**，读/写，持续集成；

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640251963428-92aad5e0-1dd9-43e2-91d8-00d3a1bb3255.png#clientId=ud4798a05-3a10-4&from=paste&height=376&id=uec15d18c&originHeight=376&originWidth=857&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28153&status=done&style=none&taskId=uf70c421e-7caa-43d8-911e-3620ee10293&title=&width=857)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640252064527-f4e8a993-5687-412c-9b87-166ba0440250.png#clientId=ud4798a05-3a10-4&from=paste&height=235&id=u0c030314&originHeight=235&originWidth=869&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21464&status=done&style=none&taskId=ua62bdf6b-beca-4a9f-b54a-c5a7610f2a1&title=&width=869)

## 配置完成

配置完成后，点击保存。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640252479825-f30d2d9f-33c1-4374-b708-47070f6d2069.png#clientId=ud4798a05-3a10-4&from=paste&height=360&id=u6c9c740f&originHeight=360&originWidth=1173&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32266&status=done&style=none&taskId=ufac0657c-0473-45b7-b211-47e40a68281&title=&width=1173)

# 立即构建

流程配置后立即构建，这个时候就会自动跑一遍这个流程了，基本上就完成了整个自动化构建部署。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640252430088-50053c3c-e681-4b1f-9a97-3d6a0bc1c36a.png#clientId=ud4798a05-3a10-4&from=paste&height=499&id=u3d8a1af4&originHeight=499&originWidth=785&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41549&status=done&style=none&taskId=u6b220fb2-172d-41d3-a1c0-81c468d6a04&title=&width=785)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1640313436798-33bf573d-2522-4b94-a229-dcd9e76a8083.png#clientId=u1fcd185c-2a9d-4&from=paste&height=399&id=uaed0853a&originHeight=399&originWidth=1445&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58481&status=done&style=none&taskId=u14530bf3-80c3-4c6b-9a93-c5edf61e5ed&title=&width=1445)

# 简单总结

最后说几句，这边要注意流程计划的触发规则为当代码源触发时自动执行，这样的话当你源代码仓库有更新时就会执行这个流程更新 public 仓库，而当 public 仓库发生改动时，你可以结合腾讯云的云开发 Webify 触发静态网站自动部署，具体的 Webify 操作可以参考《[腾讯云云开发 Webify 上手体验](https://www.yuque.com/shenweiyan/cookbook/webify-testing?view=doc_embed)》。
