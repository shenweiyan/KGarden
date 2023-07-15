最新的  [magick](https://cran.r-project.org/package=magick)  包是为能够在 R 中更现代化、简单化高质量图像处理而进行的一次努力。该包封装了目前最强大的开源图片处理库  [ImageMagick STL](https://www.imagemagick.org/Magick++/STL.html) 。

ImageMagick 库具有大量功能。当前版本的 Magick 公开了很多内容，但是作为第一个发行版本，文档仍然很少。本文简单的介绍了其中一些最重要的概念来帮助了解  `magick` 。

## 安装  magick

在 Windows 或者 MacOS，可以通过 CRAN 安装该软件包。

```r
install.packages("magick")
```

二进制 CRAN 软件包开箱即用，只需少量的工作，就可以使绝大多数的重要特性得以实现。使用 `magick_config`  可以查看您的 ImageMagick 版本支持哪些功能和格式。

```r
> library(magick)
Linking to ImageMagick 6.9.9.14
Enabled features: cairo, freetype, fftw, ghostscript, lcms, pango, rsvg, webp
Disabled features: fontconfig, x11
```

```r
> str(magick::magick_config())
List of 21
 $ version           :Class 'numeric_version'  hidden list of 1
  ..$ : int [1:4] 6 9 9 14
 $ modules           : logi FALSE
 $ cairo             : logi TRUE
 $ fontconfig        : logi FALSE
 $ freetype          : logi TRUE
 $ fftw              : logi TRUE
 $ ghostscript       : logi TRUE
 $ jpeg              : logi TRUE
 $ lcms              : logi TRUE
 $ libopenjp2        : logi FALSE
 $ lzma              : logi TRUE
 $ pangocairo        : logi TRUE
 $ pango             : logi TRUE
 $ png               : logi TRUE
 $ rsvg              : logi TRUE
 $ tiff              : logi TRUE
 $ webp              : logi TRUE
 $ wmf               : logi FALSE
 $ x11               : logi FALSE
 $ xml               : logi TRUE
 $ zero-configuration: logi TRUE
```

## 源码编译

Linux 下你需要安装  ImageMagick++ 库。在  Debian/Ubuntu，这个库叫  [libmagick++-dev](https://packages.debian.org/testing/libmagick++-dev)：

```bash
sudo apt-get install libmagick++-dev
```

在 Fedora 或者 CentOS/RHEL 我们需要安装  [ImageMagick-c++-devel](https://apps.fedoraproject.org/packages/ImageMagick-c++-devel)：

```bash
sudo yum install ImageMagick-c++-devel
```

要从 macOS 上的源代码安装，您需要来自 homebrew 的 `imagemagick@6` 。

```bash
brew install imagemagick@6
```

不幸的是，当前 homebrew  上的 `imagemagick@6` 配置禁用了许多功能，包括 librsvg 和 fontconfig。 因此，字体和 svg 渲染的质量可能不是最佳的（建议安装时至少加上  `--with-fontconfig`  和  `--with-librsvg`  选项来支持高质量的字体和 svg 渲染。CRAN 上的 OS-Xe 二进制包已经默认配置好了）。

## 图像输入输出

magick 之所以如此神奇，是因为它会自动转换并呈现所有常见的图像格式。ImageMagick 支持数十种格式并自动检测类型。使用 `magick::magick_config()`  可以列出您的 ImageMagick 版本支持的格式。

### 读和写

可以使用 `image_read` 从图像数据的文件路径，URL 或原始矢量直接读取图像。`image_info` 函数显示有关图像的一些元数据，类似 于 imagemagick 标识命令行实用程序。

```r
library(magick)
tiger <- image_read_svg('http://jeroen.github.io/images/tiger.svg', width = 350)
print(tiger)
```

```r
##   format width height colorspace matte filesize density
## 1    PNG   350    350       sRGB  TRUE        0   72x72
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571363977241-d10806a5-682d-4b4c-8dc7-339751c0eb61.png#align=left&display=inline&height=175&originHeight=350&originWidth=350&size=98851&status=done&width=175)
我们使用 `image_write` 将任何格式的图像导出为磁盘上的文件，或者如果 `path = NULL` 则导出到内存中的文件。

```r
# Render svg to png bitmap
image_write(tiger, path = "tiger.png", format = "png")
```

如果 `path` 是文件名，则 `image_write` 成功返回 `path`，以便可以将结果通过文件路径传递给函数。

### 转换格式

Magick 以原始格式将图像保留在内存中。你可以在 `image_write` 中通过指定格式参数以转换为另一种格式。或者还可以在应用转换之前，先在内部将图像转换为其他格式。如果你的原始格式是有损的，这将很有用。

```r
tiger_png <- image_convert(tiger, "png")
image_info(tiger_png)
```

```r
##   format width height colorspace matte filesize density
## 1    PNG   350    350       sRGB  TRUE        0   72x72
```

### 预览

具有内置网络浏览器（例如 RStudio）的 IDE 会在查看器中自动显示魔术图像。这就形成了一个整洁的交互式图像编辑环境。
![aaa1.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571371499649-43035f4a-a386-4678-a30a-2aeb1e981489.png#align=left&display=inline&height=768&originHeight=768&originWidth=1365&size=190879&status=done&width=1365)
另外，在 Linux 上，您可以使用 `image_display` 在 X11 窗口中预览图像。最后，`image_browse`  会在你系统默认的应用程序中为给定类型打开图像。

```r
# X11 only
image_display(tiger)

# System dependent
image_browse(tiger)
```

另一种方法是将图像转换为栅格对象，然后将其绘制在 R 的图形显示上。但是，这非常慢，并且仅在与其他绘图功能结合使用时才有用。 请参阅下面的 #raster。

## 转变

了解可用转换的最佳方法是遍历 RStudio 中 `?transformations`  页面中的示例。下面是一些示例。

### 剪切与编辑

一些转换函数采用了 `geometry`  参数，该参数需要 `AxB + C + D` 形式的特殊语法，其中每个元素都是可选的。 一些例子：

- `image_crop(image, "100x150+50")`: crop out `width:100px` and `height:150px` starting `+50px` from the left
- `image_scale(image, "200")`: resize proportionally to width: `200px`
- `image_scale(image, "x200")`: resize proportionally to height: `200px`
- `image_fill(image, "blue", "+100+200")`: flood fill with blue starting at the point at `x:100, y:200`
- `image_border(frink, "red", "20x10")`: adds a border of 20px left+right and 10px top+bottom

完整的语法，可以参考  [Magick::Geometry](http://www.imagemagick.org/Magick++/Geometry.html)  文档。

```r
# Example image
frink <- image_read("https://jeroen.github.io/images/frink.png")
print(frink)
```

```r
##   format width height colorspace matte filesize density
## 1    PNG   220    445       sRGB  TRUE    73494   72x72
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366136037-a75b9f9d-1b82-42e4-a611-b11140823c5c.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=67143&status=done&width=220)

```r
# Add 20px left/right and 10px top/bottom
image_border(image_background(frink, "hotpink"), "#000080", "20x10")
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366166735-29222db4-bb0f-4f74-9383-7a4189b3305b.png#align=left&display=inline&height=465&originHeight=465&originWidth=260&size=65910&status=done&width=260)

```r
# Trim margins
image_trim(frink)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366274140-f41c6a57-c19e-4e94-8b1b-a5c1533c5747.png#align=left&display=inline&height=431&originHeight=431&originWidth=139&size=65654&status=done&width=139)

```r
# Passport pica
image_crop(frink, "100x150+50")
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366305125-ab6d58c9-4c98-4f73-ad18-220463afb3d8.png#align=left&display=inline&height=150&originHeight=150&originWidth=100&size=18879&status=done&width=100)

```r
# Resize
image_scale(frink, "300") # width: 300px
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366322735-b025bbb2-701f-4e3a-b055-d9855c5fb51f.png#align=left&display=inline&height=607&originHeight=607&originWidth=300&size=111615&status=done&width=300)

```r
image_scale(frink, "x300") # height: 300px
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366340578-e5a52536-8f19-4f6b-a70e-1100b196c4bf.png#align=left&display=inline&height=300&originHeight=300&originWidth=148&size=36231&status=done&width=148)

```r
# Rotate or mirror
image_rotate(frink, 45)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366359054-2949de29-a259-487b-b08c-5a5f92838a7a.png#align=left&display=inline&height=473&originHeight=473&originWidth=472&size=86173&status=done&width=472)

```r
image_flip(frink)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366386677-24b88b4e-c005-41d7-97bb-b9268e5ce736.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=67639&status=done&width=220)

```r
image_flop(frink)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366403173-99d32e94-67b6-41e7-8cd9-3edb70aeeff6.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=67288&status=done&width=220)

```r
# Brightness, Saturation, Hue
image_modulate(frink, brightness = 80, saturation = 120, hue = 90)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366420331-91f363ae-bc18-4976-85d5-a8439bf40efa.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=70269&status=done&width=220)

```r
# Paint the shirt orange
image_fill(frink, "orange", point = "+100+200", fuzz = 20)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366435980-80cc29c2-57b5-4cee-a169-7fc47219ae99.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=66473&status=done&width=220)

