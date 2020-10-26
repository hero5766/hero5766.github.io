---
title: sophus
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-10-25 22:46:53
---
> 

<!-- more -->

# 简介
## 安装
- [步骤和一些问题](https://blog.csdn.net/weixin_44436677/article/details/106225704)
```bash
git clone https://github.com/strasdat/Sophus.git
cd Sophus
git checkout a621ff
```


# [使用](https://blog.csdn.net/robinhjwy/article/details/77334189)
## 添加依赖
```cmake
find_package( Sophus REQUIRED )
include_directories( ${Sophus_INCLUDE_DIRS} )
add_executable( useSophus useSophus.cpp )
target_link_libraries( useSophus ${Sophus_LIBRARIES} )
```

## 构造

构造|说明
-|-
从旋转矩阵构造|`Eigen::Matrix3d R = Eigen::AngleAxisd(M_PI/2, Eigen::Vector3d(0,0,1)).toRotationMatrix();`<br>`Sophus::SO3 SO3_R(R);`
欧拉角构造|`Sophus::SO3 SO3_v( 0, 0, M_PI/2 );`
四元数构造|`Eigen::Quaterniond q(R);`<br>`Sophus::SO3 SO3_q( q );`
李代数构造|`Eigen::Vector3d so33 (1, 1, 1);`<br>`Sophus::SO3 SO3 =Sophus::SO3::exp(so33);`


## 函数

函数|说明
-|-
.log|将李群转化成李代数
hat|将向量转成反对称阵
vee|将反对称阵转成向量


# 示例

```cpp
#include <iostream>
#include <cmath>
using namespace std; 
#include <Eigen/Core>
#include <Eigen/Geometry>
#include "sophus/so3.h"
#include "sophus/se3.h"
int main( int argc, char** argv )
{
    // 沿Z轴转90度的旋转矩阵
    Eigen::Matrix3d R = Eigen::AngleAxisd(M_PI/2, Eigen::Vector3d(0,0,1)).toRotationMatrix();
    Sophus::SO3 SO3_R(R);               // Sophus::SO(3)可以直接从旋转矩阵构造
    Sophus::SO3 SO3_v( 0, 0, M_PI/2 );  // 亦可从旋转向量构造，三个过程，先转Ｘ轴，再转Ｙ轴，再转Ｚ轴，完全跟旋转向量不搭边。瞅着过程有点像欧拉角的过程，三个轴分了三步
    Eigen::Quaterniond q(R);            // 或者四元数
    Sophus::SO3 SO3_q( q );
    // 上述表达方式都是等价的
    // 输出SO(3)时，以so(3)形式输出
    cout<<"SO(3) from matrix: "<<SO3_R<<endl;
    cout<<"SO(3) from vector: "<<SO3_v<<endl;
    cout<<"SO(3) from quaternion :"<<SO3_q<<endl;
    // 使用对数映射获得它的李代数
    Eigen::Vector3d so3 = SO3_R.log();
    cout<<"so3 = "<<so3.transpose()<<endl;
    // hat 为向量到反对称矩阵
    cout<<"so3 hat=\n"<<Sophus::SO3::hat(so3)<<endl;
    // 相对的，vee为反对称到向量
    cout<<"so3 hat vee= "<<Sophus::SO3::vee( Sophus::SO3::hat(so3) ).transpose()<<endl; // transpose纯粹是为了输出美观一些
    // 增量扰动模型的更新
    Eigen::Vector3d update_so3(1e-4, 0, 0); //假设更新量为这么多
    Sophus::SO3 SO3_updated = Sophus::SO3::exp(update_so3)*SO3_R;
    cout<<"SO3 updated = "<<SO3_updated<<endl;
    cout<<"************我是分割线*************"<<endl;
    // 对SE(3)操作大同小异
    Eigen::Vector3d t(1,0,0);           // 沿X轴平移1
    Sophus::SE3 SE3_Rt(R, t);           // 从R,t构造SE(3)
    Sophus::SE3 SE3_qt(q,t);            // 从q,t构造SE(3)
    cout<<"SE3 from R,t= "<<endl<<SE3_Rt<<endl;
    cout<<"SE3 from q,t= "<<endl<<SE3_qt<<endl;
    // 李代数se(3) 是一个六维向量，方便起见先typedef一下
    typedef Eigen::Matrix<double,6,1> Vector6d;
    Vector6d se3 = SE3_Rt.log();
    cout<<"se3 = "<<se3.transpose()<<endl;
    // 观察输出，会发现在Sophus中，se(3)的平移在前，旋转在后.
    // 同样的，有hat和vee两个算符
    cout<<"se3 hat = "<<endl<<Sophus::SE3::hat(se3)<<endl;
    cout<<"se3 hat vee = "<<Sophus::SE3::vee( Sophus::SE3::hat(se3) ).transpose()<<endl;
    // 最后，演示一下更新
    Vector6d update_se3; //更新量
    update_se3.setZero();
    update_se3(0,0) = 1e-4d;
    Sophus::SE3 SE3_updated = Sophus::SE3::exp(update_se3)*SE3_Rt;
    cout<<"SE3 updated = "<<endl<<SE3_updated.matrix()<<endl;
    return 0;
}
```

## SO3
```cpp
// 使用对数映射获得它的李代数
Eigen::Vector3d so3 = SO3_R.log();
cout<<"so3 = "<<so3.transpose()<<endl;
// hat 为向量到反对称矩阵,相当于　^　运算。
cout<<"so3 hat=\n"<<Sophus::SO3::hat(so3)<<endl;
// 相对的，vee为反对称矩阵到向量，相当于下尖尖运算
cout<<"so3 hat vee= "<<Sophus::SO3::vee( Sophus::SO3::hat(so3) ).transpose()<<endl; // transpose纯粹是为了输出美观一些
// 增量扰动模型的更新
Eigen::Vector3d update_so3(1e-4, 0, 0); //创建一个增量扰动，形式为旋转向量形式。数值上可以看出扰动非常小。
Sophus::SO3 SO3_updated = Sophus::SO3::exp(update_so3)*SO3_R;//将扰动用对数映射到旋转矩阵形式，并对原矩阵左乘进行扰动，然后看输出结果，发现也变化很小
cout<<"SO3 updated = "<<SO3_updated<<endl;
```

## SE3
```cpp
// 对SE(3)操作大同小异
Eigen::Vector3d t(1,0,0);           // 沿X轴平移1
Sophus::SE3 SE3_Rt(R, t);           // 从R,t构造SE(3)
Sophus::SE3 SE3_qt(q,t);            // 从q,t构造SE(3)
cout<<"SE3 from R,t= "<<endl<<SE3_Rt<<endl;
cout<<"SE3 from q,t= "<<endl<<SE3_qt<<endl;
// 李代数se(3) 是一个六维向量，方便起见先typedef一下
typedef Eigen::Matrix<double,6,1> Vector6d;
Vector6d se3 = SE3_Rt.log();
cout<<"se3 = "<<se3.transpose()<<endl;
// 观察输出，会发现在Sophus中，se(3)的平移在前，旋转在后.
// 同样的，有hat和vee两个算符
cout<<"se3 hat = "<<endl<<Sophus::SE3::hat(se3)<<endl;
cout<<"se3 hat vee = "<<Sophus::SE3::vee( Sophus::SE3::hat(se3) ).transpose()<<endl;
// 最后，演示一下更新
Vector6d update_se3; //更新量
update_se3.setZero();
update_se3(0,0) = 1e-4d;
Sophus::SE3 SE3_updated = Sophus::SE3::exp(update_se3)*SE3_Rt;
cout<<"SE3 updated = "<<endl<<SE3_updated.matrix()<<endl;
```

