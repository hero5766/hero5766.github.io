---
title: LeNet
tags:
  - algorithm
categories:
  - DL
date: 2021-03-30 18:56:00
---
> 
<!--more-->

# 简介
- [LeNet-5模型](http://yann.lecun.com/exdb/lenet/)是Yann LeCun教授于1998年在论文Gradient-based learning applied to document recognition中提出。它是第一个成功应用于手写数字识别问题并产生实际商业价值的卷积神经网络

# 网络结构
![LeNet-5网络结构](/images/pasted-118.png)

- 2个卷积层 (C1, C3), 2个下采样(池化) 层 (S2 and S4), 2个全连接层 (C5,F6), 随后是输出层

层|说明|输出数据维度
-|-|-
input|输入层，图片|batchx3x32x32
conv1|卷积层<br>in=3<br>out=16<br>kernel=5x5<br>padding=0<br>stride=1|batchx16x28x28
relu|激活函数
pool1|最大下采样<br>kernel=2x2<br>stride=2<br>padding=0|batchx16x14x14
conv2|卷积层<br>in=16<br>out=32<br>kernel=5x5<br>padding=0<br>stride=1|batchx32x10*10
relu|激活函数
pool2|最大下采样<br>kernel=2x2<br>stride=2<br>padding=0|batchx32x5x5
fc1|全连接层<br>in=32x5x5<br>out=120|batchx120
fc2|全连接层<br>in=120<br>out=84|batchx84
fc3|全连接层<br>in=84<br>out=10|batchx10



> 卷积层计算公式：$N=\frac{W-F+2P}{S}+1$
>   W*W：输入图片大小
>   F*F：filter大小
>   S：步长
>   P：Padding



# 代码
## pytorch
- [官方教程](https://pytorch.org/tutorials/beginner/blitz/cifar10_tutorial.html)

- pytorch
```py
import torch.nn as nn
import torch.nn.functional as F
class LeNet(nn.Module):
  def __init__(self):
    super(LeNet, self).__init__()
    self.conv1 = nn.Conv2d(3,16,5) # in_channel out_channel kernel_size stride=1
    self.pool1 = nn.MaxPool2d(2,2) # kernel_size stride padding=0
    self.conv2 = nn.Conv2d(16,32,5)
    self.pool2 = nn.MaxPool2d(2,2)
    self.fc1 = nn.Linear(32*5*5,120)
    self.fc2 = nn.Linear(120,84)
    self.fc3 = nn.Linear(84,10)
  def forward(self,x):
    x = F.relu(self.conv1(x))
    x = self.pool1(x)
    x = F.relu(self.conv2(x))
    x = self.pool2(x)
    x = x.view(-1, 32*5*5)
    x = F.relu(self.fc1(x))
    x = F.relu(self.fc2(x))
    x = self.fc3(x)
    return x
```

- tensorflow
> padding: VALID($N=\frac{W-F+1}{S}$，向上取整)、SAME($N=\frac{W}{S}$，向上取整)


```py
from tensorflow.keras import Model
from tensorflow.keras.layers import Dense, Flatten, Conv2D, MaxPool2D
class LeNet(Model):
    def __init__(self):
        super(LeNet, self).__init__()
        self.conv1 = Conv2D(16, 5, activation='relu')
        self.pool1 = MaxPool2D()
        self.conv2 = Conv2D(32,5, activation='relu')
        self.pool2 = MaxPool2D()
        self.flatten = Flatten()
        self.d1 = Dense(120, activation='relu')
        self.d2 = Dense(84, activation='relu')
        self.d3 = Dense(10, activation='softmax')
    def call(self, x, **kwargs):
        x = self.pool1(self.conv1(x))     # input[batch, 32, 32, 3] output[batch, 14, 14, 16]
        x = self.pool2(self.conv2(x))     # input[batch, 14, 14, 16] output[batch, 5, 5, 32]
        x = self.flatten(x)               # output [batch, 5*5*32]
        x = self.d1(x)                    # output [batch, 120]
        x = self.d2(x)                    # output [batch, 84]
        return self.d3(x)                 # output [batch, 10]
```



