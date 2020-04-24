# 超算平台编译RMC
在普通Linux平台编译的RMC放在超算上不一定能正常运行，需要在超算平台上直接编译RMC用于超算平台计算。核动力院天河上采用的是cmake编译，关于cmake的详细讲解，可以见[这个博客](https://blog.csdn.net/zhuiyunzhugang/article/details/88142908)。以下无脑讲解编译操作。
1.打开核动力院天河，进入
    ```/public1/home/sc30291/mayugao/RMC_ANSYS_CMake```
2. 复制其下的3D_Han，粘贴为新的文件夹如3D_Test，以供自己调试
3.进入文件夹3D_Test，进入./RMC，可见如下图所示
    ![](2020-02-08-17-51-25.png)
4.其中src中存放需要的cpp文件和h文件，需要更新可以上传替换或者编辑；build中存放编译文件
5.为了演示，先进入build文件夹，将build清空，```rm -r *```
6.保持在build文件夹中，```cmake ..```，可以看见编译已经准备好了
7.保持在build文件夹中，```make -j 40```，利用40个核并行编译
8.等待编译完成，build中出现可执行的RMC，将此RMC复制到需要的目录下就可