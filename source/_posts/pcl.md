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


# 数据操作
## 简单使用
- 生成点云并保存为pcdld文件

```cmake
cmake_minimum_required(VERSION 2.6 FATAL_ERROR)
project(MY_GRAND_PROJECT)
find_package(PCL 1.3 REQUIRED COMPONENTS common io)
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})
add_executable(pcd_write_test pcd_write.cpp)
target_link_libraries(pcd_write_test ${PCL_LIBRARIES})
```

```cpp
#include <iostream>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
int main (int argc, char** argv){
  pcl::PointCloud<pcl::PointXYZ> cloud;
  // Fill in the cloud data
  cloud.width    = 5;
  cloud.height   = 1;
  cloud.is_dense = false;
  cloud.points.resize (cloud.width * cloud.height);
  for (auto& point: cloud)  {
    point.x = 1024 * rand () / (RAND_MAX + 1.0f);
    point.y = 1024 * rand () / (RAND_MAX + 1.0f);
    point.z = 1024 * rand () / (RAND_MAX + 1.0f);
  }
  pcl::io::savePCDFileASCII ("test_pcd.pcd", cloud);
  std::cerr << "Saved " << cloud.size () << " data points to test_pcd.pcd." << std::endl;
  for (const auto& point: cloud)
    std::cerr << "    " << point.x << " " << point.y << " " << point.z << std::endl;
  return (0);
}
```


- 读取pcd点云文件并展示

```cpp
#include <iostream>
#include <fstream>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/registration/icp.h>
#include <pcl/visualization/cloud_viewer.h>
using namespace std;
using namespace pcl;
PointCloud<PointXYZI>::Ptr getPoint(const string& infile){
    fstream input(infile.c_str(), ios::in | ios::binary);
    if(!input.good()){
        cerr << "Could not read file: " << infile << endl;
        exit(EXIT_FAILURE);
    }
    input.seekg(0, ios::beg);
    PointCloud<PointXYZI>::Ptr points (new pcl::PointCloud<PointXYZI>);
    int i;
    for (i=0; input.good() && !input.eof(); i++) {
        PointXYZI point;
        input.read((char *) &point.x, 3*sizeof(float));
        input.read((char *) &point.intensity, sizeof(float));
        points->push_back(point);
    }
    input.close();
    return points;
}
void view (const pcl::PointCloud<pcl::PointXYZI>::Ptr& cloud){
    visualization::CloudViewer viewer ("Simple Cloud Viewer");
    viewer.showCloud (cloud);
    while (!viewer.wasStopped ()){
    }
}
int main (int argc, char** argv){
    PointCloud<PointXYZI>::Ptr cloud_in = getPoint("velodyne/000000.bin");
    PointCloud<PointXYZI>::Ptr cloud_out = getPoint("velodyne/000001.bin");
    view(cloud_in);
    view(cloud_out);
    return (0);
}
```

# 滤波
## 方法

方法|类|说明
-|-|-
直通滤波器|`pcl::PassThrought<pcl::PointXYZ>pass;`|想要固定某一区域的
体素滤波器|`pcl::VoxelGrid<pcl::PCLPointCloud2>vox;`<br>`pcl::ApproximateVoxelGrid<pcl::PCLPointCloud2>vox;`哈希map，速度更快|体素化
统计滤波器|`pcl::StatistcalOutlierRemoval<pcl::PointXYZ>sor;`|去除噪声
半径滤波器|`pcl::RadiusOutlierRemoval<pcl::PointXYZ>rador;`|统计半径里面多少点，不足就去除
均匀采样|`pcl::UniformSampling<pcl::PointXYZ>rador;`|`setInputCloud(cloud)`<br>`setRadiusSearch(0.5)`<br>`comput(indices)`
条件滤波器|`pcl::ConditionalRemoval<pcl::PointXYZ>condr;`|
投影滤波器|`pcl::ProjectInliers<pcl::PointXYZ>proj;`|
模型滤波器|`pcl::ModelOutlierRemoval<pcl::PointXYZ>modr;`|通过模型参数模拟，将超出阈值的点去除
索引提取|`pcl::ExtractIndices<pcl::PointXYZ>extr;`|
空间裁减滤波|`pcl::Clipper3D<pcl::PointXYZ>;`<br>`pcl::BoxCliper3D<pcl::PointXYZ>`<br>`pcl::CropBax<pcl::PointXYZ>`<br>`pcl::CropHull<pcl::PointXYZ>`|CropHull凸包
双边滤波器|`pcl::BilateralFilter<pcl::PointXYZ>bf;`|保存边界的去噪声
高斯滤波器|`pcl::filters::GuassianKernal<PointInT,PointOutT>;`|




## 直通滤波器
```cpp
#include <iostream>
#include <pcl/point_types.h>
#include <pcl/filters/passthrough.h>
int main (int argc, char** argv){
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_filtered (new pcl::PointCloud<pcl::PointXYZ>);
  // Fill in the cloud data
  cloud->width  = 5;
  cloud->height = 1;
  cloud->points.resize (cloud->width * cloud->height);
  for (auto& point: *cloud) {
    point.x = 1024 * rand () / (RAND_MAX + 1.0f);
    point.y = 1024 * rand () / (RAND_MAX + 1.0f);
    point.z = 1024 * rand () / (RAND_MAX + 1.0f);
  }
  std::cerr << "Cloud before filtering: " << std::endl;
  for (const auto& point: *cloud)
    std::cerr << "    " << point.x << " "<< point.y << " " << point.z << std::endl;
  // Create the filtering object
  pcl::PassThrough<pcl::PointXYZ> pass;
  pass.setInputCloud (cloud);
  pass.setFilterFieldName ("z");
  pass.setFilterLimits (0.0, 1.0);
  //pass.setFilterLimitsNegative (true);
  pass.filter (*cloud_filtered);
  std::cerr << "Cloud after filtering: " << std::endl;
  for (const auto& point: *cloud_filtered)
    std::cerr << "    " << point.x << " " << point.y << " "<< point.z << endl;
  return (0);
}
```

![](/images/pasted-354.png)


## 体素滤波器

