熟悉 R 绘图的朋友肯定知道，在普通绘图中，图片的大小可以直接在 `png()`  和 `pdf()`  中指定，而绘图区大小则可以用 `par()`  中的 `mar`  或 `mai`  来指定。

但是在 ggplot2 中，图片大小依然可以在 `png`  和 `pdf`  中设定，但是边界大小， `par`  函数似乎就不奏效了。至今天探索，才发现原来这个参数隐藏在 `theme`  中，其名为 `plot.margin` 。

## 1. 原图

```r
library(ggplot2)
library(ggthemes)
p <- ggplot(mtcars, aes(mpg, wt)) + geom_point(aes(colour=factor(cyl))) + guides(color=F)
p <- p + theme_solarized(light=FALSE) + scale_colour_solarized('blue')
ggsave("test0.png", units="in", dpi=300, width=4, height=4, device="png")
```

![test0.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1589967317305-aee82654-9013-467c-a51b-2f7adef7c200.png#height=300&id=mChS2&originHeight=1200&originWidth=1200&originalType=binary&ratio=1&size=36756&status=done&style=none&width=300)

## 2. 第一次调整边界参数

```r
library(ggplot2)
library(ggthemes)
p <- ggplot(mtcars, aes(mpg, wt)) + geom_point(aes(colour=factor(cyl))) + guides(color=F)
p <- p + theme_solarized(light=FALSE) + scale_colour_solarized('blue')
p <- p + theme(plot.margin=unit(rep(1,4),'lines'))
ggsave("test1.png", units="in", dpi=300, width=4, height=4, device="png")
```

![test1.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1589967386635-01505d53-070a-45a4-995a-dad35d926e52.png#height=300&id=KDKKS&originHeight=1200&originWidth=1200&originalType=binary&ratio=1&size=36194&status=done&style=none&width=300)

## 3. 第二次调整边界参数

```r
library(ggplot2)
library(ggthemes)
p <- ggplot(mtcars, aes(mpg, wt)) + geom_point(aes(colour=factor(cyl))) + guides(color=F)
p <- p + theme_solarized(light=FALSE) + scale_colour_solarized('blue')
p <- p + theme(plot.margin=unit(rep(3,4),'lines'))
ggsave("test2.png", units="in", dpi=300, width=4, height=4, device="png")
```

![test3.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1589967511021-4f986d7e-418d-40f3-909f-aa330606ec8a.png#height=300&id=zL2TY&originHeight=1200&originWidth=1200&originalType=binary&ratio=1&size=35304&status=done&style=none&width=300)

比较上述 3 幅图片，可明显发现，随着边界参数值增大，绘图区与边界的距离不断增大，从而在图片上留出更多空白区域。

此外， `plot.margin`  是否跟 `par(mar=...)`  一样遵循下、左、上、右的控制顺序呢？各位可以敲下代码，稍稍一试。
