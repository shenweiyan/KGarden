记录一下 WebStack-Hugo 纯静态网址导航的一些更新记录。

**项目开源地址：**
:::tips
Gitee：[https://gitee.com/shenweiyan/WebStack-Hugo](https://gitee.com/shenweiyan/WebStack-Hugo)
GitHub：[https://github.com/shenweiyan/WebStack-Hugo](https://github.com/shenweiyan/WebStack-Hugo)
:::

**基于该主题的导航站点：**

- [https://nav.bioitee.com/](https://nav.bioitee.com/)
- [https://hao.bioitee.com/](https://hao.bioitee.com/)
- [https://so.gd.cn/](https://so.gd.cn/)

# 更新历史

2023-02-23：

- [增加搜索位置不设置背景图的样式适应](https://github.com/shenweiyan/WebStack-Hugo/commit/5718cc6924b572cfa5603a845b991062df4552a2)。

2023-01-16：

- 精简配置，删除部分不必要的样式文件。
- 增加左侧导航滚动支持。

2022-10-08：

- 实现左导航折叠时，点击图标时锚点定位跳转至子一级分类（如果存在子一级分类）；否则跳转至对应 taxonomy。

2022-09-30：

- 修复左侧栏不展开时点击图标显示异常，增加 qrcode tooltip 支持。

```shell
- taxonomy: 常用推荐
  icon: far fa-star
  links:
    - title: 关注我们
      logo: ""
      url: https://bioitee.com
      qrcode: assets/images/logos/qrcode.jpg
      description: BioIT 爱好者！
    - title: 语雀
      logo: 语雀.jpg
      url: https://www.yuque.com/
      description: 专业的云端知识库。
```

2022-09-19

- 增加 logo 图标资源缺失或不正确时候，默认的 logo 设置参数：**defaultLogo**

```r
# logo图片资源不存在或者错误时, 默认显示的logo; 该参数如为空,将会一直加载对应的logo,直至成功为止
defaultLogo   = "assets/images/logos/default.webp"
```

2022-08-31

- 增加头部的导航栏的一键化配置，通过修改 data/headers.yml 文件，可增删头部的导航栏。

```
- item: 首页
  icon: fa fa-home
  link: "./"

- item: 作者
  icon: fa fa-book
  link: https://www.yuque.com/shenweiyan

- item: 配置
  icon: fa fa-cog
  link: ""
  list:
    - name: 源码
      url: "#"
    - name: 图标
      url: "#"
```
