---
title: SLAM
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-07-31 20:03:00
---
> SLAM
<!--more-->

# 简介

## SLAM的提出
- SLAM(Simultaneous Localization And Mapping)同步定位与构图，提出的目的就是为了解决感知、定位以及构图层的问题。目标就是确定当前环境的样子，并确定当前环境所在位置。

- 1986,ICPA,Peter Cheeseman, Jim Crowley, <strong style="color:red">Hugh Durrent-Whyte</strong>[^1][^2], 提出：可以试着利用概率估计来解决定位和构图问题
[^1]:Simultaneous Localisation and Mapping (SLAM):Part I The Essential Algorithms
[^2]:Simultaneous Localisation and Mapping (SLAM):Part II State of the Art

- 在x0初始状态，观测和控制已知的条件下，对应x_k和m的概率：
  - $P(x_k,m \vdots Z_{k:0},U_{k:0},x_0)$
  - $x_k$：状态
  - $m$：地图
  - $Z_{k:0}$：观测
  - $U_{k:0}$：控制

- SLAM提出了一个基本架构，根据输入（可见光、IMU、激光雷达、声呐），求得输出（环境及环境位置）。

基本架构|说明
-|-
前端|使用传感器获取的数据，计算对应时间的机器人状态
后端|在前端的状态之上，对漂移和误差进行估计和优化
回环|当路径回到原始位置后，对总体路径进行调整
建图|对获取的数据进行处理建图，为后续导航、决策控制等使用


## SLAM的发展
- SLAM根据特点可以分为两种：
  - 基于可见光视觉（特征、照度）：通过特征点提取和匹配，做自定位，利用多视几何的技术；
  - 激光（Scan Matching）：通过激光可以获取精确的场景模型，将相邻时间段的场景模型分析出相对位置变化，并拼接，就可以定位出自己的位置。

### 基于可见光视觉
- EKF-SLAM：利用卡尔曼滤波，是贝叶斯的一种特殊情况，也是假设噪声服从高斯分布
- FAST-SLAM：粒子滤波器，随机撒点，在大范围中寻找并趋近于最优解。必须先有图
- GRAPH-SLAM：滤波器有特征空间特别大的问题，GRAPH-SLAM将观测一定量的环境特征点，和自身位置的点连接，形成node和edge，利用图优化，可以直接做定位。
- EKF Monocular SLAM：Javier Civera,<strong style="color:red">Andrew J.Davison</strong>，用单目视觉就可以完成SLAM，大大降低了成本。
- PTAM-SLAM：Georg Klein,<strong style="color:red">David Murray</strong>
- RGBD-SLAM
- ORB-SLAM2
- LSD-SLAM

### 基于激光
- GMapping
- SLAM6D
- LOAM、V-LOAM：Ji Zhang，用视觉补偿激光，用特征提取做匹配，在kitti上榜单是最好的
- Cartographer：google开源的，用于扫地机器人
- BLAM
- SegMatch：点云

## 数据集
[kitti](http://www.evlibs.net/datasets/kitti)

## 书籍
- [视觉SLAM十四讲从理论到实践]()
- [Multiple View Geometry]()多视图几何
- [State Estimation For Robotics]()机器人学中的状态估计
- [机器人学中的状态估计]()


## 资料
- [无人驾驶平台](https://github.com/topics/autonomous-vehicles)
- [日本无人驾驶平台](https://github.com/CFFL/Autoware)

<!-- ## 相关企业 -->
<!-- 百度：https://iv.baidu.com/employ.html -->
<!-- 图森未来 -->
<!-- 腾讯：https://careers.tencent.com/home.html -->
<!-- 四维图新：https://navinfo.com/joinus -->
<!-- 驭势科技：https://www.uisee.com/joinus/index.aspx -->
<!-- 纵目科技：https://zongmu.zhiye.com -->
<!-- 滴滴：https://talent.didiglobal.com -->

## 环境配置

环境|说明
-|-
linux操作系统|16.04以上
开发环境|`sudo apt-get install cmake gcc g++ build-essential`
.deb文件|`sudo dpkg install *.deb PATH:*.deb`<br>`sudo dpky --list`<br>`sudo dpkg --remove [package name]`
cmake|

# SLAM理论基础
## 数学表达
- 四元数
## 刚体运动
- 欧式旋转

## 李群和李代数

# 位姿估计
## SLAM数学模型
- 运动模型：$x_k=f(x_{k-1},u_k)+w_k$
- 观测模型：$z_{k,j}=h(x_k,y_j)+v_{k,j}$
- $w_k,v_{k,j}$为噪声

## 基于自身量测的估计
## 基于视觉的RANSAC PNP
## 基于点云特征的ICP
## 随机概率融合（VINS、LINS）

# 非线性优化
## 优化的目标
- 状态估计：后端的目标是从噪声的数据估计内在的状态
  - 渐进式Incremental/Recursive
  - 批量式Batch
### 渐进式 
- 保持当前状态估计，新信息加入时，更新已有的估计(滤波)

线型系统|高斯噪声|滤波
-|-|-
√|√|卡尔曼滤波(KF)
×|√|做线性近似，扩展卡尔曼滤波(EKF)
×|×|非参数化，粒子滤波器(PSO)
-|-|估计当前窗口的一系列状态Sliding window filter，multiple state kalman(MSCKF)

### 批量式 
- 给定一定规模数据，计算该数据下的最优估计(优化)

## 递归
- k时刻所有待估计的量组成k时刻状态：$x_k\triangleq\left\{x_k,y_1,\cdots,y_m\right\}$
- k时刻观测统一记成$z_k=\begin{cases}x_k=f(x_{k-1},u_k)+wk\\z_k=h(x_k)+v_k\end{cases}$



# 回路和图优化
## 词袋模型
## G2O
## Ceres
## 关键帧
## 局部子图
## gstam

# 传感器
## 视觉
## 激光
## 声呐、超声
## 惯导

# 地图
## 点云
## 拓扑地图
## 语义地图
## 特征地图

# 视觉
## 相机模型
## 立体视觉
## 视觉几何

## AR/VR
- Unity3D最广泛使用的AR/VR的平台

# 深度学习和SLAM
## 单目深度估计
## 环境分割识别
## 语义理解
## 动态障碍物追踪和规避
## 新的特征表达

# SLAM使用场景
## 无人驾驶系统
## 无人机
## 室内辅助导航