```cpp
#include <iostream>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/filters/voxel_grid.h>
int main (int argc, char** argv)
{
  pcl::PCLPointCloud2::Ptr cloud (new pcl::PCLPointCloud2 ());
  pcl::PCLPointCloud2::Ptr cloud_filtered (new pcl::PCLPointCloud2 ());
  // Fill in the cloud data
  pcl::PCDReader reader;
  // Replace the path below with the path where you saved your file
  reader.read ("table_scene_lms400.pcd", *cloud); // Remember to download the file first!
  std::cerr << "PointCloud before filtering: " << cloud->width * cloud->height << " data points (" << pcl::getFieldsList (*cloud) << ")." << std::endl;
  // Create the filtering object
  pcl::VoxelGrid<pcl::PCLPointCloud2> sor;
  sor.setInputCloud (cloud);
  sor.setLeafSize (0.01f, 0.01f, 0.01f);//设置滤波时创建1cm的立方体
  sor.filter (*cloud_filtered);//执行滤波
  std::cerr << "PointCloud after filtering: " << cloud_filtered->width * cloud_filtered->height << " data points (" << pcl::getFieldsList (*cloud_filtered) << ")." << std::endl;
  pcl::PCDWriter writer;
  writer.write ("table_scene_lms400_downsampled.pcd", *cloud_filtered,   Eigen::Vector4f::Zero (), Eigen::Quaternionf::Identity (), false);
  return (0);
}
```

![](/images/pasted-355.png)

- 在每个voxel中保留了重心，可以看出并不是特别均匀。


## 统计滤波器


```cpp
#include <iostream>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/filters/statistical_outlier_removal.h>
int main (int argc, char** argv){
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_filtered (new pcl::PointCloud<pcl::PointXYZ>);
  // Fill in the cloud data
  pcl::PCDReader reader;
  // Replace the path below with the path where you saved your file
  reader.read<pcl::PointXYZ> ("table_scene_lms400.pcd", *cloud);
  std::cerr << "Cloud before filtering: " << std::endl;
  std::cerr << *cloud << std::endl;
  // Create the filtering object
  pcl::StatisticalOutlierRemoval<pcl::PointXYZ> sor;
  sor.setInputCloud (cloud);
  sor.setMeanK (50);
  sor.setStddevMulThresh (1.0);
  sor.filter (*cloud_filtered);
  std::cerr << "Cloud after filtering: " << std::endl;
  std::cerr << *cloud_filtered << std::endl;
  pcl::PCDWriter writer;
  writer.write<pcl::PointXYZ> ("table_scene_lms400_inliers.pcd", *cloud_filtered, false);
  sor.setNegative (true);
  sor.filter (*cloud_filtered);
  writer.write<pcl::PointXYZ> ("table_scene_lms400_outliers.pcd", *cloud_filtered, false);
  return (0);
}
```

![](/images/pasted-356.png)






## 条件滤波器

```cpp
#include <iostream>
#include <pcl/point_types.h>
#include <pcl/filters/radius_outlier_removal.h>
#include <pcl/filters/conditional_removal.h>
int main (int argc, char** argv){
  if (argc != 2)  {
    std::cerr << "please specify command line arg '-r' or '-c'" << std::endl;
    exit(0);
  }
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_filtered (new pcl::PointCloud<pcl::PointXYZ>);
  // Fill in the cloud data
  cloud->width  = 5;
  cloud->height = 1;
  cloud->resize (cloud->width * cloud->height);
  for (auto& point: *cloud)  {
    point.x = 1024 * rand () / (RAND_MAX + 1.0f);
    point.y = 1024 * rand () / (RAND_MAX + 1.0f);
    point.z = 1024 * rand () / (RAND_MAX + 1.0f);
  }
  if (strcmp(argv[1], "-r") == 0){
    pcl::RadiusOutlierRemoval<pcl::PointXYZ> outrem;
    // build the filter
    outrem.setInputCloud(cloud);
    outrem.setRadiusSearch(0.8);
    outrem.setMinNeighborsInRadius (2);
    outrem.setKeepOrganized(true);
    // apply filter
    outrem.filter (*cloud_filtered);
  }
  else if (strcmp(argv[1], "-c") == 0){
    // build the condition
    pcl::ConditionAnd<pcl::PointXYZ>::Ptr range_cond (new      pcl::ConditionAnd<pcl::PointXYZ> ());
    range_cond->addComparison (pcl::FieldComparison<pcl::PointXYZ>::ConstPtr (new pcl::FieldComparison<pcl::PointXYZ> ("z", pcl::ComparisonOps::GT, 0.0)));
    range_cond->addComparison (pcl::FieldComparison<pcl::PointXYZ>::ConstPtr (new pcl::FieldComparison<pcl::PointXYZ> ("z", pcl::ComparisonOps::LT, 0.8)));
    // build the filter
    pcl::ConditionalRemoval<pcl::PointXYZ> condrem;
    condrem.setCondition (range_cond);
    condrem.setInputCloud (cloud);
    condrem.setKeepOrganized(true);
    // apply filter
    condrem.filter (*cloud_filtered);
  }
  else{
    std::cerr << "please specify command line arg '-r' or '-c'" << ::endl;
    exit(0);
  }
  std::cerr << "Cloud before filtering: " << std::endl;
  for (const auto& point: *cloud)
    std::cerr << "    " << point.x << " "<< point.y << " "<< point.z << std::endl;
  // display pointcloud after filtering
  std::cerr << "Cloud after filtering: " << std::endl;
  for (const auto& point: *cloud_filtered)
    std::cerr << "    " << point.x << " "<< point.y << " "<< point.z << std::endl;
  return (0);
}
```

## 索引提取