使用 `image_fill`，我们可以从像素点开始填充。模糊参数允许填充物穿过具有相似颜色的相邻像素。值必须在 0 到 256^2 之间，指定要视为相等的颜色之间的最大几何距离。在这里，我们给教授 Frink 一件橙色的世界杯衬衫。

### 滤镜和效果

ImageMagick 还具有许多值得去检查尝试的标准效果。

```r
# Add randomness
image_blur(frink, 10, 5)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366664424-71f9ade6-c84d-4e7d-a19a-b30c497b55ad.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=73778&status=done&width=220)

```r
image_noise(frink)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366681562-0a0f20d7-f999-42aa-9930-c876c931ea9f.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=257790&status=done&width=220)

```r
# Silly filters
image_charcoal(frink)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366703127-26ab3f9b-6d53-4451-aba5-08e2128e6b13.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=45240&status=done&width=220)

```r
image_oilpaint(frink)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366723362-f5255ea1-adfc-4d4d-b4a0-5d9ea019e568.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=50703&status=done&width=220)

```r
image_negate(frink)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571366740872-8511118e-ec6d-4a9b-88a1-bc4b7ef1eb5e.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=67072&status=done&width=220)

### 核卷积

`image_convolve()` 函数在图像上实行  [kernel](<https://en.wikipedia.org/wiki/Kernel_(image_processing)>)  变换。核卷积是指在核矩阵中，图片每个像素的值根据相邻像素的加权和重新计算。例如，让我们看一下这个简单的核变换：

```r
kern <- matrix(0, ncol = 3, nrow = 3)
kern[1, 2] <- 0.25
kern[2, c(1, 3)] <- 0.25
kern[3, 2] <- 0.25
kern
```

```r
##      [,1] [,2] [,3]
## [1,] 0.00 0.25 0.00
## [2,] 0.25 0.00 0.25
## [3,] 0.00 0.25 0.00
```

该核变换将每个像素更改为其水平和垂直相邻像素的平均值，从而在下面的右侧图像中产生轻微的模糊效果：

```r
img <- image_resize(logo, "300x300")
img_blurred <- image_convolve(img, kern)
image_append(c(img, img_blurred))
```

![image.gif](https://cdn.nlark.com/yuque/0/2019/gif/126032/1571366903114-5f1667dc-6c23-4df0-a225-1f8ecb8e1101.gif#align=left&display=inline&height=225&originHeight=225&originWidth=600&size=25690&status=done&width=600)

或使用任何的  [standard kernels](http://www.imagemagick.org/Usage/convolve/)：

```r
img %>% image_convolve('Sobel') %>% image_negate()
```

![image.gif](https://cdn.nlark.com/yuque/0/2019/gif/126032/1571366965528-22a3d069-1181-43b1-abca-f52d0e71b629.gif#align=left&display=inline&height=225&originHeight=225&originWidth=300&size=9978&status=done&width=300)

```r
img %>% image_convolve('DoG:0,0,2') %>% image_negate()
```

![image.gif](https://cdn.nlark.com/yuque/0/2019/gif/126032/1571366982841-25a840f5-7fed-4b69-bc5e-bb9b5188a5d2.gif#align=left&display=inline&height=225&originHeight=225&originWidth=300&size=13371&status=done&width=300)

### 文本注释

最后，在图像的最上层打印输出一些文本往往是十分有用的：

```r
# Add some text
image_annotate(frink, "bioinit", size = 70, gravity = "southwest", color = "green")
```

![bioinit.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571369836398-a4b5f616-68a7-4f47-8aec-6b8410e74142.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=67044&status=done&width=220)

```r
# Customize text
image_annotate(frink, "Welcome to bioinit", size = 30, color = "red", boxcolor = "pink",
  degrees = 60, location = "+50+100")
