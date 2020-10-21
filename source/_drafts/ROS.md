---
title: ROS
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-07-31 20:03:00
---
> 
<!-- more -->

# 简介
## 介绍
- [官方网址](http://ros.org),[wiki](http://wiki.ros.org),[answers](http://answers.ros.org),[古月居](http://www.guyuehome.com)

- ROS诞生于2007年斯坦福STAIR项目Morgan Quigley，2008年Willow Garage接手，2013年有OSRF接管，下面是比较有代表性的版本：
  - 2010年，ROS 1.0版本
  - 2014年，ROS Indigo，2019年4月停止更新
  - 2016年，ROS Kinetic，2021年4月停止更新
  - 2017年，ROS 2.0 Ardent

## 特点
1. 点对点设计
  - 节点单元
  - 分布式网络
  - RPC+TCP/UDP通信
  - 多机协同
2. 多语言支持
  - python、`C++`、Java、lisp等
  - 语言无关的接口定义
3. 架构精简、集成度高
  - 每个节点可以单独编译
  - 集成众多开源项目：OpenCV、PCL、OpenRAVE、OpenNI
  - 接口统一、提高软件复用性
4. 组件化工具
  - 3D可视化：rviz
  - 物理仿真环境：gazebo
  - 数据记录工具：rosbag
  - Qt工具箱：rqt_*
5. 免费且开源

## 总体设计
- 通信机制、开发工具、应用功能、生态系统

### 计算图
- 节点(Node)：软件模块
- 节点管理器(ROS Master)：控制中心，提供参数管理
- 话题(Topic)：异步通信机制，传输消息
- 服务(Service)：同步通信机制，双向通信，传输请求/应答数据

#### 话题通信机制
- Talker注册
- Listener注册
- ROS Master进行信息分配
- Listener发送连接请求
- Talker确认连接请求
- 建立网络连接
- Talker向Listener发布数据

![通信建立过程，5个RPC，2个TCP](/images/pasted-299.png)


#### 服务通信机制
- Talker注册
- Listener注册
- ROS Master进行信息分配
- 建立网络连接
- Talker向Listener发布服务应答数据

![通信建立过程，3个RPC，2个TCP](/images/pasted-300.png)


#### 参数通信机制
- Talker设置变量
- Listener查询参数值
- ROS Master向Listener发送参数值

![通信建立过程，全部RPC](/images/pasted-301.png)

#### 话题和服务的区别

类别|话题|服务
-|-|-
同步性|异步|同步
通信模型|发布/订阅|C/S
底层协议|ROS TCP/ROS UDP|ROS TCP/ROS UDP
反馈机制|无|有
缓冲区|有|无
实时性|弱|强
节点关系|多对多|一对多
使用场景|数据传输|逻辑处理

### 文件系统

文件|功能说明
-|-
功能包清单(Package manifest)|记录功能包的基本信息，包含作者、许可、依赖、编译标志等
元功能包(Meta Packages)|组织多个用于同一目的的功能包
元功能包清单(Meta Packages)|类似于功能包清单，还包含运行时需要依赖的功能包或者声明一些应用标签
消息类型(Message)|ROS节点间发布/订阅的通信信息，可以使用ROS系统提供的消息类型，也可以使用.msg文件在功能包的msg文件夹下自定义需要的消息类型
服务类型(Service)|定义ROS服务器/客户端通信模型下的请求与应答数据类型，可以使用ROS系统提供的服务类型，也可以.srv文件在功能包的srv文件夹中进行定义
代码(Code)|放置功能包节点源代码的文件夹

![文件系统](/images/pasted-302.png)

### 开源社区
- 发行版(Distribution)
- 软件源(Repository)
- ROS wiki
- 邮件列表(Mailing list)
- ROS Answer
- [博客](http://www.ros.org/news)

## 安装
- 查看容器linux版本：`cat /etc/issue`
### ubuntu 16.04
- [官方安装教程](http://wiki.ros.org/)，选择合适的安装版本
- 也可以使用docker，下载ros的镜像，[教程](https://blog.csdn.net/u010904547/article/details/108375005)

安装步骤|说明
-|-
docker环境|`docker pull ubuntu:16.04`<br>`docker run -itd ubuntu:16.04 /bin/bash`<br>`docker exec -it /bin/bash`
设置sources.list|apt update <br>apt install sudo<br>`sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`<br>如果(lsb_release -sc)报错，Ubuntu-core中不包含lsb_release，通过`cat /etc/lsb-release`查看`DISTRIB_CODENAME`，确定是`xenial`，填进上面命令中:`sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu xenial main" > /etc/apt/sources.list.d/ros-latest.list'`
设置key|`sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654`
更新package|`sudo apt update`
安装ROS kinetic完整版|`sudo apt install -y ros-kinetic-desktop-full`，时间略长
初始化rosdep|`sudo rosdep init`<br>`rosdep update`
配置ROS环境|`echo 'source /opt/ros/kinetic/setup.bash' >> /root/.bashrc && source /root/.bashrc`
安装依赖项|`sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential`
测试ROS是否安装成功|小海龟仿真

- 小海龟仿真：开启三个terminal，分别执行下面的指令，可以控制小乌龟运动

```bash
roscore
rosrun turtlesim turtlesim_node
rosrun turtlesim turtle_teleop_key
rosrun rqt_graph rqt_graph
```

- 问题：`QXcbConnection: Could not connect to display Aborted`
- 解决方法：
  - 我的qt程序和打包没有任何问题，原因在于我测试的时候使用的是远程登陆方式到测试机子（裸机ubuntu环境）上运行程序，所以就是没有打开-Ｘ选项（即远程图形显示）。
  - `ssh -X 172.16.160.196（目标机子ｉｐ） -l （用户名）`，输入密码进入后
  - `export DISPLAY=192.168.17.15:0.0（自己机子的ｉｐ加上0.0）`

- [WSL2 不能使用Vcxsrv](https://blog.csdn.net/weixin_42707324/article/details/108220780)
  - 在`控制面板->Windows Defender 防火墙->允许的应用->添加其他应用`，选择vcxsrv.exe和xlancher.exe
  - WSL2使用的是VM模式，启动Vcxsrv勾选上disable access control 和输入参数-ac即可
  - 在wsl中设置(可以设置到配置文件中)：`export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0`

### ubuntu 20.04
- ubuntu20.04下的ROS Noetic[安装过程](https://blog.csdn.net/r1141207831/article/details/106327172/)，[官方链接](http://wiki.ros.org/noetic/Installation/Ubuntu)


安装步骤|说明
-|-
添加 sources.list|设置你的电脑可以从 packages.ros.org 接收软件：`sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`
添加 keys|`sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654`
安装|首先，确保你的Debian软件包索引是最新的：`sudo apt update`
安装桌面完整版 | 包含ROS、rqt、rviz、机器人通用库、2D/3D 模拟器、导航以及2D/3D感知：`sudo apt install ros-noetic-desktop-full`
环境配置|`echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc && source ~/.bashrc`

- 至此已经在Ubuntu20.04的系统中完整安装ROS  Noetic。



## 参考书
> Mastering ROS for Robotics Programming
> ROS By Example
> Programming Robots with ROS
> ROS Robotics By Example
> Learning ROS for Robotics Programming
> A Gentle Introduction to ROS
> Effective Robotics Programming With ROS
> ROS Robotics Projects
> ROS Robot Programming

## 常用命令

命令|说明
-|-
catkin_create_pkg|创建功能包
rospack|获取功能包信息
catkin_make|编译工作空间中的功能包
rosdep|自动安装功能包依赖的其他包
roscd|功能包目录跳转
roscp|拷贝功能包中的文件
rosed|编辑功能包中的文件
rosrun|运行功能包中的可执行文件
roslaunch|运行启动文件

# 工作空间
- workspace是一个存放工程开发相关文件的文件夹

目录|说明
-|-
src|代码空间(source)
build|编译空间(build)
devel|开发空间(develpment)
install|安装空间(install)

## 创建过程
- 创建工作空间

创建空间|说明
-|-
创建目录|`mkdir -p ~/catkin_ws/src && cd ~/catkin_ws/src && catkin_init_workspace`
编译空间|`cd ~/catkin_ws/ && catkin_make`
环境变量|`source devel/setup.bash`
检查|`echo $ROS_PACKAGE_PATH`

- 创建功能包
- `catkin_create_pkg 功能包名称 depend1 depend2 ...`

创建功能包|说明
-|-
创建功能包|`cd ~/catkin_ws/src && catkin_create_pkg learning_comm std_msgs rospy roscpp`
编译功能包|`cd ~/catkin_ws && cakin_make && source ~/catkin_ws/devel/setup.bash`

## 工作空间覆盖机制
- 在同一个工作空间，功能包名称唯一，但是多个工作空间中功能包名可能相同。
- ros实行的是Overlaying机制，按照环境变量中`ROS_PACKAGE_PATH`设置的顺序依次查找功能包所在路径
- 查找功能包路径：`rospack find learning_comm`

# 通信编程
## 话题编程
- 流程：
  - 创建发布者
  - 创建订阅者
  - 添加编译选项
  - 运行可执行程序

## 服务编程

## 动作编程


# 关键组件