```cpp
#include <iostream>
#include <pcl/ModelCoefficients.h>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/sample_consensus/method_types.h>
#include <pcl/sample_consensus/model_types.h>
#include <pcl/segmentation/sac_segmentation.h>
#include <pcl/filters/voxel_grid.h>
#include <pcl/filters/extract_indices.h>
int main (int argc, char** argv){
  pcl::PCLPointCloud2::Ptr cloud_blob (new pcl::PCLPointCloud2), cloud_filtered_blob (new pcl::PCLPointCloud2);
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_filtered (new pcl::PointCloud<pcl::PointXYZ>), cloud_p (new pcl::PointCloud<pcl::PointXYZ>), cloud_f (new pcl::PointCloud<pcl::PointXYZ>);
  // Fill in the cloud data
  pcl::PCDReader reader;
  reader.read ("table_scene_lms400.pcd", *cloud_blob);
  std::cerr << "PointCloud before filtering: " << cloud_blob->width * cloud_blob->height << " data points." << std::endl;
  // Create the filtering object: downsample the dataset using a leaf size of 1cm
  pcl::VoxelGrid<pcl::PCLPointCloud2> sor;
  sor.setInputCloud (cloud_blob);
  sor.setLeafSize (0.01f, 0.01f, 0.01f);
  sor.filter (*cloud_filtered_blob);
  // Convert to the templated PointCloud
  pcl::fromPCLPointCloud2 (*cloud_filtered_blob, *cloud_filtered);
  std::cerr << "PointCloud after filtering: " << cloud_filtered->width * cloud_filtered->height << " data points." << std::endl;
  // Write the downsampled version to disk
  pcl::PCDWriter writer;
  writer.write<pcl::PointXYZ> ("table_scene_lms400_downsampled.pcd", *cloud_filtered, false);
  pcl::ModelCoefficients::Ptr coefficients (new pcl::ModelCoefficients ());
  pcl::PointIndices::Ptr inliers (new pcl::PointIndices ());
  // Create the segmentation object
  pcl::SACSegmentation<pcl::PointXYZ> seg;
  // Optional
  seg.setOptimizeCoefficients (true);
  // Mandatory
  seg.setModelType (pcl::SACMODEL_PLANE);
  seg.setMethodType (pcl::SAC_RANSAC);
  seg.setMaxIterations (1000);
  seg.setDistanceThreshold (0.01);
  // Create the filtering object
  pcl::ExtractIndices<pcl::PointXYZ> extract;
  int i = 0, nr_points = (int) cloud_filtered->size ();
  // While 30% of the original cloud is still there
  while (cloud_filtered->size () > 0.3 * nr_points)  {
    // Segment the largest planar component from the remaining cloud
    seg.setInputCloud (cloud_filtered);
    seg.segment (*inliers, *coefficients);
    if (inliers->indices.size () == 0)    {
      std::cerr << "Could not estimate a planar model for the given dataset." << std::endl;
      break;
    }
    // Extract the inliers
    extract.setInputCloud (cloud_filtered);
    extract.setIndices (inliers);
    extract.setNegative (false);
    extract.filter (*cloud_p);
    std::cerr << "PointCloud representing the planar component: " << cloud_p->width * cloud_p->height << " data points." << std::endl;
    std::stringstream ss;
    ss << "table_scene_lms400_plane_" << i << ".pcd";
    writer.write<pcl::PointXYZ> (ss.str (), *cloud_p, false);
    // Create the filtering object
    extract.setNegative (true);
    extract.filter (*cloud_f);
    cloud_filtered.swap (cloud_f);
    i++;
  }
  return (0);
}
```

# 最近邻搜索

- 空间索引结构
  - BSP树
  - KD树
  - KDB树
  - R树
  - 四叉树
  - 八叉树

- 点云中常用的基础结构
  - OcTree
  - KdTree

- 作用
  - 搜索（邻域、半径、体素）
  - 将采样（体素网格、体素质心滤波）
  - 点云压缩
  - 空间变化检测
  - 空间点密度分析
  - 占用检查、占用地图
  - 碰撞检测
  - 点云合并

## KDTree
- KDTree实现

```cpp
#include <pcl/point_cloud.h>
#include <pcl/kdtree/kdtree_flann.h>
#include <iostream>
#include <vector>
#include <ctime>
int main (int argc, char** argv){
  srand (time (NULL));
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>);
  // Generate pointcloud data
  cloud->width = 1000;
  cloud->height = 1;
  cloud->points.resize (cloud->width * cloud->height);
  for (std::size_t i = 0; i < cloud->size (); ++i)  {
    (*cloud)[i].x = 1024.0f * rand () / (RAND_MAX + 1.0f);
    (*cloud)[i].y = 1024.0f * rand () / (RAND_MAX + 1.0f);
    (*cloud)[i].z = 1024.0f * rand () / (RAND_MAX + 1.0f);
  }
  pcl::KdTreeFLANN<pcl::PointXYZ> kdtree;
  kdtree.setInputCloud (cloud);
  pcl::PointXYZ searchPoint;
  searchPoint.x = 1024.0f * rand () / (RAND_MAX + 1.0f);
  searchPoint.y = 1024.0f * rand () / (RAND_MAX + 1.0f);
  searchPoint.z = 1024.0f * rand () / (RAND_MAX + 1.0f);
  // K nearest neighbor search
  int K = 10;
  std::vector<int> pointIdxNKNSearch(K);
  std::vector<float> pointNKNSquaredDistance(K);
  std::cout << "K nearest neighbor search at (" << searchPoint.x << " " << searchPoint.y << " " << searchPoint.z<< ") with K=" << K << std::endl;
  if ( kdtree.nearestKSearch (searchPoint, K, pointIdxNKNSearch, pointNKNSquaredDistance) > 0 )  {
    for (std::size_t i = 0; i < pointIdxNKNSearch.size (); ++i)
      std::cout << "    "  <<   (*cloud)[ pointIdxNKNSearch[i] ].x << " " << (*cloud)[ pointIdxNKNSearch[i] ].y << " " << (*cloud)[ pointIdxNKNSearch[i] ].z << " (squared distance: " << pointNKNSquaredDistance[i] << ")" << std::endl;
  }
  // Neighbors within radius search
  std::vector<int> pointIdxRadiusSearch;
  std::vector<float> pointRadiusSquaredDistance;
  float radius = 256.0f * rand () / (RAND_MAX + 1.0f);
  std::cout << "Neighbors within radius search at (" << searchPoint.x 
            << " " << searchPoint.y 
            << " " << searchPoint.z
            << ") with radius=" << radius << std::endl;
  if ( kdtree.radiusSearch (searchPoint, radius, pointIdxRadiusSearch, pointRadiusSquaredDistance) > 0 )
  {
    for (std::size_t i = 0; i < pointIdxRadiusSearch.size (); ++i)
      std::cout << "    "  <<   (*cloud)[ pointIdxRadiusSearch[i] ].x 
                << " " << (*cloud)[ pointIdxRadiusSearch[i] ].y 
                << " " << (*cloud)[ pointIdxRadiusSearch[i] ].z 
                << " (squared distance: " << pointRadiusSquaredDistance[i] << ")" << std::endl;
  }
  return 0;
}
```

- 法向量估计
```cpp
//创建特征估计对象
pcl::NormalEstimation<pcl::PointXYZ,pcl::Normal>ne;
//设置搜索法那个法，创建一个kd树并将其传递给特性估计器
pcl::search::KdTree<pcl::PointXYZ>::Ptr tree(new pcl::search::KdTree<pcl::PointXYZ>());
ne.setSearchMethod(tree);
//指定计算特征时使用的局部邻域大小
ne.setRadiusSearch(r);//ne.setNearestKSearch(k);
//设置输入点云
ne.setInputCloud(cloud);
//计算
pcl::PointCloud<pcl::Normal>::Ptr cloud_normals(new pcl::PointCloud<pcl::Normal>);
ne.compute(*cloud_normals);
```


## OcTree

