- [Linux 系统如何使用 Fuser 命令](https://www.4spaces.org/how-to-use-the-linux-fuser-command/)
- [fuse 命令](https://man.linuxde.net/fuser)

```bash
# 杀掉占用此设备的进程
$ sudo fuser -m -v -k /mnt/textlive/
```
