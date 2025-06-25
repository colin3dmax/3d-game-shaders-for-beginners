[:arrow_backward:](foam.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](outlining.md)

# 3D 游戏着色器入门教程

## 流动贴图（Flow Mapping）

<p align="center">
<img src="../resources/images/3WDO9xW.gif" alt="Flow Mapping" title="Flow Mapping">
</p>

流动贴图在你需要动画化液体材质时非常有用。  
就像漫反射贴图将 UV 坐标映射到漫反射颜色，法线贴图将 UV 坐标映射到法线方向，  
流动贴图将 UV 坐标映射到二维的偏移量（即流动方向）。

<p align="center">
<img src="../resources/images/b9Vw94N.png" alt="Flow Map" title="Flow Map">
</p>

上图是一个将 UV 坐标映射到正 Y 方向偏移的流动贴图。  
流动贴图使用红色和绿色通道分别存储 X 和 Y 方向的偏移值：  
- 红色通道表示 X 方向流动  
- 绿色通道表示 Y 方向流动  

这两个通道的值范围都是 `[0, 1]`，在经过换算后会映射到 `(-1, 1)` 的偏移范围。  
上图中该贴图是单一颜色，R 为 0.5，G 为 0.6。

```c
[r, g, b] =
  [r * 2 - 1, g * 2 - 1, b * 2 - 1] =
    [ x, y, z]
```

回顾法线贴图中的颜色到法线的转换方式，  
流动贴图采用类似的转换逻辑。

```c
// ...

uniform sampler2D flowTexture;

  vec2 flow = texture(flowTexture, uv).xy;
       flow = (flow - 0.5) * 2;

  // ...
```

要将流动贴图的颜色值转换为实际的流动向量，  
只需将颜色通道值减去 0.5 并乘以 2。

```c
(r, g) =
 ( (r - 0.5) * 2
 , (g - 0.5) * 2
 ) =
  ( (0.5 - 0.5) * 2
  , (0.6 - 0.5) * 2
  ) =
    (x, y) =
      (0, 0.2)
```

上面的流动贴图会将每个 UV 坐标映射为 `(0, 0.2)` 的偏移，  
表示 X 方向无移动，Y 方向每单位时间偏移 0.2。

流动值通常用于偏移其他纹理的 UV 坐标，来模拟流动效果。

<p align="center">
<img src="../resources/images/N6TWBw8.gif" alt="Foam Mask" title="Foam Mask">
</p>

```c
  // ...

  vec2 flow = texture(flowTexture, diffuseCoord).xy;
       flow = (flow - 0.5) * 2;

  vec4 foamPattern =
    texture
      ( foamPatternTexture
      , vec2
          ( diffuseCoord.x + flow.x * osg_FrameTime
          , diffuseCoord.y + flow.y * osg_FrameTime
          )
      );

  // ...
```

例如，示例程序使用流动贴图为水面添加动画。  
在上图中，流动贴图被用于驱动 [泡沫遮罩](foam.md#mask) 的移动。  
它会持续向上偏移 UV 坐标，从而让泡沫看起来像是顺流而下。

```c
          // ...

          ( diffuseCoord.x + flow.x * osg_FrameTime
          , diffuseCoord.y + flow.y * osg_FrameTime

          // ...
```

为了根据流动方向动画化 UV 坐标，你需要知道自第一帧以来经过了多少秒。  
`osg_FrameTime` 是由 Panda3D 提供的变量，  
[参考链接](https://github.com/panda3d/panda3d/blob/daa57733cb9b4ccdb23e28153585e8e20b5ccdb5/panda/src/display/graphicsStateGuardian.cxx#L930)，  
它表示从第一帧开始过去的时间戳。

### 源码参考

- [main.cxx](../demonstration/src/main.cxx)
- [base.vert](../demonstration/shaders/vertex/base.vert)
- [basic.vert](../demonstration/shaders/vertex/basic.vert)
- [normal.frag](../demonstration/shaders/fragment/normal.frag)

## Copyright

(C) 2019 David Lettier
<br>
[lettier.com](https://www.lettier.com)

[:arrow_backward:](foam.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](outlining.md)