使用|说明
检查给定坐标体素是否存在|`double X=1.,Y=2.,Z=3.;bool occupied=octree.isVoxelOccupiedAtPoint(X,Y,Z);`
获取所占用体素的中心点|`vector<PointXYZ>pointGrid;octree.getOccupiedVoxelCenters(pointGrid);`
查询点所在的体素内的近邻点|`vector<int>pointIdxVec;octree.voxelSearch(searchPoint,pointIdxVec);`
查询点所在的半径内的近邻点|`vector<int>pointIdxVec;vector<float>pointSquaredDistance;float radius=0.1;octree.radiusSearch(searchPoint,radius,pointIdxVec,pointSquaredDistance);`扫描所有候选体素<br>`octree.approxNearestSearch(searchPoint,radius,pointIdxVec,pointSquaredDistance);`只扫描搜索点体素
K紧邻搜索|`vector<int>pointIdxVec;vector<float>pointSquaredDistance;int K=10;octree.nearestSearch(searchPoint,K,pointIdxVec,pointSquaredDistance);`
光线跟踪|`PointCloud<PointXYZ>::Ptr voxelCenters(new PointCloud<PointXYZ>());Eigen::Vector3f origin(0.,0.,0.),direction(0.1,0.2,0.3);octree.getIntersectedVoxelCenters(origin,direction,voxelCenters->points)`
密度估计|`OctreePointCloudDensity<PintXYZ>`
空间变化检测(双缓冲)|`octree.setInputCloud(cloudA_ptr);`设置输入点云<br>`octree.addPointsFromInputCloud();`从输入点云构建八叉树<br>`octree.switchBuffers();`交换八叉树缓存，但cloudA对应的八叉树结构仍在内存中<br>`octree.setInputCloud(cloudB_ptr);`添加B<br>`octree.addPointsFromInputCloud();`<br>`vector<int>newPointIdxVector;`<br>`octree.getPointIndicesFromNewVoxels(newPointIdxVector);`获取A的八叉树，在B对应的八叉树中A内没有的点集
删除体素|`PointXYZ point_arg(1.,2.,3.);octree.deleteVoxelAtPoint(point);`
迭代器|`OctreePointCloud<PointXYZ>::Iterator it(octreeA);`迭代所有octree节点<br>`OctreePointCloud<PointXYZ>::LeafNodeIterator itL(octreeA);`迭代所有octree叶节点

### 示例

```cpp
#include <pcl/point_cloud.h>
#include <pcl/octree/octree_search.h>
#include <iostream>
#include <vector>
#include <ctime>
int main (int argc, char** argv){
  srand ((unsigned int) time (NULL));
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>);
  // Generate pointcloud data
  cloud->width = 1000;
  cloud->height = 1;
  cloud->points.resize (cloud->width * cloud->height);
  for (std::size_t i = 0; i < cloud->size (); ++i){
    (*cloud)[i].x = 1024.0f * rand () / (RAND_MAX + 1.0f);
    (*cloud)[i].y = 1024.0f * rand () / (RAND_MAX + 1.0f);
    (*cloud)[i].z = 1024.0f * rand () / (RAND_MAX + 1.0f);
  }
  float resolution = 128.0f;
  pcl::octree::OctreePointCloudSearch<pcl::PointXYZ> octree (resolution);
  octree.setInputCloud (cloud);
  octree.addPointsFromInputCloud ();
  pcl::PointXYZ searchPoint;
  searchPoint.x = 1024.0f * rand () / (RAND_MAX + 1.0f);
  searchPoint.y = 1024.0f * rand () / (RAND_MAX + 1.0f);
  searchPoint.z = 1024.0f * rand () / (RAND_MAX + 1.0f);
  // Neighbors within voxel search
  std::vector<int> pointIdxVec;
  if (octree.voxelSearch (searchPoint, pointIdxVec)){
    std::cout << "Neighbors within voxel search at (" << searchPoint.x << " " << searchPoint.y << " " << searchPoint.z << ")" << std::endl;
    for (std::size_t i = 0; i < pointIdxVec.size (); ++i)
   std::cout << "    " << (*cloud)[pointIdxVec[i]].x << " " << (*cloud)[pointIdxVec[i]].y << " " << (*cloud)[pointIdxVec[i]].z << std::endl;
  }
  // K nearest neighbor search
  int K = 10;
  std::vector<int> pointIdxNKNSearch;
  std::vector<float> pointNKNSquaredDistance;
  std::cout << "K nearest neighbor search at (" << searchPoint.x << " " << searchPoint.y << " " << searchPoint.z<< ") with K=" << K << std::endl;
  if (octree.nearestKSearch (searchPoint, K, pointIdxNKNSearch, pointNKNSquaredDistance) > 0)  {
    for (std::size_t i = 0; i < pointIdxNKNSearch.size (); ++i)
      std::cout << "    "  <<   (*cloud)[ pointIdxNKNSearch[i] ].x << " " << (*cloud)[ pointIdxNKNSearch[i] ].y << " " << (*cloud)[ pointIdxNKNSearch[i] ].z << " (squared distance: " << pointNKNSquaredDistance[i] << ")" << std::endl;
  }
  // Neighbors within radius search
  std::vector<int> pointIdxRadiusSearch;
  std::vector<float> pointRadiusSquaredDistance;
  float radius = 256.0f * rand () / (RAND_MAX + 1.0f);
  std::cout << "Neighbors within radius search at (" << searchPoint.x << " " << searchPoint.y << " " << searchPoint.z<< ") with radius=" << radius << std::endl;
  if (octree.radiusSearch (searchPoint, radius, pointIdxRadiusSearch, pointRadiusSquaredDistance) > 0)  {
    for (std::size_t i = 0; i < pointIdxRadiusSearch.size (); ++i)
      std::cout << "    "  <<   (*cloud)[ pointIdxRadiusSearch[i] ].x << " " << (*cloud)[ pointIdxRadiusSearch[i] ].y << " " << (*cloud)[ pointIdxRadiusSearch[i] ].z << " (squared distance: " << pointRadiusSquaredDistance[i] << ")" << std::endl;
  }
}
```


# 分割
- 几何分割
- 语义分割
- 实例分割

## 通过数据拟合获取平面
- pcl提供的RANSAC方法

采样一致性算法|说明
-|-
随机采样一致性算法|RANSAC
加权采样一致性算法|MSAC
最大似然一致性算法|MLESAC
最小中值方差一致性算法|LMEDS
渐进采样一致性算法|PROSAC

- 采样一致性接口
```cpp
pcl::SampleConsensusModel<PointT>;
pcl::SampleConsensusModelPlane<PointT>;
pcl::SampleConsensusModelLine<PointT>;
pcl::SampleConsensusModelCircle3D<PointT>;
pcl::SampleConsensusModelCircle2D<PointT>;
pcl::SampleConsensusModelSphere<PointT>;
pcl::SampleConsensusModelCone<PointT,PointT>;
pcl::SampleConsensusModelCylinder<PointT,PointT>;
pcl::SampleConsensusModelRegistration<PointT>;
pcl::SampleConsensusModelStick<PointT>;
```

