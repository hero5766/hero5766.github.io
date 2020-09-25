---
title: 神经网络实现XOR
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-09-11 18:02:36
---

> XOR

<!-- more -->

# 输入数据
> 异或数据

input|output
-|-
0 0|0
0 1|1
1 0|1
1 1|0

![逻辑关系](/images/pasted-270.png)

# 网络结构
- 使用2层网络，隐层为2个神经元时网络有时并不能完成任务，3个神经元则可以。
- 使用3层网络，也需要每层的神经元个数大于2，否则都会不能表达。
- [代码链接](https://github.com/hero576/DeepLearning/blob/master/project/XOR/MLP.ipynb)


