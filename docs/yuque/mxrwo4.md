尝试了一下 Tabby 终端（官网：[https://github.com/Eugeny/tabby](https://github.com/Eugeny/tabby)），虽然是开源的，速度和颜值都不错。Windows 7 下的体验目前感觉还不太好。
![](https://github.com/Eugeny/tabby/blob/master/docs/readme-terminal.png?raw=true#from=url&id=CWNtq&originHeight=1810&originWidth=2724&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

记录一下遇到的问题。

- Win 7 下安装 [tabby-1.0.169-setup.exe](https://github.com/Eugeny/tabby/releases/download/v1.0.169/tabby-1.0.169-setup.exe) 和 [tabby-1.0.169-portable.zip](https://github.com/Eugeny/tabby/releases/download/v1.0.169/tabby-1.0.169-portable.zip) 会闪退；1.0.168 版本正常（[issues/5465](https://github.com/Eugeny/tabby/issues/5465)）。值得一提的是这个问题终于在 [v1.0.175](https://github.com/Eugeny/tabby/tree/v1.0.175) 中修复了！

![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1650593747976-4bdcf98a-889a-49ce-b427-11fd31efa067.png#clientId=u1e00f8be-a0e1-4&from=paste&height=628&id=u2838029f&originHeight=628&originWidth=1021&originalType=binary&ratio=1&rotation=0&showTitle=false&size=66252&status=done&style=none&taskId=u9ac4e795-f902-4a0f-b01d-45ab259f898&title=&width=1021)

- SSH 设置的站点，再次连接经常会出现各种错误，连接不上。

后来个人更换 Win10 系统安装了最新的 [v1.0.176](https://github.com/Eugeny/tabby/tree/v1.0.176) 再也没有出现过这个问题。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1642061349705-72f3e804-6f56-4071-8441-51132e301fca.png#clientId=u095e274e-beb8-4&from=paste&height=302&id=uc3e106b3&originHeight=302&originWidth=804&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20526&status=done&style=none&taskId=u6115f1ac-ced2-4401-bf94-edc919e0f6d&title=&width=804)

- 配置了 SOCKET 的隧道连接，如果 SSH 终端断开，SOCKET 也会断掉（这一点 MobaXterm/Xshell 体验很好）。

- SFTP 上传下载文件，只能一层一层去点开目录，不能直接粘贴路径，很不方便。

今天发现可以参考 [Shell working directory reporting · Eugeny/tabby Wiki](https://github.com/Eugeny/tabby/wiki/Shell-working-directory-reporting) 实现 SFTP 打开当前目录：

```bash
# Bash
# ~/.bash_profile
export PS1="$PS1\[\e]1337;CurrentDir="'$(pwd)\a\]'

# ZSH
# ~/.zshrc
precmd () { echo -n "\x1b]1337;CurrentDir=$(pwd)\x07" }
```

- 终端记录太多，top 命令会导致没法上下滚动查看之前的终端记录。