拟合模型|说明
-|-
平面模型|`SACMODEL_PLANE[normal_x,normal_y,normal_z,d]`
线模型|`SACMODEL_LINE[point_on_line.x,point_on_line.y,point_on_line.z,point_direction.x,point_direction.y,point_direction.z]`
平面圆模型|`SACMODEL_CIRCLE2D[center.x,center.y,center.z,radius]`
三维圆模型|`SACMODEL_CIRCLE3D[center.x,center.y,center.z,radius,normal.x,normal.y,normal.z]`
球模型|`SACMODEL_SPHERE`
圆柱体模型|`SACMODEL_CYLINDER[point_on_axis.x,point_on_axis.y,point_on_axis.z,axis_direction.x,axis_direction.y,axis_direction.z,radius]`
圆锥体模型|`SACMODEL_CONE[apex.x,apex.y,apex.z,aixis_direction.x,aixis_direction.y,aixis_direction.z,opening_angle]`

## Plane model
```cpp
#include <iostream>
#include <pcl/ModelCoefficients.h>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/sample_consensus/method_types.h>
#include <pcl/sample_consensus/model_types.h>
#include <pcl/segmentation/sac_segmentation.h>
int  main (int argc, char** argv){
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud(new pcl::PointCloud<pcl::PointXYZ>);
  // Fill in the cloud data
  cloud->width  = 15;
  cloud->height = 1;
  cloud->points.resize (cloud->width * cloud->height);
  // Generate the data
  for (auto& point: *cloud)  {
    point.x = 1024 * rand () / (RAND_MAX + 1.0f);
    point.y = 1024 * rand () / (RAND_MAX + 1.0f);
    point.z = 1.0;
  }
  // Set a few outliers
  (*cloud)[0].z = 2.0;(*cloud)[3].z = -2.0;(*cloud)[6].z = 4.0;
  std::cerr << "Point cloud data: " << cloud->size () << " points" << std::endl;
  for (const auto& point: *cloud)
    std::cerr << "    " << point.x << " "<< point.y << " "<< point.z << std::endl;
  //模型系数
  pcl::ModelCoefficients::Ptr coefficients (new pcl::ModelCoefficients);
  pcl::PointIndices::Ptr inliers (new pcl::PointIndices);
  // Create the segmentation object
  pcl::SACSegmentation<pcl::PointXYZ> seg;
  // Optional优化模型系数
  seg.setOptimizeCoefficients (true);
  // Mandatory设置模型和采样方法
  seg.setModelType (pcl::SACMODEL_PLANE);//平面模型
  seg.setMethodType (pcl::SAC_RANSAC);//随即一致性采样
  seg.setDistanceThreshold (0.01);
  seg.setInputCloud (cloud);
  seg.segment (*inliers, *coefficients);
  if (inliers->indices.size () == 0)  {
    PCL_ERROR ("Could not estimate a planar model for the given dataset.");
    return (-1);
  }
  std::cerr << "Model coefficients: " << coefficients->values[0] << " " << coefficients->values[1] << " "<< coefficients->values[2] << " " << coefficients->values[3] << std::endl;
  std::cerr << "Model inliers: " << inliers->indices.size () << std::endl;
  for (std::size_t i = 0; i < inliers->indices.size (); ++i)
  for (const auto& idx: inliers->indices)
    std::cerr << idx << "    " << cloud->points[idx].x << " "<< cloud->points[idx].y << " "<< cloud->points[idx].z << std::endl;
  return (0);
}
```

## 聚类
- 根据平面模型分割出物体表面上的物体
- 聚类找出例如桌面上的东西
- 计算平面的凸包，分割出物体


### 分割拟合聚类
- ExtractPolygonalPrismData

```cpp
include <pcl/ModelCoefficients.h>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/sample_consensus/method_types.h>
#include <pcl/sample_consensus/model_types.h>
#include <pcl/filters/passthrough.h>
#include <pcl/filters/project_inliers.h>
#include <pcl/segmentation/sac_segmentation.h>
#include <pcl/surface/convex_hull.h>
int main (int argc, char** argv){
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>), cloud_filtered (new pcl::PointCloud<pcl::PointXYZ>), cloud_projected (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::PCDReader reader;
  reader.read ("table_scene_mug_stereo_textured.pcd", *cloud);
  // Build a filter to remove spurious NaNs
  pcl::PassThrough<pcl::PointXYZ> pass;
  pass.setInputCloud (cloud);
  pass.setFilterFieldName ("z");
  pass.setFilterLimits (0, 1.1);
  pass.filter (*cloud_filtered);
  std::cerr << "PointCloud after filtering has: " << cloud_filtered->size () << " data points." << std::endl;
  pcl::ModelCoefficients::Ptr coefficients (new pcl::ModelCoefficients);
  pcl::PointIndices::Ptr inliers (new pcl::PointIndices);
  // Create the segmentation object
  pcl::SACSegmentation<pcl::PointXYZ> seg;
  // Optional
  seg.setOptimizeCoefficients (true);
  // Mandatory
  seg.setModelType (pcl::SACMODEL_PLANE);
  seg.setMethodType (pcl::SAC_RANSAC);
  seg.setDistanceThreshold (0.01);
  seg.setInputCloud (cloud_filtered);
  seg.segment (*inliers, *coefficients);
  // Project the model inliers
  pcl::ProjectInliers<pcl::PointXYZ> proj;
  proj.setModelType (pcl::SACMODEL_PLANE);
  proj.setInputCloud (cloud_filtered);
  proj.setIndices (inliers);
  proj.setModelCoefficients (coefficients);
  proj.filter (*cloud_projected);
  // Create a Convex Hull representation of the projected inliers
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_hull (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::ConvexHull<pcl::PointXYZ> chull;
  chull.setInputCloud (cloud_projected);
  chull.reconstruct (*cloud_hull);
  std::cerr << "Convex hull has: " << cloud_hull->size () << " data points." << std::endl;
  pcl::PCDWriter writer;
  writer.write ("table_scene_mug_stereo_textured_hull.pcd", *cloud_hull, false);
  return (0);
}
```

### 欧几里德聚类算法

