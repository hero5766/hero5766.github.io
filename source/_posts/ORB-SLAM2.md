---
title: ORB-SLAM2
author: hero576
tags:
  - SLAM
categories:
  - robot
date: 2021-05-10 11:27:00
---
>
<!--more-->

# 简介
# 介绍
- ORB-SLAM是西班牙Zaragoza大学的Raul Mur-Artal编写的视觉SLAM系统。他的论文“ORB-SLAM: a versatile andaccurate monocular SLAM system"发表在2015年的IEEE Trans. on Robotics上。开源代码包括前期的ORB-SLAM和后期的ORB-SLAM2。第一个版本主要用于单目SLAM，而第二个版本支持单目、双目和RGBD三种接口。


- [ORB-SLAM论文]()
- [ORB-SLAM2论文]()
- [作者博士论文]()


## 特点
- ORB-SLAM是一个完整的SLAM系统，包括视觉里程计、跟踪、回环检测。它是一种完全基于稀疏特征点的单目SLAM系统，其核心是使用ORB（Orinted FAST and BRIEF）作为整个视觉SLAM中的核心特征。具体体现在几个方面：

  - 提取和跟踪的特征点使用ORB。ORB特征的提取过程非常快，适合用于实时性强的系统。
  - 回环检测使用词袋模型，其字典是一个大型的ORB字典。
  - 接口丰富，支持单目、双目、RGBD多种传感器输入，编译时ROS可选，使得其应用十分轻便。代价是为了支持各种接 口，代码逻辑稍为复杂。
  - 在PC机以30ms/帧的速度进行实时计算，但在嵌入式平台上表现不佳。

## 原理

- 它主要有三个线程组成：跟踪、Local Mapping（又称小图）、Loop Closing（又称大图）。

![ORB-SLAM整体流程](https://img-blog.csdn.net/20180413184944586?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xlYXJuaW5nX3RvcnRvc2ll/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- [22](https://blog.csdn.net/u010128736/article/details/53157605)

### 跟踪
- 跟踪线程相当于一个视觉里程计，流程如下：

  - 首先，对原始图像提取ORB特征并计算描述子。
  - 根据特征描述，在图像间进行特征匹配。
  - 根据匹配特征点估计相机运动。
  - 根据关键帧判别准则，判断当前帧是否为关键帧。
  - 相比于多数视觉SLAM中利用帧间运动大小来取关键帧的做法，ORB_SLAM的关键帧判别准则较为复杂。


## 安装[^1]
[^1]: [ORB-SLAM2的安装与运行_W_Tortoise的博客-CSDN博客_orbslam2运行](https://blog.csdn.net/learning_tortosie/article/details/79881165)

库名|命令
-|-
依赖|`sudo apt-get update`
git|`sudo apt-get install git`
cmake|`sudo apt-get install cmake`
Pangolin|`sudo apt-get install libglew-dev libpython2.7-dev`<br>`git clone https://github.com/stevenlovegrove/Pangolin.git`<br>`cd Pangolin`<br>`mkdir build && cd build`<br>`cmake .. && make -j4 && sudo make install `
OpenCV|`sudo apt-get install build-essential`<br>`sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev`<br>`sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev`<br>`cd opencv && mkdir build && cd build`<br>`cmake -D CMAKE_BUILD_TYPE=Release –D CMAKE_INSTALL_PREFIX=/usr/local ..&&　make –j8 && sudo make install`
Eigen3|`sudo apt-get install libeigen3-dev`
ORB-SLAM2|`git clone https://github.com/raulmur/ORB_SLAM2.git ORB_SLAM2`<br>`$ cd ORB_SLAM2 && chmod +x build.sh && ./build.sh`

- DBoW2是DBow库的改进版本，DBow库是一个开源的C++库，用于索引图像并将其转换为单词表示形式。
- g2o是一个开源的C ++框架，用于优化基于图的非线性误差函数。
- 这两个库在ORB-SLAM2项目的第三方文件夹中

- [/usr/include/c++/8/bits/stl_map.h:122:21: error: static assertion failed: std::map must have the same value_type as its allocator](https://blog.csdn.net/lixujie666/article/details/90023059)
- [error: ‘usleep’ was not declared in this scope](https://blog.csdn.net/qq_37788081/article/details/85000361)
- [boost问题](https://blog.csdn.net/u014709760/article/details/85253525)


## 数据集[^2]
[^2]:[数据集](https://www.sohu.com/a/154011668_715754)

数据集|介绍
-|-
[TUM](http://vision.in.tum.de/data/datasets/rgbd-dataset/download)|
[KITTI](http://www.cvlibs.net/datasets/kitti/eval_odometry.php)|
EuRoC|


### TUM
- 数据格式

- associate.py：用于py2
    - `python associate.py rgb.txt depth.txt >associate.txt`
    - `python associtate.py associate.txt groundtruth.txt>associate_with_groundtruth.txt`


## 样例

- TUM:`./Examples/Monocular/mono_tum Vocabulary/ORBvoc.txt Examples/Monocular/TUM3.yaml data/rgbd_dataset_freiburg1_desk`
- KITTI: `./Examples/Monocular/mono_kitti Vocabulary/ORBvoc.txt Examples/Monocular/KITTIX.yaml PATH_TO_DATASET_FOLDER/dataset/sequences/SEQUENCE_NUMBER`


# 代码分析[^3]
[^3]: [双目部分分析](https://zhuanlan.zhihu.com/p/66882733)

[sylvester的博客](https://blog.csdn.net/u010128736/)



# 地图

-[](https://blog.csdn.net/u014709760/article/details/86319090)


# python实现

- [](https://www.zhihu.com/question/265234059?sort=created)
- [py2-slam-github](https://github.com/Transportation-Inspection/visual_odometry)