```

![preview.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571369703379-1526b9d3-125c-4954-b7b8-77dfdc6ecdbd.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=66236&status=done&width=220)

```r
# Fonts may require ImageMagick has fontconfig
image_annotate(frink, "Hello world, bioinit!", font = 'Times', size = 30)
```

![he.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571369962344-a7fc94a3-0df1-42c7-ac68-561cd62ebc8a.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=68715&status=done&width=220)

大多数平台上支持的字体，包括  `"sans"`， `"mono"`， `"serif"`， `"Times"`， `"Helvetica"`， `"Trebuchet"`， `"Georgia"`， `"Palatino"` ，以及  `"Comic Sans"` 。

### 与管道结合

每个图像转换功能都会返回一个原始图像的修改后的副本。它不会影响原始图像。

```r
frink <- image_read("https://jeroen.github.io/images/frink.png")
frink2 <- image_scale(frink, "100")
image_info(frink)
```

```r
##   format width height colorspace matte filesize density
## 1    PNG   220    445       sRGB  TRUE    73494   72x72
```

```r
image_info(frink2)
```

```r
##   format width height colorspace matte filesize density
## 1    PNG   100    202       sRGB  TRUE        0   72x72
```

因此，要组合转换，你需要将它们链接起来：

```r
test <- image_rotate(frink, 90)
test <- image_background(test, "blue", flatten = TRUE)
test <- image_border(test, "red", "10x10")
test <- image_annotate(test, "This is how we combine transformations", color = "white", size = 30)
print(test)
```

```r
##   format width height colorspace matte filesize density
## 1    PNG   465    240       sRGB  TRUE        0   72x72
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571403767170-586c2d88-c714-4d82-913b-4b61b8d6a946.png#align=left&display=inline&height=240&originHeight=240&originWidth=465&size=68492&status=done&width=465)

