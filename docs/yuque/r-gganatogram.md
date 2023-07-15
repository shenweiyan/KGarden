# 摘要

将数据显示到解剖结构上，是一种可以快速观察组织相关信息的便捷技术。然而，绘制组织是一项复杂的任务（a complex task），需要解剖学和艺术方面的专业知识。虽然已经存在可用于在解剖图上显示基因表达的基于网络的应用技术，但是其他非遗传学科（non-genetic disciplines）缺乏类似的工具。此外，基于网络的工具通常缺乏与编程语言（如 R）中的包相关联的模块性（ lack the modularity associated with packages in programming languages）。这里我介绍 gganatogram，一个根据 ggplot2 的图形语法和来自 Expression Atlas 公众可用解剖图的组合，来绘制模块化物种解剖图的 R 包。这种组合允许快速，简单，模块化和可重复地生成解剖图。仅使用一个命令以及一个具有组织名称，分组，颜色和值的数据框，该工具就可以使用户能够以特定的颜色可视化特定的人体和小鼠组织，或者根据变量进行分组，或显示特定的值，例如基因-表达（gene-expression），药代动力学（pharmacokinetics）或所选组织之间的细菌负荷（bacterial load）。我希望这个工具对更广泛的生物科学界有用。也欢迎社区成员提交额外的解剖图，并将其纳入该软件包中。

# 介绍

在多细胞生物中有效地显示组织信息可能是一个费力且耗时的过程（a laborious and time consuming process）。 研究人员通常希望展示值之间的差异，例如一个生物体组织之间，或不同组中具有相似组织之间的基因表达值或药代动力学值。