```cpp
#include <pcl/ModelCoefficients.h>
#include <pcl/point_types.h>
#include <pcl/io/pcd_io.h>
#include <pcl/filters/extract_indices.h>
#include <pcl/filters/voxel_grid.h>
#include <pcl/features/normal_3d.h>
#include <pcl/kdtree/kdtree.h>
#include <pcl/sample_consensus/method_types.h>
#include <pcl/sample_consensus/model_types.h>
#include <pcl/segmentation/sac_segmentation.h>
#include <pcl/segmentation/extract_clusters.h>
int main (int argc, char** argv){
  // Read in the cloud data
  pcl::PCDReader reader;
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>), cloud_f (new pcl::PointCloud<pcl::PointXYZ>);
  reader.read ("table_scene_lms400.pcd", *cloud);
  std::cout << "PointCloud before filtering has: " << cloud->size () << " data points." << std::endl; //*
  // Create the filtering object: downsample the dataset using a leaf size of 1cm
  pcl::VoxelGrid<pcl::PointXYZ> vg;
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_filtered (new pcl::PointCloud<pcl::PointXYZ>);
  vg.setInputCloud (cloud);
  vg.setLeafSize (0.01f, 0.01f, 0.01f);
  vg.filter (*cloud_filtered);
  std::cout << "PointCloud after filtering has: " << cloud_filtered->size ()  << " data points." << std::endl; //*
  // Create the segmentation object for the planar model and set all the parameters
  pcl::SACSegmentation<pcl::PointXYZ> seg;
  pcl::PointIndices::Ptr inliers (new pcl::PointIndices);
  pcl::ModelCoefficients::Ptr coefficients (new pcl::ModelCoefficients);
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_plane (new pcl::PointCloud<pcl::PointXYZ> ());
  pcl::PCDWriter writer;
  seg.setOptimizeCoefficients (true);
  seg.setModelType (pcl::SACMODEL_PLANE);
  seg.setMethodType (pcl::SAC_RANSAC);
  seg.setMaxIterations (100);
  seg.setDistanceThreshold (0.02);
  int i=0, nr_points = (int) cloud_filtered->size ();
  while (cloud_filtered->size () > 0.3 * nr_points){
    // Segment the largest planar component from the remaining cloud
    seg.setInputCloud (cloud_filtered);
    seg.segment (*inliers, *coefficients);
    if (inliers->indices.size () == 0){
      std::cout << "Could not estimate a planar model for the given dataset." << std::endl;
      break;
    }
    // Extract the planar inliers from the input cloud
    pcl::ExtractIndices<pcl::PointXYZ> extract;
    extract.setInputCloud (cloud_filtered);
    extract.setIndices (inliers);
    extract.setNegative (false);
    // Get the points associated with the planar surface
    extract.filter (*cloud_plane);
    std::cout << "PointCloud representing the planar component: " << cloud_plane->size () << " data points." << std::endl;
    // Remove the planar inliers, extract the rest
    extract.setNegative (true);
    extract.filter (*cloud_f);
    *cloud_filtered = *cloud_f;
  }
  // Creating the KdTree object for the search method of the extraction
  pcl::search::KdTree<pcl::PointXYZ>::Ptr tree (new pcl::search::KdTree<pcl::PointXYZ>);
  tree->setInputCloud (cloud_filtered);
  std::vector<pcl::PointIndices> cluster_indices;
  pcl::EuclideanClusterExtraction<pcl::PointXYZ> ec;
  ec.setClusterTolerance (0.02); // 2cm
  ec.setMinClusterSize (100);
  ec.setMaxClusterSize (25000);
  ec.setSearchMethod (tree);
  ec.setInputCloud (cloud_filtered);
  ec.extract (cluster_indices);
  int j = 0;
  for (std::vector<pcl::PointIndices>::const_iterator it = cluster_indices.begin (); it != cluster_indices.end (); ++it){
    pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_cluster (new pcl::PointCloud<pcl::PointXYZ>);
    for (std::vector<int>::const_iterator pit = it->indices.begin (); pit != it->indices.end (); ++pit)
      cloud_cluster->push_back ((*cloud_filtered)[*pit]); //*
    cloud_cluster->width = cloud_cluster->size ();
    cloud_cluster->height = 1;
    cloud_cluster->is_dense = true;
    std::cout << "PointCloud representing the Cluster: " << cloud_cluster->size () << " data points." << std::endl;
    std::stringstream ss;
    ss << "cloud_cluster_" << j << ".pcd";
    writer.write<pcl::PointXYZ> (ss.str (), *cloud_cluster, false); //*
    j++;
  }
  return (0);
}
```

### 区域分割

```cpp
#include <iostream>
#include <vector>
#include <pcl/point_types.h>
#include <pcl/io/pcd_io.h>
#include <pcl/search/search.h>
#include <pcl/search/kdtree.h>
#include <pcl/features/normal_3d.h>
#include <pcl/visualization/cloud_viewer.h>
#include <pcl/filters/passthrough.h>
#include <pcl/segmentation/region_growing.h>
int main (int argc, char** argv){
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>);
  if ( pcl::io::loadPCDFile <pcl::PointXYZ> ("region_growing_tutorial.pcd", *cloud) == -1){
    std::cout << "Cloud reading failed." << std::endl;
    return (-1);
  }
  pcl::search::Search<pcl::PointXYZ>::Ptr tree (new pcl::search::KdTree<pcl::PointXYZ>);
  pcl::PointCloud <pcl::Normal>::Ptr normals (new pcl::PointCloud <pcl::Normal>);
  pcl::NormalEstimation<pcl::PointXYZ, pcl::Normal> normal_estimator;
  normal_estimator.setSearchMethod (tree);
  normal_estimator.setInputCloud (cloud);
  normal_estimator.setKSearch (50);
  normal_estimator.compute (*normals);
  pcl::IndicesPtr indices (new std::vector <int>);
  pcl::PassThrough<pcl::PointXYZ> pass;
  pass.setInputCloud (cloud);
  pass.setFilterFieldName ("z");
  pass.setFilterLimits (0.0, 1.0);
  pass.filter (*indices);
  pcl::RegionGrowing<pcl::PointXYZ, pcl::Normal> reg;
  reg.setMinClusterSize (50);
  reg.setMaxClusterSize (1000000);
  reg.setSearchMethod (tree);
  reg.setNumberOfNeighbours (30);
  reg.setInputCloud (cloud);
  //reg.setIndices (indices);
  reg.setInputNormals (normals);
  reg.setSmoothnessThreshold (3.0 / 180.0 * M_PI);
  reg.setCurvatureThreshold (1.0);
  std::vector <pcl::PointIndices> clusters;
  reg.extract (clusters);
  std::cout << "Number of clusters is equal to " << clusters.size () << std::endl;
  std::cout << "First cluster has " << clusters[0].indices.size () << " points." << std::endl;
  std::cout << "These are the indices of the points of the initial" <<
    std::endl << "cloud that belong to the first cluster:" << std::endl;
  int counter = 0;
  while (counter < clusters[0].indices.size ()){
    std::cout << clusters[0].indices[counter] << ", ";
    counter++;
    if (counter % 10 == 0)
      std::cout << std::endl;
  }
  std::cout << std::endl;
  pcl::PointCloud <pcl::PointXYZRGB>::Ptr colored_cloud = reg.getColoredCloud ();
  pcl::visualization::CloudViewer viewer ("Cluster viewer");
  viewer.showCloud(colored_cloud);
  while (!viewer.wasStopped ())  {}
  return (0);
}
```




# 配准

