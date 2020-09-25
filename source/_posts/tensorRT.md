---
title: TensorRT
author: hero576
tags:
  - algorithm
categories:
  - AI
date: 2020-09-24 18:32:53
---

> TensorRT
<!--more-->

# 简介
## 介绍
- TensorRT是一个高性能的深度学习推理（Inference）优化器，可以为深度学习应用提供低延迟、高吞吐率的部署推理。TensorRT可用于对超大规模数据中心、嵌入式平台或自动驾驶平台进行推理加速。TensorRT现已能支持TensorFlow、Caffe、Mxnet、Pytorch等几乎所有的深度学习框架，将TensorRT和NVIDIA的GPU结合起来，能在几乎所有的框架中进行快速和高效的部署推理。

- TensorRT 是一个C++库，从 TensorRT 3 开始提供C++ API和Python API，主要用来针对 NVIDIA GPU进行 高性能推理（Inference）加速。现在最新版TensorRT是4.0版本。

- 可以认为tensorRT是一个只有前向传播的深度学习框架，这个框架可以将 Caffe，TensorFlow的网络模型解析，然后与tensorRT中对应的层进行一一映射，把其他框架的模型统一全部 转换到tensorRT中，然后在tensorRT中可以针对NVIDIA自家GPU实施优化策略，并进行部署加速。

- 目前TensorRT4.0 几乎可以支持所有常用的深度学习框架，对于caffe和TensorFlow来说，tensorRT可以直接解析他们的网络模型；对于caffe2，pytorch，mxnet，chainer，CNTK等框架则是首先要将模型转为 ONNX 的通用深度学习模型，然后对ONNX模型做解析。而tensorflow和MATLAB已经将TensorRT集成到框架中去了。

- ONNX（Open Neural Network Exchange ）是微软和Facebook携手开发的开放式神经网络交换工具，也就是说不管用什么框架训练，只要转换为ONNX模型，就可以放在其他框架上面去inference。这是一种统一的神经网络模型定义和保存方式，上面提到的除了tensorflow之外的其他框架官方应该都对onnx做了支持，而ONNX自己开发了对tensorflow的支持。从深度学习框架方面来说，这是各大厂商对抗谷歌tensorflow垄断地位的一种有效方式；从研究人员和开发者方面来说，这可以使开发者轻易地在不同机器学习工具之间进行转换，并为项目选择最好的组合方式，加快从研究到生产的速度。

![tensorRT](/images/pasted-271.png)

## 支持的层

层|支持
-|-
Activation| ReLU, tanh and sigmoid
Concatenation|: Link together multiple tensors across the channel dimension.
Convolution| 3D，2D
Deconvolution|Fully|connected: with or without bias
ElementWise| sum, product or max of two tensors
Pooling| max and average
Padding|Flatten|LRN| cross-channel only
SoftMax| cross-channel only
RNN| RNN, GRU, and LSTM
Scale| Affine transformation and/or exponentiation by constant values
Shuffle| Reshuffling of tensors , reshape or transpose data
Squeeze| Removes dimensions of size 1 from the shape of a tensor
Unary| Supported operations are exp, log, sqrt, recip, abs and neg
Plugin| integrate custom layer implementations that TensorRT does not natively support.

- 基本上比较经典的层比如，卷积，反卷积，全连接，RNN，softmax等，在tensorRT中都是有对应的实现方式的，tensorRT是可以直接解析的。

- 但是由于现在深度学习技术发展日新月异，各种不同结构的自定义层（比如：STN）层出不穷，所以tensorRT是不可能全部支持当前存在的所有层的。那对于这些自定义的层tensorRT中有一个 Plugin 层，这个层提供了 API 可以由用户自己定义tensorRT不支持的层。

![TensorRT-plugin](/images/pasted-272.png)


## 优化方式

- TensorRT优化方法主要有以下几种方式，最主要的是两种：层间融合或张量融合（Layer & Tensor Fusion）、数据精度校准（Weight &Activation Precision Calibration）。
![TensorRT-optimize-method](/images/pasted-273.png)

- [详细介绍](https://www.cnblogs.com/qccz123456/p/11767858.html)

# 安装
## linux
- [官方指导](http://docs.nvidia.com/deeplearning/sdk/tensorrt-install-guide/index.html)

```bash
# 这里改为自己对应的cuda版本
$ sudo dpkg -i nv-tensorrt-repo-ubuntu1604-ga-cuda8.0-trt3.0-20171128_1-1_amd64.deb
$ sudo apt-get update
$ sudo apt-get install tensorrt
$ sudo apt-get install python3-libnvinfer-doc
$ sudo apt-get install uff-converter-tf
```

- 安装好后，使用 `$ dpkg -l | grep TensorRT` 命令检测是否成功
- 安装后会在 /usr/src 目录下生成一个 tensorrt 文件夹，里面包含 bin , data , python , samples 四个文件夹， samples 文件夹中是官方例程的源码； data , python 文件中存放官方例程用到的资源文件，比如caffemodel文件，TensorFlow模型文件，一些图片等；bin 文件夹用于存放编译后的二进制文件。

- 可以把 tensorrt 文件夹拷贝到用户目录下，方便自己修改测试例程中的代码。

- 进入 samples 文件夹直接 make，会在 bin 目录中生成可执行文件，可以一一进行测试学习。

- 另外tensorRT是不开源的， 它的头文件位于 /usr/include/x86_64-linux-gnu 目录下，共有七个
- tensorRT的库文件位于 /usr/lib/x86_64-linux-gnu 目录下

## windows
- [下载安装包](https://developer.nvidia.com/nvidia-tensorrt-7x-download)

- 解压，设置系统环境变量
- 复制dll文件到cuda安装目录

- 完成tensorRT安装后，测试看安装是否成功，可以直接编译刚才解压的TensorRT里的案例来测试。这里我们选用sampleMNIST来测试。[流程](https://blog.csdn.net/yangzzguang/article/details/85570663)


# 使用

- 下面代码是一个简单的build过程
```cpp
IBuilder* builder = createInferBuilder(gLogger);
// parse the caffe model to populate the network, then set the outputs
// 创建一个network对象，不过这时network对象只是一个空架子
INetworkDefinition* network = builder->createNetwork();
//tensorRT提供一个高级别的API：CaffeParser，用于解析Caffe模型
//parser.parse函数接受的参数就是上面提到的文件，和network对象
//这一步之后network对象里面的参数才被填充，才具有实际的意义
CaffeParser parser;
auto blob_name_to_tensor = parser.parse(“deploy.prototxt”,trained_file.c_str(),*network,DataType::kFLOAT);
// 标记输出 tensors
// specify which tensors are outputs
network->markOutput(*blob_name_to_tensor->find("prob"));
// Build the engine
// 设置batchsize和工作空间，然后创建inference engine
builder->setMaxBatchSize(1);
builder->setMaxWorkspaceSize(1 << 30);
//调用buildCudaEngine时才会进行前述的层间融合或精度校准优化方式
ICudaEngine* engine = builder->buildCudaEngine(*network);
```

# 实例
- 之后补充
