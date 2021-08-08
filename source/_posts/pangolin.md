---
title: pangolin
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-10-25 21:46:53
---
> pangolin用于图形可视化

<!-- more -->

# 简介
## 介绍
- Pangolin是对OpenGL进行封装的轻量级的OpenGL输入/输出和视频显示的库。可以用于3D视觉和3D导航的视觉图，可以输入各种类型的视频、并且可以保留视频和输入数据用于debug。

## 特性
- 在[官方地址](http://www.stevenlovegrove.com/?id=44)里面，对Pangolin有一个大致的介绍：
  - 管理OpenGL的视角：画出想要的3D图。可以根据自己的需要调整视角。
  - 高级的3D handler： Pangolin可以让你从很多视角看3D图，甚至可以搞一个model matrix。对3D点进行旋转平移。
  - 输入控制：很多类型都可以（linear / log scale, runtime adjustable resolution, custom data types, …）
  - 视频输入：调用了libDC1394和V4L进行视频流的输入，Pangolin提供了简单的C++接口给这些资源。可以使用图片、usb的摄像头输入、视频流、网络流等等都可以。
  - 动态画图工具，可以处理科学数据。可以支持多种视图（time-series, parametric, cumulative, …）
  - 记录视频和输入数据，可以用于debug。


## 安装
```bash
git clone https://github.com/stevenlovegrove/Pangolin
cd Pangolin
mkdir build
cd build
cmake -DCPP11_NO_BOOST=1 ..
make -j
sudo make install
```

- [安装问题](http://www.cnblogs.com/liufuqiang/p/5618335.html)

# 使用

- 一个是看[Example](https://github.com/stevenlovegrove/Pangolin/tree/master/examples)，另外一个可以看[ros网站的介绍](http://docs.ros.org/fuerte/api/pangolin_wrapper/html/namespacepangolin.html)。

- 这里总结了[博客yuntian_li](https://blog.csdn.net/weixin_43991178/article/details/105119610)中对Pangolin的使用方法


<!-- ## 类型

类型|说明
-|- -->

## 资料
[Pangolin官方地址](https://github.com/stevenlovegrove/Pangolin)
[泡泡机器人](https://mp.weixin.qq.com/s?__biz=MzI5MTM1MTQwMw==&mid=2247485156&idx=1&sn=7109ee61d663d21832b94534b6863db4&chksm=ec10b8e0db6731f62490664d3ef02491e93371d99bda926b116dc216b1411789ed78aba2a166&mpshare=1&scene=1&srcid=0505TTqkFeTOzqAV63lggaE9&pass_ticket=b%2FXHS6vjWLb%2BKGKqEBCjh7chAnhbsnjE7MJpLEZNBJmpKfemcHXmJL%2BIZ5pRwYeD#rd)
[github demo](https://github.com/stevenlovegrove/Pangolin/blob/master/examples/)
[opengl相机位置方向](https://learnopengl-cn.readthedocs.io/zh/latest/01%20Getting%20started/09%20Camera/)
[lookat函数](https://www.khronos.org/registry/OpenGL-Refpages/gl2.1/xhtml/gluLookAt.xml)
[Pangolin安装问题](http://www.cnblogs.com/liufuqiang/p/5618335.html)
[Pangolin的Example](https://github.com/stevenlovegrove/Pangolin/tree/master/examples)
[Pangolin的使用](http://docs.ros.org/fuerte/api/pangolin_wrapper/html/namespacepangolin.html)
[特性](http://www.stevenlovegrove.com/?id=44)



## 方法

方法|代码|说明
-|-|-
创建视窗对象|`pangolin::CreateWindowAndBind("Main",640,480);`|创建名称为“Main”的GUI窗口，尺寸为640×640
启动深度测试|`glEnable(GL_DEPTH_TEST);`|该功能会使得pangolin只会绘制朝向镜头的那一面像素点，避免容易混淆的透视关系出现，因此在任何3D可视化中都应该开启该功能
创建观察相机视图|`pangolin::OpenGlRenderState s_cam(pangolin::ProjectionMatrix(640,480,420,420,320,320,0.2,100),pangolin::ModelViewLookAt(2,0,2, 0,0,0, pangolin::AxisY));` |在视图中放置摄像机，在视窗中“放置”一个摄像机，给出摄像机的内参矩阵ProjectionMatrix从而在我们对摄像机进行交互操作时，Pangolin会自动根据内参矩阵完成对应的透视变换。此外，我们还需要给出摄像机初始时刻所处的位置，摄像机的视点位置（即摄像机的光轴朝向哪一个点）以及摄像机的本身哪一轴朝上。ProjectionMatrix的参数依次为<br>观察相机的图像高度<br>宽度<br>4个内参<br>最近和最远视距<br>ModelViewLookAt的参数依次为<br>相机所在的位置<br>相机所看的视点位置(一般会设置在原点)
创建交互视图|`pangolin::Handler3D handler(s_cam);`<br>`pangolin::View& d_cam = pangolin::CreateDisplay().SetBounds(0.0, 1.0, 0.0, 1.0, -640.0f/480.0f).SetHandler(&handler);`|创建一个交互式视图（view）用于显示上一步摄像机所“拍摄”到的内容，这一步类似于OpenGL中的viewport处理。setBounds()函数前四个参数依次表示视图在视窗中的范围（下、上、左、右），可以采用相对坐标（0~1）以及绝对坐标（使用Attach对象）。
清空颜色和深度缓存|`glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);`|分别清空色彩缓存和深度缓存并激活之前设定好的视窗对象（否则视窗内会保留上一帧的图形，这种“多重曝光”效果通常并不是我们需要的）
激活交互视图|`d_cam.Activate(s_cam);`|
绘制一个立方体|`pangolin::glDrawColouredCube();`|在原点绘制一个立方体
gl绘制|`glLineWidth(3);`<br>`glBegin ( GL_LINES );`<br>`glColor3f ( 0.8f,0.f,0.f );`<br>`glVertex3f( -1,-1,-1 );`<br>`glVertex3f( 0,-1,-1 );`<br>`glEnd();`|
帧循环|`pangolin::FinishFrame();`|运行帧循环以推进窗口事件
创建右侧用于显示视图视口|`pangolin::View& d_cam = pangolin::CreateDisplay().SetBounds(0.0, 1.0, pangolin::Attach::Pix(UI_WIDTH), 1.0, -640.0f/480.0f).SetHandler(new pangolin::Handler3D(s_cam));`|
创建左侧用于创建控制面板|`pangolin::CreatePanel("ui").SetBounds(0.0, 1.0, 0.0, pangolin::Attach::Pix(UI_WIDTH));`|创建一个面板，并给这个面板明明为“ui”，这里"ui"是面板的tag名称，后续所有控制的操作都通过这个tag绑定到对应的面板上
创建控制面板的控件对象|`pangolin::Var<bool> A_Button("ui.a_button", false, false);  // 按钮`<br>`pangolin::Var<double> Double_Slider("ui.a_slider", 3, 0, 5); //double滑条`<br>`pangolin::Var<int> Int_Slider("ui.b_slider", 2, 0, 5); //int滑条`<br>`pangolin::Var<std::string> A_string("ui.a_string", "Hello Pangolin");`<br>`pangolin::Var<std::function<void()>> reset("ui.Reset", SampleMethod);// `|创建参数依次为控件的tag、初始值以及是否可以toggle。<br>这一类Var对象为常见的滑条对象，创建参数依次为控件tag、初始值、最小值、最大值和logscale。<br>这一类控件同样实现按钮控件的功能，只是其在创建时传入一个std::function函数对象，因此不需要在后续循环中进行回调函数的书写。
判断状态|`pangolin::Pushed(A_Button)`



# 示例
## 简单使用
```cpp
#include <pangolin/pangolin.h>
int main( int /*argc*/, char** /*argv*/ )
{
    // 创建名称为“Main”的GUI窗口，尺寸为640×640
    pangolin::CreateWindowAndBind("Main",640,480);
    // 启动深度测试
    glEnable(GL_DEPTH_TEST);
    // 创建一个观察相机视图
    pangolin::OpenGlRenderState s_cam(
        pangolin::ProjectionMatrix(640,480,420,420,320,320,0.2,100),
        pangolin::ModelViewLookAt(2,0,2, 0,0,0, pangolin::AxisY)
    );
    // 创建交互视图
    pangolin::Handler3D handler(s_cam); //交互相机视图句柄
    pangolin::View& d_cam = pangolin::CreateDisplay()
            .SetBounds(0.0, 1.0, 0.0, 1.0, -640.0f/480.0f)
            .SetHandler(&handler);
    while( !pangolin::ShouldQuit() )
    {
        // 清空颜色和深度缓存
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
        d_cam.Activate(s_cam);
        // 在原点绘制一个立方体
        pangolin::glDrawColouredCube();
        // 绘制坐标系
       	glLineWidth(3);
        glBegin ( GL_LINES );
	    glColor3f ( 0.8f,0.f,0.f );
	    glVertex3f( -1,-1,-1 );
	    glVertex3f( 0,-1,-1 );
	    glColor3f( 0.f,0.8f,0.f);
	    glVertex3f( -1,-1,-1 );
	    glVertex3f( -1,0,-1 );
	    glColor3f( 0.2f,0.2f,1.f);
	    glVertex3f( -1,-1,-1 );
	    glVertex3f( -1,-1,0 );
	    glEnd();
        // 运行帧循环以推进窗口事件
        pangolin::FinishFrame();
    }
    return 0;
}
```

## 多线程


```cpp
#include <pangolin/pangolin.h>
#include <thread>
static const std::string window_name = "HelloPangolinThreads";
void setup() {
    // create a window and bind its context to the main thread
    pangolin::CreateWindowAndBind(window_name, 640, 480);
    // enable depth
    glEnable(GL_DEPTH_TEST);
    // unset the current context from the main thread
    pangolin::GetBoundWindow()->RemoveCurrent();
}
// 在多线程版本的HelloPangolin中，我们首先利用setup()函数创建了一个视窗用于后续的显示，但这个视窗实在主线程中创建的，因此在主线程调用玩后，需要使用GetBoundWindow()->RemoveCurrent()将其解绑。
void run() {
    // fetch the context and bind it to this thread
    pangolin::BindToContext(window_name);
    // we manually need to restore the properties of the context
    glEnable(GL_DEPTH_TEST);
    // Define Projection and initial ModelView matrix
    pangolin::OpenGlRenderState s_cam(
        pangolin::ProjectionMatrix(640,480,420,420,320,240,0.2,100),
        pangolin::ModelViewLookAt(-2,2,-2, 0,0,0, pangolin::AxisY)
    );
    // Create Interactive View in window
    pangolin::Handler3D handler(s_cam);
    pangolin::View& d_cam = pangolin::CreateDisplay()
            .SetBounds(0.0, 1.0, 0.0, 1.0, -640.0f/480.0f)
            .SetHandler(&handler);
    while( !pangolin::ShouldQuit() )
    {
        // Clear screen and activate view to render into
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
        d_cam.Activate(s_cam);
        // Render OpenGL Cube
        pangolin::glDrawColouredCube();
        // Swap frames and Process Events
        pangolin::FinishFrame();
    }
    // unset the current context from the main thread
    pangolin::GetBoundWindow()->RemoveCurrent();
}
int main( int /*argc*/, char** /*argv*/ )
{
    // create window and context in the main thread
    setup();
    // use the context in a separate rendering thread
    std::thread render_loop;
    render_loop = std::thread(run);
    render_loop.join();
    return 0;
}
//随后，我们新开一个线程，运行我们的run()函数，在run()函数中，我们首先使用BindToContext()函数将之前解绑的视窗绑定到当前线程，随后需要重新设置视窗的属性（即启动深度测试）；同样，在线程结束时，我们需要解绑视窗。其余部分的代码则与task1完全一致。
```

## 为Pangolin添加控件
```cpp
#include <pangolin/pangolin.h>
#include <string>
#include <iostream>
void SampleMethod()
{
    std::cout << "You typed ctrl-r or pushed reset" << std::endl;
    std::cout << "Window width: " << i << std::endl;
}
int main(/*int argc, char* argv[]*/)
{  
    // 创建视窗
    pangolin::CreateWindowAndBind("Main",640,480);
    // 启动深度测试
    glEnable(GL_DEPTH_TEST);
    // 创建一个摄像机
    pangolin::OpenGlRenderState s_cam(
        pangolin::ProjectionMatrix(640,480,420,420,320,240,0.1,1000),
        pangolin::ModelViewLookAt(-0,0.5,-3, 0,0,0, pangolin::AxisY)
    );
    // 分割视窗
    const int UI_WIDTH = 180;
    // 右侧用于显示视口
    pangolin::View& d_cam = pangolin::CreateDisplay()
        .SetBounds(0.0, 1.0, pangolin::Attach::Pix(UI_WIDTH), 1.0, -640.0f/480.0f)
        .SetHandler(new pangolin::Handler3D(s_cam));
    // 左侧用于创建控制面板
    pangolin::CreatePanel("ui")
        .SetBounds(0.0, 1.0, 0.0, pangolin::Attach::Pix(UI_WIDTH));
    // 创建控制面板的控件对象，pangolin中
    pangolin::Var<bool> A_Button("ui.a_button", false, false);  // 按钮
    pangolin::Var<bool> A_Checkbox("ui.a_checkbox", false, true); // 选框
    pangolin::Var<double> Double_Slider("ui.a_slider", 3, 0, 5); //double滑条
    pangolin::Var<int> Int_Slider("ui.b_slider", 2, 0, 5); //int滑条
    pangolin::Var<std::string> A_string("ui.a_string", "Hello Pangolin");
    pangolin::Var<bool> SAVE_IMG("ui.save_img", false, false);  // 按钮
    pangolin::Var<bool> SAVE_WIN("ui.save_win", false, false); // 按钮
    pangolin::Var<bool> RECORD_WIN("ui.record_win", false, false); // 按钮
    pangolin::Var<std::function<void()>> reset("ui.Reset", SampleMethod);// 
    // 绑定键盘快捷键
    // Demonstration of how we can register a keyboard hook to alter a Var
    pangolin::RegisterKeyPressCallback(pangolin::PANGO_CTRL + 'b', pangolin::SetVarFunctor<double>("ui.a_slider", 3.5));
    // Demonstration of how we can register a keyboard hook to trigger a method
    pangolin::RegisterKeyPressCallback(pangolin::PANGO_CTRL + 'r', SampleMethod);
    // Default hooks for exiting (Esc) and fullscreen (tab).
    while( !pangolin::ShouldQuit() )
    {
        // Clear entire screen
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);    
        // 各控件的回调函数
        if(pangolin::Pushed(A_Button))
            std::cout << "Push button A." << std::endl;
        if(A_Checkbox)
            Int_Slider = Double_Slider;
        // 保存整个win
        if( pangolin::Pushed(SAVE_WIN) )
        pangolin::SaveWindowOnRender("window");
		// 保存view
        if( pangolin::Pushed(SAVE_IMG) )
        d_cam.SaveOnRender("cube");
		// 录像
        if( pangolin::Pushed(RECORD_WIN) )
        pangolin::DisplayBase().RecordOnRender("ffmpeg:[fps=50,bps=8388608,unique_filename]//screencap.avi");
        d_cam.Activate(s_cam);
        // glColor3f(1.0,0.0,1.0);
        pangolin::glDrawColouredCube();
        // Swap frames and Process Events
        pangolin::FinishFrame();
    }
    return 0;
}
```

## 多视图与图片显示
- 在SLAM的可视化中，有时我们期望实时显示相机的图像和特征点跟踪情况（例如VINS-Mono），pangolin同样提供了用于显示图像的一系列功能，下面的代码分别在视窗左上角和右下角实时显示了EuRoc数据集中的图像：

```cpp
#include <opencv2/opencv.hpp>
#include <pangolin/pangolin.h>
int main(int argc, char** argv){
    // 创建视窗
    pangolin::CreateWindowAndBind("MultiImage", 752, 480);
    // 启动深度测试
    glEnable(GL_DEPTH_TEST);
    // 设置摄像机
    pangolin::OpenGlRenderState s_cam(
        pangolin::ProjectionMatrix(752, 480, 420, 420, 320, 320, 0.1, 1000),
        pangolin::ModelViewLookAt(-2, 0, -2, 0, 0, 0, pangolin::AxisY)
    );
    // ---------- 创建三个视图 ---------- //
    pangolin::View& d_cam = pangolin::Display("cam").SetBounds(0., 1., 0., 1., -752/480.).SetHandler(new pangolin::Handler3D(s_cam));
    pangolin::View& cv_img_1 = pangolin::Display("image_1").SetBounds(2/3.0f, 1.0f, 0., 1/3.0f, 752/480.).SetLock(pangolin::LockLeft, pangolin::LockTop);
    pangolin::View& cv_img_2 = pangolin::Display("image_2").SetBounds(0., 1/3.0f, 2/3.0f, 1.0, 752/480.).SetLock(pangolin::LockRight, pangolin::LockBottom);
    // 创建glTexture容器用于读取图像
    pangolin::GlTexture imgTexture1(752, 480, GL_RGB, false, 0, GL_BGR, GL_UNSIGNED_BYTE);
    pangolin::GlTexture imgTexture2(752, 480, GL_RGB, false, 0, GL_BGR, GL_UNSIGNED_BYTE);
    while(!pangolin::ShouldQuit()){
        // 清空颜色和深度缓存
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
        // 启动相机
        d_cam.Activate(s_cam);
        // 添加一个立方体
        glColor3f(1.0f, 1.0f, 1.0f);
        pangolin::glDrawColouredCube();
        // 从文件读取图像
        cv::Mat img1 = cv::imread("../task4/img/img1.png");
        cv::Mat img2 = cv::imread("../task4/img/img2.png");
        // 向GPU装载图像
        imgTexture1.Upload(img1.data, GL_BGR, GL_UNSIGNED_BYTE);
        imgTexture2.Upload(img2.data, GL_BGR, GL_UNSIGNED_BYTE);
        // 显示图像
        cv_img_1.Activate();
        glColor3f(1.0f, 1.0f, 1.0f); // 设置默认背景色，对于显示图片来说，不设置也没关系
        imgTexture1.RenderToViewportFlipY(); // 需要反转Y轴，否则输出是倒着的
        cv_img_2.Activate();
        glColor3f(1.0f, 1.0f, 1.0f); // 设置默认背景色，对于显示图片来说，不设置也没关系
        imgTexture2.RenderToViewportFlipY();
        pangolin::FinishFrame();
    }
    return 0;
}
```

## pangolin绘制数据曲线
- 使用pangolin绘制函数曲线，在本例中，我们将在一个视图中分别绘制sin ⁡ ( x ) \sin(x)sin(x)、cos ⁡ ( x ) \cos(x)cos(x)以及sin ⁡ ( x ) + cos ⁡ ( x ) \sin(x)+\cos(x)sin(x)+cos(x)的曲线，代码如下

```cpp
#include <iostream>
#include <pangolin/pangolin.h>
int main(/*int argc, char* argv[]*/)
{
  // Create OpenGL window in single line
  pangolin::CreateWindowAndBind("Main",640,480);
  // Data logger object
  pangolin::DataLog log;
  // Optionally add named labels
  std::vector<std::string> labels;
  labels.push_back(std::string("sin(t)"));
  labels.push_back(std::string("cos(t)"));
  labels.push_back(std::string("sin(t)+cos(t)"));
  log.SetLabels(labels);
  const float tinc = 0.01f;
  // OpenGL 'view' of data. We might have many views of the same data.
  pangolin::Plotter plotter(&log,0.0f,4.0f*(float)M_PI/tinc,-4.0f,4.0f,(float)M_PI/(4.0f*tinc),0.5f);
  plotter.SetBounds(0.0, 1.0, 0.0, 1.0);
  plotter.Track("$i");//坐标轴自动滚动
  // Add some sample annotations to the plot（为区域着色）
  plotter.AddMarker(pangolin::Marker::Vertical,   50*M_PI, pangolin::Marker::LessThan, pangolin::Colour::Blue().WithAlpha(0.2f) );
  plotter.AddMarker(pangolin::Marker::Horizontal,   3, pangolin::Marker::GreaterThan, pangolin::Colour::Red().WithAlpha(0.2f) );
  plotter.AddMarker(pangolin::Marker::Horizontal,    3, pangolin::Marker::Equal, pangolin::Colour::Green().WithAlpha(0.2f) );
  pangolin::DisplayBase().AddDisplay(plotter);
  float t = 0;
  // Default hooks for exiting (Esc) and fullscreen (tab).
  while( !pangolin::ShouldQuit() )
  {
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    log.Log(sin(t),cos(t),sin(t)+cos(t));
    t += tinc;
    // Render graph, Swap frames and Process Events
    pangolin::FinishFrame();
  }
  return 0;
}
```

