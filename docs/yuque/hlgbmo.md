普通用户无法查找和使用 useradd/groupad，你需要通过下面的命令切换成 root，才能查找和使用 useradd/groupadd。

```
sudo - root  # 不能直接 su root 切换
```

- **su**：只能切换到管理员用户权限，不使用管理员的登陆脚本和搜索路径。
- s**u -**：不但能切换到管理员权限，而且使用管理员登陆脚本和搜索路径。

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1632881373882-30808f40-d070-4bf4-a7e2-bbd1618adf7b.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_21%2Ctext_5rKI57u054eVIHwg5YWs5LyX5Y-377yaQmlvSVTniLHlpb3ogIU%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10#clientId=u0b5be658-8fb8-4&from=paste&height=489&id=uf11a78b3&originHeight=489&originWidth=721&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48022&status=done&style=none&taskId=u8ff0eca1-41e2-48a9-9018-837f0c84e55&title=&width=721)

## 没有 home 目录

如果我们安装最常规的方法，直接使用 useradd username -g groupname 创建用户，Debian 中默认是没有 /home/username 目录的，以至于在登录时出现报错。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1632881926504-f446a7e0-3eef-4cfa-b44a-7840b82b532f.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_22%2Ctext_5rKI57u054eVIHwg5YWs5LyX5Y-377yaQmlvSVTniLHlpb3ogIU%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10#clientId=u0b5be658-8fb8-4&from=paste&height=487&id=ufde3b531&originHeight=487&originWidth=780&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48536&status=done&style=none&taskId=u685fa2d1-3a98-4d6f-a382-f59ed8bf4a7&title=&width=780)
因此，需要通过下面的方式创建用户。

```
# 添加用户，参数-m，自动创建用户的家目录，注意一定要添加 -s /bin/bash，原因下面细说
useradd -m -s /bin/bash your_username -g your_groupname
# 或者
useradd -d /home/your_username -m -s /bin/bash your_username -g your_groupname

# 添加密码
passwd your_username

# 删除用户
userdel your_username
```

## 无法使用 tab 补全

这个问题，与其说有点坑爹。其实是不了解 Ubuntu 和 debian 的 shell 默认安装的是 dash，而不是 bash。参考网络《[解决 debian 终端命令行无法自动补全 \_杨圣亮的技术博客](https://www.yangshengliang.com/kaiyuan-shijie/linux-shijie/452.html)》的做法：

1. 安装命令补全：

```
apt-get install bash-completion
```

2. 在 /etc/profile 里追加：

```
if [ -f /etc/bash_completion ]; then
. /etc/bash_completion
fi
```

3. 刷新 /etc/profile 配置文件，使其生效。

```
source /etc/profile
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1632885569599-62110413-0a50-40cf-bc67-84b6f48c2389.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_25%2Ctext_5rKI57u054eVIHwg5YWs5LyX5Y-377yaQmlvSVTniLHlpb3ogIU%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10#clientId=u393f6950-1ef2-4&from=paste&height=545&id=udfb0777e&originHeight=545&originWidth=863&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53641&status=done&style=none&taskId=u16bdf8e0-bc29-4925-862e-8a51e8c0461&title=&width=863)
这是因为 Ubuntu 和 debian 的 shell 默认安装的是 dash，而不是 bash！解决方法如下：

```bash
root@VM-8-4-debian:~# usermod --shell /bin/bash shenweiyan
root@VM-8-4-debian:~# grep shenweiyan /etc/passwd
shenweiyan:x:1001:1001::/home/shenweiyan:/bin/bash
```

## 正确创建用户

CentOS 和 Debian 使用 useradd 命令的区别，以下面的命令举例：

```bash
useradd shenweiyan
```

这条命令在 Debian 下不会做如下几件事：

1. **不会创建家目录；**
2. **默认 shell 是 /bin/sh；**
3. **而 /bin/sh 默认是软连接到 /bin/dash 解释器 /bin/sh -> dash。**

如果需要创建这些内容则必须指定参数：

```bash
useradd -m -s /bin/bash shenweiyan
```

## 用 bash 作为默认 shell

Debian 中/bin/sh 是 dash 的链接，如果大家想用 bash 作为默认 shell 的话，可以用以下的办法修改：

```bash
# https://unix.stackexchange.com/questions/442510/how-to-use-bash-for-sh-in-ubuntu
sudo dpkg-reconfigure dash
```

然后在弹出来的界面中选择“yes” 来保持使用 dash 作为 /bin/sh 首选；或者选择“no” 把 /bin/sh 切换为 bash！
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1632901851803-38096f18-823f-4fc6-9102-03ced6bfc640.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_37%2Ctext_5rKI57u054eVIHwg5YWs5LyX5Y-377yaQmlvSVTniLHlpb3ogIU%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10#clientId=u1cf2fc99-7e56-4&from=paste&height=235&id=uf8ed7a40&originHeight=235&originWidth=1296&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18449&status=done&style=none&taskId=uee11f305-227d-47ae-b48a-22cd2af61c3&title=&width=1296)

## sudo 出现 unable to resolve host

![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1637828351205-0036c016-8267-4273-b99b-d145dda787c4.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_19%2Ctext_5rKI57u054eVIHwg5YWs5LyX5Y-377yaQmlvSVTniLHlpb3ogIU%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10#clientId=uf4783c0d-8f61-4&from=paste&height=67&id=u829f7063&originHeight=67&originWidth=668&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6191&status=done&style=none&taskId=u106a9dcd-dad8-4ec6-a200-2f40cadf388&title=&width=668)
/etc/hosts 内容修改成如下：

```shell
121.23.19.88(或者 127.0.0.1)    kvm-bioitee
```
