---
title: ROS
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-10-23 14:55:38
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

### ARM
- ARM板安装[教程](http://wiki.ros.org/indigo/Installation/UbuntuARM)

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

### 流程
#### 创建发布者
```cpp
int main(int argc, char **argv)
{
  // ROS节点初始化
  ros::init(argc, argv, "talker");
  // 创建节点句柄
  ros::NodeHandle n;
  // 创建一个Publisher，发布名为chatter的topic，消息类型为std_msgs::String
  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
  // 设置循环的频率
  ros::Rate loop_rate(10);
  int count = 0;
  while (ros::ok())
  {
	// 初始化std_msgs::String类型的消息
    std_msgs::String msg;
    std::stringstream ss;
    ss << "hello world " << count;
    msg.data = ss.str();
	// 发布消息
    ROS_INFO("%s", msg.data.c_str());
    chatter_pub.publish(msg);
	// 循环等待回调函数
    ros::spinOnce();
	// 按照循环频率延时
    loop_rate.sleep();
    ++count;
  }
  return 0;
}
```


#### 创建订阅者
```cpp
// 接收到订阅的消息后，会进入消息回调函数
void chatterCallback(const std_msgs::String::ConstPtr& msg)
{
  // 将接收到的消息打印出来
  ROS_INFO("I heard: [%s]", msg->data.c_str());
}
int main(int argc, char **argv)
{
  // 初始化ROS节点
  ros::init(argc, argv, "listener");
  // 创建节点句柄
  ros::NodeHandle n;
  // 创建一个Subscriber，订阅名为chatter的topic，注册回调函数chatterCallback
  ros::Subscriber sub = n.subscribe("chatter", 1000, chatterCallback);
  // 循环等待回调函数
  ros::spin();
  return 0;
}
```

#### 添加编译选项
- 修改功能包项目目录下的`vi ~/catkin_ws/src/learning_comm/CMakeList.txt`文件，增加编译选项，

```makefile
add_executable(talker src/talker.cpp)
target_link_libraries(talker ${catkin_LIBRARIES}) # 链接第三方依赖
#add_dependencies(talker ${PROJECT_NAME}_generate_messages_cpp) # 添加其他第三方依赖
add_executable(listener src/listener.cpp)
target_link_libraries(listener ${catkin_LIBRARIES})
#add_dependencies(listener ${PROJECT_NAME}_generate_messages_cpp)
```

- 在工作空间路径下，执行`cd ~/catkin_ws/ && catkin_make`


#### 运行可执行程序
- 启动roscore
- 执行：`rosrun learning_comm talker`，`rosrun learning_comm listener`

### 自定义话题
#### 定义msg文件
- 创建`person.msg`文件，一般放在功能包下的`msg`文件夹下
```c
string name
uint8 sex
uint8 age
uint8 unknown=0
uint8 male=1
uint8 female=2
```

#### 在package.xml添加功能包依赖
```xml
<build_depend>message_generation</build_depend>
<exec_depend>message_runtime</exec_depend>
```

#### 在CMakeList.txt添加编译选项
```makefile
find_package(... message_generation)
catkin_package(CATKIN_DEPENDS geometry_msgs roscpp rospy std_msgs message_runtime)
add_message_files(FILES person.msg)
generate_messages(DEPENDENCIES std_msgs)
```

#### 查看自定义话题
- `rosmsg show person`可以查看到自定义内容，就可以包含头文件使用了

## 服务编程
- 流程：
  - 创建服务器
  - 创建客户端
  - 添加编译选项
  - 运行可执行程序

### 创建srv定义
- 定义srv文件，与msg一样，在功能包中创建srv文件夹，添加`AddTwoInts.srv`文件
```yaml
int64 a
int64 b
---
int64 sum
```

  - `---`分割了输入和输出数据

- 在package.xml添加功能包依赖
```xml
<build_depend>message_generation</build_depend>
<exec_depend>message_runtime</exec_depend>
```

- 在CMakeList.txt添加编译选项
```makefile
find_package(... message_generation)
catkin_package(CATKIN_DEPENDS geometry_msgs roscpp rospy std_msgs message_runtime)
add_service_files(FILES AddTwoInts.srv)
```

### 创建服务端
```cpp
// service回调函数，输入参数req，输出参数res
bool add(learning_comm::AddTwoInts::Request  &req,
         learning_comm::AddTwoInts::Response &res)
{
  // 将输入参数中的请求数据相加，结果放到应答变量中
  res.sum = req.a + req.b;
  ROS_INFO("request: x=%ld, y=%ld", (long int)req.a, (long int)req.b);
  ROS_INFO("sending back response: [%ld]", (long int)res.sum);
  return true;
}
int main(int argc, char **argv)
{
  // ROS节点初始化
  ros::init(argc, argv, "add_two_ints_server");
  // 创建节点句柄
  ros::NodeHandle n;
  // 创建一个名为add_two_ints的server，注册回调函数add()
  ros::ServiceServer service = n.advertiseService("add_two_ints", add);
  // 循环等待回调函数
  ROS_INFO("Ready to add two ints.");
  ros::spin();
  return 0;
}
```

### 创建客户端
```cpp
int main(int argc, char **argv)
{
  // ROS节点初始化
  ros::init(argc, argv, "add_two_ints_client");
  // 从终端命令行获取两个加数
  if (argc != 3)
  {
    ROS_INFO("usage: add_two_ints_client X Y");
    return 1;
  }
  // 创建节点句柄
  ros::NodeHandle n;
  // 创建一个client，请求add_two_int service，service消息类型是learning_comm::AddTwoInts
  ros::ServiceClient client = n.serviceClient<learning_comm::AddTwoInts>("add_two_ints");
  // 创建learning_comm::AddTwoInts类型的service消息
  learning_comm::AddTwoInts srv;
  srv.request.a = atoll(argv[1]);
  srv.request.b = atoll(argv[2]);
  // 发布service请求，等待加法运算的应答结果
  if (client.call(srv))
  {
    ROS_INFO("Sum: %ld", (long int)srv.response.sum);
  }
  else
  {
    ROS_ERROR("Failed to call service add_two_ints");
    return 1;
  }
  return 0;
}
```

#### 添加编译选项
- 修改功能包项目目录下的`vi ~/catkin_ws/src/learning_comm/CMakeList.txt`文件，增加编译选项，

```makefile
add_executable(server src/server.cpp)
target_link_libraries(server ${catkin_LIBRARIES}) # 链接第三方依赖
add_dependencies(server ${PROJECT_NAME}_gencpp) # 添加其他第三方依赖
add_executable(client src/client.cpp)
target_link_libraries(client ${catkin_LIBRARIES})
add_dependencies(client ${PROJECT_NAME}_gencpp)
```

- 在工作空间路径下，执行`cd ~/catkin_ws/ && catkin_make`


#### 运行可执行程序
- 启动roscore
- 执行：`rosrun learning_comm server`，`rosrun learning_comm client`

## 动作编程
- 动作(action)：是一种问答通信值，带有连续反馈，可以再任务过程中运行的基于ROS消息机制的实现

Action接口|说明
-|-
goal|发布任务目标
cancel|请求取消任务
status|通知客户端当前状态
feedback|周期反馈任务运行的监控数据
result|向客户端发送任务的执行结果，只发送一次

### 创建action定义
- 定义action文件，在功能包中创建action文件夹，添加`DoDishes.action`文件
```yaml
# Define the goal
uint32 dishwasher_id  # Specify which dishwasher we want to use
---
# Define the result
uint32 total_dishes_cleaned
---
# Define a feedback message
float32 percent_complete
```
  - `---`分割了目标信息、结果信息、周期反馈信息的定义


- 在package.xml添加功能包依赖
```xml
<build_depend>actionlib</build_depend>
<build_depend>actionlib_msgs</build_depend>
<exec_depend>actionlib</exec_depend>
<exec_depend>actionlib_msgs</exec_depend>
```

- 在CMakeList.txt添加编译选项
```makefile
find_package(actionlib_msgs actionlib)
add_action_files(DIRECTORY action FILES DoDishes.action)
generate_messages(DEPENDENCIES actionlib_msgs)
```

### 创建服务器
```cpp
typedef actionlib::SimpleActionServer<learning_comm::DoDishesAction> Server;
// 收到action的goal后调用该回调函数
void execute(const learning_comm::DoDishesGoalConstPtr& goal, Server* as)
{
    ros::Rate r(1);
    learning_comm::DoDishesFeedback feedback;
    ROS_INFO("Dishwasher %d is working.", goal->dishwasher_id);
    // 假设洗盘子的进度，并且按照1hz的频率发布进度feedback
    for(int i=1; i<=10; i++)
    {
        feedback.percent_complete = i * 10;
        as->publishFeedback(feedback);
        r.sleep();
    }
    // 当action完成后，向客户端返回结果
    ROS_INFO("Dishwasher %d finish working.", goal->dishwasher_id);
    as->setSucceeded();
}
int main(int argc, char** argv)
{
    ros::init(argc, argv, "do_dishes_server");
    ros::NodeHandle n;
    // 定义一个服务器
    Server server(n, "do_dishes", boost::bind(&execute, _1, &server), false);
    // 服务器开始运行
    server.start();
    ros::spin();
    return 0;
}
```

### 创建客户端
```cpp
typedef actionlib::SimpleActionClient<learning_comm::DoDishesAction> Client;
// 当action完成后会调用该回调函数一次
void doneCb(const actionlib::SimpleClientGoalState& state,
        const learning_comm::DoDishesResultConstPtr& result)
{
    ROS_INFO("Yay! The dishes are now clean");
    ros::shutdown();
}
// 当action激活后会调用该回调函数一次
void activeCb()
{
    ROS_INFO("Goal just went active");
}
// 收到feedback后调用该回调函数
void feedbackCb(const learning_comm::DoDishesFeedbackConstPtr& feedback)
{
    ROS_INFO(" percent_complete : %f ", feedback->percent_complete);
}
int main(int argc, char** argv)
{
    ros::init(argc, argv, "do_dishes_client");
    // 定义一个客户端
    Client client("do_dishes", true);
    // 等待服务器端
    ROS_INFO("Waiting for action server to start.");
    client.waitForServer();
    ROS_INFO("Action server started, sending goal.");
    // 创建一个action的goal
    learning_comm::DoDishesGoal goal;
    goal.dishwasher_id = 1;
    // 发送action的goal给服务器端，并且设置回调函数
    client.sendGoal(goal,  &doneCb, &activeCb, &feedbackCb);
    ros::spin();
    return 0;
}
```

#### 添加编译选项
- 修改功能包项目目录下的`vi ~/catkin_ws/src/learning_comm/CMakeList.txt`文件，增加编译选项，

```makefile
add_executable(DoDishes_server src/DoDishes_server.cpp)
target_link_libraries(DoDishes_server ${catkin_LIBRARIES}) # 链接第三方依赖
add_dependencies(DoDishes_server ${${PROJECT_NAME}_EXPORTED_TARGETS}) # 添加其他第三方依赖
add_executable(DoDishes_client src/DoDishes_client.cpp)
target_link_libraries(DoDishes_client ${catkin_LIBRARIES})
add_dependencies(DoDishes_client ${${PROJECT_NAME}_EXPORTED_TARGETS})
```

- 在工作空间路径下，执行`cd ~/catkin_ws/ && catkin_make`


#### 运行可执行程序
- 启动roscore
- 执行：`rosrun learning_comm server`，`rosrun learning_comm client`


# 分布式通信
- ROS是一种分布式软件框架，节点之间通难过松耦合的方式进行组合

## 实现多级通信
- 记录每台服务器的ip地址：`ipconfig`，可以通过`/etc/hosts`文件进行映射`xx.xx.xx.x s1`，使用`ping s1`查看是否连通
- 多台分布式服务中，只有一台是ROS Master，其余服务必须设置ROS Master的IP到环境变量：`echo "export ROS_MASTER_URI=http://s1:11311" >> ~/.bashrc`

# 关键组件
## Launch文件
- 使用Launch文件通过xml文件实现多节点的配置和启动，可自动启动ROS Master
- [官方介绍](http://wiki.ros.org/roslaunch/XML)

- 结构：
```
launch
|--node
|--param
|--rosparam
|--arg
```
标签|说明
-|-
launch|根元素
node|启动节点，属性：<br>pkg：节点所在的功能包名称<br>type：节点的可执行文件名称<br>name：节点运行时的名称<br>args：节点参数<br>output：控制输出<br>respawn：是否重新启动<br>required：是否必须，若是则此节点必须启动<br>ns：命名空间
param|设置ROS系统运行中的参数，储存在参数服务器中，属性<br>name：参数名<br>value：参数值
rosparam|加载参数文件中多个参数：`<rosparam file="params.yaml" command="load" ns="params" />`
arg|launch文件内部的局部变量，仅限于launch文件使用，<br>设置：`<arg name="arg-name" default="arg-value" />`<br>调用：`<param name="foo" value="$(arg arg-name)">`。属性<br>name：参数名<br>value：参数值
remap|重映射ROS计算图资源的命名：`<remap from="/turtlebot/cmd_vel" to="/cmd_vel" />`，属性：<br>from：原命名<br>to：映射之后的名称
include|包含其他launch文件，`<include file="$(dirname)/other.launch" />`，属性：<br>file：其他launch文件路径


- 示例
```xml
 <launch>
    <!-- 海龟仿真器 -->
    <node pkg="turtlesim" type="turtlesim_node" name="sim"/>
    <!-- 键盘控制 -->
    <node pkg="turtlesim" type="turtle_teleop_key" name="teleop" output="screen"/>
    <!-- 两只海龟的tf广播 -->
    <node pkg="learning_tf" type="turtle_tf_broadcaster" args="/turtle1" name="turtle1_tf_broadcaster" />
    <node pkg="learning_tf" type="turtle_tf_broadcaster" args="/turtle2" name="turtle2_tf_broadcaster" />
    <!-- 监听tf广播，并且控制turtle2移动 -->
    <node pkg="learning_tf" type="turtle_tf_listener" name="listener" />
 </launch>
```

## TF坐标变换
- TF可以提供和管理机器人相关的坐标变换、位置等信息的计算。
- 实现TF坐标变换的方式：广播TF变换、监听TF变换

### 实现TF广播器
- 定义TF广播器
- 创建坐标变换值
- 发送坐标变换

```cpp
#include <ros/ros.h>
#include <tf/transform_broadcaster.h>
#include <turtlesim/Pose.h>
std::string turtle_name;
void poseCallback(const turtlesim::PoseConstPtr& msg)
{
    // tf广播器
    static tf::TransformBroadcaster br;
    // 根据乌龟当前的位姿，设置相对于世界坐标系的坐标变换
    tf::Transform transform;
    transform.setOrigin( tf::Vector3(msg->x, msg->y, 0.0) );
    tf::Quaternion q;
    q.setRPY(0, 0, msg->theta);
    transform.setRotation(q);
    // 发布坐标变换
    br.sendTransform(tf::StampedTransform(transform, ros::Time::now(), "world", turtle_name));
}
int main(int argc, char** argv)
{
    // 初始化节点
    ros::init(argc, argv, "my_tf_broadcaster");
    if (argc != 2)
    {
        ROS_ERROR("need turtle name as argument"); 
        return -1;
    };
    turtle_name = argv[1];
    // 订阅乌龟的pose信息
    ros::NodeHandle node;
    ros::Subscriber sub = node.subscribe(turtle_name+"/pose", 10, &poseCallback);
    ros::spin();
    return 0;
};
```

### 实现TF监听器
- 定义TF监听器
- 查找坐标变换

```cpp
#include <ros/ros.h>
#include <tf/transform_listener.h>
#include <geometry_msgs/Twist.h>
#include <turtlesim/Spawn.h>
int main(int argc, char** argv)
{
    // 初始化节点
    ros::init(argc, argv, "my_tf_listener");
    ros::NodeHandle node;
    // 通过服务调用，产生第二只乌龟turtle2
    ros::service::waitForService("spawn");
    ros::ServiceClient add_turtle =
    node.serviceClient<turtlesim::Spawn>("spawn");
    turtlesim::Spawn srv;
    add_turtle.call(srv);
    // 定义turtle2的速度控制发布器
    ros::Publisher turtle_vel =
    node.advertise<geometry_msgs::Twist>("turtle2/cmd_vel", 10);
    // tf监听器
    tf::TransformListener listener;
    ros::Rate rate(10.0);
    while (node.ok())
    {
        tf::StampedTransform transform;
        try
        {
            // 查找turtle2与turtle1的坐标变换
            listener.waitForTransform("/turtle2", "/turtle1", ros::Time(0), ros::Duration(3.0));
            listener.lookupTransform("/turtle2", "/turtle1", ros::Time(0), transform);
        }
        catch (tf::TransformException &ex) 
        {
            ROS_ERROR("%s",ex.what());
            ros::Duration(1.0).sleep();
            continue;
        }
        // 根据turtle1和turtle2之间的坐标变换，计算turtle2需要运动的线速度和角速度
        // 并发布速度控制指令，使turtle2向turtle1移动
        geometry_msgs::Twist vel_msg;
        vel_msg.angular.z = 4.0 * atan2(transform.getOrigin().y(),
        vel_msg.linear.x = 0.5 * sqrt(pow(transform.getOrigin().x(), 2) +
                                      pow(transform.getOrigin().y(), 2));
        turtle_vel.publish(vel_msg);
        rate.sleep();
    }
    return 0;
};
```

### 编译代码
```makefile
add_executable(turtle_tf src/turtle_tf.cpp)
target_link_libraries(turtle_tf ${catkin_LIBRARIES}) # 链接第三方依赖
```

## Qt工具箱

工具|说明
-|-
rqt_console|日志输出工具
rqt_graph|计算图可视化工具
rqt_plot|数据绘图工具
rqt_reconfigure|参数动态配置工具

## Rviz可视化平台
- Rviz是一款三维可视化工具，可以很好的兼容基于ROS软件框架的机器人平台
- 启动：`rosrun rviz`

## Gazebo物理仿真环境
- 三维物理仿真平台，可以用测测试机器人算法，带有物理属性
- 启动：`roslaunch gazebo_ros empty_world.launch`


# 机器人系统
## 构建

### 连接摄像头
- 下载：`sudo apt-get install ros-kinetic-usb-cam`
- 打开示例：`roslaunch usb_cam usb_cam-test.launch`
- 使用Qt查看主题：`rqt_image_view`
- 主题：`~<camera_name>/image`，类型：`sensor_msgs/Image`

参数| 类型| 默认值| 描述
-|-|-|-
~video_device| string| "/dev/video0"| 摄像头设备号|
~image_width| int| 640 图像横向分辨率|
~image_height| int| 480 图像纵向分辨率|
~pixel_format| string| "mjpeg" |像素编码，可选值： mjpeg， yuyv， uyvy|
~io_method| string| "mmap" |IO通道，可选值： mmap， read， userptr|
~camera_frame_id| string| "head_camera"| 摄像头坐标系|
~framerate| int| 30 |帧率|
~brightness| int| 32 |亮度， 0~255|
~saturation| int| 32 |饱和度， 0~255|
~contrast| int| 32 |对比度， 0~255|
~sharpness| int| 22 |清晰度， 0~255|
~autofocus| bool| false |自动对焦|
~focus| int| 51 |焦点（非自动对焦状态下有效）|
~camera_info_url| string| - |摄像头校准文件路径|
~camera_name| string| "head_camera"| 摄像头名称

- launch文件
```xml
<launch>
  <node name="usb_cam" pkg="usb_cam" type="usb_cam_node" output="screen">
    <param name="video_device" value="/dev/video0" />
    <param name="image_width" value="640" />
    <param name="image_height" value="480" />
    <param name="pixel_format" value="yuyv" />
    <param name="camera_frame_id" value="usb_cam" />
    <param name="io_method" value="mmap" />
  </node>
  <node name="image_view" pkg="image_view" type="image_view" respawn="false" output="screen">
    <remap from="image" to="/usb_cam/image_raw"/>
    <param name="autosize" value="true"/>
  </node>
</launch>
```

### 连接kinect

- 安装
```bash
sudo apt-get install ros-kinetic-freenect-*
git clone https://github.com/avin2/SensorKinect.git
cd SensorKinect/Bin
tar xvf SensorKinect093-Bin-Linux-x86-v5.1.2.1.tar.bz2
tar xvf SensorKinect093-Bin-Linux-x64-v5.1.2.1.tar.bz2
sudo ./install.sh
```

- 话题和服务

话题名称 |类型| 描述
-|-|-
rgb/camera_info| sensor_msgs/CameraInfo| RGB相机校准信息
rgb/image_raw| sensor_msgs/Image| RGB相机图像数据
depth/camera_info| sensor_msgs/CameraInfo| 深度相机校准信息
depth/image_raw| sensor_msgs/Image| 深度相机数据
depth_registered/camera_info| sensor_msgs/CameraInfo| 配准后的深度相机校准信息
depth_registered/image_raw| sensor_msgs/Image| 配准后的深度相机数据
ir/camera_info| sensor_msgs/CameraInfo| 红外相机校准信息
ir/image_raw| sensor_msgs/Image| 红外相机数据
projector/camera_info| sensor_msgs/CameraInfo| 深度相机校准信息
/diagnostics |diagnostic_msgs/DiagnosticArray |传感器诊断信息

Services名称 |类型| 描述
-|-|-
rgb/set_camera_info |sensor_msgs/SetCameraInfo|设置RGB相机的校准信息
ir/set_camera_info |sensor_msgs/SetCameraInfo|设置红外相机的校准信息

- 启动kinect的launch文件
```xml
<launch>
  <include file="$(find freenect_launch)/launch/freenect.launch">
    <arg name="publish_rf" value="false"/>
    <arg name="depth_registration" value="true"/>
    <arg name="rgb_processing" value="true"/>
    <arg name="ir_processing" value="false"/>
    <arg name="depth_processing" value="false"/>
    <arg name="depth_registered_prossing" value="true"/>
    <arg name="displarity_processing" value="false"/>
    <arg name="displarity_registered_processing" value="false"/>
    <arg name="sw_registered_prossing" value="false"/>
    <arg name="hw_registered_prossing" value="true"/>
  </include>
</launch>
```

### 连接激光雷达
- 安装
```bash
sudo apt-get install ros-kinetic-rplidar-ros
rosrun rplidar_ros rplidarNode
rostopic list
```

- 话题和服务

项目|名称 |类型| 描述
-|-|-|-
Topic发布 |scan |sensor_msgs/Laser Scan |发布激光雷达数据
Services |stop_motor |std_srvs/Empty |停止旋转电机
Services |start_motor |std_srvs/Empty |开始旋转电机


- 参数

参数 |类型| 默认值| 描述
-|-|-|-
serial_port| string| "/dev/ttyUSB0" |激光雷达串口名称
serial_baudrate| int| 115200| 串口波特率
frame_id| string| "laser"| 激光雷达坐标系
inverted| bool| false| 是否倒置安装
angle_compensate| bool| true| 角度补偿


- 增加当前用户的串口权限:`sudo gpasswd --add USER_NAME xxx`

## URDF机器人建模
- [官方教程](http://wiki.ros.org/urdf/Tutorials)

### URDF定义
- Unified Robot Description Format，同一机器人描述格式，使用XML格式描述机器人模型，有n个连杆link和m个关节joint组成。

```xml
<robot name="robot1">
  <link>..</link>
  <joint>..</joint>
</robot>
```

- link：描述机器人外观和物理属性：尺寸size、颜色color、形状shape、惯性矩阵inertial matrix、碰撞参数collision properties
  - visual：外观参数
  - inertial：惯性参数
  - collision：碰撞属性

- joint：描述关节运动学和动力学属性，包括关节运动的位置和速度限制。

关节类型|描述
-|-
continuous|旋转关节，可以围绕单轴无限旋转
revolute|旋转关节，类似于continuous，但有角度极限
prismatic|滑动关节，沿某一轴线移动，带有位置极限
planar|平面关节，允许在平面正交方向上平移和旋转
floating|滑动关节，允许进行平移、旋转
fixed|固定关节，不允许运动的特殊关节

标签|说明
-|-
parent|主连接
child|副连接
calibration|关节参考位置，用来校准关节的绝对位置
dynamics|描述关节的物理属性，例如阻尼脂、物理静摩擦力等，经常在动力学仿真中用到
limit|描述运动的一些极限值，包括关节运动的上下限位置、速度限制、力矩限制等
mimc|描述关节与已有关节的关系
safety_controller|描述安全控制器参数

- joint必须制定关节连接杆的主连接和副链接

```xml
<joint name="joint1" type="continuous">
  <parent link="parent_link" />
  <child link="child_link" />
  <calibration ... />
  <dynamics dampling ... />
  <limit effort ... />
</joint>
```

### 创建建模的功能包

- 创建指令：`catkin_create_pkg mbot_description urdf xacro`，mbot_description是自定义的名称
- 创建目录结构：
  - urdf：存放机器人模型的urdf或xacro文件
  - meshes：放置urdf中引用的模型渲染文件
  - launch：保存相关启动文件
  - config：保存rviz的配置文件

- 建模
```xml
<launch>
	<param name="robot_description" textfile="$(find mbot_description)/urdf/mbot_base.urdf" />
	<!-- 设置GUI参数，显示关节控制插件 -->
	<param name="use_gui" value="true"/>
	<!-- 运行joint_state_publisher节点，发布机器人的关节状态  -->
	<node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher" />
	<!-- 运行robot_state_publisher节点，发布tf  -->
	<node name="robot_state_publisher" pkg="robot_state_publisher" type="state_publisher" />
	<!-- 运行rviz可视化界面 -->
	<node name="rviz" pkg="rviz" type="rviz" args="-d $(find mbot_description)/config/mbot_urdf.rviz" required="true" />
</launch>
```

display_mrobot_chassis_urdf.launch文件中，节点robot_state_publisher标签的type写错了，不是type=“state_publisher”，而是type=“robot_state_publisher”

### 模型文件转pdf
- 安装：`sudo apt-get install liburdfdom-tools`
- `urdf_to_graphiz mbot.urdf`

![模型文件](/images/pasted-305.png)


## URDF进化版本-xacro模型文件
- URDF模型文件太过冗长，使用xacro可以精简代码，提供可编程的接口。
- [官方介绍](http://wiki.ros.org/xacro)

### 模型定义
#### 常量
- 定义：`<xacro:property name="M_PI" value="3.14159">`
- 使用：`<origin xyz="0 0 0" rpy="${M_PI/2} 0 0">`

#### 数学计算
- 定义：`<origin xyz="0 ${(motor_length+wheel_length)/2} 0" rpy="0 0 0">`
- 所有数学运算都会转换为浮点数，以保证运算精度

#### 宏定义
- 定义：`<xacro:macro name="name" params="A B C"></xacro:macro>`
- 使用：`<name A="A_value" B="B_value" C="C_value>"`

#### 文件包含
- `<xacro:include filename="$(find mbot_description)/urdf/xacro/mbot_base.xacro">`


### 模型创建
- 创建模型有两种方法
  - 转换为urdf：`rosrun xacro xacro.py mbot.xacro > mbot.urdf`
  - 在launch中直接调用xacro文件解析器：`<arg name="model" default="$(find xacro)/xacro --inorder'$(find mbot_description)/urdf/xacro/mbot.xacro'" />`，`<param name="robot_description" command="$(arg model)" />`


# ArbotiX+rviz功能仿真
- ArbotiX是一款控制电机、舵机的硬件控制板，提供了ROS的功能包和一个差速器，通过接收速度控制指令，更新机器人里程计状态。[wiki](http://wiki.ros.org/arbotix)

## 使用
- 安装：`sudo apt-get install ros-indigo-arbotix-*`，kinetic需要源码安装：`git clone https://github.com/vanadiumlabs/arbotix_ros.git && catkin_make`

## 搭建仿真环境
- 创建launch文件
```xml
<node name="arbotix" pkg="arbotix_python" type="arbotix_driver" output="screen">
  <rosparam file="$(find mbot_description)/config/fake_mbot_arbotix.yaml" command="load">
  <param name="sim" value="true">
</node>
```

- 创建配置文件：`fake_mbot_arbotix.yaml`
```yaml
controllers: {
  base_controller: {
    type: diff_controller, 
    base_frame_id: base_footprint, 
    base_width: 0.26, 
    ticks_meter: 4100, 
    Kp: 12, 
    Kd: 12, 
    Ki: 0, 
    Ko: 50, 
    accel_limit: 1.0 
  }
}
```

- 启动仿真器：`roslaunch mbot_description arbotix_mbot_with_camera_xacro.launch`

- 启动键盘控制：`roslaunch mbot_teleop mbot_teleop.launch`

## 导航仿真案例
- 参考：《ROS by Example》
```bash
roslaunch rbx1_bringup fake_turtlebot.launch
roslaunch rbx1_nav fake_amcl.lauch map:test_map.yaml
rosrun rviz rviz -d `rospack find rbx1_nav` /amcl.rviz
```

# Gazebo物理仿真
- [官方仿真教程](http://www.gazebosim.org/tutorials)
- [插件](http://www.guyuehome.com/388)

## ros_control功能包
- ros_control功能包为开发者提供机器人控制中间件，包含一系列控制接口、传动装置接口、硬件接口、控制器工具等。[具体介绍](https://github.com/ros-controls/ros-control/wiki/controller_interface)
- [使用总结](http://www.guyuehome.com/890)

![控制器流程图](/images/pasted-306.png)

- 控制器管理器：提供一种通用的接口来管理不同的控制器
- 控制器：读取硬件状态，发布控制命令，完成每个joint控制
- 硬件资源：为上下两层提供硬件资源的接口
- 机器人硬件抽象：机器人硬件抽象和硬件资源直接打交道，通过write和read方式完成硬件操作
- 真实机器人：执行接收到的命令

## 机器人仿真
### 配置机器人模型
- 为link添加惯性参数和碰撞属性
```xml
<xacro:macro name="cylinder_inertial_matrix" params="m r h">
    <inertial>
        <mass value="${m}" />
        <inertia ixx="${m*(3*r*r+h*h)/12}" ixy = "0" ixz = "0"
            iyy="${m*(3*r*r+h*h)/12}" iyz = "0"
            izz="${m*r*r/2}" /> 
    </inertial>
</xacro:macro>
<link name="${prefix}_wheel_link">
    <visual>...</visual>
    <collision>
        <origin xyz="0 0 0" rpy="${M_PI/2} 0 0" />
        <geometry>
            <cylinder radius="${wheel_radius}" length = "${wheel_length}"/>
        </geometry>
    </collision>
    <cylinder_inertial_matrix  m="${wheel_mass}" r="${wheel_radius}" h="${wheel_length}" />
</link>
```

- 为link添加gazebo标签
```xml
<gazebo reference="${prefix}_wheel_link">
    <material>Gazebo/Gray</material>
</gazebo>
<gazebo reference="base_footprint">
    <turnGravityOff>false</turnGravityOff>
</gazebo>
```

- 为joint添加传动装置
```xml
<transmission name="${prefix}_wheel_joint_trans">
    <type>transmission_interface/SimpleTransmission</type>
    <joint name="${prefix}_wheel_joint" >
        <hardwareInterface>hardware_interface/VelocityJointInterface</hardwareInterface>
    </joint>
    <actuator name="${prefix}_wheel_joint_motor">
        <hardwareInterface>hardware_interface/VelocityJointInterface</hardwareInterface>
        <mechanicalReduction>1</mechanicalReduction>
    </actuator>
</transmission>
```

- 添加gazebo控制器插件
```xml
<gazebo>
    <plugin name="differential_drive_controller" 
            filename="libgazebo_ros_diff_drive.so">
        <rosDebugLevel>Debug</rosDebugLevel>
        <publishWheelTF>true</publishWheelTF>
        <robotNamespace>/</robotNamespace>
        <publishTf>1</publishTf>
        <publishWheelJointState>true</publishWheelJointState>
        <alwaysOn>true</alwaysOn>
        <updateRate>100.0</updateRate>
        <legacyMode>true</legacyMode>
        <leftJoint>left_wheel_joint</leftJoint>
        <rightJoint>right_wheel_joint</rightJoint>
        <wheelSeparation>${wheel_joint_y*2}</wheelSeparation>
        <wheelDiameter>${2*wheel_radius}</wheelDiameter>
        <broadcastTF>1</broadcastTF>
        <wheelTorque>30</wheelTorque>
        <wheelAcceleration>1.8</wheelAcceleration>
        <commandTopic>cmd_vel</commandTopic>
        <odometryFrame>odom</odometryFrame> 
        <odometryTopic>odom</odometryTopic> 
        <robotBaseFrame>base_footprint</robotBaseFrame>
    </plugin>
</gazebo> 
```

### 创建仿真环境

- 仿真的环境搭建有两种方式：
  - gazebo直接添加默认模型
  - 使用building editor

#### 创建空的仿真环境
- 创建空的仿真环境
```xml
<launch>
    <!-- 设置launch文件的参数 -->
    <arg name="paused" default="false"/>
    <arg name="use_sim_time" default="true"/>
    <arg name="gui" default="true"/>
    <arg name="headless" default="false"/>
    <arg name="debug" default="false"/>
    <!-- 运行gazebo仿真环境 -->
    <include file="$(find gazebo_ros)/launch/empty_world.launch">
        <arg name="debug" value="$(arg debug)" />
        <arg name="gui" value="$(arg gui)" />
        <arg name="paused" value="$(arg paused)"/>
        <arg name="use_sim_time" value="$(arg use_sim_time)"/>
        <arg name="headless" value="$(arg headless)"/>
    </include>
    <!-- 加载机器人模型描述参数 -->
    <param name="robot_description" command="$(find xacro)/xacro --inorder '$(find mbot_description)/urdf/xacro/gazebo/mbot_gazebo.xacro'" /> 
    <!-- 运行joint_state_publisher节点，发布机器人的关节状态  -->
    <node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher" ></node> 
    <!-- 运行robot_state_publisher节点，发布tf  -->
    <node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher"  output="screen" >
        <param name="publish_frequency" type="double" value="50.0" />
    </node>
    <!-- 在gazebo中加载机器人模型-->
    <node name="urdf_spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen"
          args="-urdf -model mrobot -param robot_description"/> 
</launch>
```

- 进入空的环境：`roslaunch mbot_gazebo view_mbot_gazebo_empty_world.launch`

> 默认模型需要下载，外网下载过慢，可以提前下载目录：`~/.gazebo/models`，[下载链接](https://bitbucket.org/org/osrf/gazebo_models/downloads)


### 开始仿真
- 启动仿真环境：`roslaunch mbot_gazebo view_mbot_gazebo_play_ground.launch`
- 键盘控制：`roslaunch mbot_teleop mbot_teleop.launch`

- 还有摄像头控制、kinect控制、雷达控制等launch示例，可参照古月居的代码


# 机器视觉
## ROS中的图像数据
- 显示图像的类型：`roslaunch usb_cam usb_cam-test.launch`,`rostopic info /usb_cam/image_raw`
- 查看原始图像消息：`rosmsg show sensor_msgs/Image`
- 查看压缩图像消息：`rosmsg show sensor_msgs/CompressedImage`

- 查看三维图像类型：`roslaunch freenect freenect.launch`,`rostopic info /camera/depth_registered/points`
- 查看三维图像消息：`rosmsg show sensor_msgs/PointCloud2`

## 摄像头标定
- 由于摄像头的内部或外部原因，图像会发生畸变，造成误差，通过标定，可以校准获取的图像。

### 标定过程
#### 普通摄像头
- 安装标定功能包：`sudo apt-get install ros-kinetic-camera-calibration`
- 标定流程：
  - 启动摄像头：`roslaunch robot_vision usb_cam.launch`
  - 启动标定包：`rosrun camera_calibration cameracalibrator.py --size 8X6 --square 0.024 image:=/usb_cam/image_raw camera:=/usb_cam`
    - size：指定棋盘格的内部交点个数
    - square：对应每个棋盘格的边长，单位米
    - image、camera：设置摄像头发布的图像话题
  - 在图像界面中移动棋盘格，使X、Y、Size、Skew满足标定的需求，完成后输出标定的值。
```yaml
image_width: 640
image_height: 480
camera_name: head_camera
camera_matrix:
  rows: 3
  cols: 3
  data: [514.453865, 0.000000, 368.311232, 0.000000, 514.428903, 224.580904, 0.000000, 0.000000, 1.000000]
distortion_model: plumb_bob
distortion_coefficients:
  rows: 1
  cols: 5
  data: [0.031723, -0.045138, 0.004146, 0.006205, 0.000000]
rectification_matrix:
  rows: 3
  cols: 3
  data: [1.000000, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000, 0.000000, 0.000000, 1.000000]
projection_matrix:
  rows: 3
  cols: 4
  data: [516.422974, 0.000000, 372.887246, 0.000000, 0.000000, 520.513672, 226.444701, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000]
```


#### kinect摄像头
- 启动kinect：`roslaunch robot_vision freenect.launch`
- 标定彩色摄像头：`rosrun camera_calibration cameracalibrator.py image:=/image/rgb/image_raw camera:=/camera/rgb --size 8x6 --square 0.024`
- 标定红外摄像头：`rosrun camera_calibration cameracalibrator.py image:=/image/ir/image_raw camera:=/camera/ir --size 8x6 --square 0.024`

### 使用标定文件
#### 普通摄像头
- 在lauch文件中，配置`camera_info`
```xml
<launch>
    <node name="usb_cam" pkg="usb_cam" type="usb_cam_node" output="screen" >
        <param name="video_device" value="/dev/video0" />
        <param name="image_width" value="1280" />
        <param name="image_height" value="720" />
        <param name="pixel_format" value="yuyv" />
        <param name="camera_frame_id" value="usb_cam" />
        <param name="io_method" value="mmap"/>
        <param name="camera_info_url" type="string" value="file://$(find robot_vision)/camera_calibration.yaml" />
    </node>
</launch>
```

#### kinect摄像头
```xml
<launch>
    <!-- Launch the freenect driver -->
    <include file="$(find freenect_launch)/launch/freenect.launch">
        <arg name="publish_tf"                      value="false" /> 
        <!-- use device registration -->
        <arg name="depth_registration"              value="true" /> 
        <arg name="rgb_processing"                  value="true" />
        <arg name="ir_processing"                   value="false" />
        <arg name="depth_processing"                value="false" />
        <arg name="depth_registered_processing"     value="true" />
        <arg name="disparity_processing"            value="false" />
        <arg name="disparity_registered_processing" value="false" />
        <arg name="sw_registered_processing"        value="false" />
        <arg name="hw_registered_processing"        value="true" />
        <arg name="rgb_camera_info_url" value="file://$(find robot_vision)/kinect_rgb_calibration.yaml" />
        <arg name="depth_camera_info_url" value="file://$(find robot_vision)/kinect_depth_calibration.yaml" />
    </include>
</launch>
```

## ROS+OpenCV
- OpenCV是机器视觉中开源计算机视觉库，实现了图像处理和计算机视觉方面的很多通用算法，[ROS](http://wiki.ros.org/opencv_apps)也提供了OpenCV的联合调用。

- ros安装：`sudo apt-get install ros-kinetic-vision-opencv libopencv-dev python-opencv`

- 由于OpenCV的图像数据格式，和ROS不同，中间需要bridge环节将数据进行转换。[具体介绍](http://wiki.ros.org/cv_bridge)
```py
self.bridge = CvBridge()
cv_image = self.bridge.imgmsg_to_cv2(data, "bgr8")
self.image_pub.publish(self.bridge.cv2_to_imgmsg(cv_image, "bgr8"))
```

## 二维码
- 安装二维码功能包：`sudo apt-get install ros-kinetic-ar-track-alvar`
- 创建二维码：
  - `roscd robot_vision/config`
  - 查看选项：`rosrun ar_track_alvar createMarker`
  - 创建大小为5*5的数字0：`rosrun ar_track_alvar createMarker -s 5 0`

- 二维码的识别
```xml
<launch>
    <node pkg="tf" type="static_transform_publisher" name="world_to_cam" args="0 0 0.5 0 1.57 0 world usb_cam 10" />
    <arg name="marker_size" default="5" />
    <arg name="max_new_marker_error" default="0.08" />
    <arg name="max_track_error" default="0.2" />
    <arg name="cam_image_topic" default="/usb_cam/image_raw" />
    <arg name="cam_info_topic" default="/usb_cam/camera_info" />
    <arg name="output_frame" default="/usb_cam" />
    <node name="ar_track_alvar" pkg="ar_track_alvar" type="individualMarkersNoKinect" respawn="false" output="screen">
        <param name="marker_size"           type="double" value="$(arg marker_size)" />
        <param name="max_new_marker_error"  type="double" value="$(arg max_new_marker_error)" />
        <param name="max_track_error"       type="double" value="$(arg max_track_error)" />
        <param name="output_frame"          type="string" value="$(arg output_frame)" />
        <remap from="camera_image"  to="$(arg cam_image_topic)" />
        <remap from="camera_info"   to="$(arg cam_info_topic)" />
    </node>
    <!-- rviz view /-->
    <node pkg="rviz" type="rviz" name="rviz" args="-d $(find robot_vision)/config/ar_track_camera.rviz"/>
</launch>
```

## 物体识别框架
- ORK(Object Recognition Kitchen)，传入3维模板，可以在实际场景中使用kinect进行目标匹配，找到物体和对应位姿。


# 机器语音
- [机器听觉](http://www.guyuehome.com/514)
## 科大讯飞APK
- 在[官网注册](https://www.xfyun.cn)开发者账号，下载sdk。可以使用sdk与ROS结合实现语音的识别和合成。


# ROS SLAM功能包
## gmapping功能包
- 基于激光雷达，Rao-输出Blackwelized粒子滤波算法，二维栅格地图，需要机器人提供历程及信息。输出的topic：`nav_msgs/OccupancyGrid`
- OpenSlam开源了很多的SLAM算法，其中的gmapping被ROS收录，算法介绍可查看[链接](http://openslam.org/gmapping)

### 安装和使用
- 安装：`sudo apt-get install ros-kinetic-gmapping`

- 话题和服务

类型|名称|类型|描述
-|-|-|-
topic订阅|tf|tf/tfMessage|用户用于激光雷达坐标系，基坐标系，里程计坐标系之间的变换
topic订阅|scan|sensor_msgs/LaserScan|激光雷达扫描数据
topic发布|map_metadata|nav_msgs/MapMetaData|发布地图Meta数据
topic发布|map|nav_msgs/OccupancyGrid|发布地图栅格数据
topic发布|~entropy|std_msgs/Float64|发布机器人姿态分布熵的估计
service|dynamic_map|nav_msgs/GetMap|获取地图数据

- TF变换

类型|TF变换|描述
-|-|-
必须的TF变换|<scan frame>->base_link|激光雷达坐标系与基坐标系之间的变换，一般由robot_state_publisher或static_transform_publisher发布
必须的TF变换|base_link->odom|基坐标系与里程基坐标系之间的变换，一般由里程计节点发布
发布的TF变换|map->odom|地图坐标系与机器人里程计坐标系之间的变换，估计机器人在地图中的位姿

### 栅格地图取值原理

![栅格地图取值原理](/images/pasted-307.png)
![栅格地图取值原理](/images/pasted-308.png)


### 使用方法
- 配置gmapping节点
```xml
<launch>
    <arg name="scan_topic" default="scan" />
    <node pkg="gmapping" type="slam_gmapping" name="slam_gmapping" output="screen" clear_params="true">
        <param name="odom_frame" value="osdom"/>
        <param name="map_update_interval" value="5.0"/>
        <!-- Set maxUrange < actual maximum range of the Laser -->
        <param name="maxRange" value="5.0"/>
        <param name="maxUrange" value="4.5"/>
        <param name="sigma" value="0.05"/>
        <param name="kernelSize" value="1"/>
        <param name="lstep" value="0.05"/>
        <param name="astep" value="0.05"/>
        <param name="iterations" value="5"/>
        <param name="lsigma" value="0.075"/>
        <param name="ogain" value="3.0"/>
        <param name="lskip" value="0"/>
        <param name="srr" value="0.01"/>
        <param name="srt" value="0.02"/>
        <param name="str" value="0.01"/>
        <param name="stt" value="0.02"/>
        <param name="linearUpdate" value="0.5"/>
        <param name="angularUpdate" value="0.436"/>
        <param name="temporalUpdate" value="-1.0"/>
        <param name="resampleThreshold" value="0.5"/>
        <param name="particles" value="80"/>
        <param name="xmin" value="-1.0"/>
        <param name="ymin" value="-1.0"/>
        <param name="xmax" value="1.0"/>
        <param name="ymax" value="1.0"/>
        <param name="delta" value="0.05"/>
        <param name="llsamplerange" value="0.01"/>
        <param name="llsamplestep" value="0.01"/>
        <param name="lasamplerange" value="0.005"/>
        <param name="lasamplestep" value="0.005"/>
        <remap from="scan" to="$(arg scan_topic)"/>
    </node>
</launch>
```
## hector_slam功能包
- 使用高斯牛顿的方法进行地图构建，与gmapping不同，不需要里程计数据，只需要激光雷达进行栅格地图的构建，对深度信息依赖比较强，实际效果不如gmapping
- 输出地图话题：`nav_msgs/OccupancyGrid`

### 使用
- 安装：`sudo apt-get install ros-kinetic-hector-slam`

- 话题和服务

类型|名称|类型|描述
-|-|-|-
topic订阅|scan|sensor_msgs/LaserScan|激光雷达扫描的深度数据
topic订阅|syscommand|std_msgs/String|系统命令，如果字符串为reset，地图和机器人姿态重置为初始状态
topic发布|map_metadata|nav_msgs/MapMetaData|发布地图Meta数据
topic发布|map|nav_msgs/OccupancyGrid|发布地图栅格数据
topic发布|poseupdate|geometry_msgs/PoseWithCovarianceStamped|估计机器人姿态(具有高斯的不确定性)
service|dynamic_map|nav_msgs/GetMap|获取地图数据

- TF变换

类型|TF变换|描述
-|-|-
必须的TF变换|<scan frame>->base_link|激光雷达坐标系与基坐标系之间的变换，一般由robot_state_publisher或static_transform_publisher发布
发布的TF变换|map->odom|地图坐标系与机器人里程计坐标系之间的变换，估计机器人在地图中的位姿

- 配置`hector_mapping`节点
```xml
<launch>
    <node pkg = "hector_mapping" type="hector_mapping" name="hector_mapping" output="screen">
        <!-- Frame names -->
        <param name="pub_map_odom_transform" value="true"/>
        <param name="map_frame" value="map" />
        <param name="base_frame" value="base_footprint" />
        <param name="odom_frame" value="odom" />
        <!-- Tf use -->
        <param name="use_tf_scan_transformation" value="true"/>
        <param name="use_tf_pose_start_estimate" value="false"/>
        <!-- Map size / start point -->
        <param name="map_resolution" value="0.05"/>
        <param name="map_size" value="2048"/>
        <param name="map_start_x" value="0.5"/>
        <param name="map_start_y" value="0.5" />
        <param name="laser_z_min_value" value = "-1.0" />
        <param name="laser_z_max_value" value = "1.0" />
        <param name="map_multi_res_levels" value="2" />
        <param name="map_pub_period" value="2" />
        <param name="laser_min_dist" value="0.4" />
        <param name="laser_max_dist" value="5.5" />
        <param name="output_timing" value="false" />
        <param name="pub_map_scanmatch_transform" value="true" />
        <!-- Map update parameters -->
        <param name="update_factor_free" value="0.4"/>
        <param name="update_factor_occupied" value="0.7" />    
        <param name="map_update_distance_thresh" value="0.2"/>
        <param name="map_update_angle_thresh" value="0.06" />
        <!-- Advertising config --> 
        <param name="advertise_map_service" value="true"/>
        <param name="scan_subscriber_queue_size" value="5"/>
        <param name="scan_topic" value="scan"/>
    </node>
</launch>
```

## cartographer功能包
- 2016年谷歌开源的基于图网络的优化方法在二维或三维条件下的定位及建图功能包，主要基于激光雷达
### 使用
- 安装

安装|说明
-|-
安装工具|`sudo apt-get update`<br>`sudo apt-get install -y python-wstool python-rosdep ninja-build`
初始化工作空间|`cd catkin_goole_ws`<br>`wstool init src`
设置下载地址|`wstool merge -t src https://raw.githubusercontent.com/googlecartographer/cartographer_ros/master/cartopgrapher_ros.rosinstall`<br>`wstool update -t src`
下载功能包|`rosdep update`<br>`rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y`
编译功能包|`catkin_make_isolated --install --use-ninja`<br>`source install_isolated/setup.bash`

- demo演示
  - 启动demo演示：`wget -P ~/Downloads https://storage/googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag`，`roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=~/Downloads/cartographer_paper_deutsches_museum.bag`
  - 启动3D slam demo演示：`wget -P ~/Downloads https://storage/googleapis.com/cartographer-public-data/bags/backpack_3d/b3-2016-04-05-14-14-00.bag`，`roslaunch cartographer_ros demo_backpack_3d.launch bag_filename:=~/Downloads/b3-2016-04-05-14-14-00.bag`
  - 启动Revo LDS demo演示：`wget -P ~/Downloads https://storage/googleapis.com/cartographer-public-data/bags/revo_lds/cartographer_paper_revo_lds.bag`，`roslaunch cartographer_ros demo_revo_lds.launch bag_filename:=~/Downloads/cartographer_paper_revo_lds.bag`
  - 启动PR2 demo演示：`wget -P ~/Downloads https://storage/googleapis.com/cartographer-public-data/bags/pr2/2011-09-15-08-32-46.bag`，`roslaunch cartographer_ros demo_pr2.launch bag_filename:=~/Downloads/2011-09-15-08-32-46.bag`

- 配置节点
```xml
<!-- 请复制该文件到cartographer_ros/cartographer_ros/launch中使用 -->
<launch>  
  <param name="/use_sim_time" value="true" />  
  <node name="cartographer_node" pkg="cartographer_ros"  
        type="cartographer_node" args="  
            -configuration_directory $(find cartographer_ros)/configuration_files  
            -configuration_basename rplidar.lua"  
        output="screen">  
    <remap from="scan" to="scan" />  
  </node>  
  <node name="rviz" pkg="rviz" type="rviz" required="true"  
        args="-d $(find cartographer_ros)/configuration_files/demo_2d.rviz" />  
</launch>
```

## ORB SLAM
- 基于特征点的实时单目SLAM系统，实时解算摄像机的移动轨迹，构建三维点云地图，不仅使用手持设备，也可用户汽车行驶过程中获取的连续图像
- Raul Mur-Artal, J. M. M. Montiel和Juan D. Tardos于2015年发表在IEEE Transactions on Robotics上

### 使用
- 安装

安装|说明
-|-
安装工具和源码|`sudo apt-get install libboost-all-dev libblas-dev liblapck-dev`<br>`git clone https://github.com/raulmur/ORB_SLAM2.git`
安装eigen3.2|[下载链接](http://eigen.tuxfamily.org/index.php?title=Main_Page)<br>`mkdir build && cd build &&cmake.. && make && make install`
安装g2o|`cd ORB_SLAM-master/Thirdparty/g2o`<br>`mkdir build && cd build &&cmake.. -DCMAKE_BUILD_TYPE=Release&& make`
编译DBow2|`cd ORB_SLAM-master/Thirdparty/DBoW2`<br>`mkdir build && cd build &&cmake.. -DCMAKE_BUILD_TYPE=Release&& make`
编译ORB_SLAM|`cd ORB_SLAM && mkdir build && cd build &&cmake.. -DCMAKE_BUILD_TYPE=Release&& make`
编译功能包|`export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:ORB_SLAM_PATH/ORB_SLAM2/Examples/ROS`<br>`chmod +x build_ros.sh && ./build_ros.sh && source ORB_SLAM2/Examples/ROS/ORB_SLAM2/build/devel/setup.bash`

- demo
  - 单目SLAM示例：`rosrun ORB_SLAM2 Mono Vocabulary/ORBvoc.txt Examples/ROS/ORB_SLAM2/Asus.yaml`，`rosbag play  rgbd_dataset_freiburg1_desk.bag /camera/rgb/image_color:=/camera/image_raw`，Vocabulary参数算法文件在ORBSLAM2/Vocabulary中，Asus.yaml相机参数设置文件，也可以对camera进行标注产生
  - AR示例：`rosrun ORB_SLAM2 MonoAR Vocabulary/ORBvoc.txt Examples/ROS/ORB_SLAM2/Asus.yaml`，`rosbag play  rgbd_dataset_freiburg1_desk.bag /camera/rgb/image_color:=/camera/image_raw`

# ROS导航
- 详细内容可以查看《ROS by Example》

![基于move_base的导航框架](/images/pasted-309.png)

- 基于move_base的导航框架，白色部分是ROS已经实现的，蓝色部分需要单独实现的。中间是move_base功能包，左侧是amcl定位的功能包

- 安装：`sudo apt-get install ros-kinetic-navigation`

## move_base功能包
- 全局路径规划：全局最优路径规划、Dijkstra或A*算法
- 本地实时规划：
  - 规划机器人每个周期内的线速度、角速度，尽量符合全局最优路径
  - 实时避障
  - Trajectory Rollout和Dynamic Window Approaches算法
  - 搜索躲避和行进的多条路径，综合各评价标准选择最优路径

## amcl
- 蒙特卡洛定位算法，二维环境定位，针对已有地图使用例子滤波器跟踪一个机器人的姿态

# moveit机械臂控制
- moveit是一个集成化的开发平台，由一系列操作的功能包组成，包括路径规划、操作控制、3D感知、运动学、控制与导航算法，提供友好的GUI，方便的应用于工业领域。

## 系统架构
![核心节点move_group](/images/pasted-310.png)













