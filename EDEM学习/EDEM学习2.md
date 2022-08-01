# EDEM学习

[TOC]

## 基本使用

- 离散单元法
- 单位设置



## 案例

### 1. 传送带输送

- 颗粒材料，物性，接触参数
  - 颗粒
    - 可从Option中particle display导入颗粒模板，如.stl文件，导入选择单位，还有修复，以及method是**绘制网格**。导入颗粒模板后可以使用，然后进行填充。
    - 颗粒模板相当于是个边界。多少个球填充？——进行球面数量敏感度分析。计算量增大。
    - highlight sphere可以看到是哪个球面当前设置的
    - 右键可以add sphere
    - 粒径分布，userdefined，添加不同scale及其比例；还可以Import导入execl或者csv；还可以直接粘贴表格
    - 计算性质，可基于球计算，或者手动输入**（多次点击计算为什么物性会变化，物性也会变化？）**；导入根据模板计算
- Equipment 材料
  - 材料，接触参数
- 几何
  - 导入几何
    - merge section会把所有的part合并
    - 导入后选中一个右键有Merge geometry，将一体的合并
  - 颗粒生成
    - 可以用api生成
    - 添加颗粒工厂,factory，模拟生成过程
      - 真实工况并没有，虚拟
      - polygon，调整形状和位置
      - 右键添加factory，注意一定之前要设置虚拟
      - 每种颗粒对应不同的颗粒工厂
      - 颗粒初始速度
    - 运动设置，add kinematic
      - 直线输运，不同运动阶段设置不同的kinematic，启动阶段，匀速阶段，运动方向两点矢量，参数cad建模时确定；也可以通过pick选点，但是可能误差
- 物理，接触模型
  - 基础模型，不同颗粒的作用模型
  - 滚动摩擦模型
  - 2018有些不一样
- 环境
  - 计算域
  - 重力
- 求解
  - 时间步，瑞利时间步，不推荐auto time step，手动设置
  - 数据保存时间间隔
  - 模拟网格，cell size，网格是用于接触检索的。大小对于精度没什么影响，但是影响占用内存情况，计算速度情况
  - 计算核数
  - 开始求解，开启自动更新，显示粒子，设置透明度等
- 后处理
  - 对几何和粒子进行后处理
  - 隐藏某些部件，每个part可以设置透明度
  - EDEM画的网格都是三角形网格
  - setup selections
    - grid bin 统计
    - 图表
### 2.螺旋输送机
- 保存材料至材料库，transfer material，还可以保存接触参数
- 颗粒工厂的dynamic和static两种，通过右键transfer factory type，static一次性生成，可以fill section，但是其实填充率只有30%多。推荐dynamic生成
- 设置初始速度是可以提高工厂生成效率
- 计算设置时，保存文件设置可以selected save，保存用户想保存的
- 录制动画，中途可暂停，改变视角
- 轨迹查看，selection，手动选择某颗粒，然后取消所有颗粒的视图，显示particle selection中的颗粒，以显示stream，所有步

### 3. SAG Mill磨粉机
- GEMM数据库，根据经验总结的，平衡计算时间规模等提供参数。粒子大小，堆积密度，堆积角
- JKR表面能，含湿需要
- 设置gemm后自动添加颗粒材料和设备材料
- 粒径分布中cap功能，取某个分布区间
- 颗粒工厂可以建体，如圆柱体
- 计算周期性边界，计算域贴合几何，在某方向周期性，从此出去，另一面出来，减少计算量
- 接触模型中可加磨损模型，wear
- selection可以用import selection bin自定义bin，.stp文件不规则结构的bin统计，link position to mill绑定结构，随动转动

### 4. bonded particle
- 块体，伸出，压断
- 颗粒，设置接触半径，小于接触半径时触发接触模型产生联结键
- max attempt to place particle是生成颗粒时尝试位置，不被占的位置，否则就要生成失败，需要重新生成，生成效率低
- 静态生成fill只是视觉上的，填充率其实只有30%左右，可以加个polygon板去压实，添加平动运动
- 模型需要加bonding模型，设置参数
- bond模型对时间步比较敏感，步长小一些
- 参看bond生成情况，可以在data brower里面看。particle limit限制以更好生成bond键
- 后处理查看bond键
- 在之前的计算基础上进行后续计算
- 保存simulation data注意保存时间，如只保留最后状态
- 还没压就炸开了，可能是bond参数设置问题

### 5. 颗粒替换

- 可以通过建立几何，去限制颗粒运动范围，比如壳，压板
- 颗粒替换需要用颗粒工厂api，plugin factories，需要外部文件输入，Particle_cluster_data.txt和particle_replacement_prefs.txt，名称要一致，大小写敏感，还有个particlereplacement.dll，是例如c++写的代码程序，描述这个replace过程，移除大颗粒（需要体积力），替换小颗粒
- bonding模型对时间步敏感

### 6. Heat transfer

- 颗粒间传热
- 物理模型那加上P to P 的 heat conduction，赋值导热系数
- 不能光heat conduction，还需要加particle body force加上temperature update，设置热容
- 颗粒工厂会出现parameters里面temperature初始值
- 这个模型不适用于颗粒和壁面传热，但是有相应模型
- 可以和其他软件耦合，fluent等，可以开发加入热辐射等
- 后处理里面可以添加文本，text, position text勾选后可以移动位置

### 7. Flexible plane

- 柔性变化面
- 先压平成键，然后输出为simulation deck，选时间为0s, 打开生成的dem文件，可以删掉之前的几何和工厂等，再以此初始建立新的几何等，进行模拟

## 经验教训

- 可以直接在体中加factory，设置为均匀体内出现
- 建立颗粒后记得property需要calculate点击，不然要报错
- 步长太大粒子会穿过面跑出去，丢失粒子，一定要合理设置步长，通常设置20%瑞利步长
- EDEM破解版如果打开报错license问题，按照之前破解时的方法启动服务，即去bin文件夹下去管理员权限开启WlmAdmin.exe，服务器里add file选择之前的lic

