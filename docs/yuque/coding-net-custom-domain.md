![biying-1.jpg](https://cdn.nlark.com/yuque/0/2021/jpeg/126032/1610009468142-06b0c85b-6c52-4d07-8295-fb7231b051f7.jpeg#align=left&display=inline&height=1080&originHeight=1080&originWidth=1920&size=466934&status=done&style=none&width=1920)

上一篇文章，我们介绍了怎么在 coding.net 部署个人的静态网站/博客站点，今天我们聊一下怎么来自定义已经部署好站点的域名地址。

基于腾讯云的自定义域名示例站点预览：
[Creative - Start Bootstrap Theme](https://startbootstrap-creative.bioitee.com/)

基于非腾讯云的自定义域名示例站点预览：
[Creative - Start Bootstrap Theme](https://startbootstrap-creative.ncbix.com/)

第一步，进入部署好站点的"静态网站"基本信息页面。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608705673067-2f13c975-7810-48b0-805b-9b7eeb636fe0.png#align=left&display=inline&height=473&originHeight=473&originWidth=1006&size=69856&status=done&style=none&width=1006)
第二步，从"静态网站"基本信息页面进入"自定义域名"页面。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608705812481-0e203e17-4579-43df-9ab9-6dcc9522bb93.png#align=left&display=inline&height=631&originHeight=631&originWidth=1006&size=100399&status=done&style=none&width=1006)
第三步，选择"新建域名"。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608705894268-b20f77ef-d4f6-420d-b501-1dd9a8ec2e08.png#align=left&display=inline&height=631&originHeight=631&originWidth=1006&size=98699&status=done&style=none&width=1006)

新建域名，有两种情况，我们先介绍第一种情况：你的域名是在腾讯云注册的。

1. 新建自定义域名，点击“确定”后，会自动生成一个 CNAME 记录。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608706184862-2a807c8f-92fe-4bdf-be9d-3c2f8b601270.png#align=left&display=inline&height=285&originHeight=285&originWidth=472&size=18849&status=done&style=none&width=472)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608706545538-829ffbe4-ccc0-4f7a-988b-3ad95de9f43c.png#align=left&display=inline&height=572&originHeight=572&originWidth=968&size=89223&status=done&style=none&width=968)

点击"审核中"，可以看到对应证书在腾讯云中的详细信息。等待约 10 分钟，"审核中"的状态会自动变更为"待验证"。再过约 10 分钟，对应证书状态将会由"待验证" 变更为"已签发"，即表示证书已经申请成功。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608707768989-ca5def8c-9207-4618-9946-4a6b53e63caa.png#align=left&display=inline&height=657&originHeight=657&originWidth=1191&size=123848&status=done&style=none&width=1191)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608708052079-ca9ec609-5f60-4bb7-af28-87f44501eae5.png#align=left&display=inline&height=521&originHeight=521&originWidth=1279&size=97632&status=done&style=none&width=1279)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608708354869-16ddb208-59fe-413d-9fe8-7ddb6f090b63.png#align=left&display=inline&height=529&originHeight=529&originWidth=1295&size=105008&status=done&style=none&width=1295)

2. 添加 CNAME 记录。登陆腾讯云域名解析中心，添加一个 CNAME 记录。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608709431389-820870fd-1a0e-4df1-a48d-a904e552d121.png#align=left&display=inline&height=157&originHeight=157&originWidth=1215&size=17953&status=done&style=none&width=1215)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608709504184-8eeb7434-8a99-4b05-b991-b744840b76f4.png#align=left&display=inline&height=522&originHeight=522&originWidth=1191&size=120755&status=done&style=none&width=1191)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608709583679-2c390d00-e8ac-4cb0-aec2-48b0a2e7e749.png#align=left&display=inline&height=592&originHeight=592&originWidth=979&size=422697&status=done&style=none&width=979)

3. 配置证书。选择腾讯云产品里面的"CDN 与加速"→["内容分发网络"](https://console.cloud.tencent.com/cdn)，选择["证书管理"](https://console.cloud.tencent.com/cdn/certificate)→“配置证书”。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608709943909-6de61fce-cbb3-4aed-aaa8-9e9dfa6d2214.png#align=left&display=inline&height=500&originHeight=500&originWidth=988&size=82004&status=done&style=none&width=988)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608710049270-81ffd6ef-c0f9-4b8e-a4d8-49dadf471e96.png#align=left&display=inline&height=704&originHeight=704&originWidth=1094&size=106382&status=done&style=none&width=1094)

4. 自定义域名完成，开启 https 访问。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608710339444-7374c94a-06c8-46e1-bf6b-a57578692c05.png#align=left&display=inline&height=566&originHeight=566&originWidth=988&size=451848&status=done&style=none&width=988)

接下来，我们来看另外一种情况：新建非腾讯云注册的域名应该怎么处理。

1. 新建自定义域名，点击“确定”后，自动生成一个 CNAME 记录。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608710477563-4b799034-c872-439d-813f-beeac29a808c.png#align=left&display=inline&height=288&originHeight=288&originWidth=473&size=18680&status=done&style=none&width=473)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608710588957-afceac73-ec7d-4cff-9561-7f558fe3b387.png#align=left&display=inline&height=587&originHeight=587&originWidth=979&size=117420&status=done&style=none&width=979)

点击"审核中"，可以看到对应证书在腾讯云中的详细信息。添加非腾讯云注册的自定义域名，证书的状态不会自动由"审核中"更为"待验证" 和 "已签发"，需要一些额外的配置步骤。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608710693912-f6a6b818-207f-449b-b020-8d139007bbc1.png#align=left&display=inline&height=630&originHeight=630&originWidth=1137&size=122501&status=done&style=none&width=1137)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608710825697-e1792185-2d4c-4e8b-9f59-8b310d09d21c.png#align=left&display=inline&height=537&originHeight=537&originWidth=1137&size=103155&status=done&style=none&width=1137)

2. 添加 CNAME 记录。登陆域名供应商的解析中心，添加一个 CNAME 记录。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608712547938-fd598b35-0e4f-4607-afb5-cc13dcc3b9b6.png#align=left&display=inline&height=341&originHeight=341&originWidth=1121&size=51428&status=done&style=none&width=1121)

3. 获取 DNS 验证记录。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608711061749-9f518989-b1d1-4938-b57e-92cd0fb7e91d.png#align=left&display=inline&height=534&originHeight=534&originWidth=1137&size=102624&status=done&style=none&width=1137)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608711343114-af018f38-241e-47ea-810f-e366ce80efe0.png#align=left&display=inline&height=560&originHeight=560&originWidth=1137&size=136387&status=done&style=none&width=1137)

3. 在对应域名的供应商添加 TXT 解析记录。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608711546568-fbfc717f-2a75-4583-8ac9-386d730cac38.png#align=left&display=inline&height=395&originHeight=395&originWidth=1501&size=74425&status=done&style=none&width=1501)
回到腾讯云"我的证书"，等待几分钟，证书状态将会由"待验证" 自动变更为"已签发"，即表示证书已经申请成功。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608711795119-89680048-a0de-44d2-8e8c-377c9720d199.png#align=left&display=inline&height=534&originHeight=534&originWidth=1137&size=107195&status=done&style=none&width=1137)
coding.net 自定义域名的证书状态，在几分钟后，也会变更为"已颁发"。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608711982175-e2324758-9ed3-4724-bea7-0e47d65e4547.png#align=left&display=inline&height=549&originHeight=549&originWidth=1137&size=112295&status=done&style=none&width=1137)

4. 配置证书。选择腾讯云产品里面的"CDN 与加速"→["内容分发网络"](https://console.cloud.tencent.com/cdn)，选择["证书管理"](https://console.cloud.tencent.com/cdn/certificate)→“配置证书”。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608709943909-6de61fce-cbb3-4aed-aaa8-9e9dfa6d2214.png#align=left&display=inline&height=500&originHeight=500&originWidth=988&size=82004&status=done&style=none&width=988)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608712205968-55a7cd0f-274e-434e-b0a6-38b31f44146b.png#align=left&display=inline&height=720&originHeight=720&originWidth=1303&size=176345&status=done&style=none&width=1303)

5. 自定义域名完成，开启 https 访问。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1608712681850-5645cad3-b35f-4838-987b-595a3e83ec72.png#align=left&display=inline&height=566&originHeight=566&originWidth=1033&size=485545&status=done&style=none&width=1033)
