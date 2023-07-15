[一个小清新的知识管理和问题讨论平台](https://mp.weixin.qq.com/s/fL7CaYo2xvuleihlY8XDIA)

在《[一个小团队使用的知识管理方案与工具](https://www.yuque.com/shenweiyan/cookbook/zwtn5w?view=doc_embed)》中我们介绍了一些 Confluence 的基本特性，今天我们来看看这个工具的一些安装部署问题。

## 1. Java 与 PostgreSQL 安装配置

### 1.1 Oracle Java JDK 8

[https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html](https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html)
注册一个 oracle 账号就行，需要的文件：jdk-8u251-linux-x64.tar.gz

1. 创建目录，解压。

```bash
mkdir /usr/java
tar zvxf jdk-8u251-linux-x64.tar.gz -C /usr/java
```

2. 环境配置，修改 ~/.bashrc（或者 /etc/profile）文件。

```bash
vim ~/.bashrc
```

3. 添加以下内容。

```bash
export JAVA_HOME=/usr/java/jdk1.8.0_251
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

4. 使环境变量生效。

```bash
source ~/.bashrc
```

5. 检查配置是否成功。

```bash
$ java -version
java version "1.8.0_251"
Java(TM) SE Runtime Environment (build 1.8.0_251-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.251-b08, mixed mode)
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597988512688-0f6dd3be-695a-4b87-808a-7b3814bf06d0.png#align=left&display=inline&height=124&originHeight=124&originWidth=670&size=13827&status=done&style=none&width=670)

### 1.2 PostgreSQL

PostgreSQL 是 Atlassian 官方推荐用于 Confluence 的数据库，接下来我们需要安装 PostgreSQL，并执行一些必要的设置。

#### 1. 安装

Linux 下 PostgreSQL 数据库的安装请参考《[Linux 下 PostgreSQL 源码编译安装](https://www.yuque.com/bioitee/mp/linux-postgresql-install?view=doc_embed)》一文，这里不赘述。

#### 2. 配置

创建用于 Confluence 的数据库。详细可以参考《[How to install Confluence on Centos7 with PostgreSQ](https://comtronic.com.au/how-to-install-confluence-on-centos7-with-postgresql/)》。

```bash
$ sudo -u postgres -i
$ psql
psql (10.13)
Type "help" for help.

postgres=# CREATE USER confluencedbuser PASSWORD 'confluencedbpassword';
postgres=# CREATE DATABASE confluencedb WITH ENCODING 'UNICODE' LC_COLLATE 'C' LC_CTYPE 'C' TEMPLATE template0;
postgres=# GRANT ALL PRIVILEGES ON DATABASE confluencedb to confluencedbuser;
\q
-bash-4.2$ exit
```

## 2. 下载安装 Confluence

### 2.1 下载

```bash
$ mkdir /data/apps/atlassian
$ cd /data/apps/atlassian
$ wget https://product-downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-7.2.1-x64.bin
```

### 2.2 安装

给 atlassian-confluence-7.2.1-x64.bin 添加可执行权限后进行安装。
![mkdir-atlassian-dir.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597994540592-096bdaf6-5061-4ea6-b63b-54e513ed0b91.png#align=left&display=inline&height=360&originHeight=360&originWidth=794&size=32779&status=done&style=none&width=794)

然后，执行 ./atlassian-confluence-7.2.1-x64.bin 安装，注意安装完成后先不要急着启动！！！
![atlassian-confluence-install.jpg](https://cdn.nlark.com/yuque/0/2020/jpeg/126032/1597994581226-3d6b7d38-5366-4983-85f4-0ac374651ca7.jpeg#align=left&display=inline&height=973&originHeight=973&originWidth=1106&size=671615&status=done&style=none&width=1106)

### 2.3 破解

由于 Confluence 本身是一个收费的软件，想要免费安装，可以使用 pengzhile 提供的在 GitHub 上开源了对 Atlassian 家所有产品（包括插件市场所有收费插件）的破解方法。

- [Gitee: pengzhile/atlassian-agent](https://gitee.com/pengzhile/atlassian-agent)
- [GitHub: pengzhile/atlassian-agent](https://github.com/pengzhile) (DMCA takedown)
- [**编译好的包**](https://pan.baidu.com/s/1AucTmTNPSG85hhWF7mkIcQ)，提取码：`n4ug`

具体做法如下。

1. 将 atlassian-agent.jar 放在一个你不会随便删除的位置（你服务器上的所有 Atlassian 服务可共享同一个 atlassian-agent.jar）

```bash
tar zvxf atlassian-agent-v1.2.3.tar.gz -C /data/apps/atlassian
```

2. 非常重要！！！需要设置环境变量 JAVA_OPTS，避免开机失效。在 [atlassian-agent](https://gitee.com/pengzhile/atlassian-agent) 的说明中提供了 3 个建议，大家可以根据自己需要去设置，这里我们选择第二种方法：把 JAVA_OPTS 环境放到服务安装所在 bin 目录下的 setenv.sh 或 setenv.bat 中。

```bash
# vim /data/apps/atlassian/confluence/bin/setenv.sh

CATALINA_OPTS="-javaagent:/data/apps/atlassian/atlassian-agent-v1.2.3/atlassian-agent.jar ${JAVA_OPTS}"
```

![cinfluence-setenv.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597995687911-fbda3b21-b0b7-46be-90e0-2c47bc8d5ccc.png#align=left&display=inline&height=458&originHeight=458&originWidth=1096&size=68445&status=done&style=none&width=1096)

3. 准备就绪后，我们就要开启 Configuration 了，找到启动脚本并启动 start-confluence.sh，启动完成后我们可以通过浏览器进行访问。

![start_confluence.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597996164699-0354cc87-643e-4629-bb60-89cb03f5bf28.png#align=left&display=inline&height=488&originHeight=488&originWidth=1097&size=40045&status=done&style=none&width=1097)

### 2.4 配置

在浏览器中打开 **http://localhost:8090** 或者 **http://<你服务器的公网 IP>:8090 **，打开 Confluence 的配置页面。

选择中文，产品安装。
![01-产品安装.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597999463517-e03b1fa7-f268-46a8-ba65-4d39b7e46624.png#align=left&display=inline&height=656&originHeight=656&originWidth=1016&size=65578&status=done&style=none&width=1016)

获取应用。
![02-获取应用.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597999481268-bdc835dd-c251-472d-8538-4948d9e95876.png#align=left&display=inline&height=656&originHeight=656&originWidth=1016&size=87512&status=done&style=none&width=1016)

获得服务器 ID 及授权码提示。
![03-授权码.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1597999558030-923f3ab3-2c1b-48c3-9b62-f723850247e8.png#align=left&display=inline&height=656&originHeight=656&originWidth=1016&size=65756&status=done&style=none&width=1016)

### **2.5 授权码**

当我们在后台服务器通过命令行执行 java -jar /atlassian-agent.jar 时应该可以看到输出的 KeyGen 参数帮助。

- 破解 Confluence 时，选择 conf 即可，具体命令如下：

```bash
# 将server ID复制（-m 邮箱 -n 用户名 -o 公司名 -s SERVER ID）
java -jar atlassian-agent.jar -p conf -m ishenweiyan@foxmail.com -n shenweiyan -o wiki-test -s B7DP-BX09-325B-DJLP
```

- 破解团队日程表时，选择 tc 即可，具体命令如下：

```bash
# 将server ID复制（-m 邮箱 -n 用户名 -o 公司名 -s SERVER ID）
java -jar atlassian-agent.jar -p tc -m ishenweiyan@foxmail.com -n shenweiyan -o wiki-test -s B7DP-BX09-325B-DJLP
```

- 破解 Confluence Questions 时，选择 questions 即可，具体命令如下：

```bash
# 将server ID复制（-m 邮箱 -n 用户名 -o 公司名 -s SERVER ID）
java -jar atlassian-agent.jar -p question -m ishenweiyan@foxmail.com -n shenweiyan -o wiki-test -s B7DP-BX09-325B-DJLP
```

将生成的授权码粘贴，下一步。
![04-授权码.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598001355751-af8acfeb-05f8-4569-b7d4-8ff660a022cd.png#align=left&display=inline&height=656&originHeight=656&originWidth=1016&size=89504&status=done&style=none&width=1016)

设置数据库，生产环境建议独立的数据。
![05-设置数据库.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598001374721-33ae973e-0067-449d-9862-e10d222d2a96.png#align=left&display=inline&height=656&originHeight=656&originWidth=1016&size=66087&status=done&style=none&width=1016)

选择 PostgreSQL 数据库，填写数据库信息，测试数据库连通性。
![06-设置-测试数据库.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598001862995-16c9855e-93e4-4161-97c6-e6d1fde294f3.png#align=left&display=inline&height=659&originHeight=659&originWidth=1018&size=87269&status=done&style=none&width=1018)

过一会你就可以看到这个页面了，安装成功了。

![](https://cdn.nlark.com/yuque/0/2020/png/126032/1598001771847-aa01a787-70ec-4964-ad18-cdc06afab608.png#align=left&display=inline&height=810&originHeight=810&originWidth=1189&size=0&status=done&style=none&width=1189)

## 3. 使用与管理

### 3.1 使用

#### 1. 选择站点

如果是全新的站点，那就选择空白站点，如果只是升级或者备份恢复，那就选择从备份还原站点。这里我们选择"示范站点"。
![07-示范站点.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598001920237-cd80c940-12d9-4a8d-ab6c-e5051c054e25.png#align=left&display=inline&height=654&originHeight=654&originWidth=1015&size=82985&status=done&style=none&width=1015)

#### 2. 配置用户管理

配置用户，如果有 Jira 环境的话 ，可以集成 Jira 的用户管理，也可以使用 confluence 自己的用户，那就选择“在 confluence 中管理用户和组”。
![08-配置用户管理.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598001951863-055ffae5-0afb-4081-8bbf-24222861041b.png#align=left&display=inline&height=656&originHeight=656&originWidth=1016&size=82194&status=done&style=none&width=1016)
![09-设置系统管理员.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598002011191-65473231-9088-4783-b42e-12889e52efe4.png#align=left&display=inline&height=656&originHeight=656&originWidth=1016&size=64885&status=done&style=none&width=1016)
![10-设置成功.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598002021046-ad683b4a-8eab-44cd-85b4-b8d3c0b0552e.png#align=left&display=inline&height=656&originHeight=656&originWidth=1016&size=51738&status=done&style=none&width=1016)

#### 3. 创建空间

![创建空间.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598582166091-c2808867-976c-4acb-88f0-9621faa9a205.png#align=left&display=inline&height=659&originHeight=659&originWidth=1018&size=58642&status=done&style=none&width=1018)
空间创建完成后，就会自动进入已经创建好的空间，接下来，enjoy！
![进入空间.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598582293050-886a8944-603a-46fd-8623-70bd6059c0c5.png#align=left&display=inline&height=630&originHeight=630&originWidth=1018&size=117719&status=done&style=none&width=1018)

### 3.2 站点管理

点击右上角，可以进行站点管理，常用的如 **"一般配置"**：
![一般配置.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598582463306-e8b5c8f5-104e-46b7-95a0-266dbe855bbd.png#align=left&display=inline&height=628&originHeight=628&originWidth=1016&size=122139&status=done&style=none&width=1016)
点击一般配置，授权管理，可以查看到授权情况：
![授权信息.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598582522058-f3a6ac78-3c54-41bc-8620-6a99079479fe.png#align=left&display=inline&height=668&originHeight=668&originWidth=1028&size=132983&status=done&style=none&width=1028)
当然本文介绍的方式仅用于学习体验，有能力的还是建议支持正版！！！

## 4. 备份恢复

Confluence 本身会有每日的备份管理，就是定时备份任务（“一般配置”→“每日备份管理”）。
![每日备份管理.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598583040635-886e95c5-1b79-43c2-8c84-22e88dee40e1.png#align=left&display=inline&height=673&originHeight=673&originWidth=975&size=106854&status=done&style=none&width=975)
由于一些其他原因需要新建一个 Confluence 环境，然后还原的，可以点下面的**“备份与还原”**。

- 如果您的导出文件很小(小于 25 MB)，请直接在浏览器“备份与还原”页面直接进行上传。
- 而较大的文件需要从主目录导入。需要提前将备份的文件放到 **/path/to/application-data/confluence/restore** 下。

具体备份与还原，可以参考 **“一般配置”** → **“备份与还原” **页面的详细介绍。
![备份与还原-Confluence.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1598584281685-4a26b0d2-3c74-400e-bf33-8ba06dfa457e.png#align=left&display=inline&height=1176&originHeight=1176&originWidth=947&size=156510&status=done&style=none&width=947)

## 5. 参考资料

1. 武培轩，《[CentOS 7 安装 JAVA 环境（JDK 1.8）](https://www.cnblogs.com/wupeixuan/p/11433922.html)》，博客园
2. 王良付，《[CentOS 7.6 安装 Confluence 7.2](https://liangfu.wang/2020/01/22/CentOS-7-6-%E5%AE%89%E8%A3%85-Confluence-7-2/)》，良付の博客
3. 胖哥叨逼叨，《[Atlassian Confluence 部署-confluence 安装](https://www.pangshare.com/1919.htm)》，WordPress
4. Agile Project Management \* DIY Electronics，《[How to install Confluence on Centos7 with PostgreSQL](https://comtronic.com.au/how-to-install-confluence-on-centos7-with-postgresql/)》，Comtronic Blog
