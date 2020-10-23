---
title: Google Colab
author: hero576
tags:
  - skills
categories:
  - common
date: 2020-10-21 18:00:45
---

> 
<!-- more -->

# 简介

## 介绍
- 谷歌推出的一个免费GPU服务器，官方对其的说明是：
> Colaboratory是一个研究项目，可免费使用。

​- Colaboratory 旨在帮助传播机器学习培训和研究成果，它是一个 Jupyter 笔记本环境，不需要进行任何设置就可以使用，并且完全在云端运行可以方便的使用Keras,TensorFlow,PyTorch等框架进行深度学习应用的开发。虚拟机配置T4 GPU，12G内存，39G硬盘空间。

​- [Google Colab官方文档](https://research.google.com/colaboratory/local-runtimes.html)
- [教程链接](https://blog.csdn.net/qq_42617455/article/details/104260490)

- Colaboratory最多只能运行12小时，时间一到就会清空VM上所有数据。这包括我们安装的软件，包括我们下载的数据，存放的计算结果，建议保存必要的数据。

# 准备工作
## 账户注册
- 谷歌浏览器
- 一个谷歌账号
- [Google Colab](https://colab.research.google.com)
​- [谷歌云盘](https://drive.google.com)

## 查看硬件信息
- bash代码可以直接运行，前面加上`!`符号

查看信息|说明
-|-
显示出你所使用的所有硬件|`from tensorflow.python.client import device_lib`<br>`device_lib.list_local_devices()`
查看CPU信息|`!cat /proc/cpuinfo`
查看内存信息|`!cat /proc/meminfo`
查看版本信息|`!cat /proc/version`
查看设备|`!cat /proc/devices`
查看GPU|`!/opt/bin/nvidia-smi`


## 库的安装
- Colab自带了Tensorflow、Matplotlib、Numpy、Pandas等深度学习基础库，如果还需要其他依赖，如Keras，需要自己安装，安装命令如下：`pip install keras`


# 运行代码
- 首先在左上角`修改 -> 笔记本设置 ->硬件加速器`设置为GPU

![设置GPU](/images/pasted-311.png)

- 下面三种方式，第一种最快，第三种最慢

## 使用记事本
- 新建一个Python记事本，写入代码，直接运行（与Jupyter的使用类似）

![使用记事本](/images/pasted-312.png)


## 利用Google Drive
### Google Drive云端硬盘配置
- 如果要跑自己的数据集就需要先把项目和数据集上传到Google Drive，可以直接把文件夹拖进去，或者新建一个文件，第一次用Google Drive需要在关联更多应用里关联Google Colaboratory
![关联Google Colaboratory](/images/pasted-313.png)

### 加载Google Drive云端硬盘

```py
from google.colab import drive 	drive.mount('/content/drive') 
```

-​ 或者在左侧文件栏中点击装载Goole云端硬盘，自动生成上述代码后运行，点击运行后会生成一个链接，点进去登录云盘，把验证码复制过来，输入之后enter

![装载Goole云端硬盘](/images/pasted-313.png)


### 切换当前文件夹
- 在左侧栏中找到项目存放路径，运行以下代码：
```py
​import os 
os.chdir("/content/drive/My Drive/Colab Notebooks/test_Mnist")
```

![切换当前文件夹](/images/pasted-313.png)


### 运行代码
```bash
​python your_filename 
```


## 连接到本地的Jupyter服务器
- 具体可参考[Colab官方文档](https://research.google.com/colaboratory/local-runtimes.html)

### 本地配置
- 打开本地Jupyter，安装并启用jupyter_http_over_ws扩展程序（一次性），输入代码：
```
！pip install jupyter_http_over_ws ！jupyter serverextension enable *--py jupyter_http_over_ws 
```
### 连接远程
- 以管理员身份运行Anaconda Prompt，输入命令：（port对应的端口号是自定义的）
```bash
jupyter notebook --NotebookApp.allow_origin='https://colab.research.google.com' --port=8899 --NotebookApp.port_retries=0 
```
- 然后会弹出Jupyter的新界面，注意使用的浏览器切换成谷歌

### 执行代码
- 在Colab右上角选择连接到本地代码执行程序，输入自定义的端口号：
![连接到本地](/images/pasted-314.png)

> 补充：使用Pytorch
> 输入以下代码，运行后会输出一个链接，点击进入visdom，如果不行，修改URL，将https， 修改为http便可
```py
! npm install -g localtunnel  # 8097是自己设置的端口号，可修改为自己要用的端口号
 get_ipython().system_raw('python3 -m pip install visdom')  
 get_ipython().system_raw('python3 -m visdom.server -port 8097 >> visdomlog.txt 2>&1 &')    
 get_ipython().system_raw('lt --port 8097 >> url.txt 2>&1 &')    
 import time  
 time.sleep(5) 
 ! cat url.txt  
 import visdom  
 time.sleep(5)  
 vis = visdom.Visdom(port='8097')    
 print(vis) 
 time.sleep(3)  
 vis.text('testing')  
 ! cat visdomlog.txt 
```
