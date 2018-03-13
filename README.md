# 延迟着色法

OpenGL 延迟着色法优化拥有大量光源的场景

## 依赖
* glfw3.lib 推荐在[官方网站](http://www.glfw.org/download.html)下载源代码，然后自行编译。本项目编译使用的是CMake和Visual Studio 2015.
* GLAD 打开GLAD的[在线服务](http://glad.dav1d.de/)可轻松配置。本项目使用OpenGL 4.3.
* stb_image.h 是[Sean Barrett](https://github.com/nothings)的一个非常流行的单头文件图像加载库，可以在[这里](https://github.com/nothings/stb/blob/master/stb_image.h)下载。本项目使用其来加载纹理图片。
* GLM 一个只有头文件的库，不用链接和编译。可以在它们的[网站](http://glm.g-truc.net/0.9.5/index.html)上下载。本项目使用其作为数学库。
* Assimp 一个非常流行的模型导入库，可以在[下载页面](http://assimp.org/main_downloads.html)选择相应的版本，自行使用CMake 和 Visual Studio 2015编译。

## 思想

**延迟（Defer）** 或 **推迟（Postpone）** 大部分计算量非常大的渲染（像是 **光照** ）到后期进行处理。

2个阶段：

	1. 先渲染场景一次，获得几何信息，存在 **G缓冲** 的纹理中
	2. 光照处理阶段，使用G缓冲内的纹理数据

## G缓冲

G缓冲（G-buffer）是对所有用来存储光照相关的数据，并在最后的光照处理阶段中使用的所有纹理的总称。
在 **正向渲染（Forward Rendering）** 中照亮一个片段所需要的所有数据：

* 一个3D **位置** 向量来计算（插值）片段位置变量供lightDir和viewDir使用
* 一个RGB漫反射 **颜色向量**，也就是 **反照率（Albedo）**
* 一个3D **法向量** 来判断平面的斜率
* 一个镜面强度（Specular Intensity）浮点值
* 所有光源的位置和颜色向量
* 玩家或观察者的位置向量

在一个或多个屏幕大小的纹理中存储所有逐片段数据并在之后光照处理阶段中使用。

对于每一个片段需要存储的数据有：一个位置向量、一个法向量、一个颜色向量，一个镜面值强度，可使用 **多渲染目标（Multiple Render Targets）** 。

## 效果

![延迟着色法](https://github.com/SweeneyChoi/DeferredShading/blob/master/image/deferredshading.png)
