我们在 Linux 服务器上安装软件，有时候需要在服务器下载 GitHub 上 Release 的一些资源(软件包)，这时候我们可以使用 wget 或者 curl 进行处理，这里拿 htslib 开源的配置中心 [HTSlib](https://github.com/samtools/htslib) 为例，下载他的 Release 版本。
![image.png](https://shub-1251708715.cos.ap-guangzhou.myqcloud.com/elog-docs-images/FrJ3Wgrjg32ZA0B7aW_OxPSi6Mdt.png)

## wget

```bash
wget --no-check-certificate --content-disposition https://github.com/samtools/htslib/releases/download/1.15.1/htslib-1.15.1.tar.bz2
```

wget 使用注意点，详细说说每个参数的含义，用法，并举例。

```bash
#https://blog.csdn.net/xiliunian/article/details/104313511
nohup wget -c --no-check-certificate --no-proxy https://figshare.com/ndownloader/files/30835246 &
nohup wget -c https://s3-eu-west-1.amazonaws.com/pfigshare-u-files/30835246/ILD_alldataset_population_noSCT.rds?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIYCQYOYV5JSSROOA/20230224/eu-west-1/s3/aws4_request&X-Amz-Date=20230224T170124Z&X-Amz-Expires=10&X-Amz-SignedHeaders=host&X-Amz-Signature=102fc7fe3c73fd86b3b7458a0585acd79611095e5cba52b2903044c2ba341c26 &

nohup wget -c --no-check-certificate --user-agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3" https://figshare.com/ndownloader/files/30835246  &

nohup wget -c --no-check-certificate --user-agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3" https://figshare.com/ndownloader/files/30836776  &

nohup curl https://s3-eu-west-1.amazonaws.com/pfigshare-u-files/30835246/ILD_alldataset_population_noSCT.rds?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIYCQYOYV5JSSROOA/20230224/eu-west-1/s3/aws4_request&X-Amz-Date=20230224T170124Z&X-Amz-Expires=10&X-Amz-SignedHeaders=host&X-Amz-Signature=102fc7fe3c73fd86b3b7458a0585acd79611095e5cba52b2903044c2ba341c26 &
nohup curl -C https://s3-eu-west-1.amazonaws.com/pfigshare-u-files/30835246/ILD_alldataset_population_noSCT.rds?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIYCQYOYV5JSSROOA/20230224/eu-west-1/s3/aws4_request&X-Amz-Date=20230224T170820Z&X-Amz-Expires=10&X-Amz-SignedHeaders=host&X-Amz-Signature=f786583613c9276e963a35e3bdc59c013c8a25b62755a122079467dac31498c8 &

nohup curl -C  https://s3-eu-west-1.amazonaws.com/pfigshare-u-files/30835246/ILD_alldataset_population_noSCT.rds?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIYCQYOYV5JSSROOA/20230224/eu-west-1/s3/aws4_request&X-Amz-Date=20230224T184948Z&X-Amz-Expires=10&X-Amz-SignedHeaders=host&X-Amz-Signature=642c27ba7ad0b2c062746bf4179deb8799236d3e694b184195120c937d4f7def &
```

> 参考：[wget 小细节（geo 数据 ，figshare 数据）](https://mp.weixin.qq.com/s/xehzFpcqvG860RZv-2Hndw)

- **-r, --recursive**
  这个选项用于递归地下载整个网站或目录。例如：','wget -r http://www.example.com/
  这个命令将下载 www.example.com 网站的所有内容，包括子目录和链接。
- **-nH, --no-host-directories**
  这个选项用于在下载时不创建目标文件夹的主机名目录。例如：
  wget -nH http://www.example.com/files/file.txt
  这个命令将在当前目录下创建一个名为 file.txt 的文件，而不是在 www.example.com/files/ 目录下创建。
- **-N, --timestamping**
  这个选项用于只下载更新过的文件。例如：
  wget -N http://www.example.com/files/file.txt
  如果 file.txt 本地已经存在并且与远程文件的时间戳相同，那么 wget 将不会下载文件。如果本地文件的时间戳比远程文件的时间戳早，那么 wget 将下载文件。
- **-nd, --no-directories**
  这个选项用于在下载时不创建目标文件夹。例如：
  wget -nd http://www.example.com/files/file.txt
  这个命令将在当前目录下创建一个名为 file.txt 的文件，而不是在 www.example.com/files/ 目录下创建。
- **-P, --directory-prefix**
  这个选项用于指定要将文件下载到的目录。例如：
  wget -P /home/user/downloads/ http://www.example.com/files/file.txt
  这个命令将在 /home/user/downloads/ 目录下创建一个名为 file.txt 的文件。
- **-c, --continue**
  这个选项用于在中断的地方继续下载文件。例如：
  wget -c http://www.example.com/files/file.txt
  如果文件下载已经开始，但由于某种原因中断了，那么 wget 将在中断的地方继续下载文件。
- **-O, --output-document**
  这个选项用于将下载的文件保存为指定的文件名。例如：
  wget -O newfile.txt http://www.example.com/files/file.txt
  这个命令将下载文件 file.txt 并将其保存为名为 newfile.txt 的文件。
- **-q, --quiet**
  这个选项用于静默下载，不输出下载进度信息。例如：
  wget -q http://www.example.com/files/file.txt
  这个命令将在后台下载文件 file.txt。
- **-t, --tries**
  这个选项用于指定在下载过程中尝试重新连接的次数。例如：
  wget -t 5 http://www.example.com/files/file.txt
  这个命令将在下载过程中尝试重新连接 5 次。
- **-b, --background**
  这个选项用于在后台下载文件。

## curl

```bash
curl -LjO https://github.com/samtools/htslib/releases/download/1.15.1/htslib-1.15.1.tar.bz2
```

- **-L, --location**

Follow redirects，如果服务器报告请求的页面已移动到不同的位置（用 Location: 标头和 3XX 响应代码指示），此选项将使 curl 在新位置重做请求，即重定向。)

- **-O, --remote-name**

Write output to a file named as the remote file，把输出写到该文件中，保留远程文件的文件名。

- **-j, --junk-session-cookies**

Ignore session cookies read from file
