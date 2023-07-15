折腾 docker，向 Docker Hub 提交镜像的时候发现原来自己在 2014 年就已经注册过 Docker Hub 的账号了，而且在 [https://hub.docker.com/u/shenweiyan](https://hub.docker.com/u/shenweiyan) 也看到了自己在 Docker Hub 的一些镜像信息。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1622681362070-529456e1-1100-42b7-9501-d38a0fd3280a.png#clientId=u03c03f40-6940-4&from=paste&height=1135&id=ue85e96c0&originHeight=1135&originWidth=1281&originalType=binary&size=99227&status=done&style=none&taskId=u161ef18a-16f7-4f2b-a86e-535d7873d1e&width=1281)
悲催的是，自己把密码给忘记了，找回密码甚至发现连注册邮箱都忘记了！！！

于是回去翻历史邮件（个人一直以来都喜欢把所有的邮箱 163、outlook、gmail ...... 都是设置自动转发到 QQ 邮箱，或者通过 QQ 邮箱代收代发！）感谢这个好习惯！！终于让我找到了线索！！！
![docker-passwd-reset.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1622682748331-9ba7db6f-caa0-4569-b532-ea2d53aa8185.png#clientId=u03c03f40-6940-4&from=ui&id=u69bbbfa6&originHeight=360&originWidth=773&originalType=binary&size=23612&status=done&style=none&taskId=ud5ed94b0-34a4-4250-98d8-59cded254dc)
由于注册的 gmail 账号很早前就已经被自己注销删除了，于是想着去恢复（或者重新注册）！
![gmail.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1622683444187-02f4b1bf-c129-4b99-801c-f6c99f6d45bb.png#clientId=u03c03f40-6940-4&from=ui&id=u52237613&originHeight=300&originWidth=740&originalType=binary&size=40927&status=done&style=none&taskId=ucc237fc8-9a02-430f-8e3d-630dec73e33)
自己操作恢复不了，于是参考了 YouTube 的 《[Couldn't find your google account But username is taken | How to resolve?](https://www.youtube.com/watch?v=QDy9voxTHW4)》的视频教程给 Google Team 求助。
![restore_gmail.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1622683978050-54c563d1-92ca-448e-8192-bf7dee705429.png#clientId=u03c03f40-6940-4&from=ui&id=ud22f596f&originHeight=368&originWidth=701&originalType=binary&size=23484&status=done&style=none&taskId=u2219e095-4f67-4439-815e-0cc6d7a009d)
Gmail 恢复无望！

接着，通过 Email 和 [Recover your Docker Hub account | Docker Documentation](https://docs.docker.com/docker-hub/2fa/recover-hub-account/) 中的 [Contact Support form](https://hub.docker.com/support/contact/?category=2fa-lockout) 给 Docker Hub 发送求助！
![help-dockerhub.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1622685074720-11a6a406-b2ed-42cb-bbc5-52e712cc8011.png#clientId=u03c03f40-6940-4&from=ui&id=u4ed6837f&originHeight=516&originWidth=753&originalType=binary&size=38441&status=done&style=none&taskId=u4ada3d8b-95d8-4bf9-8705-8fd11cd366b)

等待了 1 到 2 天，终于收到 Docker Hub 的答复，根据链接，重置密码成功！
![dockerhub-reset-passwd.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1622685325482-8a48742f-8255-4e24-a2f2-988dcf9c9277.png#clientId=u03c03f40-6940-4&from=ui&id=u947feb6e&originHeight=544&originWidth=739&originalType=binary&size=40020&status=done&style=none&taskId=u7507a146-e7cb-4303-add7-a821485e057)
最后，简单总结一下。

1. 找回来 Docker Hub 的账号，本来没抱多大希望，但试试也无妨，努力一下或许就会有好结果。

2. 有些人可能有多个邮箱账号在使用，如 QQ/163/gmail/outlook 等等，可以把这些邮箱的邮件通过转发（或设置代收/代发）的方式，在一个常用的主邮箱里面进行统一管理，一来省心省力，二来方便检索。

3. 个人比较喜欢 QQ 邮箱（可以设置成 foxmail 作为你的 QQ 主显邮箱），配合日历、记事本、在线文档使用，简直不要太爽！![qq-mail.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1622686021089-0e2be64a-7489-4ea3-8c3b-1fd6c2fbca36.png#clientId=u03c03f40-6940-4&from=ui&id=u432c6ab1&originHeight=736&originWidth=1201&originalType=binary&size=76508&status=done&style=none&taskId=ue97a1011-f1a1-4a6a-bb32-ed0ea270ea0)
