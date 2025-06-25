[:arrow_backward:](deferred-rendering.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](blur.md)

# 3D 游戏着色器入门

## 雾（Fog）

<p align="center">
<img src="../resources/images/uNRxZl4.gif" alt="Fog" title="Fog">
</p>

雾（在 Blender 中被称为雾气）为场景增加了大气朦胧感，  
可以营造神秘氛围，同时柔化物体突然出现在相机视野中的突兀感（pop-in）。

```c
// ...

uniform vec4 color;

uniform vec2 nearFar;

// ...
```

要计算雾效，你需要设置雾的颜色、近距离（near）和远距离（far）。

```c
// ...

uniform sampler2D positionTexture;

  // ...

  vec2 texSize  = textureSize(positionTexture, 0).xy;
  vec2 texCoord = gl_FragCoord.xy / texSize;

  vec4 position = texture(positionTexture, texCoord);

  // ...
```

除了雾的属性，还需要当前片元的顶点位置 `position`。

```c
  float fogMin = 0.00;
  float fogMax = 0.97;
```

`fogMax` 控制当雾最浓时场景还能看到多少。  
`fogMin` 控制当雾最淡时雾本身还能看到多少。

```c
  // ...

  float near = nearFar.x;
  float far  = nearFar.y;

  float intensity =
    clamp
      (   (position.y - near)
        / (far        - near)
      , fogMin
      , fogMax
      );

  // ...
```

示例代码使用了线性模型来计算雾的强度。  
你也可以使用指数模型实现更自然的衰减效果。

当顶点距离小于或等于雾的近距 `near` 时，雾强度为 `fogMin`。  
随着顶点接近远距 `far`，雾强度线性增加，趋近于 `fogMax`。  
超过 `far` 的部分强度会被限制为 `fogMax`。

```c
  // ...

  fragColor = vec4(color.rgb, intensity);

  // ...
```

设置片元的颜色为雾的颜色，Alpha 通道为雾的强度。  
`intensity` 越接近 1，越多雾色、越少场景原色。  
当 `intensity` 为 1 时，只显示雾，完全遮挡原场景。

```c
// ...

uniform sampler2D baseTexture;
uniform sampler2D fogTexture;

  // ...

  vec2 texSize  = textureSize(baseTexture, 0).xy;
  vec2 texCoord = gl_FragCoord.xy / texSize;

  vec4 baseColor = texture(baseTexture, texCoord);
  vec4 fogColor  = texture(fogTexture,  texCoord);

  fragColor = baseColor;

  // ...

  fragColor = mix(fragColor, fogColor, min(fogColor.a, 1));

  // ...
```

通常你会在计算光照的 shader 中一并处理雾效。  
但你也可以把雾效拆分出来，单独输出到一个 framebuffer 纹理中。  
就像在 GIMP 里添加图层一样，这里用雾纹理叠加原场景，节省了在每个 shader 中重复计算的成本。

### 源码参考

- [main.cxx](../demonstration/src/main.cxx)
- [basic.vert](../demonstration/shaders/vertex/basic.vert)
- [position.frag](../demonstration/shaders/fragment/position.frag)
- [normal.frag](../demonstration/shaders/fragment/normal.frag)
- [fog.frag](../demonstration/shaders/fragment/fog.frag)
- [outline.frag](../demonstration/shaders/fragment/outline.frag)
- [scene-combine.frag](../demonstration/shaders/fragment/scene-combine.frag)


## Copyright

(C) 2019 David Lettier
<br>
[lettier.com](https://www.lettier.com)

[:arrow_backward:](deferred-rendering.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](blur.md)
