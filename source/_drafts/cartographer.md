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


## 环境编译[^1][^2]
[^1]: [李太白](https://blog.csdn.net/tiancailx/article/details/90757522)
[^2]: [李太白github](https://github.com/xiangli0608/cartographer_detailed_comments_ws)

- [普通版官方流程](https://google-cartographer.readthedocs.io/en/latest/index.html)
```bash
#!/bin/bash
sudo apt update
sudo apt-get install -y clang cmake g++ git google-mock libboost-all-dev libcairo2-dev libcurl4-openssl-dev libeigen3-dev libgflags-dev libgoogle-glog-dev liblua5.2-dev libsuitesparse-dev lsb-release ninja-build stow
# Install Ceres Solver and Protocol Buffers support if available.
# No need to build it ourselves.
if [[ "$(lsb_release -sc)" = "focal" || "$(lsb_release -sc)" = "buster" ]]
then
  sudo apt-get install -y python3-sphinx libgmock-dev libceres-dev protobuf-compiler
else
  sudo apt-get install -y python-sphinx
  if [[ "$(lsb_release -sc)" = "bionic" ]]
  then
    sudo apt-get install -y libceres-dev
  fi
fi

set -o errexit
set -o verbose

git clone https://github.com/abseil/abseil-cpp.git
cd abseil-cpp
git checkout d902eb869bcfacc1bad14933ed9af4bed006d481
mkdir build
cd build
cmake -G Ninja \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_POSITION_INDEPENDENT_CODE=ON \
  -DCMAKE_INSTALL_PREFIX=/usr/local/stow/absl \
  ..
ninja
sudo ninja install
cd /usr/local/stow
sudo stow absl

# Build and install Ceres.
cd - & cd ../../
VERSION="1.13.0"
git clone https://ceres-solver.googlesource.com/ceres-solver
cd ceres-solver
git checkout tags/${VERSION}
mkdir build
cd build
cmake .. -G Ninja -DCXX11=ON
ninja
#CTEST_OUTPUT_ON_FAILURE=1 ninja test
sudo ninja install
VERSION="v3.4.1"

# Build and install proto3.
cd ../../
git clone https://github.com/google/protobuf.git
cd protobuf
git checkout tags/${VERSION}
mkdir build
cd build
cmake -G Ninja -DCMAKE_POSITION_INDEPENDENT_CODE=ON -DCMAKE_BUILD_TYPE=Release   -Dprotobuf_BUILD_TESTS=OFF   ../cmake
ninja
sudo ninja install

# Build and install Cartographer.
cd ../../
git clone https://github.com/cartographer-project/cartographer
cd cartographer
mkdir build
cd build
cmake .. -G Ninja
ninja
#CTEST_OUTPUT_ON_FAILURE=1 ninja test
sudo ninja install
```

- [ros版官方流程](https://google-cartographer-ros.readthedocs.io/en/latest/compilation.html)














