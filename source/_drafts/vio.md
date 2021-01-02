---
title: vio
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-12-15 11:21:15
---
> 
<!--more-->

# 简介
## 介绍

- VIO：（Visual-Inertial Odometry），以视觉与 IMU 融合实现里程计
- IMU（Inertial Measurement Unit），惯性测量单元
  - 典型 6 轴 IMU 以较高频率（≥ 100Hz）返回被测量物体的角速度与加速度
  -  受自身温度、零偏、振动等因素干扰，积分得到的平移和旋转容易漂移
- 视觉 Visual Odometry
  - 以图像形式记录数据，频率较低（15 − 60Hz 居多）
  - 通过图像特征点或像素推断相机运动