使用 `magrittr` 管道语法，可以使这一过程更具可读性。

```r
image_read("https://jeroen.github.io/images/frink.png") %>%
  image_rotate(270) %>%
  image_background("blue", flatten = TRUE) %>%
  image_border("red", "10x10") %>%
  image_annotate("The same thing with pipes", color = "white", size = 30)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571404067582-7fc5a16c-8f6a-400a-baa9-ce9a41800937.png#align=left&display=inline&height=240&originHeight=240&originWidth=465&size=68052&status=done&width=465)

## 图像向量

上面的示例涉及的是单个图像。但是，Magick 中的所有函数都已经过向量化，以支持图层、合成或动画的处理。

标准的基本方法 `[` ，`[[`，`c()` 和 `length()`  可用于处理图像向量，然后可以将其视为图层或帧。

```r
# Download earth gif and make it a bit smaller for vignette
earth <- image_read("https://jeroen.github.io/images/earth.gif") %>%
  image_scale("200x") %>%
  image_quantize(128)

length(earth)
```

```r
## [1] 44
```

```r
earth
```

![image.gif](https://cdn.nlark.com/yuque/0/2019/gif/126032/1571404482983-57993fcf-e477-444a-8508-b2c088e31f69.gif#align=left&display=inline&height=200&originHeight=200&originWidth=200&size=237428&status=done&width=200)

```r
head(image_info(earth))
```

```r
##   format width height colorspace matte filesize density
## 1    GIF   200    200        RGB FALSE        0   72x72
## 2    GIF   200    200        RGB FALSE        0   72x72
## 3    GIF   200    200        RGB FALSE        0   72x72
## 4    GIF   200    200        RGB FALSE        0   72x72
## 5    GIF   200    200        RGB FALSE        0   72x72
## 6    GIF   200    200        RGB FALSE        0   72x72
```

```r
rev(earth) %>%
  image_flip() %>%
  image_annotate("meanwhile in Australia", size = 20, color = "white")
```

![image.gif](https://cdn.nlark.com/yuque/0/2019/gif/126032/1571404528216-39cef120-0102-45f2-90d7-be2cf56a7099.gif#align=left&display=inline&height=200&originHeight=200&originWidth=200&size=324322&status=done&width=200)

### 图层

我们可以像在 Photoshop 中那样，将图层堆叠在一起：

```r
bigdata <- image_read('https://jeroen.github.io/images/bigdata.jpg')
frink <- image_read("https://jeroen.github.io/images/frink.png")
logo <- image_read("https://jeroen.github.io/images/Rlogo.png")
img <- c(bigdata, logo, frink)
img <- image_scale(img, "300x300")
image_info(img)
```

```r
##   format width height colorspace matte filesize density
## 1   JPEG   300    225       sRGB FALSE        0   72x72
## 2    PNG   300    232       sRGB  TRUE        0   72x72
## 3    PNG   148    300       sRGB  TRUE        0   72x72
```

打印的图案相互镶嵌叠加在一起，从而扩展输出画布，使所有内容都适合：

```r
image_mosaic(img)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571404734099-24501fa4-75e2-4d48-95ea-6a5fde3b9bec.png#align=left&display=inline&height=300&originHeight=300&originWidth=300&size=87282&status=done&width=300)

Flattening  扁平化处理将各个图层合并成一张与第一张图片大小相同的图片：

```r
image_flatten(img)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571405161840-eb8f469a-76b6-412a-ace6-bf2c869bd064.png#align=left&display=inline&height=225&originHeight=225&originWidth=300&size=80467&status=done&width=300)

Flattening  和 mosaic 处理允许指定其他的复合运算符操作([composite operators](https://www.imagemagick.org/Magick++/Enumerations.html#CompositeOperator))：

```r
image_flatten(img, 'Add')
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571405411395-d162a0b9-6814-4ffd-a015-1e6f0c6bce66.png#align=left&display=inline&height=225&originHeight=225&originWidth=300&size=166194&status=done&width=300)

```r
image_flatten(img, 'Modulate')
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571405436852-2308d6e1-9ae8-48bb-86f3-831c5b363fd5.png#align=left&display=inline&height=225&originHeight=225&originWidth=300&size=40095&status=done&width=300)

```r
image_flatten(img, 'Minus')
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571405462410-4143125d-a04c-4721-9052-51d990f6ac30.png#align=left&display=inline&height=225&originHeight=225&originWidth=300&size=36730&status=done&width=300)

### 合并

Appending  追加意味着将框架彼此相邻放置：

```r
image_append(image_scale(img, "x200"))
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571405608571-3ffae337-e9a8-4106-ace7-96e783c5f2b1.png#align=left&display=inline&height=200&originHeight=200&originWidth=625&size=178774&status=done&width=625)

使用 `stack = TRUE`  可以将它们放置在彼此的顶部：

```r
image_append(image_scale(img, "100"), stack = TRUE)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571405677462-b6a743a6-6eae-4aa9-a57b-1cfeb469ffdf.png#align=left&display=inline&height=355&originHeight=355&originWidth=100&size=46278&status=done&width=100)

合成允许在特定位置上组合两个图像：

```r
bigdatafrink <- image_scale(image_rotate(image_background(frink, "none"), 300), "x200")
image_composite(image_scale(bigdata, "x400"), bigdatafrink, offset = "+180+100")
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571405764948-75c0f0fd-8705-4398-97a4-ac569584903a.png#align=left&display=inline&height=400&originHeight=400&originWidth=533&size=431139&status=done&width=533)

### 页面

当读取 PDF 文档时，每个页面都成为向量的一个元素。注意，PDF 是在读取时呈现的，因此需要立即指定密度。

```r
manual <- image_read_pdf('https://cloud.r-project.org/web/packages/magick/magick.pdf', density = 72)
image_info(manual)
```

```r
##    format width height colorspace matte filesize density
## 1     PNG   612    792       sRGB  TRUE        0   72x72
## 2     PNG   612    792       sRGB  TRUE        0   72x72
## 3     PNG   612    792       sRGB  TRUE        0   72x72
## 4     PNG   612    792       sRGB  TRUE        0   72x72
## 5     PNG   612    792       sRGB  TRUE        0   72x72
## 6     PNG   612    792       sRGB  TRUE        0   72x72
## 7     PNG   612    792       sRGB  TRUE        0   72x72
## 8     PNG   612    792       sRGB  TRUE        0   72x72
## 9     PNG   612    792       sRGB  TRUE        0   72x72
## 10    PNG   612    792       sRGB  TRUE        0   72x72
## 11    PNG   612    792       sRGB  TRUE        0   72x72
## 12    PNG   612    792       sRGB  TRUE        0   72x72
## 13    PNG   612    792       sRGB  TRUE        0   72x72
## 14    PNG   612    792       sRGB  TRUE        0   72x72
## 15    PNG   612    792       sRGB  TRUE        0   72x72
## 16    PNG   612    792       sRGB  TRUE        0   72x72
## 17    PNG   612    792       sRGB  TRUE        0   72x72
## 18    PNG   612    792       sRGB  TRUE        0   72x72
## 19    PNG   612    792       sRGB  TRUE        0   72x72
## 20    PNG   612    792       sRGB  TRUE        0   72x72
## 21    PNG   612    792       sRGB  TRUE        0   72x72
## 22    PNG   612    792       sRGB  TRUE        0   72x72
## 23    PNG   612    792       sRGB  TRUE        0   72x72
## 24    PNG   612    792       sRGB  TRUE        0   72x72
## 25    PNG   612    792       sRGB  TRUE        0   72x72
## 26    PNG   612    792       sRGB  TRUE        0   72x72
## 27    PNG   612    792       sRGB  TRUE        0   72x72
## 28    PNG   612    792       sRGB  TRUE        0   72x72
## 29    PNG   612    792       sRGB  TRUE        0   72x72
## 30    PNG   612    792       sRGB  TRUE        0   72x72
## 31    PNG   612    792       sRGB  TRUE        0   72x72
## 32    PNG   612    792       sRGB  TRUE        0   72x72
## 33    PNG   612    792       sRGB  TRUE        0   72x72
## 34    PNG   612    792       sRGB  TRUE        0   72x72
## 35    PNG   612    792       sRGB  TRUE        0   72x72
```

```r
manual[1]
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571406026171-f59acaba-02a2-4a3d-ab75-42f645a8658c.png#align=left&display=inline&height=792&originHeight=792&originWidth=612&size=35697&status=done&width=612)

### 动画

除了将矢量元素视为图层之外，我们还可以将它们制作成动画帧！

```r
image_animate(image_scale(img, "200x200"), fps = 1, dispose = "previous")
```

![image.gif](https://cdn.nlark.com/yuque/0/2019/gif/126032/1571406143098-f13a0d12-6fa9-4267-9a3b-0e177b2c3253.gif#align=left&display=inline&height=150&originHeight=150&originWidth=200&size=47658&status=done&width=200)

变形创建了 `n` 个图像序列，这些序列逐渐地将一个图像变形为另一个图像。这样，它就变成了动画。

```r
newlogo <- image_scale(image_read("https://jeroen.github.io/images/Rlogo.png"), "x150")
oldlogo <- image_scale(image_read("https://developer.r-project.org/Logo/Rlogo-3.png"), "x150")
frames <- image_morph(c(oldlogo, newlogo), frames = 10)
image_animate(frames)
```

![image.gif](https://cdn.nlark.com/yuque/0/2019/gif/126032/1571406281279-46422262-8568-438d-a358-8c786292d385.gif#align=left&display=inline&height=150&originHeight=150&originWidth=197&size=143027&status=done&width=197)

如果您读入现有的 GIF 或视频文件，则每一帧都会变成一个图层：

```r
# Foreground image
banana <- image_read("https://jeroen.github.io/images/banana.gif")
banana <- image_scale(banana, "150")
image_info(banana)
```

```r
##   format width height colorspace matte filesize density
## 1    GIF   150    148       sRGB  TRUE        0   72x72
## 2    GIF   150    148       sRGB  TRUE        0   72x72
## 3    GIF   150    148       sRGB  TRUE        0   72x72
## 4    GIF   150    148       sRGB  TRUE        0   72x72
## 5    GIF   150    148       sRGB  TRUE        0   72x72
## 6    GIF   150    148       sRGB  TRUE        0   72x72
## 7    GIF   150    148       sRGB  TRUE        0   72x72
## 8    GIF   150    148       sRGB  TRUE        0   72x72
```

处理各个帧并将其放回动画中：

```r
# Background image
background <- image_background(image_scale(logo, "200"), "white", flatten = TRUE)

# Combine and flatten frames
frames <- image_composite(background, banana, offset = "+70+30")

# Turn frames into animation
animation <- image_animate(frames, fps = 10)
print(animation)
```

```r
##   format width height colorspace matte filesize density
## 1    gif   200    155       sRGB  TRUE        0   72x72
## 2    gif   200    155       sRGB  TRUE        0   72x72
## 3    gif   200    155       sRGB  TRUE        0   72x72
## 4    gif   200    155       sRGB  TRUE        0   72x72
## 5    gif   200    155       sRGB  TRUE        0   72x72
## 6    gif   200    155       sRGB  TRUE        0   72x72
## 7    gif   200    155       sRGB  TRUE        0   72x72
## 8    gif   200    155       sRGB  TRUE        0   72x72
```

![image.gif](https://cdn.nlark.com/yuque/0/2019/gif/126032/1571406402520-423a1535-5913-4e06-94a0-9e95488ce051.gif#align=left&display=inline&height=155&originHeight=155&originWidth=200&size=89259&status=done&width=200)

动画可以另存为 GIF 格式的  MPEG 文件：

```r
image_write(animation, "Rlogo-banana.gif")
```

## 绘图与图形

该软件包的一个相对较新的组件是一个本地的 R 图形设备，它生成 magick 图像对象。它可以像用于绘制绘图的常规设备一样使用它，也可以打开一个使用像素坐标绘制到现有图像的设备。

### 图形设备

`image_graph()` 函数可打开一个新的图形设备，类似于 `png()` 或 `x11()` 。它返回要写入绘图的图像对象。绘图设备中的每个“page”都将成为图像对象中的一帧。

```r
# Produce image using graphics device
fig <- image_graph(width = 400, height = 400, res = 96)
ggplot2::qplot(mpg, wt, data = mtcars, colour = cyl)
dev.off()
```

我们可以使用常规图像操作轻松地对图形进行后处理。

```r
# Combine
out <- image_composite(fig, frink, offset = "+70+30")
print(out)
```

```r
## # A tibble: 1 x 7
##   format width height colorspace matte filesize density
##   <chr>  <int>  <int> <chr>      <lgl>    <int> <chr>
## 1 PNG      400    400 sRGB       TRUE         0 72x72
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571407000357-cb0feb53-944a-4c8b-a424-f43b90c2a3c9.png#align=left&display=inline&height=400&originHeight=400&originWidth=400&size=66043&status=done&width=400)

### 绘图设备

使用图形设备的另一种方法是使用像素坐标在现有图像的上面绘制。

```r
# Or paint over an existing image
img <- image_draw(frink)
rect(20, 20, 200, 100, border = "red", lty = "dashed", lwd = 5)
abline(h = 300, col = 'blue', lwd = '10', lty = "dotted")
text(30, 250, "Hoiven-Glaven", family = "monospace", cex = 4, srt = 90)
palette(rainbow(11, end = 0.9))
symbols(rep(200, 11), seq(0, 400, 40), circles = runif(11, 5, 35),
  bg = 1:11, inches = FALSE, add = TRUE)
dev.off()
```

```r
print(img)
```

```r
## # A tibble: 1 x 7
##   format width height colorspace matte filesize density
##   <chr>  <int>  <int> <chr>      <lgl>    <int> <chr>
## 1 PNG      220    445 sRGB       TRUE         0 72x72
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571407139092-e128d9ce-ba0c-4f7f-ae2e-e6ad3fb61a7d.png#align=left&display=inline&height=445&originHeight=445&originWidth=220&size=80400&status=done&width=220)

默认情况下，`image_draw()` 将所有空白设置为 0，并使用图形坐标来匹配图像大小，像素(宽度 x 高度)为左上角 (0,0)。注意，这意味着 y 轴从上到下递增，这与典型的图形坐标相反。您可以通过向 `image_draw` 传递定制的 `xlim`、`ylim` 或 `mar` 值来覆盖所有这些。

### 动画图形

图形设备支持多帧，这使它很容易创建动画图形。下面的代码展示了如何使用 magick 图形设备实现来自非常酷的 [gganimate](https://github.com/dgrtwo/gganimate)  包的示例。

```r
library(gapminder)
library(ggplot2)
img <- image_graph(600, 340, res = 96)
datalist <- split(gapminder, gapminder$year)
out <- lapply(datalist, function(data){
  p <- ggplot(data, aes(gdpPercap, lifeExp, size = pop, color = continent)) +
    scale_size("population", limits = range(gapminder$pop)) + geom_point() + ylim(20, 90) +
    scale_x_log10(limits = range(gapminder$gdpPercap)) + ggtitle(data$year) + theme_classic()
  print(p)
})
dev.off()
animation <- image_animate(img, fps = 2)
print(animation)
```

```r
## # A tibble: 12 x 7
##    format width height colorspace matte filesize density
##    <chr>  <int>  <int> <chr>      <lgl>    <int> <chr>
##  1 gif      600    340 sRGB       TRUE         0 72x72
##  2 gif      600    340 sRGB       TRUE         0 72x72
##  3 gif      600    340 sRGB       TRUE         0 72x72
##  4 gif      600    340 sRGB       TRUE         0 72x72
##  5 gif      600    340 sRGB       TRUE         0 72x72
##  6 gif      600    340 sRGB       TRUE         0 72x72
##  7 gif      600    340 sRGB       TRUE         0 72x72
##  8 gif      600    340 sRGB       TRUE         0 72x72
##  9 gif      600    340 sRGB       TRUE         0 72x72
## 10 gif      600    340 sRGB       TRUE         0 72x72
## 11 gif      600    340 sRGB       TRUE         0 72x72
## 12 gif      600    340 sRGB       TRUE         0 72x72
```

![image.gif](https://cdn.nlark.com/yuque/0/2019/gif/126032/1571407398362-7081e08c-758e-4bc4-a0f0-0f19577dfbe4.gif#align=left&display=inline&height=340&originHeight=340&originWidth=600&size=143883&status=done&width=600)

要将其写入文件，您只需执行以下操作：

```r
image_write(animation, "gapminder.gif")
```

## 光栅图像

Magick 图像也可以转换为光栅对象，来用于 R 的图形设备。因此，我们可以将它与其他图形工具相结合。然而，请注意，R 的图形设备非常慢，且它有一个非常不同的坐标系，它会降低图像的质量。

### 基础格栅

Base R 有一个 as.raster 格式，它可将图像转换为字符串向量。Paul Murrell  在  [Raster Images in R Graphics](https://journal.r-project.org/archive/2011-1/RJournal_2011-1_Murrell.pdf)  一文中给出了一个很好的概述。

```r
plot(as.raster(frink))
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571407922083-b20af564-e572-4625-aed6-7548f4d40f34.png#align=left&display=inline&height=960&originHeight=960&originWidth=1344&size=158352&status=done&width=1344)

```r
# Print over another graphic
plot(cars)
rasterImage(frink, 21, 0, 25, 80)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571407956028-b089c903-ed99-473c-9f5b-2e651392646a.png#align=left&display=inline&height=960&originHeight=960&originWidth=1344&size=140624&status=done&width=1344)

### grid 包

`grid` 包使您更容易在图形设备上叠加栅格，而不必调整绘图的 x/y 坐标。

```r
library(ggplot2)
library(grid)
qplot(speed, dist, data = cars, geom = c("point", "smooth"))
```

```r
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

```r
grid.raster(frink)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571408144100-826b55ec-e9d4-4c39-836f-0da5fef706d7.png#align=left&display=inline&height=576&originHeight=576&originWidth=960&size=163843&status=done&width=960)

### raster 包

raster 包具有自己的 bitmaps  类，这些类对于空间应用程序很有用。将图像转换为栅格的最简单方法是将其导出为 `tiff` 文件：

```r
tiff_file <- tempfile()
image_write(frink, path = tiff_file, format = 'tiff')
r <- raster::brick(tiff_file)
raster::plotRGB(r)
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571408309237-4dc17391-efbe-4329-ab0e-8c5ea2decf2f.png#align=left&display=inline&height=384&originHeight=384&originWidth=1344&size=73075&status=done&width=1344)

你还可以手动将位图数组(bitmap array )转换为栅格对象，但这似乎会删除一些元数据：

```r
buf <- as.integer(frink[[1]])
rr <- raster::brick(buf)
raster::plotRGB(rr, asp = 1)
```

## OCR 文本提取

`magick`  最新的功能是利用 OCR 技术从图片中提取文本。这需要 `tesseract` 包。

```r
install.packages("tesseract")
```

```r
img <- image_read("http://jeroen.github.io/images/testocr.png")
print(img)
```

```r
## # A tibble: 1 x 7
##   format width height colorspace matte filesize density
##   <chr>  <int>  <int> <chr>      <lgl>    <int> <chr>
## 1 PNG      640    480 sRGB       TRUE     23359 72x72
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1571449892209-b8ce6f94-af33-4fc8-b59a-a0d0da8ae277.png#align=left&display=inline&height=480&originHeight=480&originWidth=640&size=5367&status=done&width=640)

```r
# Extract text
cat(image_ocr(img))
```

```r
## This is a lot of 12 point text to test the
## ocr code and see if it works on all types
## of file format.
##
## The quick brown dog jumped over the
## lazy fox. The quick brown dog jumped
## over the lazy fox. The quick brown dog
## jumped over the lazy fox. The quick
## brown dog jumped over the lazy fox.
```

## 总结

本文章篇幅很长，对 magick 包的各种使用非常直观详细。作为与 “ImageMagick” 绑定，可用的最全面的开源图像处理库，magick 包支持许多常见格式（png，jpeg，tiff，pdf 等）和操作（旋转，缩放，修剪，修剪，翻转，模糊等）。这些所有操作都是通过 Magick ++ STL 矢量化的，这意味着它们可以在单个帧或一系列帧上进行操作，以处理图层，拼贴或动画。

在 RStudio 中，可以将图像打印到控制台后会自动预览，从而形成一个交互式编辑环境。因此，强烈推荐大家在 RStudio 中进行使用和测试。

更多  magick、ImageMagick 相关的 R 语言高级图像处理操作，有兴趣的童鞋可以自行去研究学习，也欢迎大家留言交流。

> **参考资料：**
>
> [https://cran.r-project.org/web/packages/magick/vignettes/intro.html](https://cran.r-project.org/web/packages/magick/vignettes/intro.html)
