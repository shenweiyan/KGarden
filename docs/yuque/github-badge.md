GitHub 项目的 README.md 中可以添加徽章（Badge）对项目进行标记和说明，这些好看的小图标不仅简洁美观，而且还包含了清晰易读的信息。

GitHub 徽标的官方网站是  [http://shields.io/](http://shields.io/)，我们可以在官网预览绝大部分的徽标样式，然后选择自己喜欢的（当然首先需要适用于自己的目标项目）徽标，添加到自己的项目文档中去。

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1566372610540-403c6104-d739-4a28-9141-5ead8d8fed77.png#align=left&display=inline&height=99&originHeight=99&originWidth=679&size=4679&status=done&style=none&width=679)

### 生成已有的标签

- 打开  GitHub 徽标的官方网站  [http://shields.io/](http://shields.io/)，输入你在 Github 的 repo URL，即可看到 issues、forks、stars 相关的图标及代码。

![galaxy-badges.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1566738475988-cc06f341-eb6d-4fa2-bfc9-5d84e0ed5670.png#align=left&display=inline&height=793&originHeight=793&originWidth=1364&size=93070&status=done&style=none&width=1364)

- 点击图标或者代码，即可进入设置页，设置图标的颜色、样式等属性，最后选择你想要的 markdown 或者 URL 代码到 README.md 或者项目的文档页面即可。

![galaxy-star.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1566738487674-6f8b7ecf-106f-4d82-b5df-8e173d25f300.png#align=left&display=inline&height=999&originHeight=999&originWidth=1363&size=58871&status=done&style=none&width=1363)

- 一些常见比较好玩的  Badges

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1566354267965-1cce470f-faf8-48c0-a3db-361da2dae48e.png#align=left&display=inline&height=551&originHeight=551&originWidth=675&size=34739&status=done&style=none&width=675)

### 生成自定义徽章标签

如果我们想要生成的徽章字样和颜色  [shields.io](http://shields.io/)  上面没有怎么办?例如我们想生成一个类似的徽章我们应该怎么做呢?

首先，我们进去  [https://shields.io/](https://shields.io/) ，在 shields 网站拉到最后，也就是 "You Badge" 部分填写自定义的 label、message、color 信息。
![bioitee-badge.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1614656582637-f04dd9ed-efd7-434e-b07b-e449de557c26.png#align=left&display=inline&height=666&originHeight=666&originWidth=1153&size=63059&status=done&style=none&width=1153)

然后，点击 "Make Badge" 按钮就可以生成我们想要的任何徽章了！
![bioitee-badge-img.png](https://cdn.nlark.com/yuque/0/2021/png/126032/1614656833707-95258db4-a0d3-4530-810b-fa002480cfcd.png#align=left&display=inline&height=125&originHeight=125&originWidth=615&size=17335&status=done&style=none&width=615)

- 如果我们在写 markdown 的时候想为我们的徽章或者进度条添加点击跳转的超链接，可以使用超链接和图片的语法嵌套来写，具体可以参照 markdown 标准语法。
- 自定义徽章和进度条由于参数是写死的，不会根据网络的数据自动变化上面的文字，所以，这些标签是静态的，修改的时候需要我们手动更改 URL。
