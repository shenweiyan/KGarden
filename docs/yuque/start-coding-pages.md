很多人喜欢在 github pages / gitee pages 发布自己的个人博客，前者由于服务器位于国外可能会导致国内的访问有时候很慢（你也可以使用 CDN 进行加速），后者如果想要配置自定义域名需要开通 Gitee Pages Pro 付费服务。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608969577270-df468d10-4454-4dfd-9dc3-46bac65deb06.png#align=left&display=inline&height=196&originHeight=391&originWidth=763&size=37705&status=done&style=none&width=381.5)
[Gitee Pages Pro 暂时关闭个人用户购买入口](https://gitee.com/help/articles/4228)

这里介绍一下，由腾讯云提供支持的 coding.net 代码托管平台提供的静态网站功能，为免费博客、静态站点提供一个解决方法，以供参考。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608704558117-1ef514aa-882b-475e-b293-f497785b8e2c.png#align=left&display=inline&height=495&originHeight=495&originWidth=988&size=119625&status=done&style=none&width=988)

本文章最后搭建完成的示例静态站点，可以点击这里进行预览：
[Creative - Start Bootstrap Theme](https://coding-pages-bucket-396338-8151423-8649-429346-1251708715.cos-website.ap-guangzhou.myqcloud.com/)

首先，进入你的 coding.net 主页，选择组边导航栏的"项目"，然后"创建项目"。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608692834786-b4df7728-9780-439e-a2d0-74e13a684b20.png#align=left&display=inline&height=502&originHeight=502&originWidth=1040&size=90444&status=done&style=none&width=1040)
选择“代码托管项目”。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608692896710-10da345e-c77e-4be9-bbd0-a75d8e938976.png#align=left&display=inline&height=502&originHeight=502&originWidth=1040&size=101471&status=done&style=none&width=1040)
填写项目基本信息，点击"完成创建"。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608693374268-cf42103f-7597-47a1-bdb4-0fb469734f4c.png#align=left&display=inline&height=571&originHeight=571&originWidth=1022&size=57442&status=done&style=none&width=1022)
第二步，创建完成项目后，进入创建好的项目，在"代码仓库"中"新建代码仓库"。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608693516882-4c81200f-6dea-4d53-aa5f-73663bf4258a.png#align=left&display=inline&height=657&originHeight=657&originWidth=1040&size=99142&status=done&style=none&width=1040)
填写新建代码仓库信息。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608694316375-c0e97c06-d13c-4f5a-8f36-c56beaeaac2e.png#align=left&display=inline&height=673&originHeight=673&originWidth=1033&size=98710&status=done&style=none&width=1033)
第三步，在创建好的代码仓库中，选择"新建/上传"，或者通过 git 的方式，上传文件或文件夹。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608695441117-e7dbcc8b-6825-4fa6-9b5e-9290d4051194.png#align=left&display=inline&height=621&originHeight=621&originWidth=1040&size=85758&status=done&style=none&width=1040)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608702718001-30050553-b3db-42b3-b61d-78f6b6ad289d.png#align=left&display=inline&height=542&originHeight=542&originWidth=1003&size=71925&status=done&style=none&width=1003)
第四步，开启项目设置中的“持续部署”功能。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608702645245-5b6948bc-543f-42c2-b247-86cb1a1e2caa.png#align=left&display=inline&height=524&originHeight=524&originWidth=968&size=85612&status=done&style=none&width=968)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608701954990-fc757ee6-0f4b-4ef3-8180-11e2a203d07c.png#align=left&display=inline&height=579&originHeight=579&originWidth=1032&size=74091&status=done&style=none&width=1032)
第五步，构建"静态网站"。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608702546076-b27fb90c-8b59-490c-b236-40dfd6c1af6b.png#align=left&display=inline&height=524&originHeight=524&originWidth=968&size=87101&status=done&style=none&width=968)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608702188538-d9fff896-9d25-4d1e-bafc-1cd269879dc1.png#align=left&display=inline&height=485&originHeight=485&originWidth=1040&size=74517&status=done&style=none&width=1040)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608702340714-ee0610f1-71cc-42d5-8d73-9d68f9302b3f.png#align=left&display=inline&height=716&originHeight=716&originWidth=930&size=73900&status=done&style=none&width=930)
第六步，部署成功，访问站点。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608702482839-9f41a7b8-3116-4f54-b793-e0fe8ad60338.png#align=left&display=inline&height=454&originHeight=454&originWidth=968&size=69135&status=done&style=none&width=968)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608702798138-317dfd08-5496-49d1-95ee-c4050d807a3b.png#align=left&display=inline&height=559&originHeight=559&originWidth=899&size=388181&status=done&style=none&width=899)
到这里，你在 coding.net 上的静态网站（博客）就已经部署完成，部署完成后 coding.net 会自动生成一个很长的 url，你可以通过这个 URL 访问你的站点。

附上本文章最后搭建完成的示例静态站点：
[Creative - Start Bootstrap Theme](https://coding-pages-bucket-396338-8151423-8649-429346-1251708715.cos-website.ap-guangzhou.myqcloud.com/)

当然，你也可以配置一个更加容易访问的自定义域名，我们在下一篇推文中再详细如何配置，敬请期待。
