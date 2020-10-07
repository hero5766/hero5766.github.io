---
title: PyTorch
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-10-07 11:52:15
---


# 简介
## 介绍
- [官网]()

## 安装
- 在官网选择自己的环境安装


# 变量
# 数据类型
cpu类型|gpu类型|说明
-|-|-
torch.ShortTensor|torch.cuda.ShortTensor|16-bit
torch.IntTensor|torch.cuda.IntTensor|32-bit
torch.LongTensor|torch.cuda.LongTensor|64-bit
torch.FloatTensor|torch.cuda.FloatTensor|32-bit
torch.DoubleTensor|torch.cuda.DoubleTensor|64-bit
torch.ByteTensor|torch.cuda.ByteTensor|8-bit
torch.CharTensor|torch.cuda.CharTensor|8-bit
torch.MalfTensor|torch.cuda.MalfTensor|16-bit

## 方法

函数|说明
-|-
类型推断|`a.type()`
类型判断|`isinstance(a,torch.FloatTensor)`
gpu转化|`data=data.cuda()`
张量形状|`a.shape`
张量尺寸|`a.size()`
内存|`a.numel()`
维度|`a.dim()`

## 标量

函数|说明
-|-
创建标量| `a = torch.tensor(1.)`

## 向量/张量

创建矩阵|说明
-|-
list矩阵| `a = torch.tensor([1.])`
numpy矩阵| `torch.from_numpy(np.ones(2))`
给定维度| `a = torch.FloatTensor(1)`
list矩阵| `a = torch.FloatTensor([2,3])`
全一矩阵| `torch.ones(2)`
全零矩阵|`torch.zeros(3)`
对角矩阵|`torch.eye(3)`
随机正态分布矩阵| `torch.randn(2,3)`<br>`torch.normal(mean=torch.full([10],0),std=torch.arange(1,0,-0.1))`
随机均匀[0,1]矩阵| `torch.rand(2,3)`
随机聚云分布|`torch.randint(1,10)`
给定值的矩阵|`torch.full([2,3],7)`
递增矩阵|`torch.arange(0,10,2)`
等分数列|`torch.linspace(0,10,steps=4)`
等指数列|`torch.logspace(0,-1,steps=4)`
随机打散|`torch.randperm(10)`




## 初始化

创建矩阵|说明
-|-
未初始化数据| `torch.empty(2,3)`<br>`a = torch.FloatTensor(2,3)`,易出现nan，inf等数据
随机均匀初始化|`torch.rand(2,3)`
根据传入的tensor随机初始化|`torch.rand_like(a)`


## 索引和切片

函数|说明
-|-
索引|`a[0,0]`
切片|`a[1:,2:,:,::2]`
索引|`a.index_select(1,[1,2])`
索引|`a[:,0,...]`
掩码索引|`mask = x.ge(0.5)`<br>`torch.masked_select(a,mask)`
打平后根据索引取|`torch.take(a,torch.torch.tensor([0,2,0]))`


## 维度变换

函数|说明
-|-
更改维度|view/reshape两者相同，`a.view(4,28*28)`
减少、增加维度|squeeze/unsqueeze，squeeze挤压维度不为1则返回原值。`a.unsqueeze(0)`
扩展|expand扩展数据/repeat扩展数据并复制数据，`a.expand(-1,32,14,14)`,`a.repeat(1,32,14,14)`拷贝次数
转置|transpose/t/permute，t只适应于矩阵，transpose维度交换。`a.transpose(1,3).contiguous().view(4,3*32*32).view(4,3,32,32)`维度污染<br>`a.transpose(1,3).contiguous().view(4,3*32*32).view(4,32,32,3).transpose(1,3)`<br>a.permute(0,2,3,1)

## 自动扩展

函数|说明
-|-














