---
title: SLAM
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-11-01 20:03:00
---
> 
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

<!-- # SLAM理论基础 -->
<!-- ## 数学表达
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
## 随机概率融合（VINS、LINS） -->

<!-- # 非线性优化
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
- k时刻观测统一记成$z_k=\begin{cases}x_k=f(x_{k-1},u_k)+wk\\z_k=h(x_k)+v_k\end{cases}$ -->


# 后端优化
- 在视觉里程计、激光里程计获取到的实时姿态之后，后端整合起来做全局的优化，使得最终结果能更接近真实环境

## 后端（Backend）
- 从带噪声的数据估计内在状态——状态估计问题--Estimated the inner state from noisy data
- 渐进式（Incremental）
  - 保持当前状态的估计，在加入新信息时，更新已有的估计（滤波）
  - 线性系统+高斯噪声=卡尔曼滤波器
  - 非线性系统+高斯噪声+线性近似=扩展卡尔曼
  - 非线性系统+非高斯噪声+非参数化=粒子滤波器
  - Sliding window filter & multiple state Kalman (MSCKF)
- 批量式（Batch）
  - 给定一定规模的数据，计算该数据下的最优估计（优化）

### 数学描述
- 定义在$t=0,...,N$的时间内，机器人位姿为$x_0,...,x_n$，同时又路标$y_1,...,y_m$
- 运动和观测方程为：
  - $\begin{cases}x_k=f(x_{k-1},u_k)+w_k\\z_{k,j}=h(y_i,x_k)+v_{k,j}\end{cases}$，$k=1,...n,\;j=1,...,m$
  - 其中运动和观测都受到噪声影响，纯视觉SLAM没有运动方程

### 卡尔曼滤波
- 从贝叶斯滤波器方法来推导卡尔曼滤波

- 用随机变量表示状态，估计其概率分布
- k时刻所有待估计的量组成k时刻状态：
  - $x_k\sideset{\;}{\;}=^\{x_k,y_1,...,y_m\}$
- k时刻观测统一记成：$z_k$方程简化为：
  - $\begin{cases}x_k=f(x_{k-1,u_k}+w_k)\\z_{k}=h(x_k)+v_k\end{cases}$，$k=1,...n$
- 根据过去0到k时刻的数据，估计当前状态
  - $P(x_k|x_0,u_{1:k},z_{1:k})$
- 贝叶斯展开Bayes，分母与状态无关，所以简化为：
  - $P(x_k|x_0,u_{1:k},z_{1:k})\propto P(z_k|x_k)P(x_k|x_0,u_{1:k},z_{1:k-1})$
  - 当前状态正比于似然和先验的乘积
- $x_{k-1}$当做已知量，作为条件求余$x_k$的后验分布，先验部分按条件概率展开：
  - $P(x_k|x_0,u_{1:k},z_{1:k-1})=\int P(x_k|x_{k-1},x_0,u_{1:k},z_{1:k-1})P(x_{k-1}|x_0,u_{1:k},z_{1:k-1})dx_{k-1}$
  - 其中：
  - $P(x_k|x_{k-1},x_0,u_{1:k},z_{1:k-1})$是k时刻受先前状态的影响的条件概率
  - $P(x_{k-1}|x_0,u_{1:k},z_{1:k-1})$是上一时刻的状态估计

- 一阶马尔可夫性：假设k时刻状态之和k-1时刻有关
- 不假设马尔科夫性：假设k时刻状态与先前所有时刻有关

- 这里假设一阶马尔科夫的情况
  - $P(x_k|x_{k-1},x_0,u_{1:k},z_{1:k-1})=P(x_k|x_k-1,u_k)$
  - $p(x_{k-1}|x_0,u_{1:k},z_{1:k-1})=P(x_{k-1}|x_0,u_{1:k-1},z_{1:k-1})$


**高斯分布的线型变换**
- 假设$x\sim N(\mu,\Sigma),y=Ax+b$，则y也服从高斯分布
  - $E(y)=A\mu+b$
  - $Cov(y)=E[(y-E[y])(y-E[y])^T]=A\Sigma A^T$ 