## ICP
**算法步骤：**
- (1)对点集A中每个点Pa施加出事变换T0，得到Pa’；
- (2)从点集B中寻找距离点Pa’最近的点Pb，形成对应点对；
- (3)求解最优变换△T；
- (4)根据前后两次的迭代误差以及迭代次数等条件判断是否收敛，如果收敛，则输出最终结果：`T=△T*T0`，否则`T0=△T*T0`，重复步骤(1)


- $\triangle T=\argmin\sum_{i=1}{n}\|p_{bi}-(Rp_{ai}+t)\|^2_2$
- $\triangle T=\begin{bmatrix}R&t\\0&1\end{bmatrix}$


**终止条件：**
- (1)最大迭代次数
- (2)收敛标准：估计点云变换不再变化：当前变换和上次变换的差值之和小于用户定义的阈值
- (3)找到解：误差平方和小于用户自定义阈值


### 简单使用

```cpp
pcl::IterativeClosestPoint<pcl::PointXYZ,pcl::PointXYZ> icp;
icp.setInputCloud(cloud_in);
icp.setInputTarget(cloud_out);
icp.setMaximumIterations(nr_iterations);
icp.setTransformationEpsilon(epsilon);
icp.setEuclideanFitnessEpsilon(distance);
pcl::PointCloud<pcl::PointXYZ>Final;
icp.align(Final);
```



### 接口

常用接口|说明
-|-
实例化|`pcl::IterativeClosestPoint<pcl::PointXYZ, pcl::PointXYZ> icp;`
最大迭代次数|`icp.setMaximumIterations(100);`
源点云，转换的点云|`icp.setInputSource(cloud_in);`
目标点云，参考点云|`icp.setInputTarget(cloud_out);`
设置查找近邻时是否双向搜索|`icp.setUseReciprocalCorrespondences(false);`
设置最大临近点距离，超过则不参与计算|`icp.setMaxCorrespondenceDistance(0.5);`
设置匹配对剔除器|`icp.addCorrespondenceRejector(rejector);`
设置迭代误差收敛阈值|`icp.setEuclideanFitnessEpsilon(0.0001);`
设置icp计算方法：SVD、LM|`icp.setTransformationEstimation(te);`
执行运算|`icp.align(*cloud_icp);`
获取icp是否收敛|`icp.hasConverged();`
获取icp匹配得到的变换矩阵|`Eigen::Matrix4f transformation_matrix = icp.getFinalTransformation();`
获取icp得分|`icp.getFitnessScore();`


## 点云配准方法

```cpp
Eigen::Matrix4f transformation;
TransformationEstimationSVD<PointT,PointT>svd;
svd.estimateRigidTransformation(src,target,corres,trans);
```

## 广义配准
- ICP使用最近邻迭代搜索，容易调入局部极小，并且初始变换矩阵影响算法结果。广义配准利用点云特征匹配，可以很好的估计两组点云的变换状态，能够得到良好的效果。


**算法步骤：**
- (1)采集原始点云，分析两个原点云关键点k；
- (2)在每个关键点k处，计算相应的特征描述子f
- (3)根据特征描述子f和对应的xyz坐标值，估计对应匹配关系；
- (4)因为噪声或虚假匹配，并非所有的对应关系都有效，根据规则排除虚假对应匹配；
- (5)根据有效对应匹配关系，估计刚体变换关系；

### Matching Features
```cpp
CorrespondenceEstimation<FeatureT,FeatureT>est;
est.setInputClout(source_features);
est.setInputTarget(target_features);
est.determineCorrespondences(correspondences);
```

### 虚假匹配
**用sac排除虚假匹配(outliers)**
- (1)选择三对对应匹配对d，m；
- (2)估计选择样本的变换关系(R,t)；
- (3)用((Rd+t)-m)2<c确定内点对；
- (4)重复N次，使得(R,t)有更多的内点；

```cpp
CorrespondenceRejectorSampleConsensus<PointT> sac;
sac.setInputCloud(source);
sac.setTargetCloud(target);
sac.setInlierThreshold(epsilon);
sac.setMaxIterations(N);
sac.setInputCorrespondences(correspondences);
sac.getCorrespondences(inliers);
Eigen::Matrix4f t_matrix = sac.getBestTransformation();
```

**sac-ia(Sampled Consesus-Initial Alignment)**
- (1)在原点云中选择n个点d；
- (2)对每个点d：1、选择k个最近匹配；2、选择其中一个为匹配点m；
- (3)估计选择样本的变换关系(R,t)；
- (4)用((Rd+t)-m)2<c确定内点对；
- (5)重复N次，使得(R,t)有更多的内点；

```cpp
pcl::SampleConsensusInitalAlignment<PointT,PointT,FeatureT> sac_ia;
sac_ia.setNumberOfSamples(n);
sac_ia.setMinSampleDistance(d);
sac_ia.setCorrespondenceRandomness(k);
sac_ia.setMaximumIterations(N);
sac_ia.setInputCloud(source);
sac_ia.setInputTarget(target);
sac_ia.setSourceFeatures(source_features);
sac_ia.setTargetFeatures(target_features);
sac_ia.align(aligned_source);
Eigen::Matrix4f t_matrix = sac_ia.getFinalTransformation();
```

### 接口

方法|说明
-|-
广义配准实例化|`CorrespondenceEstimation<FeatureT,FeatureT>est;`
输入点云|`est.setInputSource(source_feature);`
目标点云|`est.setInputTarget(target_feature);`
执行运算|`est.determineCorrespondences(correspondences);`
RANSAC去除虚假匹配|`CorrespondenceRejectorSampleConsensus<PointT>sac;`
输入点云|`sac.setInputSource(source);`
目标点云|`sac.setInputTarget(target);`
inlier阈值|`sac.InlierThreshold(epsilon);`
最大迭代次数|`sac.setMaximumIterations(100);`
执行运算|`sac.setInputCorrespondences(correspondences);`
执行运算|`sac.getInputCorrespondences(inliers);`
获取匹配的变换矩阵|`Eigen::Matrix4f transformation_matrix = sac.getBestTransformation();`
SAC-IA初始对齐|`SampleConsensusInitialAlignment<PointT,PointT,FeatureT>sac_ia;`
输入点云|`sac_ia.setInputCloud(source);`
目标点云|`sac_ia.setInputTarget(target);`
特征点个数|`sac_ia.setNumberOfSamples(n);`
最小距离|`sac_ia.setMinSampleDistance(d);`
随机参数|`sac_ia.setCorrespondenceRandomness(k);`
最大迭代次数|`sac_ia.setMaximumIterations(100);`
输入特征|`sac_ia.setSourceFeatures(source_features);`
目标特征|`sac_ia.setTargetFeatures(target_features);`
获取匹配的变换矩阵|`Eigen::Matrix4f transformation_matrix = sac_ia.getFinalTransformation();`

