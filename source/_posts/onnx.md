---
title: ONNX
author: hero576
tags:
  - algorithm
categories:
  - AI
date: 2020-09-20 17:25:23
---

> ONNX
<!--more-->

# 简介

## 介绍
- Open Neural Network Exchange（ONNX，开放神经网络交换）格式，是一个用于表示深度学习模型的标准，可使模型在不同框架之间进行转移。
- 它使得不同的人工智能框架（如Pytorch, MXNet）可以采用相同格式存储模型数据并交互。 ONNX的规范及代码主要由微软，亚马逊 ，Facebook 和 IBM 等公司共同开发，以开放源代码的方式托管在Github上。目前官方支持加载ONNX模型并进行推理的深度学习框架有： Caffe2, PyTorch, MXNet，ML.NET，TensorRT 和 Microsoft CNTK，并且 TensorFlow 也非官方的支持ONNX。

- 典型的几个线路：
  - `Pytorch -> ONNX -> TensorRT`
  - `Pytorch -> ONNX -> TVM`
  - `TF –> onnx –> ncnn`

## 简单流程
- 使用pytorch训练好模型，生成`model.pt`权重文件；
- 使用ONNX将`model.pt`转化为`model.onnx`
- 这个`.onnx`权重文件不仅包含了权重值，也包含了神经网络的网络流动信息以及每一层网络的输入输出信息和一些其他的辅助信息。
- 拿[netron](https://github.com/lutzroeder/netron)这个工具来可视化(读取ONNX文件)

## Protobuf介绍
- ONNX既然是一个文件格式，那么我们就需要一些规则去读取它，或者写入它，ONNX采用的是protobuf这个序列化数据结构协议去存储神经网络权重信息。
- Protobuf是一种平台无关、语言无关、可扩展且轻便高效的序列化数据结构的协议，可以用于网络通信和数据存储。我们可以通过protobuf自己设计一种数据结构的协议，然后使用各种语言去读取或者写入，通常我们采用的语言就是C++。


# ONNX使用
## ONNX的数据格式

- ONNX中最核心的就是`onnx.proto`这个文件，这个文件中定义了ONNX这个数据协议的规则和一些其他信息。

## 安装
- 同时也将protobuf安装

```bash
pip install onnx
```

## 导出ONNX文件
### pytorch
- 其中model为pytorch的模型，dummy_input为输入，export_params=True代表连带参数一并输出，注释的dummy_input2，dummy_input3，torch.onnx.export对应的是多个输入的例子。
- 直接将PlainC3AENetCBAM替换成需要转换的模型，然后修改pthfile，输入和onnx模型名字然后执行即可。

```py
import io
import torch
import torch.onnx
from models.C3AEModel import PlainC3AENetCBAM
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
def test():
  model = PlainC3AENetCBAM()
  pthfile = r'/home/joy/Projects/models/emotion/PlainC3AENet.pth'
  loaded_model = torch.load(pthfile, map_location='cpu')
  # try:
  #   loaded_model.eval()
  # except AttributeError as error:
  #   print(error)
  model.load_state_dict(loaded_model['state_dict'])
  # model = model.to(device)
  #data type nchw
  dummy_input1 = torch.randn(1, 3, 64, 64)
  # dummy_input2 = torch.randn(1, 3, 64, 64)
  # dummy_input3 = torch.randn(1, 3, 64, 64)
  input_names = [ "actual_input_1"]
  output_names = [ "output1" ]
  # torch.onnx.export(model, (dummy_input1, dummy_input2, dummy_input3), "C3AE.onnx", verbose=True, input_names=input_names, output_names=output_names)
  torch.onnx.export(model, dummy_input1, "C3AE_emotion.onnx", verbose=True, input_names=input_names, output_names=output_names)
if __name__ == "__main__":
 test()
```

### keras
- 将keras的h5模型转化为onnx，安装工具库，[官方文档](https://pypi.org/project/keras2onnx/)，[GitHub](https://github.com/onnx/keras-onnx)
```bash
pip install keras2onnx
```

- 转化
```py
import keras
import keras2onnx
import onnx
from keras.models import load_model
model = load_model('/root/notebook/model/river_model5.h5')  
onnx_model = keras2onnx.convert_keras(model, model.name)
temp_model_file = '/root/notebook/model/model.onnx'
onnx.save_model(onnx_model, temp_model_file)
```