**卡尔曼滤波器的推导**
- 线性模型和高斯噪声：
  - $\begin{cases}x_k=A_kx_{k-1}+u_k+w_k\\z_k=C_kx_k+v_k\end{cases}$，$k=1,...,n$，A是状态转移矩阵
  - 其中：$w_k\sim N(0,R)$,$v_k\sim N(0,Q)$
- 状态的高斯分布:
  - $P(x_{k-1})=N(\hat{x}_{k-1},\hat{P}_{k-1})$
  - 上式是后验的表示，先验为：$\bar{x}_k,\bar{P}_k$

- 根据高斯分布的线型变换，k-1时刻后验通过运动方程推算k时刻先验：
  - 预测：$P(x_{k-1}|x_0,u_{1:k},z_{1:k-1})=N(A_k\hat(x)_{k-1}+u_k,A_k\hat{P}_kA_k^T+R)$
  - 简化记作：$\bar{x}_k=A_k\hat(x)_{k-1}+u_k$，$\bar(P)_k=A_k\hat{P}_{k-1}A_k^T+R$
- 根据观测方程可知
  - $P(z_k|x_k)=N(C_kx_k,Q)$
- 根据贝叶斯展开：
  - $P(x_k|x_0,u_{1:k},z_{1:k})\propto P(z_k|x_k)P(x_k|x_0,u_{1:k},z_{1:k-1})$
- 两边均是高斯分布，所以比较指数部分的二次项和一次项部分即可
  - 指数部分：$(x_k-\hat{x}_k)^T\hat{P}_k^{-1}(x_k-\hat{x}_k)=(z_k-C_kx_k)^TQ^{-1}(z_k-C_kx_k)+(x_k-\bar{x_k})^T\bar{P}_k^{-1}(x_k-\bar{x_k})$
  - 比较$x_k$的二次和一次项系数
    - 对于二次项：$\hat{P}^{-1}=C_k^TQ^{-1}C_k+\bar{P}_k^{-1}$
    - 对于一次项：$-2\hat{x}_k^T\hat{P}_k^{-1}x_k=-2z_k^TQ^{-1}C_kx_k-2\bar{x}_k^T\bar{P}_k^{-1}x_k$，整理后：$\hat{P}_k^{-1}\hat{x}_k=C_kQ^{-1}z_k+\bar{P}_k^{-1}\bar{x}_k$
    - 两边同乘$\hat{P}_k$，并定义$K=\hat{P}_kC_kQ^{-1}$
      - $\hat{x}_k=\hat{P}_kC_kQ^{-1}z_k+\hat{P}_k\bar{P}_k^{-1}\bar{x}_k$
      - $=Kz_k+(I-KC_k)\bar{x}_k=\bar{x}_k+K(z_k-C_k\bar{x}_k)$:更新公式

### 小车为例做卡尔曼估计 
![](/images/pasted-353.png)
**数学描述**
- t时刻状态：
  - $x_t=(p_t,v_t)$，pt为位置，vt为速度
  - 控制量为油门产生的加速度$u_t$

**运动方程**
- 运动状态方程可以写成：
  - $p_t=p_{t-1}+v_{t-1}\times \triangle t+\frac{1}{2}u_t\times \triangle t^2$
  - $v_t=v_{t-1}+u_t\times\triangle t$
- 因为他们之间呈线性关系，可以用矩阵化表达：
  - $\begin{bmatrix}p_t\\v_t\end{bmatrix}=\begin{bmatrix}1&\triangle t\\0&1\end{bmatrix}\begin{bmatrix}p_{t-1}\\v_{t-1}\end{bmatrix}+\begin{bmatrix}\frac{\triangle t^2}{2}\\\triangle t\end{bmatrix}u_t$
- 上式可以抽象为：
  - 运动方程：$X_t^-=A_t\hat{X}_{t-1}+B_tu_t$
  - A：状态转移矩阵
  - B：控制矩阵
  - $\hat{X}$：表示后验估计值
  - $X_t^-$：表示推算出来的还要优化的值

