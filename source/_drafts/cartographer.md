---
title: Cartographer论文
author: hero576
tags:
  - algorithm
categories:
  - AI
date: 2020-09-10 15:57:46
---
<!-- more -->
# 简介
## 介绍
- 论文：Real-Time Loop Closure in 2D LIDAR SLAM
- 谷歌公司提出的
- [官方文档](https://google-cartographer.readthedocs.io/)
- [官方ROS文档](https://google-cartographer-ros.readthedocs.io/)

## 相关工作
- 传统方法有累计误差
- 使用地图搜索的方法
- 粒子滤波算法，大地图会有性能问题

## slam发展历程：
    - 1987首次提出
    - 滤波算法的SLAM
    - 1990位姿图
    - 2007粒子滤波
    - 2010位子图稀疏性
    - 2014提出了scantomap的slam（Hecor）
    - 2014提出基于位姿图的slam（Karto）scantosubmap
    - 2016提出基于位姿图的开源框架

## local 2D SLAM 前端
- 优化位姿
- 二维栅格地图

## closing loops回环检测
- 稀疏位姿优化
- 分支定界算法


## 环境编译
- [ros版官方流程](https://google-cartographer-ros.readthedocs.io/en/latest/compilation.html)