虽然条形图和热图提供了组间差异的信息视图，但很难立即观察到生物学意义（图 1a-b）。 与解剖图相比，后者更易于快速发现组织或组之间的差异，并可立即为这些观察提供生物学背景（biological context）（图 1c）。 这还有一个额外的好处，即无论是阅读论文还是参加讲座，观众都必须花费更少的时间和精力来掌握结果。
![](https://cdn.nlark.com/yuque/0/2018/png/126032/1541230221147-271d2842-2dd5-40fe-997c-a1cc038c4393.png#align=left&display=inline&height=725&originHeight=965&originWidth=994&status=done&width=747#height=965&id=CG7nf&originHeight=965&originWidth=994&originalType=binary&ratio=1&status=done&style=none&width=994)
Figure 1. Comparison between barplot (top left), heatmap (top right), and anatogram (bottom) to display tissue values between groups.

已经存在几种在不同组织中显示基因表达的在线工具（如 Expression Atlas、Semantic Body Browser、TISSUES 2.0）。 虽然这些工具提供了关于各种组织和生物体中基因表达的重要信息，但除遗传学之外的其他学科由于(缺少)对基因的关注而无法利用这些应用（other disciplines besides genetics are unable to utilise these applications due to the focus on genes）。 此外，这些工具通常只包含一组可预测的预定义实验，导致难以呈现我们自己的数据。 还有其他值得注意是，使用这些工具，重建绘图或从结果中自动创建绘图可能很费力。

这里介绍的 gganatogram，是一个利用 Expression Atlas 中公开提供的鼠和人体解剖图，基于 ggplot2 的开源 R 软件包。 使用此软件包，任何 R 用户都可以轻松快速地查看具有指定颜色，组和值的解剖图。 使用 ggplot2 中熟悉的语法，该程序可用于模块化解剖图的生成。

# 方法

## 实现（Implementation）

gganatogram 存储在 [neuroconductor](https://neuroconductor.org/package/gganatogram)，一个用于快速测试（rapid testing）和可重现计算成像软件（reproducible computational imaging software）传播的开源平台上， 其开发版本可以在 [github/jespermaag/gganatogram](https://github.com/jespermaag/gganatogram) 找到，它允许社区通过创建坐标文件来发布包的 issues，提交请求或添加解剖图。

```r
source("https://neuroconductor.org/neurocLite.R")
neuro_install("gganatogram", release = "stable",
release_repo = latest_neuroc_release(release = "stable"))
```

开发版本可以从 github 安装：

```r
devtools::install_github("jespermaag/gganatogram")
```

简而言之，为了生成包含所有组织坐标的主列表对象，作者从 Expression Atlas 下载了 SVG 文件（可从 [gganatogram GitHub](https://github.com/ebi-gene-expression-group/anatomogram/tree/master/src/svg) 中获取）并使用自定义的 python 脚本（可从 [GitHub](https://github.com/jespermaag/gganatogram/blob/master/data-raw/getCoord.py) 中获得）处理它们。 该脚本通过 SVG 文件来提取名称，坐标和 SVG 转换信息。 然后在 R 中对它们进行后处理以创建构成组织坐标的 rda 文件。

## 操作（Operation）

gganatogram 需要满足 R≥3.0.0，ggplot2 v.3.0.0 和 ggpolypath v.0.1.0 。 该程序应该能够在任何具有 R 系统要求的计算机上运行。可以使用包含器官名称，颜色，类型或值的基本 data.frame 生成图形。 根据 data.frame 的顺序，一次一个地绘制器官。 每个连续行的组织将在前一个上面分层。 gganatogram 包提供了四个这样的 data.frame，其中包含可用于绘图的所有组织，每个人和小鼠一个，并按性别划分。

```r
hgMale_key, hgFemale_key, mmMale_key, mmFemale_key
```

这些数据框已经指定了颜色，类型和指定的随机数，以便于开始绘图。

```r
head(hgFemale_key)
            organ  colour      type     value
1        pancreas  orange digestion 10.373146
2           liver  orange digestion 19.723172
3           colon  orange digestion 14.853335
4     bone_marrow #41ab5d     other 19.681587
5 urinary_bladder  orange digestion 14.914273
6         stomach  orange digestion  2.667599
```

主要功能是 gganatogram()。 默认情况下，没有任何参数，它会绘制具有标准 ggplot2 参数的男性人的轮廓。 通过添加几个选项，可以快速更改为女性，通过选定的颜色填充指定的器官，或者根据值填充器官（图 2）。

```r
library(gganatogram)
library(gridExtra)
organPlot <- data.frame(organ = c("heart", "leukocyte", "nerve", "brain", "liver", "stomach", "colon"),
    type = c("circulation", "circulation", "nervous␣system", "nervous␣system", "digestion", "digestion", "digestion"),
    colour = c("red", "red", "purple", "purple", "orange", "orange", "orange"),
    value = c(10, 5, 1, 8, 10, 5, 10),
    stringsAsFactors=F)

A <- gganatogram()
B <- gganatogram(fillOutline="#a6bddb", sex="female") + theme_void()
C <- gganatogram(data=organPlot, fillOutline="#a6bddb", organism="human",
      sex="female", fill="colour")+ theme_void()
D <- gganatogram(data=organPlot, fillOutline="#a6bddb", organism="human",
      sex="female", fill="value")+ theme_void()
grid.arrange(A, B, C, D, ncol=4)
```

![](https://cdn.nlark.com/yuque/0/2018/png/126032/1541235122425-ea06fe8e-1a33-4c4b-989f-abb1137bfeaf.png#align=left&display=inline&height=251&originHeight=335&originWidth=998&status=done&width=747#height=335&id=WK0Sk&originHeight=335&originWidth=998&originalType=binary&ratio=1&status=done&style=none&width=998)
(A) Default plot generated by calling gganatogram(), (B) adding female, plotting specified organs by (C) colour, (D) value.

# 示例

要绘制每个生物体的所有组织，请使用每个生物体和性别所提供的密钥文件。 这将按照每个 data frame 的顺序显示所有组织。 要更改器官彼此叠加的顺序，请重新排序数据框以使这些组织位于底部。

```r
library(gganatogram)
library(gridExtra)
hgMale <- gganatogram(data=hgMale_key, fillOutline="#a6bddb", organism="human",
           sex="male", fill="colour") + theme_void()

hgFemale <- gganatogram(data=hgFemale_key, fillOutline="#a6bddb",
             organism="human", sex="female", fill="colour") + theme_void()

mmMale <- gganatogram(data=mmMale_key, fillOutline="#a6bddb", organism="mouse",
           sex="male", fill="colour") + theme_void()

mmFemale <- gganatogram(data=mmFemale_key, outline = T, fillOutline="#a6bddb",
             organism="mouse", sex="female", fill="colour") + theme_void()

grid.arrange(hgMale, hgFemale, mmMale, mmFemale, ncol=4)
```

![](https://cdn.nlark.com/yuque/0/2018/png/126032/1541235847941-52d3c111-cb53-481a-be3f-bc1b93f03c55.png#align=left&display=inline&height=238&originHeight=318&originWidth=998&status=done&width=747#height=318&id=Vms7I&originHeight=318&originWidth=998&originalType=binary&ratio=1&status=done&style=none&width=998)

```r
hgMale <- gganatogram(data=hgMale_key, fillOutline='#440154FF', organism='human', sex='male', fill="value") + theme_void() +  scale_fill_viridis()
hgFemale <- gganatogram(data=hgFemale_key, fillOutline='#440154FF', organism='human', sex='female', fill="value") + theme_void() +  scale_fill_viridis()
mmMale <- gganatogram(data=mmMale_key, fillOutline='#440154FF', organism='mouse', sex='male', fill="value") + theme_void() +  scale_fill_viridis()
mmFemale <- gganatogram(data=mmFemale_key, outline = T, fillOutline='#440154FF', organism='mouse', sex='female', fill="value")  +theme_void()   +  scale_fill_viridis()

grid.arrange(hgMale, hgFemale, mmMale, mmFemale, ncol=2)
```

![AllSpeciesPlotValue-1.svg](https://cdn.nlark.com/yuque/0/2019/svg/126032/1558245234454-02d7e25b-d6c5-48f3-9282-e830154cd10d.svg#align=left&display=inline&height=480&name=AllSpeciesPlotValue-1.svg&originHeight=480&originWidth=672&size=2203107&status=done&width=672#height=480&id=JwBtg&originHeight=480&originWidth=672&originalType=binary&ratio=1&status=done&style=none&width=672)

要使用 gganatogram 函数，我们需要具有器官（organ）、颜色（colour）和相关值（value）的数据框（data frame）。

```r
organPlot <- data.frame(organ = c("heart", "leukocyte", "nerve", "brain", "liver", "stomach", "colon"),
 type = c("circulation", "circulation",  "nervous system", "nervous system", "digestion", "digestion", "digestion"),
 colour = c("red", "red", "purple", "purple", "orange", "orange", "orange"),
 value = c(10, 5, 1, 8, 2, 5, 5),
 stringsAsFactors=F)

 head(organPlot)
#>       organ           type colour value
#> 1     heart    circulation    red    10
#> 2 leukocyte    circulation    red     5
#> 3     nerve nervous system purple     1
#> 4     brain nervous system purple     8
#> 5     liver      digestion orange     2
#> 6   stomach      digestion orange     5
```

使用 gganatogram 函数，根据颜色填充器官。

```r
gganatogram(data=organPlot, fillOutline='#a6bddb', organism='human', sex='male', fill="colour")
```

![1.svg](https://cdn.nlark.com/yuque/0/2019/svg/126032/1558245599419-a8410c35-38ff-44bf-8cc9-54ef848f0b17.svg#align=left&display=inline&height=480&name=1.svg&originHeight=480&originWidth=288&size=265512&status=done&width=288#height=480&id=M1FVj&originHeight=480&originWidth=288&originalType=binary&ratio=1&status=done&style=none&width=288)

当然，我们可以使用 ggplot 主题和函数来调整绘图。

```r
gganatogram(data=organPlot, fillOutline='#a6bddb', organism='human', sex='male', fill="colour") +
theme_void()
```

![2.svg](https://cdn.nlark.com/yuque/0/2019/svg/126032/1558245728061-a6d666b7-edda-4811-8504-fd197622e61d.svg#align=left&display=inline&height=480&name=2.svg&originHeight=480&originWidth=288&size=240837&status=done&width=288#height=480&id=TSP2Z&originHeight=480&originWidth=288&originalType=binary&ratio=1&status=done&style=none&width=288)

我们也可以根据给予每个器官的值来填充各个组织的不同颜色；甚至，gganatogram 可以绘制细胞亚结构的相关组织图；或者是其他物种对应的解剖表达图谱。
![4.svg](https://cdn.nlark.com/yuque/0/2019/svg/126032/1558246311099-6a4b2f5e-e4fc-47d1-b757-1687eefd8ded.svg#align=left&display=inline&height=480&name=4.svg&originHeight=480&originWidth=672&size=556965&status=done&width=672#height=480&id=Sjl0Q&originHeight=480&originWidth=672&originalType=binary&ratio=1&status=done&style=none&width=672)
![3.svg](https://cdn.nlark.com/yuque/0/2019/svg/126032/1558246269308-293c4c4b-20ad-4789-9ff5-9814398bfd8a.svg#align=left&display=inline&height=480&name=3.svg&originHeight=480&originWidth=672&size=285630&status=done&width=672#height=480&id=PUeSU&originHeight=480&originWidth=672&originalType=binary&ratio=1&status=done&style=none&width=672)

更多 gganatogram 解剖图谱绘制示例可以参考其在 Github 的介绍，[https://github.com/jespermaag/gganatogram](https://github.com/jespermaag/gganatogram)， 感兴趣的童鞋可以去尝试研究一下。

总的来说，gganatogram 是一款强大的 R 包，它可以根据 ggplot2 和 Expression Atlas 的信息轻松地显示解剖图，这些工具组合成了一个更加强大的工具来绘制和显示组织信息。

# 参考资料

1. Maag JLV. [gganatogram: An R package for modular visualisation of anatograms and tissues based on ggplot2](https://f1000research.com/articles/7-1576/v1) [version 1; referees: 1 approved]. F1000Research 2018, 7:1576 (doi: 10.12688/f1000research.16409.1)
2. Jesper Maag，[Create anatograms using ggplot2](https://github.com/jespermaag/gganatogram)，Github