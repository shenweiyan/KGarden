Win10 系统刚安装后，打开“此电脑”，在资源管理器上面会有图片/文档/音乐/3D 对象视频等 7 个文件夹，对于有强迫症的朋友来说，第一件事就是将这几个文件夹删除。

- ① 红色框：右侧管理器窗口
- ② 绿色框：左侧栏

![两处不同位置的文件夹](https://cdn.nlark.com/yuque/0/2022/png/126032/1645777331032-645d2162-72ea-467a-9ebd-f0286088cfe8.png#clientId=u972bad65-3f74-4&from=paste&id=ud7f68df2&originHeight=631&originWidth=895&originalType=url&ratio=1&rotation=0&showTitle=true&size=238312&status=done&style=none&taskId=u8d589fed-f89f-4350-b273-4ac20c199a2&title=%E4%B8%A4%E5%A4%84%E4%B8%8D%E5%90%8C%E4%BD%8D%E7%BD%AE%E7%9A%84%E6%96%87%E4%BB%B6%E5%A4%B9 "两处不同位置的文件夹")

个人用的是 Win10 专业版（21H1），网上找了不少方法都没能解决想要删除/隐藏资源管理器中的 "**图片/视频/文档/下载/音乐/桌面**" 这些文件夹的方法。

有时候自己胡乱更改，甚至删除注册表，一不小心还会导致电脑出现各种异常，没有备份注册表的情况下就恢复不回来了！下面的解决方法，完整原文参考：《[Win10 删除资源管理器“图片/视频/文档/下载/音乐/桌面"等文件夹方法 - 知乎](https://zhuanlan.zhihu.com/p/346784646)》。

# 一、打开注册表编辑器

按下 Win+R，输入 **regedit** 回车，打开注册表。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1645777530087-0a934032-1809-4371-ad4f-0b3abffcf9d0.png#clientId=u972bad65-3f74-4&from=paste&id=ua6c0fbea&originHeight=228&originWidth=397&originalType=url&ratio=1&rotation=0&showTitle=false&size=61312&status=done&style=none&taskId=u7b39bb90-8716-4e2d-8fd0-b492ba60220&title=)

![两处文件夹的注册表根位置](https://cdn.nlark.com/yuque/0/2022/png/126032/1645777554485-ae624f2d-ebe6-479d-8b36-ce4aee1add6a.png#clientId=u972bad65-3f74-4&from=paste&id=u8474f0e2&originHeight=614&originWidth=294&originalType=url&ratio=1&rotation=0&showTitle=true&size=165374&status=done&style=none&taskId=u2b1ca3da-c28e-4ed2-a085-5c8eb208d3f&title=%E4%B8%A4%E5%A4%84%E6%96%87%E4%BB%B6%E5%A4%B9%E7%9A%84%E6%B3%A8%E5%86%8C%E8%A1%A8%E6%A0%B9%E4%BD%8D%E7%BD%AE "两处文件夹的注册表根位置")

# 二、删除红色框 ① 处的文件夹

1. 定位到以下位置：

HKEY_LOCAL_MACHINE
|-SOFTWARE
|-Microsoft
|-Windows
|-CurrentVersion
|-Explorer
|-MyComputer
|-NameSpace

```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace
```

![红色①处的注册表项](https://cdn.nlark.com/yuque/0/2022/png/126032/1645777759322-aa075612-f6a5-48d3-a15c-864bf0604872.png#clientId=u972bad65-3f74-4&from=paste&id=uf13e9ea4&originHeight=578&originWidth=717&originalType=url&ratio=1&rotation=0&showTitle=true&size=309066&status=done&style=none&taskId=u4c965b86-9a3a-4d02-9aa6-bef0ae80c60&title=%E7%BA%A2%E8%89%B2%E2%91%A0%E5%A4%84%E7%9A%84%E6%B3%A8%E5%86%8C%E8%A1%A8%E9%A1%B9 "红色①处的注册表项")

2. 找到相应的键值进行删除操作（删除之前先做好备份），或者注释（推荐）。

   - 删除【下载】文件夹： {088e3905-0323-4b02-9826-5d99428e115f}
   - 删除【图片】文件夹： {24ad3ad4-a569-4530-98e1-ab02f9417aa8}
   - 删除【音乐】文件夹： {3dfdf296-dbec-4fb4-81d1-6a3438bcf4de}
   - 删除【文档】文件夹： {d3162b92-9365-467a-956b-92703aca08af}
   - 删除【视频】文件夹： {f86fa3ab-70d2-4fc7-9c99-fcbf05467f3a}
   - 删除【桌面】文件夹： {B4BFCC3A-DB2C-424C-B029-7FE99A87C641}
   - 删除【3D 对象】文件夹： {0DB7E03F-FC29-4DC6-9020-FF41B59E513A}

![选中项右键选择删除或者左键选中，按 Delete 直接删除](https://cdn.nlark.com/yuque/0/2022/png/126032/1645777917836-9a2ab245-0f11-4f80-bb81-ecf890779a31.png#clientId=u972bad65-3f74-4&from=paste&id=u058f3427&originHeight=581&originWidth=717&originalType=url&ratio=1&rotation=0&showTitle=true&size=305769&status=done&style=none&taskId=u72a3a68c-3009-449d-a3d6-21f543d48e2&title=%E9%80%89%E4%B8%AD%E9%A1%B9%E5%8F%B3%E9%94%AE%E9%80%89%E6%8B%A9%E5%88%A0%E9%99%A4%E6%88%96%E8%80%85%E5%B7%A6%E9%94%AE%E9%80%89%E4%B8%AD%EF%BC%8C%E6%8C%89%20Delete%20%E7%9B%B4%E6%8E%A5%E5%88%A0%E9%99%A4 "选中项右键选择删除或者左键选中，按 Delete 直接删除")
如果不想删除，可以在每个注册表前边加一个减号“-”，同样起作用。
![2022.02.25-16.13.44.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1645778258986-43eecc40-d60f-4fd9-be34-5ed8243babe1.png#clientId=u972bad65-3f74-4&from=ui&id=u7390829d&originHeight=474&originWidth=725&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41296&status=done&style=none&taskId=ubcd0e5f1-1eb0-4336-bd53-4f82fe55e6f&title=)

# 三、删除绿色框 ② 处的文件夹

1. 定位以下位置：

HKEY_LOCAL_MACHINE
|-SOFTWARE
|-WOW6432Node
|-Microsoft
|-Windows
|-CurrentVersion
|-Explorer
|-MyComputer
|-NameSpace
![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1645778364638-647ad87e-6b59-402c-b4cb-4e355e48a99b.png#clientId=u972bad65-3f74-4&from=paste&id=u0f0e5202&originHeight=671&originWidth=720&originalType=url&ratio=1&rotation=0&showTitle=false&size=356803&status=done&style=none&taskId=u8b43e899-9bdc-4102-bd0b-5f9b623460b&title=)

2. 找到相应的键值进行删除操作（删除之前先做好备份），或者注释（推荐）。

   - 删除【下载】文件夹： {088e3905-0323-4b02-9826-5d99428e115f}
   - 删除【图片】文件夹： {24ad3ad4-a569-4530-98e1-ab02f9417aa8}
   - 删除【音乐】文件夹： {3dfdf296-dbec-4fb4-81d1-6a3438bcf4de}
   - 删除【文档】文件夹： {d3162b92-9365-467a-956b-92703aca08af}
   - 删除【视频】文件夹： {f86fa3ab-70d2-4fc7-9c99-fcbf05467f3a}
   - 删除【桌面】文件夹： {B4BFCC3A-DB2C-424C-B029-7FE99A87C641}
   - 删除【3D 对象】文件夹： {0DB7E03F-FC29-4DC6-9020-FF41B59E513A}

# 四、重启下 explorer 进程

打开任务管理器。
![在任务栏空白处鼠标右键选择任务管理器](https://cdn.nlark.com/yuque/0/2022/png/126032/1645778569475-843abbe2-df82-4526-b592-8a7dda1762b4.png#clientId=u972bad65-3f74-4&from=paste&height=490&id=u2e6c0ae1&originHeight=490&originWidth=440&originalType=binary&ratio=1&rotation=0&showTitle=true&size=75439&status=done&style=none&taskId=u584a789c-cc3e-4512-985d-7b275d416b8&title=%E5%9C%A8%E4%BB%BB%E5%8A%A1%E6%A0%8F%E7%A9%BA%E7%99%BD%E5%A4%84%E9%BC%A0%E6%A0%87%E5%8F%B3%E9%94%AE%E9%80%89%E6%8B%A9%E4%BB%BB%E5%8A%A1%E7%AE%A1%E7%90%86%E5%99%A8&width=440 "在任务栏空白处鼠标右键选择任务管理器")

2. 重新启动 "Windows 资源管理器"。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/126032/1645778641751-20ac703f-f1ec-48b0-bcf5-e4306b3da3a0.png#clientId=u972bad65-3f74-4&from=paste&height=593&id=ude4847ae&originHeight=593&originWidth=666&originalType=binary&ratio=1&rotation=0&showTitle=false&size=62109&status=done&style=none&taskId=uc6a9a96e-5012-4951-af41-f4c97ee1b21&title=&width=666)

以上就是如何删除资源管理器中的图片/文档/音乐/视频等文件夹的操作方法，如果也有强迫症希望自己的资源管理器看着干干净净，可以试试这些方法。

# 五、参考资料

1. [小马哥看世界](https://www.zhihu.com/people/harvim)，《[Win10 删除资源管理器“图片/视频/文档/下载/音乐/桌面"等文件夹方法](https://zhuanlan.zhihu.com/p/346784646)》，知乎
