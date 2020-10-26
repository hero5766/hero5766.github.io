---
title: ceres
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-10-26 17:07:20
---
> ceres最小二乘法

<!-- more -->

# 简介
## 介绍
- Ceres solver 是谷歌开发的一款用于非线性优化的库，在谷歌的开源激光雷达slam项目cartographer中被大量使用。
- [Ceres官网](http://ceres-solver.org/nnls_tutorial.html)


## 安装
```bash
apt-get install libgoogle-glog-dev
git clone https://github.com/ceres-solver/ceres-solver
```

## 仿函数
- 仿函数的技巧，即在CostFunction结构体内，对`()`进行重载。这样的话，该结构体的一个实例就能具有类似一个函数的性质，在代码编写过程中就能当做一个函数一样来使用。

```cpp
//仿函数的示例代码
#include<iostream>
using namespace std;
class Myclass
{
public:
  Myclass(int x):_x(x){};
  int operator()(const int n)const{
    return n*_x;
  }
private:
    int _x;
};
int main()
{
    Myclass Obj1(5);
    cout<<Obj1(3)<<endl;
    system("pause");
return 0;
}
```


# 使用[^1]
[^1]: [链接](https://www.jianshu.com/p/e5b03cf22c80)

## 引用
```cmake
//CMakeLists.txt：
cmake_minimum_required(VERSION 2.8)
project(ceres)

find_package(Ceres REQUIRED)
include_directories(${CERES_INCLUDE_DIRS})

add_executable(use_ceres use_ceres.cpp)
target_link_libraries(use_ceres ${CERES_LIBRARIES})
```

## 例程
- 使用Ceres求解非线性优化问题，一共分为三个部分：
  1. 构建cost fuction，即代价函数，也就是寻优的目标式。这个部分需要使用仿函数（functor）这一技巧来实现，做法是定义一个cost function的结构体，在结构体内重载（）运算符，具体实现方法后续介绍。
  2. 通过代价函数构建待求解的优化问题。
  3. 配置求解器参数并求解问题，这个步骤就是设置方程怎么求解、求解过程是否输出等，然后调用一下Solve方法。

## 求导法

求导法|说明
-|-
自动求导法|AutoDiffCostFunction
数值求导法|NumericDiffCostFunction


# 示例
## 简单函数的最优值
- 求x使得1/2*(10-x)^2取到最小值

```cpp
#include<iostream>
#include<ceres/ceres.h>
using namespace std;
using namespace ceres;
//第一部分：构建代价函数，重载（）符号，仿函数的小技巧
struct CostFunctor {
   template <typename T>
   bool operator()(const T* const x, T* residual) const {
     residual[0] = T(10.0) - x[0];
     return true;
   }
};
//主函数
int main(int argc, char** argv) {
  google::InitGoogleLogging(argv[0]);
  // 寻优参数x的初始值，为5
  double initial_x = 5.0;
  double x = initial_x;
  // 第二部分：构建寻优问题
  Problem problem;  
  CostFunction* cost_function = new AutoDiffCostFunction<CostFunctor, 1, 1>(new CostFunctor); //使用自动求导，将之前的代价函数结构体传入，第一个1是输出维度，即残差的维度，第二个1是输入维度，即待寻优参数x的维度。
  problem.AddResidualBlock(cost_function, NULL, &x); //向问题中添加误差项，本问题比较简单，添加一个就行。
  //第三部分： 配置并运行求解器
  Solver::Options options;
  options.linear_solver_type = ceres::DENSE_QR; //配置增量方程的解法
  options.minimizer_progress_to_stdout = true;//输出到cout
  Solver::Summary summary;//优化信息
  Solve(options, &problem, &summary);//求解!!!
  std::cout << summary.BriefReport() << "\n";//输出优化的简要信息
  //最终结果
  std::cout << "x : " << initial_x << " -> " << x << "\n";
  return 0;
}
```

## 曲线拟合
- $y=e^{3x^2+2x+1}$
- 整个代码的思路还是先构建代价函数结构体，然后在[0,1]之间均匀生成待拟合曲线的1000个数据点，加上方差为1的白噪声，数据点用两个vector储存（x_data和y_data），然后构建待求解优化问题，最后求解，拟合曲线参数。

```cpp
#include<iostream>
#include<opencv2/core/core.hpp>
#include<ceres/ceres.h>
using namespace std;
using namespace cv;
//构建代价函数结构体，abc为待优化参数，residual为残差。
struct CURVE_FITTING_COST
{
  CURVE_FITTING_COST(double x,double y):_x(x),_y(y){}
  template <typename T>
  bool operator()(const T* const abc,T* residual)const
  {
    residual[0]=_y-ceres::exp(abc[0]*_x*_x+abc[1]*_x+abc[2]);
    return true;
  }
  const double _x,_y;
};
//主函数
int main()
{
  //参数初始化设置，abc初始化为0，白噪声方差为1（使用OpenCV的随机数产生器）。
  double a=3,b=2,c=1;
  double w=1;
  RNG rng;
  double abc[3]={0,0,0};

//生成待拟合曲线的数据散点，储存在Vector里，x_data，y_data。
  vector<double> x_data,y_data;
  for(int i=0;i<1000;i++)
  {
    double x=i/1000.0;
    x_data.push_back(x);
    y_data.push_back(exp(a*x*x+b*x+c)+rng.gaussian(w));
  }
//反复使用AddResidualBlock方法（逐个散点，反复1000次）
//将每个点的残差累计求和构建最小二乘优化式
//不使用核函数，待优化参数是abc
  ceres::Problem problem;
  for(int i=0;i<1000;i++)
  {
    problem.AddResidualBlock(
      new ceres::AutoDiffCostFunction<CURVE_FITTING_COST,1,3>(
        new CURVE_FITTING_COST(x_data[i],y_data[i])
      ),
      nullptr,
      abc
    );
  }
//配置求解器并求解，输出结果
  ceres::Solver::Options options;
  options.linear_solver_type=ceres::DENSE_QR;
  options.minimizer_progress_to_stdout=true;
  ceres::Solver::Summary summary;
  ceres::Solve(options,&problem,&summary);
  cout<<"a= "<<abc[0]<<endl;
  cout<<"b= "<<abc[1]<<endl;
  cout<<"c= "<<abc[2]<<endl;
return 0;
}
```
- 求解优化问题中（比如拟合曲线），数据中往往会有离群点、错误值什么的，最终得到的寻优结果很容易受到影响，此时就可以使用一些损失核函数来对离群点的影响加以消除。要使用核函数，只需要把上述代码中的NULL或nullptr换成损失核函数结构体的实例。
- Ceres库中提供的核函数主要有：TrivialLoss 、HuberLoss、 SoftLOneLoss 、 CauchyLoss。
- 比如此时要使用CauchyLoss，只需要将nullptr换成new CauchyLoss(0.5)就行（0.5为参数）。
- 下面两图别为Ceres官网上的例程的结果，可以明显看出使用损失核函数之后的曲线收离群点的影响更小。


## BundleAdjustment
- 在给定一组实测图像特征位置及其对应关系的情况下，进行束调整的目标是找到使重投影误差最小的三维点位置和相机参数。该优化问题通常表示为非线性最小二乘问题，其误差为观测到的特征位置与对应的三维点在相机成像平面上投影之差的L2平方模。
- 模板针孔相机模型。相机使用9个参数进行参数化:3个参数用于旋转，3个参数用于平移，1个参数用于焦距，2个参数用于径向畸变。主点没有建模(即假设位于图像中心)。

```cpp
#include <cstdio>
#include <iostream>
#include "ceres/ceres.h"
#include "ceres/rotation.h"
//这个类用去读取BAL数据集相机、照片等相关信息的类，大致了解下
// Read a Bundle Adjustment in the Large dataset.
class BALProblem {
 public:
  ~BALProblem() {
    delete[] point_index_;
    delete[] camera_index_;
    delete[] observations_;
    delete[] parameters_;
  }
  int num_observations()       const { return num_observations_;               }
  const double* observations() const { return observations_;                   }
  double* mutable_cameras()          { return parameters_;                     }
  double* mutable_points()           { return parameters_  + 9 * num_cameras_; }
  //每个相机对应的内参和外参
  double* mutable_camera_for_observation(int i) {
    return mutable_cameras() + camera_index_[i] * 9;
  }
  //对应数据点所在观测下的坐标
  double* mutable_point_for_observation(int i) {
    return mutable_points() + point_index_[i] * 3;
  }
  bool LoadFile(const char* filename) {
    FILE* fptr = fopen(filename, "r");
    if (fptr == NULL) {
      return false;
    };
    FscanfOrDie(fptr, "%d", &num_cameras_);
    FscanfOrDie(fptr, "%d", &num_points_);
    FscanfOrDie(fptr, "%d", &num_observations_);
    point_index_ = new int[num_observations_];
    camera_index_ = new int[num_observations_];
    observations_ = new double[2 * num_observations_];
    num_parameters_ = 9 * num_cameras_ + 3 * num_points_;
    parameters_ = new double[num_parameters_];
    for (int i = 0; i < num_observations_; ++i) {
      FscanfOrDie(fptr, "%d", camera_index_ + i);
      FscanfOrDie(fptr, "%d", point_index_ + i);
      for (int j = 0; j < 2; ++j) {
        FscanfOrDie(fptr, "%lf", observations_ + 2*i + j);
      }
    }
    for (int i = 0; i < num_parameters_; ++i) {
      FscanfOrDie(fptr, "%lf", parameters_ + i);
    }
    return true;
  }
 private:
    template<typename T>
    void FscanfOrDie(FILE *fptr, const char *format, T *value) {
	int num_scanned = fscanf(fptr, format, value);
	if (num_scanned != 1) {
	LOG(FATAL) << "Invalid UW data file.";
      }
  }
  int num_cameras_;
  int num_points_;
  int num_observations_;
  int num_parameters_;
  int* point_index_;
  int* camera_index_;
  double* observations_;
  double* parameters_;
};
//模板针孔相机模型。相机使用9个参数进行参数化:3个参数用于旋转，
//3个参数用于平移，1个参数用于焦距，2个参数用于径向畸变。
//主点没有建模(即假设位于图像中心)。
struct SnavelyReprojectionError {
  SnavelyReprojectionError(double observed_x, double observed_y)
      : observed_x(observed_x), observed_y(observed_y) {}
  template <typename T>
  bool operator()(const T* const camera,
                  const T* const point,
                  T* residuals) const {
//把空间点变成像素坐标p=R*Pw+t；
    T p[3];
    ceres::AngleAxisRotatePoint(camera, point, p);
    // camera[3,4,5] are the translation.
    p[0] += camera[3];
    p[1] += camera[4];
    p[2] += camera[5];
    //考虑相机的径向畸变，并进行校正，计算最终的像素坐标
    T xp = - p[0] / p[2];
    T yp = - p[1] / p[2];
    const T& l1 = camera[7];
    const T& l2 = camera[8];
    T r2 = xp*xp + yp*yp;
    T distortion = 1.0 + r2  * (l1 + l2  * r2);
    // Compute final projected point position.
    const T& focal = camera[6];
    T predicted_x = focal * distortion * xp;
    T predicted_y = focal * distortion * yp;
    //预测像素坐标和观测坐标在x，y方向上的误差
    // The error is the difference between the predicted and observed position.
    residuals[0] = predicted_x - observed_x;
    residuals[1] = predicted_y - observed_y;
    return true;
  }
  // Factory to hide the construction of the CostFunction object from
  // the client code.
  static ceres::CostFunction* Create(const double observed_x,
                                     const double observed_y) {
    return (new ceres::AutoDiffCostFunction<SnavelyReprojectionError, 2, 9, 3>(
                new SnavelyReprojectionError(observed_x, observed_y)));
  }
  double observed_x;
  double observed_y;
};
int main(int argc, char** argv) {
  google::InitGoogleLogging(argv[0]);
  if (argc != 2) {
    std::cerr << "usage: simple_bundle_adjuster <bal_problem>\n";
    return 1;
  }
  BALProblem bal_problem;
  if (!bal_problem.LoadFile(argv[1])) {
    std::cerr << "ERROR: unable to open file " << argv[1] << "\n";
    return 1;
  }
  const double* observations = bal_problem.observations();
  ceres::Problem problem;
  for (int i = 0; i < bal_problem.num_observations(); ++i) {
    ceres::CostFunction* cost_function =
        SnavelyReprojectionError::Create(observations[2 * i + 0],
                                         observations[2 * i + 1]);
    problem.AddResidualBlock(cost_function,
                             NULL /* squared loss */,
                             bal_problem.mutable_camera_for_observation(i),
                             bal_problem.mutable_point_for_observation(i));
  }
  ceres::Solver::Options options;
  options.linear_solver_type = ceres::DENSE_SCHUR;
  options.minimizer_progress_to_stdout = true;
  ceres::Solver::Summary summary;
  ceres::Solve(options, &problem, &summary);
  std::cout << summary.FullReport() << "\n";
  return 0;
}
```