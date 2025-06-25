[:arrow_backward:](pixelization.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](dilation.md)

# 3D游戏着色器入门

## 锐化效果（Sharpen）

<p align="center">
<img src="../resources/images/VFDKNvl.gif" alt="锐化效果" title="锐化效果">
</p>

锐化效果会增强图像边缘的对比度，  
当你的图像看起来有点模糊时，这个效果非常有用。

```c
  // ...

  float amount = 0.8;

  // ...
```

你可以通过调整 `amount` 来控制锐化的强度。  
`amount` 为零时图像保持不变。  
你也可以尝试负值，得到奇特的视觉效果。

```c
  // ...

  float neighbor = amount * -1;
  float center   = amount * 4 + 1;

  // ...
```

邻近像素的权重是 `amount * -1`，  
当前像素的权重是 `amount * 4 + 1`。

```c
  // ...

  vec3 color =
        texture(sharpenTexture, vec2(gl_FragCoord.x + 0, gl_FragCoord.y + 1) / texSize).rgb
      * neighbor

      + texture(sharpenTexture, vec2(gl_FragCoord.x - 1, gl_FragCoord.y + 0) / texSize).rgb
      * neighbor
      + texture(sharpenTexture, vec2(gl_FragCoord.x + 0, gl_FragCoord.y + 0) / texSize).rgb
      * center
      + texture(sharpenTexture, vec2(gl_FragCoord.x + 1, gl_FragCoord.y + 0) / texSize).rgb
      * neighbor

      + texture(sharpenTexture, vec2(gl_FragCoord.x + 0, gl_FragCoord.y - 1) / texSize).rgb
      * neighbor
      ;

  // ...
```

邻近的像素是当前像素的上下左右四个方向。  
对邻近像素和当前像素分别乘以它们的权重后，求和得到最终颜色。

```c
    // ...

    fragColor = vec4(color, texture(sharpenTexture, texCoord).a);

    // ...
```

这个和即为最终的片元颜色。

### 源码参考

- [main.cxx](../demonstration/src/main.cxx)
- [basic.vert](../demonstration/shaders/vertex/basic.vert)
- [sharpen.frag](../demonstration/shaders/fragment/sharpen.frag)


## Copyright

(C) 2019 David Lettier
<br>
[lettier.com](https://www.lettier.com)

[:arrow_backward:](pixelization.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](dilation.md)
