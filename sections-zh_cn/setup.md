[:arrow_backward:](gamma-correction.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](building-the-demo.md)

# 3D 游戏着色器入门教程

## 环境搭建

<p align="center">
<img src="../resources/images/fYpIWNk.gif" alt="环境搭建" title="环境搭建">
</p>

以下是用于开发和测试示例代码的环境配置。

### 开发环境

示例代码是在以下环境中开发和测试的：

- Linux manjaro 5.10.42-1-MANJARO
- OpenGL 渲染器字符串：GeForce GTX 970/PCIe/SSE2
- OpenGL 版本字符串：4.6.0 NVIDIA 465.31
- g++ (GCC) 11.1.0
- Panda3D 1.10.9

### 材质

每个用于构建 `mill-scene.egg` 的 [Blender](https://blender.org) 材质都有如下五种纹理，顺序如下：

- Diffuse（漫反射）
- Normal（法线）
- Specular（镜面高光）
- Reflection（反射）
- Refraction（折射）

所有模型都在相同位置使用相同类型的贴图，这样就可以使着色器通用化，从而减少重复编写代码的需求。

如果某个物体使用的是其自身的顶点法线，则会使用一张“纯蓝色”的法线贴图。

<p align="center">
<img src="../resources/images/tFmKgoH.png" alt="平面法线贴图" title="平面法线贴图">
</p>

上图是一个平面法线贴图的示例。
它只包含一种颜色——纯蓝色 `(red = 128, green = 128, blue = 255)`。
这种颜色表示的是一个单位法线，方向为正 z 轴 `(0, 0, 1)`。

```c
(0, 0, 1) =
( round((0 * 0.5 + 0.5) * 255)
, round((0 * 0.5 + 0.5) * 255)
, round((1 * 0.5 + 0.5) * 255)
) =
(128, 128, 255) =
( round(128 / 255 * 2 - 1)
, round(128 / 255 * 2 - 1)
, round(255 / 255 * 2 - 1)
) =
(0, 0, 1)
```

这里你可以看到单位法线 `(0, 0, 1)` 是如何转换为纯蓝色 `(128, 128, 255)` 的，
以及纯蓝色又是如何还原回单位法线的。
你将在 [法线贴图](normal-mapping.md) 技术中深入学习这一过程。

<p align="center">
<img src="../resources/images/R9FgZKx.png" alt="高光贴图" title="高光贴图">
</p>

上图是其中一张使用的高光贴图。
红色和蓝色通道会根据相机角度控制镜面反射的强度。
绿色通道控制镜面的光滑度（高光锐利度）。
你将在 [光照](lighting.md) 和 [菲涅尔因子](fresnel-factor.md) 章节中进一步了解这部分内容。

反射和折射贴图则用于遮罩出物体是否具备反射性、折射性或两者兼有。
对于反射贴图，红色通道控制反射的强度，绿色通道控制反射的清晰度或模糊度。

### Panda3D

示例代码使用了 [Panda3D](https://www.panda3d.org/) 作为连接着色器之间的“胶水”。
这对本文所介绍的技术本身没有直接影响，
因此你可以将这里学到的知识应用到你所使用的任意引擎或技术栈中。

Panda3D 确实提供了一些便利功能。
我会指出这些部分，
以便你能在自己的技术栈中找到等效功能，
或者在没有的情况下自行实现。

为了演示程序，Panda3D 的三个配置项被修改。
它们位于 [config.prc](../demonstration/config.prc) 文件中。
被修改的配置项如下：

- `gl-coordinate-system default`
- `textures-power-2 down`
- `textures-auto-power-2 1`

关于详细说明，你可以参考 Panda3D 手册中的 [配置说明页面](http://www.panda3d.org/manual/?title=Configuring_Panda3D)。

Panda3D 默认使用 z 轴朝上的右手坐标系，而 OpenGL 使用的是 y 轴朝上的右手坐标系。
`gl-coordinate-system default` 设置可以避免你在着色器中手动进行坐标系转换。

`textures-auto-power-2 1` 表示在系统支持的情况下允许使用非 2 的幂次方大小的纹理。
这对于 SSAO 以及其他需要依赖屏幕/窗口尺寸的技术非常实用，
因为实际屏幕尺寸往往不是 2 的幂次方。

`textures-power-2 down` 会在系统仅支持 2 的幂次方纹理尺寸时，自动将纹理尺寸向下缩放为最近的 2 的幂。

## 版权声明

(C) 2019 David Lettier
<br>
[lettier.com](https://www.lettier.com)

[:arrow_backward:](gamma-correction.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#版权声明)
[:arrow_forward:](building-the-demo.md)
