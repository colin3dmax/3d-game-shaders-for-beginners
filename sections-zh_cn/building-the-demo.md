[:arrow_backward:](setup.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](running-the-demo.md)

# 3D 游戏着色器入门教程

## 构建演示程序

<p align="center">
<img src="../resources/images/PQcDnIu.gif" alt="构建演示程序" title="构建演示程序">
</p>

在你尝试运行演示程序之前，需要先编译示例代码。

### 依赖项

在你编译示例代码之前，需要先为你的平台安装  
[Panda3D](https://www.panda3d.org/)。  
Panda3D 支持 Linux、Mac 和 Windows 平台。

### Linux

首先，为你的发行版[安装](https://www.panda3d.org/manual/?title=Installing_Panda3D_in_Linux)  
[Panda3D SDK](https://www.panda3d.org/download/sdk-1-10-9/)。

确保找到 Panda3D 的头文件和库文件路径。  
头文件和库文件通常分别位于 `/usr/include/panda3d/` 和 `/usr/lib/panda3d/`。

接下来，克隆该代码仓库并切换到示例目录下：

```bash
git clone https://github.com/lettier/3d-game-shaders-for-beginners.git
cd 3d-game-shaders-for-beginners/demonstration
```

现在将源代码编译为目标文件：

```bash
g++ \
  -c src/main.cxx \
  -o 3d-game-shaders-for-beginners.o \
  -std=gnu++11 \
  -O2 \
  -I/path/to/python/include/ \
  -I/path/to/panda3d/include/
```

完成目标文件编译后，通过链接依赖项生成可执行文件：

```bash
g++ \
  3d-game-shaders-for-beginners.o \
  -o 3d-game-shaders-for-beginners \
  -L/path/to/panda3d/lib \
  -lp3framework \
  -lpanda \
  -lpandafx \
  -lpandaexpress \
  -lpandaphysics \
  -lp3dtoolconfig \
  -lp3dtool \
  -lpthread
```

有关更多帮助，请参阅 [Panda3D 手册](https://www.panda3d.org/manual/?title=How_to_compile_a_C++_Panda3D_program_on_Linux)。


### Mac

首先为 Mac 安装 [Panda3D SDK](https://www.panda3d.org/download/sdk-1-10-9/)。

确保找到 Panda3D 的头文件和库文件路径。

接下来，克隆代码仓库并切换目录：

```bash
git clone https://github.com/lettier/3d-game-shaders-for-beginners.git
cd 3d-game-shaders-for-beginners
```

然后将源代码编译为目标文件。
你需要找到 Python 2.7 和 Panda3D 的头文件路径：

```bash
clang++ \
  -c main.cxx \
  -o 3d-game-shaders-for-beginners.o \
  -std=gnu++11 \
  -g \
  -O2 \
  -I/path/to/python/include/ \
  -I/path/to/panda3d/include/
```

创建目标文件后，通过链接依赖项生成可执行文件。
你需要定位 Panda3D 的库文件位置：

```bash
clang++ \
  3d-game-shaders-for-beginners.o \
  -o 3d-game-shaders-for-beginners \
  -L/path/to/panda3d/lib \
  -lp3framework \
  -lpanda \
  -lpandafx \
  -lpandaexpress \
  -lpandaphysics \
  -lp3dtoolconfig \
  -lp3dtool \
  -lpthread
```

有关更多帮助，请参阅 [Panda3D 手册](https://www.panda3d.org/manual/?title=How_to_compile_a_C++_Panda3D_program_on_macOS)。

### Windows

首先为 [Windows 安装](https://www.panda3d.org/manual/?title=Installing_Panda3D_in_Windows)
[Panda3D SDK](https://www.panda3d.org/download/sdk-1-10-9/)。

确保找到 Panda3D 的头文件和库文件路径。

接下来，克隆该代码仓库并切换目录：

```bash
git clone https://github.com/lettier/3d-game-shaders-for-beginners.git
cd 3d-game-shaders-for-beginners
```

有关更多帮助，请参阅 [Panda3D 手册](https://www.panda3d.org/manual/?title=Running_your_Program&language=cxx)。

## Copyright

(C) 2019 David Lettier
<br>
[lettier.com](https://www.lettier.com)

[:arrow_backward:](setup.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](running-the-demo.md)