## 配准API

类|说明
-|-
`CorrespondenceEstimation`|确定两点云中对应匹配关系
`CorrespondenceRejectorFeatures`|能够实现基于特征匹配的对应点对提取
`SampleConsensusPrerejective`|实现了基于RANSAC方法的对应对外点去除，用于姿态评估和点云配准中的几何一致性限制
`TransformationEstimation3Point`|实现使用三个匹配点计算两点云变换矩阵
`TransfromationFromCorrsespondences`|实现三个及以上匹配点间变换矩阵的计算
`TransfromationEstimationDQ`<br>`TransfromationEstimationDualQuaternion`|实现基于对偶四元数的点云姿态估计
`TransfromationEstimationLM`|实现基于对应匹配点对，用LM优化的变换矩阵估计
`TransfromationEstimationPointToPlane`|实现基于LM优化的匹配点对间最小点到平面距离的最优变换矩阵计算
`TransfromationEstimationPointtoPlaneWeighted`|在TransfromationEstimationPointToPlane基础上加入了对应匹配点的权重，以适应更为复杂场景点云配准任务
`TransfromationEstimationPointToPlaneLLS`|基于现行最小二乘的点到平面距离累计最小化约束，对应点到平面的距离计算加入法向量约束
`TransfromationEstimationPointToPlaneLLSWeighted`|带权重的PointToPlaneLLS
`TransfromationEstimationSVD`|使用SVD分解策略完成匹配点对的变换矩阵计算
`TransfromationEstimationEuclidean`|实现了给定对应匹配的变换，并计算可靠性得分
`IterativeClosestPoint`|ICP算法
`IterativeClosestPointNoLinear`|ICP后端使用LM作为优化
`GeneratlizedIterativeClosestPoint`|一般化ICP算法
`GeneratlizedIterativeClosestPoint6D`|集成L*a*b颜色空间到Generalized-ICP中
`IterativeClosestPointWithNormal`|以点到平面的最小距离作为迭代优化对象实现ICP配准
`FPCSInitialAlignment`|
`KFPCSInitialAlignment`|
`NormalDistributionsTransform`|NDT方法


# 关键点
- pcl::keypoints类

## ISS

ISS|说明
-|-
实例化|`pcl::ISSKeypoint3d<pcl::PointXYZ，pcl::PointXYZ>iss_detector;`
-|`pcl::search::KdTree<pcl::PointXYZ>::Ptr tree(new pcl::search::KdTree<pcl::PointXYZ>)`<br>`iss_detector.setSearchMethod(tree)`
-|`iss_detector.setSalientRadius(support_radius)`
-|`iss_detector.setNonMaxRadius(nms_radius);`
-|`iss_detector.setThreshold21(0.975);`
-|`iss_detector.setThreshold32(0.975);`
-|`iss_detector.setMinNeighbors(5);`
-|`iss_detector.setNumberOfThreads(4);`
-|`iss_detector.setInputCloud(cloud);`
-|`iss_detector.compute(keyPoints);`


## Harris3D

Harris3D|说明
-|-
实例化|`pcl::HarrisKeypoint3d<pcl::PointXYZ，pcl::PointXYZI,pcl::Normal>harris;`输出保存在I分量
-|`harris.setInputCloud(point_cloud_ptr);`
-|`harris.setNonMaxSupression(true);`
块半径|`harris.setRadius(0.02f);`
数量阈值|`harris.setThreshold(0.01f);`
-|`harris.compute(cloud_out);`

## SIFT

SIFT|说明
-|-
实例化|`pcl::SIFTKeypoint<pcl::PointXYZ，pcl::PointWithScale>sift_src;`
-|`sift_src.setSearchMethod(tree);`
-|`sift_src.setScales(min_scale,n_octaves,n_scales_per_octave);`min_scale：尺度空间中最小尺度,n_octaves：尺度空间层数,n_scales_per_octave：尺度空间层中计算的尺度个数
-|`sift_src.setMinimumContrast(min_contrast);`min_contrast：根据点云设置，越小关键点越多
-|`sift_src.setInputCloud(cloud_src);`
-|`sift_src.compute(result);`


# 描述子
- pcl::features类
- 局部描述子
  - Spin Image
  - 3DSC
  - PFH/FPFH
  - SHOT/SHOT for RGBD
- 全局描述子
  - 基于直方图
    - Shape Distributions
    - 3D Shape Histograms
    - Orientation Histograms
    - Viewpoint Feature Histogram
    - Clustered VFH
    - OUR-CVFH
  - 基于变换
    - 3D Fourier Transform
    - Angular Radial Tr.
    - 3D Radon Tr
    - Spherical Harmonics
    - wavelets
  - 基于2D
    - Fourier Descriptors
    - Zernike Moments
    - SIFT
    - SURF
  - 基于连通图
    - topology-based
    - Reeb graph
    - skeleton-based

## 方法

方法|说明
Spin Image|`pcl::SpinImageEstimation`
3D Unique shape Contexts|`pcl::ShapeContext3DEstimation`
PFH/FPFH|`pcl::PFHEstimation`<br>`pcl::FPFHEstimation`
SHOT/SHOT for RGBD|`pcl::SHOTEstimation`<br>`pcl::SHOTColorEstimation`


## 示例

```cpp
#include <iostream>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/registration/icp.h>
int main (int argc, char** argv)
{
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_in (new pcl::PointCloud<pcl::PointXYZ>(5,1));
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_out (new pcl::PointCloud<pcl::PointXYZ>);
  // Fill in the CloudIn data
  for (auto& point : *cloud_in)  {
    point.x = 1024 * rand() / (RAND_MAX + 1.0f);
    point.y = 1024 * rand() / (RAND_MAX + 1.0f);
    point.z = 1024 * rand() / (RAND_MAX + 1.0f);
  }
  std::cout << "Saved " << cloud_in->size () << " data points to input:" << std::endl;
  for (auto& point : *cloud_in)
    std::cout << point << std::endl;
  *cloud_out = *cloud_in;
  std::cout << "size:" << cloud_out->size() << std::endl;
  for (auto& point : *cloud_out)
    point.x += 0.7f;
  std::cout << "Transformed " << cloud_in->size () << " data points:" << std::endl;
  for (auto& point : *cloud_out)
    std::cout << point << std::endl;
  pcl::IterativeClosestPoint<pcl::PointXYZ, pcl::PointXYZ> icp;
  icp.setInputSource(cloud_in);
  icp.setInputTarget(cloud_out);
  pcl::PointCloud<pcl::PointXYZ> Final;
  icp.align(Final);
  std::cout << "has converged:" << icp.hasConverged() << " score: " <<
  icp.getFitnessScore() << std::endl;
  std::cout << icp.getFinalTransformation() << std::endl;
 return (0);
}
```
















