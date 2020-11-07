---
title: g2o
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-10-25 21:46:53
---
> g2o图优化

<!-- more -->

# 简介
- ceres可以求解最优化问题，而g2o在此基础上，对so(3)，se(3)也有支持，是slam中常用的库。

## 介绍
- [教程链接](https://www.cnblogs.com/gaoxiang12/p/5304272.html)
- 2o全称是什么？来跟我大声说一遍：General Graph Optimization！你可以叫它g土o，g二o，g方o，总之我也不知道该怎么叫它……
- 所谓的通用图优化。
- 为何叫通用呢？g2o的核里带有各种各样的求解器，而它的顶点、边的类型则多种多样。通过自定义顶点和边，事实上，只要一个优化问题能够表达成图，那么就可以用g2o去求解它。常见的，比如bundle adjustment，ICP，数据拟合，都可以用g2o来做。甚至我还在想神经网络能不能写成图优化的形式呢……
- 从代码层面来说，g2o是一个c++编写的项目，用cmake构建。它的github地址在：https://github.com/RainerKuemmerle/g2o 
- 它是一个重度模板类的c++项目，其中矩阵数据结构多来自Eigen


## 安装
```bash
git clone https://github.com/RainerKuemmerle/g2o
```

## 目录

如你所见，g2o项目中含有若干文件夹。刨开那些gitignore之类的零碎文件，主要有以下几个：

目录|说明
-|-
EXTERNAL  |三方库，有ceres, csparse, freeglut，可以选择性地编译；
cmake_modules  |给cmake用来寻找库的文件。我们用g2o时也会用它里头的东西，例如FindG2O.cmake
doc     |文档。包括g2o自带的说明书（难度挺大的一个说明文档）。
g2o      |最重要的源代码都在这里！
script    |在android等其他系统编译用的脚本，由于我们在ubuntu下就没必要多讲了。

最重要的就是g2o的源代码文件啦，我们同样地介绍一下各文件夹的内容：

目录|说明
-|-
apps    |一些应用程序。好用的g2o_viewer就在这里。其他还有一些不常用的命令行工具等。
core    |核心组件，很重要！基本的顶点、边、图结构的定义，算法的定义，求解器接口的定义在这里。
examples  |一些例程，可以参照着这里的东西来写。不过注释不太多。
solvers    |求解器的实现。主要来自choldmod, csparse。在使用g2o时要先选择其中一种。
stuff    |对用户来讲可有可无的一些工具函数。
types    |各种顶点和边，很重要！我们用户在构建图优化问题时，先要想好自己的顶点和边是否已经提供了定义。如果没有，要自己实现。如果有，就用g2o提供的即可。


- 就经验而言，solvers给人的感觉是大同小异，而 types 的选取，则是 g2o 用户主要关心的内容。然后 core 下面的内容，我们要争取弄的比较熟悉，才能确保使用中出现错误可以正确地应对。
- 那么，g2o最基本的类结构是怎么样的呢？我们如何来表达一个Graph，选择求解器呢？我们祭出一张图：
![](/images/pasted-319.png)

- 先看上半部分。SparseOptimizer 是我们最终要维护的东东。它是一个Optimizable Graph，从而也是一个Hyper Graph。一个 SparseOptimizer 含有很多个顶点 （都继承自 Base Vertex）和很多个边（继承自 BaseUnaryEdge, BaseBinaryEdge或BaseMultiEdge）。这些 Base Vertex 和 Base Edge 都是抽象的基类，而实际用的顶点和边，都是它们的派生类。我们用 SparseOptimizer.addVertex 和 SparseOptimizer.addEdge 向一个图中添加顶点和边，最后调用 SparseOptimizer.optimize 完成优化。
- 在优化之前，需要指定我们用的求解器和迭代算法。从图中下半部分可以看到，一个 SparseOptimizer 拥有一个 Optimization Algorithm，继承自Gauss-Newton, Levernberg-Marquardt, Powell's dogleg 三者之一（我们常用的是GN或LM）。同时，这个 Optimization Algorithm 拥有一个Solver，它含有两个部分。一个是 SparseBlockMatrix ，用于计算稀疏的雅可比和海塞； 一个是用于计算迭代过程中最关键的一步
$$HΔx=-b$$

- 这就需要一个线性方程的求解器。而这个求解器，可以从 PCG, CSparse, Choldmod 三者选一。

- 综上所述，在g2o中选择优化方法一共需要三个步骤：

  - 选择一个线性方程求解器，从 PCG, CSparse, Choldmod中选，实际则来自 g2o/solvers 文件夹中定义的东东。
  - 选择一个 BlockSolver 。
  - 选择一个迭代策略，从GN, LM, Doglog中选。
- 这样一来，读者是否对g2o就更清楚的认识了呢？

# 示例
## 双视图bundle adjustment
- [代码的git地址](https://github.com/gaoxiang12/g2o_ba_example)

```cpp
/**
 * BA Example
 * Author: Xiang Gao
 * Date: 2016.3
 * Email: gaoxiang12@mails.tsinghua.edu.cn
 *
 * 在这个程序中，我们读取两张图像，进行特征匹配。然后根据匹配得到的特征，计算相机运动以及特征点的位置。这是一个典型的Bundle Adjustment，我们用g2o进行优化。
 */
// for std
#include <iostream>
// for opencv
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/features2d/features2d.hpp>
#include <boost/concept_check.hpp>
// for g2o
#include <g2o/core/sparse_optimizer.h>
#include <g2o/core/block_solver.h>
#include <g2o/core/robust_kernel.h>
#include <g2o/core/robust_kernel_impl.h>
#include <g2o/core/optimization_algorithm_levenberg.h>
#include <g2o/solvers/cholmod/linear_solver_cholmod.h>
#include <g2o/types/slam3d/se3quat.h>
#include <g2o/types/sba/types_six_dof_expmap.h>
using namespace std;
// 寻找两个图像中的对应点，像素坐标系
// 输入：img1, img2 两张图像
// 输出：points1, points2, 两组对应的2D点
int     findCorrespondingPoints( const cv::Mat& img1, const cv::Mat& img2, vector<cv::Point2f>& points1, vector<cv::Point2f>& points2 );
// 相机内参
double cx = 325.5;
double cy = 253.5;
double fx = 518.0;
double fy = 519.0;
int main( int argc, char** argv )
{
    // 调用格式：命令 [第一个图] [第二个图]
    if (argc != 3)
    {
        cout<<"Usage: ba_example img1, img2"<<endl;
        exit(1);
    }
    // 读取图像
    cv::Mat img1 = cv::imread( argv[1] );
    cv::Mat img2 = cv::imread( argv[2] );

    // 找到对应点
    vector<cv::Point2f> pts1, pts2;
    if ( findCorrespondingPoints( img1, img2, pts1, pts2 ) == false )
    {
        cout<<"匹配点不够！"<<endl;
        return 0;
    }
    cout<<"找到了"<<pts1.size()<<"组对应特征点。"<<endl;
    // 构造g2o中的图
    // 先构造求解器
    g2o::SparseOptimizer    optimizer;
    // 使用Cholmod中的线性方程求解器
    g2o::BlockSolver_6_3::LinearSolverType* linearSolver = new  g2o::LinearSolverCholmod<g2o::BlockSolver_6_3::PoseMatrixType> ();
    // 6*3 的参数
    g2o::BlockSolver_6_3* block_solver = new g2o::BlockSolver_6_3( linearSolver );
    // L-M 下降
    g2o::OptimizationAlgorithmLevenberg* algorithm = new g2o::OptimizationAlgorithmLevenberg( block_solver );

    optimizer.setAlgorithm( algorithm );
    optimizer.setVerbose( false );
    // 添加节点
    // 两个位姿节点
    for ( int i=0; i<2; i++ )
    {
        g2o::VertexSE3Expmap* v = new g2o::VertexSE3Expmap();
        v->setId(i);
        if ( i == 0)
            v->setFixed( true ); // 第一个点固定为零
        // 预设值为单位Pose，因为我们不知道任何信息
        v->setEstimate( g2o::SE3Quat() );
        optimizer.addVertex( v );
    }
    // 很多个特征点的节点
    // 以第一帧为准
    for ( size_t i=0; i<pts1.size(); i++ )
    {
        g2o::VertexSBAPointXYZ* v = new g2o::VertexSBAPointXYZ();
        v->setId( 2 + i );
        // 由于深度不知道，只能把深度设置为1了
        double z = 1;
        double x = ( pts1[i].x - cx ) * z / fx;
        double y = ( pts1[i].y - cy ) * z / fy;
        v->setMarginalized(true);
        v->setEstimate( Eigen::Vector3d(x,y,z) );
        optimizer.addVertex( v );
    }
    // 准备相机参数
    g2o::CameraParameters* camera = new g2o::CameraParameters( fx, Eigen::Vector2d(cx, cy), 0 );
    camera->setId(0);
    optimizer.addParameter( camera );

    // 准备边
    // 第一帧
    vector<g2o::EdgeProjectXYZ2UV*> edges;
    for ( size_t i=0; i<pts1.size(); i++ )
    {
        g2o::EdgeProjectXYZ2UV*  edge = new g2o::EdgeProjectXYZ2UV();
        edge->setVertex( 0, dynamic_cast<g2o::VertexSBAPointXYZ*>   (optimizer.vertex(i+2)) );
        edge->setVertex( 1, dynamic_cast<g2o::VertexSE3Expmap*>     (optimizer.vertex(0)) );
        edge->setMeasurement( Eigen::Vector2d(pts1[i].x, pts1[i].y ) );
        edge->setInformation( Eigen::Matrix2d::Identity() );
        edge->setParameterId(0, 0);
        // 核函数
        edge->setRobustKernel( new g2o::RobustKernelHuber() );
        optimizer.addEdge( edge );
        edges.push_back(edge);
    }
    // 第二帧
    for ( size_t i=0; i<pts2.size(); i++ )
    {
        g2o::EdgeProjectXYZ2UV*  edge = new g2o::EdgeProjectXYZ2UV();
        edge->setVertex( 0, dynamic_cast<g2o::VertexSBAPointXYZ*>   (optimizer.vertex(i+2)) );
        edge->setVertex( 1, dynamic_cast<g2o::VertexSE3Expmap*>     (optimizer.vertex(1)) );
        edge->setMeasurement( Eigen::Vector2d(pts2[i].x, pts2[i].y ) );
        edge->setInformation( Eigen::Matrix2d::Identity() );
        edge->setParameterId(0,0);
        // 核函数
        edge->setRobustKernel( new g2o::RobustKernelHuber() );
        optimizer.addEdge( edge );
        edges.push_back(edge);
    }
    cout<<"开始优化"<<endl;
    optimizer.setVerbose(true);
    optimizer.initializeOptimization();
    optimizer.optimize(10);
    cout<<"优化完毕"<<endl;
    //我们比较关心两帧之间的变换矩阵
    g2o::VertexSE3Expmap* v = dynamic_cast<g2o::VertexSE3Expmap*>( optimizer.vertex(1) );
    Eigen::Isometry3d pose = v->estimate();
    cout<<"Pose="<<endl<<pose.matrix()<<endl;
    // 以及所有特征点的位置
    for ( size_t i=0; i<pts1.size(); i++ )
    {
        g2o::VertexSBAPointXYZ* v = dynamic_cast<g2o::VertexSBAPointXYZ*> (optimizer.vertex(i+2));
        cout<<"vertex id "<<i+2<<", pos = ";
        Eigen::Vector3d pos = v->estimate();
        cout<<pos(0)<<","<<pos(1)<<","<<pos(2)<<endl;
    }
    // 估计inlier的个数
    int inliers = 0;
    for ( auto e:edges )
    {
        e->computeError();
        // chi2 就是 error*\Omega*error, 如果这个数很大，说明此边的值与其他边很不相符
        if ( e->chi2() > 1 )
        {
            cout<<"error = "<<e->chi2()<<endl;
        }
        else
        {
            inliers++;
        }
    }
    cout<<"inliers in total points: "<<inliers<<"/"<<pts1.size()+pts2.size()<<endl;
    optimizer.save("ba.g2o");
    return 0;
}
int     findCorrespondingPoints( const cv::Mat& img1, const cv::Mat& img2, vector<cv::Point2f>& points1, vector<cv::Point2f>& points2 )
{
    cv::ORB orb;
    vector<cv::KeyPoint> kp1, kp2;
    cv::Mat desp1, desp2;
    orb( img1, cv::Mat(), kp1, desp1 );
    orb( img2, cv::Mat(), kp2, desp2 );
    cout<<"分别找到了"<<kp1.size()<<"和"<<kp2.size()<<"个特征点"<<endl;
    cv::Ptr<cv::DescriptorMatcher>  matcher = cv::DescriptorMatcher::create( "BruteForce-Hamming");
    double knn_match_ratio=0.8;
    vector< vector<cv::DMatch> > matches_knn;
    matcher->knnMatch( desp1, desp2, matches_knn, 2 );
    vector< cv::DMatch > matches;
    for ( size_t i=0; i<matches_knn.size(); i++ )
    {
        if (matches_knn[i][0].distance < knn_match_ratio * matches_knn[i][1].distance )
            matches.push_back( matches_knn[i][0] );
    }
    if (matches.size() <= 20) //匹配点太少
        return false;
    for ( auto m:matches )
    {
        points1.push_back( kp1[m.queryIdx].pt );
        points2.push_back( kp2[m.trainIdx].pt );
    }
    return true;
}
```






