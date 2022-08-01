# Manim学习笔记

Manim是一款数学动画制作引擎，是Youtobe频道3Blue1Brown的作者，斯坦福大学的数学系学生Grant Sanderson创建，用于解说高等数学。2020年，一批开发爱好者fork了Grant的仓库，改进公布了现在的manim社区版

- manim github: https://github.com/3b1b/manim 
- manim community github: https://github.com/ManimCommunity/manim/

现在的manim主分支也进行了升级为manimgl，即利用OpenGL进行渲染，使得渲染视频的速度更快。



B站学习视频：https://www.bilibili.com/video/BV1W4411Z7Zt?spm_id_from=333.337.search-card.all.click&vd_source=e4ae5ce198ba62d55816864598092fc4



其他一些参考教程：

https://zhuanlan.zhihu.com/p/378999796



注意，由于版本有很多，不同版本以及新旧版之间的使用都有所差距，所以一定要注意教程使用的版本是什么，有些比较老的教程现在已经不太适用了。



## 安装

需要先安装anaconda、latex、ffmpeg

anaconda和latex的安装比较简单，ffmpeg需要注意，一定要完整版的，不能是某些大型软件中自带的阉割版本。可以通过```ffmpeg -version``` 查看版本信息，应该会列出8个部件，如果太少就是阉割版的。可以通过调整path中的顺序，将完整版的地址放在前面，就会默认调用完整版的软件。

一切准备就绪后

```python
pip install manimgl
```

即可安装。

其下载的库文件在Anaconda对应的site-packages下的manimlib中。

这里就不介绍如何用github的源代码直接安装了。

## 测试

自己在喜欢的地方创建一个manim_workdir，再搞一个helloword文件夹，（以上随意哈），创建python脚本：

```python
#helloworld.py
from manimlib import *

class SquareToCircle(Scene):
    def construct(self):
        circle = Circle()
        circle.set_fill(BLUE, opacity=0.5)
        circle.set_stroke(BLUE_E, width=4)

        square = Square()
        square.set_stroke(BLACK, width=4)
        self.play(ShowCreation(square))
        self.wait()
        self.play(ReplacementTransform(square, circle))
        self.wait()
```

当前工作目录打开命令行，进入anaconda 的虚拟环境，例如base，随后输入：

```bash
manimgl helloworld.py SquareToCircle
```

即可实时预览渲染动画。

## 一些小问题

- 如果电脑上有多个anaconda版本，注意anaconda要保持一致，在哪个anaconda版本下安装的就要用哪个版本的运行，虚拟环境也要保持一致

- GUI窗口位置，可以通过修改manimlib下的default_config/yml文件中的window_position实现，默认是右上角UR，可改为OO，便会居中。这是全局修改，以后每个project都是居中。但也可以单独的project修改，就在该py文件目录下创建custom_config.yml，内容为：

  ```bash
  window_position : OO
  ```

  但是我觉得右上角挺好的，因为中途中断运行GUI时，他不会置顶，每次都要跑到背景后面去，很阻碍视线。

- 设置临时文件夹。在default_config.yml里面，有个temporary_storage，默认是空，这样每次运行会有warning，可以在某个地方建立一个文件夹存放临时文件，并将该地址索引config文件中。

## 案例学习

### SquareToCircle

```python
from manimlib import *

class SquareToCircle(Scene):
    def construct(self):
        circle = Circle()
        circle.set_fill(BLUE, opacity=0.5)
        circle.set_stroke(BLUE_E, width=4)

        square = Square()
        square.set_stroke(BLACK, width=4)
        self.play(ShowCreation(square))
        self.wait()
        self.play(ReplacementTransform(square, circle))
        self.wait()
```

一个py文件中可以有多个class，一个class对应一个渲染的视频序列，而construct定义视频有什么基本对象，动作是什么。

注意：变量名、函数名、宏名的大小写不可混淆，并非大小写不区分。

```python
circle = Circle()
circle.set_fiil(BLUE, opacity = 0.5)
circle.set_stroke(BLUE_E, width = 4)
```

创建了一个圆形对象，并设置了圆形填充颜色和线条属性，填充蓝色，透明度0.5,线条深蓝色，宽度4。但此时并没有进行演示，还没有显示出来。

```python
square = Square()
square.set_stroke(BLACK, width = 4)
```

接着又创建了一个正方形，线条为黑色， 宽度4.

创建的基本对象已经完成了。现在需要定义这些对象是如何摆放、如何进场离场的。

```python
self.play(ShowCreation(square))
self.wait()
```

展示正方形的生成过程，为默认的锚点逆时针绘制。随后等待1s，wait默认的参数是1s，你也可以修改。

```python
self.play(ReplacementTransform(square,circle))
self.wait()
```

播放将square转换到circle的动画的过渡动画，随后又暂停1s。

没有语句后，最终动画停止，会进入交互模式，可以用鼠标结合键盘进行视角变换：

> - 鼠标滚轮，图像上下平移
> - z+鼠标滚轮，图像放缩
> - s+鼠标移动，图像跟随鼠标移动
> - d+鼠标移动，改变图像三维视角
> - r，视角复位
> - q 或者ESC 退出程序

注意：

- 运行时输入```manimgl start.py SquareToCircle```，其中后面的场景类如果不写或者写错了，会弹出让你选择（如果py文件中有多个场景的话），如果只有一个则会直接运行这个
- 不知道为什么一旦运行起来，输入法就会自动切换到中文，导致之后输入的时候是中文输入法，因此尽量先调成全英文模式的微软键盘

前面只是预览播放，若要渲染导出mp4视频，需要加参数-w：

```python
manimgl start.py SquareToCircle -w
```

此时没有预览，会直接调用ffmpeg渲染视频。

若要导出gif，可以再加-i，

```python
manimgl start.py SquareToCircle -w -i
```

如果要修改视频背景颜色，可以加参数-c后加颜色

```python
manimgl start.py SquareToCircle -c white
```

除了前面的直接代码一遍过模式，还有ipython交互模式，就是实时编程渲染

```python
self.embed()
```

在脚本中加入这句话，那么便进入交互模式，进入ipython输入，可以输入一句运行一次，便于调试和观察单句运行情况。

在ipython模式下，可以不加```self```，可以直接用命令，例如```self.play```，可以写成```play```

但是注意，此时不要去碰GUI，一碰就容易让GUI无响应卡死。若要进入GUI的交互，需要在ipython中输入

```python
touch()
```

才能用之前的鼠标或者键盘改变视图等等，要退出这个GUI交互模式，q即可，即可回复到ipython模式而不会退出。

要退出ipython模式，需要在ipython中输入

```python
exit()
```

------

为了让所有的命令都用py脚本来实现，不用每次输入命令，可以在py中加入main函数

```pyhon
from os import system
...
if __name__ == "__main__":
	system("manimgl start.py SquareToCircle")
```

此处要注意文件名要对上，如果文件名有时要改，又不想修改py里面的代码，可以用预编译宏：

```python
system("manimgl {} SquareToCircle".format(__file__))
```

或者

```python
system(f"manimgl {__file__} SquareToCircle")
```

