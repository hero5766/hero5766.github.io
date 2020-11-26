---
title: pcl
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-10-03 11:51:26
---
> 

<!-- more -->

# 简介
- [官方网址](https://pointclouds.org/)
- [官方教程](https://pcl.readthedocs.io/projects/tutorials/en/latest/)

## 介绍
- PCL(Point Cloud Library)收集了点云相关研究的一个开源的`C++`库，实现了大量点云相关的通用算法和高效数据结构，设计点云获取、滤波、分割、配准、检索、特征提取、识别、追踪、曲面重建、可视化等。同时具有高可移植性，在ROS、Android、Ubuntu、等主流linux平台都可以使用

## 版本
- 

## 结构
- 对于3D点云处理来说，PCL完全是一个模块化的现代化C++模块库，主要基于Boost、Eigen、FLANN、VTKCUDA、OpenNI、QHull等第三方库。
    - OpenMP、GPU、CUDA：高性能计算技术，通过并行化实现高性能
    - FLANN：K近邻搜索操作的架构基于FLANN(Fast Library for Approximate Nearest Neighbors)，是目前技术中最快的
    - Boost：目前PCL所有的模块和算法都是通过Boost共享指针来传送数据，从而避免多次复制系统中已存在的数据需要

### 调用流程
- PCL中一个处理管道的基本接口程序：
  - 创建处理对象，例如：过滤、特征估计、分割等
  - 使用setInputCloud通过输入点云数据，处理模块
  - 设置算法相关参数
  - 调用计算得到输出

### 模块介绍

模块|说明
-|-
filters|实现采样、去除离群点、特征提取、拟合估计等数据过滤
features|实现多种三维特征，如曲面发现、曲率、边界点估计、矩不变量、主曲率、PFH、FPFH特征、旋转图像、积分图像、NARF描述子、RIFT、相对标准差、数据前度的筛选等
IO|实现数据的输入和输出操作，例如点云数据文件PCD文件的读写
segmentation|实现聚类提取，如通过采样一致性方法对一系列参数模型进行模型拟合点云分割提取
surface|表面重建技术，如网格重建、凸包重建、移动最小二乘法平滑等
register|实现点云配准方法，ICP
keypoints|实现不同的关键点的提取方法，这可以用来作为预处理步骤，决定在哪里提取特征描述符
range|实现支持不同点云数据继承的范围图像

## 环境
### windows

组件搭建|说明
-|-
kinect v1 SDK|[link](https://www.microsoft.com/en-us/download/details.aspx?id=40278)
openni|[openni1](http://fivedots.coe.psu.ac.th/~ad/kinect/installation.html)，[openni2](https://structure.io/openni)，openni1.x的组件安装之后，我的kinect无法连接，只能使用openni2
VTK|[link](https://vtk.org/download/)，不能使用太高的版本，否则visualization这个组件无法生成项目
glut|Glut库太老了，x64编译时一直报错，需要使用freeglut替代

- 缺失的组件在windows下都可以通过vcpkg工具进行查找和自动编译需要注意x86和x64的问题，vcpkg安装的库默认x86，如果系统是x64的：`vcpkg install xxx --triplet x64-windows`

vcpkg|说明
-|-
vcpkg search|搜索缺失包名
vcpkg install|进行安装，将lib、include路径配置到cmake
下载过慢问题|可以单独下载文件，重命名后再放入/vcpkg/download


PCL安装|说明
-|-
下载PCL|[下载链接](https://github.com/PointCloudLibrary/pcl)
cmake编译|必须选择BUILD_CUDA才能编译kinfu，如果使用openni2，需要在with中勾选该选项


### linux
- 依赖
- 其中vtk以及qt这两个库，建议大家还是从源码编译安装比较好。

```bash
sudo apt-get update  
sudo apt-get install -y git build-essential linux-libc-dev  
sudo apt-get install -y cmake cmake-gui   
sudo apt-get install -y libusb-1.0-0-dev libusb-dev libudev-dev  
sudo apt-get install -y mpi-default-dev openmpi-bin openmpi-common    
sudo apt-get install -y libflann1.8 libflann-dev  
sudo apt-get install -y libeigen3-dev  
sudo apt-get install -y libboost-all-dev  
sudo apt-get install -y libvtk5.10-qt4 libvtk5.10 libvtk5-dev  
sudo apt-get install -y libqhull* libgtest-dev  
sudo apt-get install -y freeglut3-dev pkg-config  
sudo apt-get install -y libxmu-dev libxi-dev   
sudo apt-get install -y mono-complete  
sudo apt-get install -y qt-sdk openjdk-8-jdk openjdk-8-jre 
```

`sudo apt-get install -y git build-essential linux-libc-dev  cmake cmake-gui libusb-1.0-0-dev libusb-dev libudev-dempi-default-dev openmpi-bin libflann1.8 libflann-dev  libeigen3-dev  libboost-all-dev  libvtk5.10-qt4 libvtk5.10 libvtk5-dev libqhull* libgtest-dev  freeglut3-dev pkg-config  libxmu-dev libxi-dev   mono-complete  qt-sdk openjdk-8-jdk openjdk-8-jre openmpi-common`

- vtk安装：[下载地址](https://vtk.org/download/)：VTK-8.2.0.tar.gz和VTKData-8.2.0.tar.gz

- 执行
```bash
mkdir build
cd build
cmake ../ -DBUILD_SHARED_LIBS=ON -DBUILD_TESTING=ON -DCMAKE_BUILD_TYPE=Release -DVTK_WRAP_PYTHON=ON
make -j8
sudo make install
```


### 样例数据

- [官方pcd数据下载](https://github.com/PointCloudLibrary/data)


### 测试
- 读取pcd文件并进行展示
```
.\pcl_viewerd.exe ism_test_cat.pcd -bc 255,255,255
```

![test_cat](/images/pasted-284.png)

# PCL开发
## 编写自定义PCL类
- 假设我们将新的算法bilateral作为滤波库的一部分，首先在目录中建立三个文件
  - include/pcl/filters/bilateral.h：包含定义和声明
  - include/pcl/filters/impl/bilateral.hpp：包含模板类的具体实现
  - src/bilateral.cpp：包含具体的不同点类型的模板类实例化
- 我们定义新的类，命名为BilateralFilter。滤波器接口规定每个算法必须有两个声明和实现可供使用，一个操作`PointCloud<T>`，一个操作`Point-Cloud2`。这里实现`PointCloud<T>`

### bilateral.h
- 类的声明，实现Filter类的的各种setters和getters虚函数。
```cpp
#ifndef PCL_FILTERS_BILATERAL_H_
#define PCL_FILTERS_BILATERAL_H_
#include <pcl/filters/filter.h>
namespace pcl{
  template<typename PointT>
  class BilateralFilter:public Filter<PointT>{
    using Filter<PointT>::input_; //using声明后确保不需输入完整引用即可访问父类成员变量input_
    typedef typename Filter<PointT>::PointCloud PointCloud;//方便使用PointCloud类
    typedef typename pcl::KdTree<PointT>::Ptr KdTreePtr;//方便使用Ptr类
public:
  BilateralFilter():sigma_s_(0),sigma_r_(std::numeric_limits<double>::max()){}
  void setSigmaS(const double sigma_s){
    sigma_s_=sigma_s
  }
  double getSigmaS(){
    return sigma_s_;
  }
  void setSigmaR(const double sigma_r){
    sigma_r_=sigma_r
  }
  double getSigmaR(){
    return sigma_r_;
  }
  void applyFilter(PointCloud &output);
  double computePointWeight(const int pid,const std::vector<int>&indices,const std::vector<float>&distances);
  void setSearchMethod(const KdTreePtr &tree){tree_=tree;}
private:
  double sigma_s_;
  double sigma_r_;
  KdTreePtr tree_;
  };
}
#endif //PCL_FILTERS_BILATERAL_H_
```

### bilateral.hpp
- 完成声明函数的具体实现，applyFilter和computePointWeight
  - applyFilter：利用输入数据构建的kd树，把所有输入数据复制到输出，然后计算新的点的强度，复制到点云数据output，
  - computePointWeight：通过传递一个要计算的强度重量的point索引，索引指示的点是由欧式空间的邻域组成。
- 完成类声明PCL_INSTANTIATE用于实例化模板类
```cpp
#ifndef PCL_FILTERS_BILATERAL_IMPL_H_
#define PCL_FILTERS_BILATERAL_IMPL_H_
#include <pcl/filters/bilateral.h>
template<typename PointT>double pcl::BilateralFilter<PointT>::computePointWeight(const int pid,const std::vector<int>&indices,const std::vector<float>&distances){
  double BF=0,W=0;
  for(size_t nid=0;n_id<indices.size();++n_id){
    double id=indices[n_id];
    double dist=std::sqrt(distances[n_id]);
    double intensity_dist = abs(input_->points[pid].intensity-input_->points[id].intensity);
    double weight = kernel(dist,sigma_s_)*kernel(intensity_dist,sigma_r_);
    BF += weight*input_->points[id].intensity;
    W += weight;
  }
  return (BF/W);
}
template<typename PointT>void pcl::BilateralFilter<PointT>::applyFilter(PointCloud &output){
  if(sigma_s_==0){
    PCL_ERROR("[pcl::BilateralFilter::applyFilter] Need a sigma_s value given before continuing.\n");return;
  }
  if (!tree_){
    if (input_->isOrganized())//点云有序则使用OrganizedDataIndex有序搜索方法
      tree_.reset(new pcl::OrganizedDataIndex<PointT>());
    else//否则使用KdTreeFLANN中的Kd树
      tree_.reset(new pcl::KdTreeFLANN<PointT>(false));
  }
  tree_->setInputCloud(input_);
  std::vector<int>k_indices;
  std::vector<float>k_distances;
  output = *input_;
  for (size_t point_id=0;point_id<input_->points.size();++point_id){
    tree_->radiusSearch(point_id,sigma_s_*2,k_indices,k_distances);
    output.points[point_id].intensity=computePointWeight(point_id,k_indices,k_distances);
  }
}
#define PCL_INSTANTIATE_BilateralFilter(T) template class PCL_EXPORTS pcl::BilateralFilter<T>;
#endif //PCL_FILTERS_BILATERAL_IMPL_H_
```

### bilateral.cpp
- 为BilateralFilter类实例化并预编译实例化若干模板，增加编译时间，但是能够提高使用时的速度
```cpp
#include <pcl/point_types.h>
#include <pcl/impl/instantiate.hpp>
#include <pcl/filters/bilateral.h>
#include <pcl/filters/impl/bilateral.hpp>
using namespace pcl;
//template class PCL_EXPORTS BilateralFilter<PointXYZ>; 
//template class PCL_EXPORTS BilateralFilter<PointXYZI>; 
//template class PCL_EXPORTS BilateralFilter<PointXYRGB>; 
//... 上面这种根据类型添加会使代码变得冗余
PCL_INSTANTIATE(BilateralFilter,PCL_XYZ_POINT_TYPES);
//使用宏PCL_INSTANTIATE，可以对所有在point_types.h中定义的XYZ点类型对模板进行实例化
```

- 需要实现模板类`PCL_INSTANTIATE`和`pcl::filter`的纯虚函数，才能成功编译


### CMakeLists.txt
- 将新建的文件添加到编译文件中，就可以实现编译连接的过程
```make
set(srcs
  src/bilateral.cpp
)
set(incs
  include/pcl/$(SUBSYS_NAME)/bilateral.h
)
set(impl_incs
  include/pcl/$(SUBSYS_NAME)/impl/bilateral.hpp
)
```
## PCL自定义点类型
### PCL定义的点类型PointT
- 在point_types.hpp中有完成目录，如果存在用户就不需要重复定义了

PointT|成员|说明
-|-|-
PointXY|float x,y|点数据类型xy坐标
PointXYZ|float x,y,z|点数据类型xyz坐标
PointXYZI|float x,y,z,intensity|xyz坐标,indensity:强度值
PointXYZRGB|float x,y,z,rgb|xyz坐标,RGB
PointXYZRGBA|float x,y,z;uint32_t rgba|xyz坐标,RGBA
InterestPoint|float x,y,z,strength|xyz坐标,strength关键点的强度测量值
Normal|float normal[3],curvature|法向量以及曲率的测量值
PointNormal|float x,y,z,normal[3],curvature|xyz,法向量及曲率
PointXYZRGBNormal|float x,y,z,rgb,normal[3],curvature|xyzrgb,法向量及曲率
PointXYZINormal|float x,y,z,intensity,normal[3],curvature|xyz,强度值,法向量及曲率
PointWithRange|float x,y,z,range|xyz,range包含视点到采样点的距离测量值
PointWithViewpoint|float x,y,z,vp_x,vp_y,vp_z|xyz,vp_x,vp_y,vp_z
MomentInvariants|float j1,j2,j3|包含采样曲面上面片的3个不变矩的point类型，描述面片上质量的分布情况
PrincipalRadiiRSD|float r_min,r_max|包含曲面块上两个RSD半径的point类型
Boundary|uint8_t boundary_point|存储一个点是否位于曲面边界上
PrincipalCurvature|float principal_curvature[3],pc1,pc2|包含给定点主曲率的简单point类型
PFHSignature125|float pfh[125]|包含给定点PFH(点特征直方图)的point类型
FPFHSignature33|float fpfh[33]|包含给定点FPFH(快速点特征直方图)的point类型
VFHSignature308|float vfh[308]|包含给定点VFH(快速点特征直方图)的point类型
Narf36|float xyz,roll,pitch,yaw,descriptor[36]|包含给定点NARF(归一化对齐半径特征)的point类型
BorderDescription|int x,y;BorderTraits traits|给定点边界类型
IntensityGradient|float gradie[3]|给定点强度的梯度
Histogram|float histogram[N]|n维直方图
PointWithScale|float x,y,z,scale|x,y,z,scale几何操作尺度
PointSurfel|float x,y,z,normal[3],rgba,radius,confidence,curvature|复杂point信息

- 几种PointT类型示例：
```cpp
//PointXY
struct {float x,y;};
//PointXYZ
union{
  float data[4];
  struct {float x,y,z;};
}
//PointXXYZI:intensity
union{
  struct{
    float intensity;
  }
  float data_c[4];
};
//PointXXYRGBA:uint32_t rgba
union{
  struct{
    uint32_t rgba;
  }
  float data_c[4];
}
//PointXXYRGB:rgb
union{
  struct{
    float rgb;
  }
  float data_c[4];
}
//InterestPoint:strength
union{
  struct{
    float strength;
  }
  float data_c[4];
}
//Normal:float normal,curvature;法向量，以及曲率的测量值
union{
  float data_n[4];
  float normal[3];
  struct{
    float normal_x,normal_y,normal_z;
  }
};
union{
  struct{
    float curvature;
  }
  float data_c[4];
}
//PointNormal:xyz,normal[3],curvature;xyz数据，采样点对应发现和曲率
union{
  float data[4];
  struct{
    float x,y,z;
  }
};
union{
  float data_n[4];
  float normal[3];
  struct{
    float normal_x,normal_y,normal_z;
  }
};
union{
  struct{
    float curvature;
  }
  float data_c[4];
}
//PointXTZRGBNormal:xyz,rgb,normal[3],curvature
```

### 新增PointT类型
- 定义新的类型，需要包含PCL特定的类/算法模板头文件的实现，这样才能在其共同使用。
```cpp
#include <pcl/filters/bilateral.h>
#include <pcl/filters/impl/bilateral.hpp>
struct MyPointType{
  float test;
}
```

- 如果是库的一部分，可以被他人使用，需要将自己的类型显式实例化。
```cpp
#include <pcl/point_types.h>
#include <pcl/point_cloud.h>
#include <pcl/io/pcd_io.h>
struct MyPointType{
  PCL_ADD_POINT4D;
  float test;
  EIGEN_MAKE_ALIGNED_OPERAROE_NEW
}EIGEN_ALIGN16;
POINT_CLOUD_REGISTER_POINT_STRUCT(MyPointType,(float,x,x),(float,y,y),(float,z,z),(float,test,test))
int main(int argc,char **argv){
  pcl::PointCloud<MyPointType>cloud;
  cloud.points.resize(2);
  cloud.width=2;
  cloud.height=1;
  cloud.points[0].test=1;
  cloud.points[1].test=1;
  cloud.points[0].x=cloud.points[0].y=cloud.points[0].z=0;
  cloud.points[1].x=cloud.points[1].y=cloud.points[1].z=3;
  pcl::io::savePCDFile("test.pcd",cloud);
}
```


## 许可
- 建议每个文件报验一个描述代码作者的许可，使用户对约束有清晰的了解。PCL是100%的BSD许可的，可以在文件中以`C++`注释的形式嵌入该许可证。如果需要声明其他版权，则添加类似内容即可
```cpp
/*
 * Copyright (c) XXX, respective authors.
*/
```

## 代码注释
- PCL支持Doxygen的文档生成，注释中可以对API进行完善

## 异常处理
### 自定义异常
- PCL规定新定义的异常类必须继承PCLException类，具体定义在pcl/exceptions.h中
```cpp
/* * \class MyException
 * \brief An exception that is thrown when i want it.
*/
class PCL_EXPROTS MyException:public PCLException{
public:
  MyException(const std::string&error_description,const std::string& file_name="",const std::string& function_name="",unsigned line_number=0) throw():pcl::PCLException(error_description,file_name,function_name,line_number){}
};
```
### 使用异常
- 为了方便使用自定义异常，定义宏
```cpp
#define PCL_THROW_EXCEPTION(ExceptionName,message){
  std::ostringstream s;
  s<<message;
  throw ExceptionName(s.str(),__FILE__,"",__LINE__);
}
```

- 异常抛出
```cpp
if(my_requirements!=the_parameters_used_)
  PCL_THROW_EXCEPTION(MyExcption,"My requirements are not set"<<the_parameter_used);
```

### 异常处理
```cpp
try{
  myObject.myFunc(some_params);
}
catch(pcl::MyException& e){
  //MyException
}
#if 0
catch(exception& e){
  //任意异常的铺货
}
#endif
```

# io
## OpenNI
- openni是一个多语言，跨平台的框架，定义了一套用于编写通用自然交互应用的API。提供了一组基于传感器设备实现的API和一组有视觉和音频传感器的中间件实现的API。这样就可以独立的使用传感器和中间件，减少模块的开发。

