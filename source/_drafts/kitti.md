---
title: KITTI
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-11-20 13:50:20
---
> 
<!-- more -->

- 数据集[官网下载地址](http://www.cvlibs.net/datasets/kitti/eval_object.php?obj_benchmark=3d)


# 整体介绍[^1]
[^1]: [链接](https://blog.csdn.net/u010368556/article/details/78259027)

- KITTI数据集由德国卡尔斯鲁厄理工学院和丰田美国技术研究院联合创办，是目前国际上最大的自动驾驶场景下的计算机视觉算法评测数据集。KITTI包含市区、乡村和高速公路等场景采集的真实图像数据，每张图像中最多达15辆车和30个行人，还有各种程度的遮挡与截断。整个数据集由389对立体图像和光流图，39.2 km视觉测距序列以及超过200k 3D标注物体的图像组成[^2]，以10Hz的频率采样及同步。原始数据采集于2011年的5天，共有180GB数据。
[^2]: Andreas Geiger and Philip Lenz and Raquel Urtasun. Are we ready for Autonomous Driving? The KITTI Vision Benchmark Suite. CVPR, 2012 

- 评测计算机视觉技术在车载环境下的性能，包含

评测|说明
-|-
立体图像|stereo
光流|optical flow
视觉测距|visual odometry
3D物体检测|object detection
3D跟踪|tracking

# 类别

类别|说明
-|-
分类|Road, City, Residential, Campus, Person
3D物体检测细分|car, van, truck, pedestrian, pedestrian(sitting), cyclist, tram, misc

![](/images/pasted-373.png)



# 数据形式
- 论文[^3]中提及的数据组织形式，可能是早期的版本，与目前KITTI数据集官网公布的形式不同，本文稍作介绍。 
[^3]: Andreas Geiger and Philip Lenz and Christoph Stiller and Raquel Urtasun. Vision meets Robotics: The KITTI Dataset. IJRR, 2013 

- 如图所示，一个视频序列的所有传感器数据都存储于data_drive文件夹下，其中date和drive是占位符，表示采集数据的日期和视频编号。时间戳记录在Timestamps.txt文件。 
![](/images/pasted-374.png)






