- 增加噪声，首先是估计值的噪声：$X_{t-1}\sim N(\hat{X}_{t-1},\Sigma)$，高维时$\sigma$为协方差$\Sigma$
- 然后引入噪声：$w_t\sim N(0,Q)$，噪声在传播过程中的不确定性。
- 整理得到：
  - $X_{t-1}\sim N(\hat{X}_{t-1},\Sigma)$
  - 经过线型变换：$X_t=A_tX_{t-1}+B_tu_t+w_t$，使得状态方程也满足高斯分布
  - $X_t\sim N(A_t\hat{X}_{t-1}+B_tu_t,A\Sigma_{t-1}A^T+Q)$

- **预测方程1**，状态转移
  - $X_t^-=A_t\hat{X}_{t-1}+B_tu_t+w_t$
- **预测方程2**，方差转移
  - $\Sigma_t^-=A\Sigma_{t-1}A^T+Q$

**观测方程**
- 观测方程：$Z_t=HX_t+v_t$
  - 其中：
  - $Z_t$：t时刻观测值，观测小车速度
  - $H$：小车状态X和Z之间有变换关系h(x)，这里假设是线型函数：$H=[0,1]$,$X_t=[p_t,v_t]^T$，相乘得到速度
  - $v_t\sim N(0,R)$：观测噪声

**滤波融合**
- 运动方程得到状态估计$\bar{X}_t$，观测方程可以推算$X_t$，需要对两个估计值进行融合滤波
  - $X_{update}=X_{predict}+g(X_{observe}-X_{predict})$,$g\in[0,1]$
  - 其中g是卡尔曼增益。由两者的方差计算出来的，方差越小，不确定越低，权重越高：$g=\frac{\Sigma_{predict}}{\Sigma_{predict}+\Sigma_{observe}}$

**更新方程**
- **状态更新**
  - $\hat{X}_t=X_t^-+K_t(Z_t-HX_t^-)$
- **卡尔曼增益更新**
  - $K_t=\Sigma_t^-H^T(H\Sigma_t^-H^T+R)^{-1}$：增益有两个作用
    - 将残差从观测映射到状态域
    - 承担g的作用，在两种状态估计之间根据方差得到权重
- **不确定性更新**：
    - $\Sigma_t=(I-K_tH)\Sigma_t^-$


### EKF
- 卡尔曼滤波器的非线性扩展
- 当f,h为非线性函数时：
  - $\begin{cases}x_k=f(x_{k-1},u_k)+w_k\\z_k=h(x_k)+v_k\end{cases}$,$k=1,...,n$
- 在工作点附近进行一阶泰勒展开：
  - $x_k\approx f(\hat{x}_{k-1},u_k)+\frac{\partial f}{\partial x_{k-1}}(x_{k-1}-\hat{x}_{k-1})+w_k$，定义$F=\frac{\partial f}{\partial x_{k-1}}$
  - $z_k\approx h(\bar{x}_k)+\frac{\partial h}{\partial x_k}(x_k-\hat{x}_k+n_k$，定义$H=\frac{\partial h}{\partial x_k}$

- 预测部分：$\bar{x}_k=f(\hat{x}_{k-1},u_k)$,$\bar{P}_k=F\hat{P}_kF^T+R_k$
- 卡尔曼增益：$K_k=\bar{P}_kH^T(H\bar{P}_kH^T+Q_k)^{-1}$
- 更新部分：$\hat{x_k}=\bar{x}_k+K_k(z_k-h(\bar{x}_k))$,$\hat{P}_k=(I-K_kH)\bar{P}_k$

**特点**
- 简单，适用各种传感器形式
- 抑郁做多传感器融合

- 一阶马尔科夫性过于简单，可能会发散，数据不能有outlier
- 线性化误差
- 需要存储所有状态量的均值和方差，平方增长

### BA
- EKF是一阶马尔科夫性，而BA是任意多的连接的

### Pose Graph
- 顶点仅由相机位姿组成，边为相机位姿间的约束


# 回环检测
**回环检测的意义**
- VO和后端都存在误差
- SLAM的建图与定位是耦合的——误差将会累计

**Loop Closing步骤**
- 检测到回环的发生
- 计算回环修选帧与当前帧的运动
- 验证回环是否成立
- 闭环


<!--
# 回路和图优化
## 词袋模型
## G2O
## Ceres 
## 关键帧
## 局部子图
## gstam
-->
<!-- # 传感器
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

-->


