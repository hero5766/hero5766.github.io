---
title: opencv
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-07-27 19:17:00
---
> opencv的使用
<!--more-->

# 简介
## opencv历史
- opencv1.0，主要由c语言编写，性能好，学习成本较高；
- opencv2.0，c++，python的支持；
- opencv3.0，进一步提高可用性；
- opencv4.0，机器学习库的增加；

## c++环境配置
- 添加include库文件目录:`视图->其他窗口->属性管理器->[对应项目名]->Debug|x64->Microsoft.Cpp.x64.user->右键属性->VC++目录->包含目录`，添加：`opencv/build/include`,`opencv/build/include/opencv2`
- 添加lib动态链接库目录：`库目录`，添加：`opencv/build/x64/vc14/lib`
- 添加附加依赖性，lib中的*.lib文件，在调试时使用带_d的：`连接器->输入->附加依赖项`，添加：`opencv_world410d.lib`
- 添加opencv的dll文件到系统环境变量；
- 重启VS

vs版本|目录
-|-
Visual Studio 6 | vc6 
Visual Studio 2003 | vc7 
Visual Studio 2005 | vc8 
Visual Studio 2008 | vc9 
Visual Studio 2010 | vc10 
Visual Studio 2012 | vc11 
Visual Studio 2013 | vc12 
Visual Studio 2015 | vc14 
Visual Studio 2017 | vc15

- 测试是否成功
```c++
#include <opencv/opecv.hpp>
#include <iostream>
using namespace std;
using namespace cv;
int main(){
  Mat src = imread("./lena.jpg");
  if(src.empty())return -1;
  namedWindow("input",WINDOW_AUTOSIZE);
  imshow("input",src);
  waitKey(0);
  destroyAllWindows();
  return 0;
}
```

## python安装
- [ubuntu安装](https://blog.csdn.net/wohu1104/article/details/107993979)

```python
pip install opencv-python
pip install opencv-contrib-python
pip install pytessract
```

## 关于lena
- [这里有详细介绍](https://blog.csdn.net/u014061630/article/details/84388147)

# 系统操作
## 文件操作

|函数|说明|示例|
|-|-|-|
|imread(filename,mode)|打开图片，默认BGR彩色图像，支持灰度，以及任意格式<br>IMREAD_COLOR：默认模式，彩色BGR<br>IMREAD_GRAYSCALE：灰度图像<br>IMREAD_ANYCOLOR：任意色彩类型<br>IMEAD_UNCHANGED：可以接收带透明通道图像||
|imshow(winName,src)|显示图片，不支持透明通道||
|imsave(filename,src)|根据文件名格式保存图片，支持各种格式||

- 对于带有透明通道的图片，在imshow是无法显示的，必须通过IMEAD_UNCHANGED参数打开文件，并保存，使用图片浏览器查看。

## 窗口操作

|函数|说明|示例|
|-|-|-|
|namedWindow(winName,mode)|窗口设置<br>WINDOW_AUTOSIZE：根据内容自适应，不能改变窗口大小<br>WINDOW_FREERATIO：任意比例，可以改变大小<br>WINDOW_NORMAL：也可以更改||

# 色彩空间
## 常见色彩空间

|色彩空间||
|-|-|
|RGB|非设备依赖的色彩空间|
|HSV|直方图效果最好|
|Lab|两通道|
|YC<sub>b</sub>C<sub>r</sub>||

## 色彩空间的相互转换

### BGR2HSV
- 公式

- 使用HSV过滤绿色背景

```c++
cvtColor(src,hsv,COLOR_BGR2HSV);
inRange(hsv,Scalar(35,43,46),Scalar(77,255,255),mask);
bitwise_not(mask,mask);
bitwise_and(frame,frame,dst,mask);
imshow("mask",mask);
imshow("dst",dst);
```


### 代码实现

```c++
Mat hsv,lab;
cvtColor(src,hsv,COLOR_BGR2HSV);
cvtColor(src,lab,COLOR_BGR2Lab);
```



## 彩色图像的饱和度和亮度

# 卷积和傅里叶变换
## 卷积
### 数学描述
- 连续定义
$$(f*g)(n)=\int_{\infty }^{-\infty} f(\tau)g(n-\tau)d\tau$$

- 离散定义
$$(f*g)(n)=\sum_{\tau=-\infty }^{\infty} f(\tau)g(n-\tau)$$

### 图像卷积的作用
1. 模糊
2. 梯度
3. 边缘
4. 锐化

### 二维离散卷积


## 二维离散的傅里叶变换
## 傅里叶幅度谱与相位谱
## 普残差显著性监测
## 卷积与傅里叶变换
## 通过傅里叶快速变换计算卷积

# 图像数字化
## numpy的ndarry
## Mat类
### 属性

|属性|说明|
|-|-|
|cols|列数，图像宽度|
|rows|行数，图像高度|
|channels()|通道数|
|depth()|深度|
|type()|图像类型枚举|

### 图像类型枚举
- 图像深度

|图像深度|说明|
|-|-|
|CV_8U|8位无符号(字节)类型|
|CV_8S|8位有符号(字节)类型|
|CV_16U|16位无符号类型|
|CV_16S|16位有符号类型|
|CV_32S|32位有符号整型类型|
|CV_32F|32位浮点类型|
|CV_64F|32位双精度类型|

- 图像通道

|通道|说明|举例|
|-|-|-|
|C1|单通道|CV_8UC1|
|C2|双通道|CV_8UC2|
|C3|三通道|CV_8UC3|
|C4|四通道|CV_8UC4|

### 创建Mat

|功能|代码|说明|
|-|-|-|
|空白Mat对象|`Mat t1 = Mat(256,256,CV_8UC1);t1.Scalar(0);`<br>`Mat t2 = Mat(256,256,CV_8UC3);t2.Scalar(0,0,0);`<br>`Mat t3 = Mat(Size(256,256),CV_8UC1);t3.Scalar(0);`<br>`Mat t4 = Mat::Zero(Size(256,256),CV_8UC1);`<br>||
|从现有图像创建|`Mat t1 = src;//指针`<br>`Mat t2 = src.clone();//拷贝`<br>`Mat t3; src.copyTo(t3);`<br>`Mat t4 = Mat::zero(src.size(),src.type());`||
|创建有填充值的|scalar函数可以填充内容||
|创建单通道和多通道|在创建对象时设定mat的类型||


### 遍历与访问像素值
- 基于数组的遍历
```c++
for(int row=0;row<src.rows;row++){
  for(int col=0;col<src.cols;col++){
    if (src.channels()==3){
      Vec3b pixel = src.at<Vec3b>(row,col);
      int blue=pixel[0],green=pixel[1],red=pixel[2];
    }else if(src.channels()==1){
      int pv=src.at<uchar>(row,col);
    }
  }
}
```

- 基于指针的遍历
```c++
for(int row=0;row<src.rows;row++){
  unchar* curr_row = src.ptr<unchar>(row);
  for(int col=0;col<src.cols;col++) {
    if (src.channels()==3) {
      int blue = *curr_row++;
      int green = *curr_row++;
      int red = *curr_row++ ;
    }else if(src.channels()==1) {
      int pv = *curr_row++;
    }
  }
}
```

### opencv的Vec类
- OpenCV中的向量模板类，具体有Vec2b，Vec2s，Vec3b，Vec3s，Vec3i，Vec3f等

|符号|定义|说明|
|-|-|-|
|Vec3b|`typedef Vec<uchar,3> Vec3b;` |Vec3b就是有3个uchar类型元素的向量|
|Vec3s|`typedef Vec<short,3> Vec3s;` |Vec3s就是有3个short int(短整型)类型元素的向量|

## 矩阵运算
## 图像算术操作
- 在对输入图像进行加减乘除操作时，需要保证两个操作矩阵的大小、类型一致。
- unchar加减乘除的越界处理。

|函数|说明|示例|
|-|-|-|
|Mat add(src1,src2)|加||
|Mat subtract(src1,src2)|减||
|Mat multiply(src1,src2)|乘||
|Mat divide(src1,src2)|除||
|void addWeighted(src1,weight1,src2,weight2,base,dst)|两个图片分别乘以系数相加，可以通过提高系数增加对比度||

```c++
Mat src1 = imread("./1.jpg");
Mat src2 = imread("./2.jpg");
Mat dst1 = add(src1,src2);
Mat dst2 = subtract(src1,src2);
Mat dst3 = multiply(src1,src2);
Mat dst4 = divide(src1,src2);
Mat dst5; addWeighted(src1,1.2,src2,0.5,0.0,dst5);
```

## 图像位操作
- 未操作包含了：与或非异或。
- 通常使用图片未操作的效率更高。
- 位操作中还可以根据mask非零区域进行局部处理，mask通道数必须是1。

|函数|说明|示例|
|-|-|-|
|bitwise_not(src,dst,mask)|非||
|bitwise_and(src1,src2,dst,mask)|与||
|bitwise_or(src1,src2,dst,mask)|或||
|bitwise_xor(src1,src2,dst,mask)|异或||

```c++
Mat src = imread("./1.jpg");
Mat mask = Mat::zeros(src.size(),CV_8UC1);
for(int row=0;row<src.rows;row++)
  for(int col=0;col<src.cols;col++)
    mask.at<unchar>(row,col)=255;
Mat dst1; bitwise_not(src,dst1,Mat());
Mat dst2; bitwise_and(src,src,dst1,Mat());
Mat dst3; bitwise_or(src,src,dst1,Mat());
Mat dst4; bitwise_xor(src,src,dst1,Mat());
```

## 像素信息统计
- 图像最大值、最小值、均值、方差、直方图：像素分布

|函数|说明|示例|
|-|-|-|
|void minMaxLoc(src,double &min,double &max,Point &minloc,Point &maxloc)|获取最大最小值和位置，src必须是单通道的||
|Scalar mean(src)|均值，返回每个通道的均值||
|void meanStdDev(src,meanMat,stdMat)|均值和方差||

- 均值和方差
```c++
Mat src = imread("./1.jpg",IMREAD_GRAYSCALE);
double xmin,xmax;Point xminloc,xmaxloc;
minMaxLoc(src,&xmin,&xmax,&xminloc,&xmaxloc);
src = imread("./1.jpg");
Scalar s = mean(src);
Mat xmean,mstd;
meanStdDev(src,xmean,mstd);
printf("stddev:%2.f,%2.f,%2.f",mstd.at<double>(0,0),mstd.at<double>(1,0),mstd.at<double>(2,0));
```

- 像素值统计-直方图
```c++
Mat src = imread("./1.jpg",IMREAD_GRAYSCALE);
vector<int>hist(256);
for (int i=0;i<256;i++){
  hist[i]=0;
}
for(int row=0;row<src.rows;row++){
  for(int col=0;col<src.cols;col++){
    int pv = src.at<unchar>(row,col)
    hist[pv]++;
  }
}
```

## 图像绘制与填充
- 点、线、矩形、圆、椭圆、多边形、文字
- 当设置线宽值小于零，opencv会自动填充区域。

|函数|说明|举例|
|-|-|-|
|line(src,Piont,Point,Scalar color,int lineWeight,int lineType)||
|rectangle(src,Rect,Scalar color,int lineWeight,int lineType)|||
|circle(src,Piont,int radius,Scalar color,int lineWeight,int lineType)|||
|ellipse(src,RotatedRect,Scalar color,int lineWeight,int lineType)|||
|putText(src,string,Point,int fontFace,double scale,Scalar color,int lineThickness,int lineType)|||

```c++
Mat canvas=Mat::zeros(Size(512,512),CV_8UC3);
line(canvas,Point(10,10),Point(20,123),Scalar(0,0,255),LINE_8);
Rect rect(100,100,200,200);
rectangle(canvas,rect,Scalar(255,0,0),1,8);
circle(canvas,Piont(255,255),100,Scalar(255,0,0),1,8);
RotatedRect rt;
rt.center = Point(256,256);
rt.angle = 45;  // 椭圆的旋转角度
rt.size = Size(100,200);
ellipse(canvas,rt,Scalar(255,0,0),-1,8);
putText(canvas,"hello",Point(100,50),FONT_HERSHEY_SIMPLEX,1.0,Scalar(0,0,255),2,8);
imshow("canvas",canvas);
```

- 随机绘制
```c++
Mat canvas=Mat::zeros(Size(512,512),CV_8UC3);
int x1=0,x2=0,y1=0,y2=0;
RNG rng(12345);
while(true){
  x1 = (int)rng.uniform(0,512);
  x2 = (int)rng.uniform(0,512);
  y1 = (int)rng.uniform(0,512);
  y2 = (int)rng.uniform(0,512);
  Scalar color = Scalar((int)rng.uniform(0,256),(int)rng.uniform(0,512),(int)rng.uniform(0,512));
  int lineWeight = (int)rng.uniform(1,20);
  line(canvas,Point(x1,y1),Point(x2,y2),color,lineWeight,LINE_8);
  imshow("canvas",canvas);
  //canvas = Scalar(0,0,0); //清空画布
  char c = waitKey(10); //等待0.01秒
  if(c==27){  // esc
    break;
  }
}
```


## 图像通道
- 通道分离合并

|函数|说明|举例|
|-|-|-|
|split(src,Vector<mat>)|||
|merge(Vector<mat>,dst)|||

```c++
//分离
vector<Mat> rgbCh(3);
split(src, rgbCh);
Mat blue=rgbCh[0],green=rgbCh[1],red=rgbCh[2];
//合并
Mat dst;vector<Mat> merge_src;
merge_src.push_back(blue);merge_src.push_back(green);merge_src.push_back(red); //根据bgr的顺序
merge(merge_src,dst);
```

```c++
Mat src = imread("./1.jpg",IMREAD_GRAYSCALE);
if(src.empty())return -1;
vector<Mat> rgbCh;split(src, rgbCh);
rgbCh[0] = Scalar(0);  //去除了蓝色
bitwise_not(rgbCh[1],rgbCh[1]); //绿色取反
Mat dst;
merge(rgbCh,dst);
imshow("merge",dst);
```

## 截取兴趣域
```c++
Mat src = imread("./1.jpg",IMREAD_GRAYSCALE);
Rect roi;roi.x=100,roi.y=100,roi.width=200;roi.height=200;
Mat dst = src(roi).clone()  //不克隆的话，兴趣域指向同一块地址
rectangle(src,roi,Scalar(0,0,255),1,8);
imshow("src",src);
imshow("dst",dst);
waitKey(0);destroyAllWindows();
```

# 集合变换
## 仿射变换
## 投影变换
## 极坐标变换

# 对比度增强
## 直方图
- 统计与显示：对于CV_8UC1的灰度图像，像素的取值范围是0~255，假使统计范围个数bin=16，每个bin的范围0~15。直方图展示的就是符合每个bin的像素个数。

|函数|说明|举例|
|-|-|-|
|calcHist()|计算直方图||
|normalize(src,dst,)|归一化||

```c++
Mat src = imread("./1.jpg",IMREAD_GRAYSCALE);
if(src.empty())return -1;
vector<Mat> rgbCh;split(src, rgbCh);
Mat blue=rgbCh[0],green=rgbCh[1],red=rgbCh[2];
//计算直方图
Mat hist_b,hist_g,hist_r;
calcHist(&blue,1,0,Mat(),hist_b,1,256,{float[]{0,255}},true,false);
calcHist(&green,1,0,Mat(),hist_g,1,256,{float[]{0,255}},true,false);
calcHist(&red,1,0,Mat(),hist,hist_r,256,{float[]{0,255}},true,false);
//绘制直方图
//将获取的结果归一化
Mat HistShow = Mat::zeros(Size(600,400),CV_8UC3);
int margin=50;
normalize(hist_b,hist_b,0,HistShow-2*margin,NORM_MINMAX,-1,Mat());
normalize(hist_g,hist_b,0,HistShow-2*margin,NORM_MINMAX,-1,Mat());
normalize(hist_r,hist_b,0,HistShow-2*margin,NORM_MINMAX,-1,Mat());
//绘制
float step = (HistShow-2*margin)/256.0
for(int i=0;i<255;i++){
  float bh1 = hist_b.at<float>(i,0),bh2 = hist_b.at<float>(i+1,0);
  float gh1 = hist_b.at<float>(i,0),gh2 = hist_b.at<float>(i+1,0);
  float rh1 = hist_b.at<float>(i,0),rh2 = hist_b.at<float>(i+1,0);
  line(HistShow,Point(step*i,50+nm-bh1),Point(step*i,50+nm-bh2),Scalar(255,0,0),1,8,0);
  line(HistShow,Point(step*i,50+nm-gh1),Point(step*i,50+nm-gh2),Scalar(0,255,0),1,8,0);
  line(HistShow,Point(step*i,50+nm-rh1),Point(step*i,50+nm-rh2),Scalar(0,0,255),1,8,0);
}
imshow("HistShow",HistShow);
waitKey(0);
```

## 直方图均衡化
- 直方图均衡化后的图像会有比较好的显示效果，暗部变量，举例：64*64大小的图像，8个bin，像素总数4096

|k|$r_k$|$n_k$|$\frac{n_k}{n}$|$S_k=\sum_{j=0}^{k}{\frac{n_j}{n}}$|$p_S(s_k)$|
|-|-|-|-|-|-|
|0|$\frac{0}{7}$|790|0.19|0.19->$\frac{1}{7}$->$s_0$|0.19|
|1|$\frac{1}{7}$|1023|0.25|0.44->$\frac{3}{7}$->$s_1$|0.25|
|2|$\frac{2}{7}$|850|0.21|0.65->$\frac{5}{7}$->$s_2$|0.21|
|3|$\frac{3}{7}$|656|0.16|0.81->$\frac{6}{7}$->$s_3$|0.24|
|4|$\frac{4}{7}$|329|0.08|0.89->$\frac{6}{7}$->$s_3$|0.24|
|5|$\frac{5}{7}$|245|0.06|0.95->$\frac{7}{7}$->$s_4$|0.11|
|6|$\frac{6}{7}$|122|0.03|0.98->$\frac{7}{7}$->$s_4$|0.11|
|7|$\frac{7}{7}$|81|0.02|1.0->$\frac{7}{7}$->$s_4$|0.11|

```c++
Mat src = imread("./1.jpg",IMREAD_GRAYSCALE),gray,dst;
cvtColor(src,gray,COLOR_BGR2GRAY);
imshow("gray",gray);
equalizeHist(gray,dst);
imshow("hist",dst);
```

## 直方图比较
- 直方图比较可以评价两幅图的直方图分布的相似性。

- 相关性
$$d(H_1,H_2)=\frac{\sum_I{(H_1(I)-\hat{H}_1)(H_2(I)-\hat{H}_2)}}{\sqrt{\sum_I{(H_1(I)-\hat{H}_1)^2}\sum_I{(H_2(I)-\hat{H}_2)^2}}}$$

$$\hat{H}_k=\frac{1}{N}\sum_J{H_k(J)}$$

- 卡方
$$d(H_1,H_2)=\sum_I{\frac{(H_1(I)-H_2(I))^2}{H_1(I)}}$$

- 交叉
$$d(H_1,H_2)=\sum_I{min(H_1(I),H_2(I))}$$

- 巴氏距离
$$d(H_1,H_2)=\sqrt{1-\frac{1}{\sqrt{H_1H_2N^2}}\sum_I{\sqrt{H_1(I)-H_2(I)}}}$$

```c++
Mat src1 = imread("./1.jpg",IMREAD_GRAYSCALE);
Mat src2 = imread("./2.jpg",IMREAD_GRAYSCALE);
int histSize[] = {256,256,256};
int channels[] = {0,1,2};
Mat hist1,hist2;
float c1[] = {0,255};
float c2[] = {0,255};
float c3[] = {0,255};
const float *histRanges[]={c1,c2,c3};
calcHist(&src1,1,channels,Mat(),hist1,3,histSize,histRanges,true,false);
calcHist(&src2,1,channels,Mat(),hist2,3,histSize,histRanges,true,false);
//归一化
normalize(hist1,hist1,0,1.0,NORM_MINMAX,-1,Mat())l;
normalize(hist2,hist2,0,1.0,NORM_MINMAX,-1,Mat())l;
//巴氏距离
double h12 = compareHist(hist1,hist2,HISTCMP_BHATTACHARYYA);
double h11 = compareHist(hist1,hist1,HISTCMP_BHATTACHARYYA);
printf("%.2f,%.2f",h12,h11)
```

## 图像查找表与颜色表
- 查找表是一维或多维的数组，存储了不同输入值所对应的输出值。根据一对一或多对一的函数，定义了如何将像素转换为新的值。

|函数|说明|举例|
|-|-|-|
|LUT(src,lut,dst)|颜色替换||
|applyColorMap(src,dst,COLORMAP_AUTUMIN)|自带12种颜色查找表替换||


```c++
Mat src = imread("./1.jpg");
Mat color = imread("./lut.jpg");
Mat lut = Mar::zeros(256,i,CV_8U3);
for(int i=0;i<256;i++){
  lut.at<Vec3b>(i,0)=color.at<Vec3b>(10,i);
}
Mat dst;
LUT(src,lut,dst);
imshow("color",color);imshow("dst",dst);
```

## 线性变换
## 直方图正规化
## 伽马变换
## 全局直方图均值化
## 直方图反向投影
- 原理：如果有一个物品图1具有很明显的直方图特征，比如颜色或者明暗呈有规律的变化，就可以利用直方图特征，在目标图2中查找这个特征。

- 步骤：
  1. 计算直方图：计算直方图采用HSV中的H和S二维空间进行转换，并归一化
  2. 计算比率：$R=\frac{H^1_{x.y}}{H^2_{x.y}}$目标图2肯定直方图特征较为复杂，比率结果仅在物品图直方图较高
  3. 卷积模糊：由于bin是有大小的，一般取48，直接输出会变得很尖锐，所以需要进行模糊
  4. 反向输出

### 代码实现

```c++
Mat model,src;
Mat model_hsv,src_hsv;
cvtColor(model,model_hsv,COLOR_BGR2HSV);
cvtColor(src,src_hsv,COLOR_BGR2HSV);
int h_bins=32,s_bin=32;
int histSize[]={h_bins,s_bins};
int channels[] = {0,1};
Mat roiHist;
float h_range[]={0,180},s_range[]={0,255};
const float* ranges[]={h_range,s_range};
calHist(&model_hsv,1,channels,Mat{},roiHist,2,histSize(),ranges,true,false);
normalize(roiHist,roiHist,NORM_MINMAX,-1,Mat());
MatND backproj;
calcBackproject(&src_hsv,1channels,roiHist,backproj,ranges,1.0);
imshow("backproject",backproj);
```

## 限制对比度的自适应直方图均值化



# 频率域滤波
## 低通滤波和高通滤波
## 带通和带阻滤波
## 自定义滤波
### 卷积核

```c++
//均值滤波卷积核
int k=15;
Mat meankernel = Mat::ones(k,k,CV_32F)/(float)(k*k);
//自定义滤波 src,dst,depth,卷积核，锚点，提升亮度，边缘填充方式
filter2D(src,dst,-1,meankernel,Point(-1,-1),0,BORDER_DEFAULT)
//非均值滤波，此时滤波后的范围是-255~255，需要改变输出的类型，提升亮度为127，检测出轮廓
Mat robot = (Mat_<int>(2,2)<<1,0,0,-1);
filter2D(src,dst,CV_32F,robot,Point(-1,-1),127,BORDER_DEFAULT);
//将正负取绝对值
convertScaleAbs(dst,dst);
```

## 同态滤波

# 图像平滑
## 均值卷积
### 代码实现
- 自己实现，均值卷积，所有卷积核系数是一样的
```c++
Mat src = imread("./1.jpg");
Mat dst = Mar::zeros(src.size(),src.type());
int h = src.rows,w = src.cols;
//对于边缘不做处理
for (int row=1;row<h-1;row++){
  for (int col=1;col<w-1;col++){
    int sb = src.at<Vec3b>(row-1,col-1)[0] + src.at<Vec3b>(row-1,col)[0] + src.at<Vec3b>(row-1,col+1)[0] +
    src.at<Vec3b>(row,col-1)[0] + src.at<Vec3b>(row,col)[0] + src.at<Vec3b>(row,col+1)[0] +
    src.at<Vec3b>(row+1,col-1)[0] + src.at<Vec3b>(row+1,col)[0] + src.at<Vec3b>(row+1,col+1)[0];
    int sg = src.at<Vec3b>(row-1,col-1)[1] + src.at<Vec3b>(row-1,col)[1] + src.at<Vec3b>(row-1,col+1)[1] +
    src.at<Vec3b>(row,col-1)[1] + src.at<Vec3b>(row,col)[1] + src.at<Vec3b>(row,col+1)[1] +
    src.at<Vec3b>(row+1,col-1)[1] + src.at<Vec3b>(row+1,col)[1] + src.at<Vec3b>(row+1,col+1)[1];
    int sr = src.at<Vec3b>(row-1,col-1)[2] + src.at<Vec3b>(row-1,col)[2] + src.at<Vec3b>(row-1,col+1)[2] +
    src.at<Vec3b>(row,col-1)[2] + src.at<Vec3b>(row,col)[2] + src.at<Vec3b>(row,col+1)[2] +
    src.at<Vec3b>(row+1,col-1)[2] + src.at<Vec3b>(row+1,col)[2] + src.at<Vec3b>(row+1,col+1)[2];
    src.at<Vec3b>(row,col)[0]=sb/9;
    src.at<Vec3b>(row,col)[1]=sg/9;
    src.at<Vec3b>(row,col)[2]=sr/9;
  }
}
imshow("conv",dst);
```

- 用opencv的函数
```c++
Mat src = imread("./1.jpg");
Mat dst;
blur(src,dst,Size(3,3),Point(-1,-1),BORDER_DEFAULT);
imshow("conv",dst);
//size：核的大小
//Point(-1,-1)：锚定位置
//BORDER_DEAULT：边缘处理的方式，使用边缘填充的方式
```

- 卷积的边缘处理

|函数|方法|
|-|-|
|BORDER_CONSTANT|0000/abcd/0000|
|BORDER_REPLICATE|aaaa/abcd/dddd|
|BORDER_WRAP|bcd/abcd/abc|
|BORDER_REFLECT_101|dcb/abcd/cba|
|BORDER_DEFAULT|dcb/abcd/cba|

```c++
int border = 8;
copyMakeBorder(src,dst,border,boder,boder,border,BORDER_DEFAULT);
copyMakeBorder(src,dst,border,boder,boder,border,BORDER_CONSTANT,SCalar(0,0,255));
```

- 锚定位置的选择
  - 不同锚定位置，卷积后的图像不同。选择尽量使得奇数的卷积核，尽量使得锚定点在中心位置。

![锚定位置](/images/pasted-69.png)

## 高斯平滑
## 均值平滑
## 中值平滑
## 双边滤波
## 联合双边滤波
## 导向滤波

# 图像模糊
## 高斯模糊
- 一阶高斯函数：$f(x)=e^{-\frac{x^2}{2\delta^2}}$
- 二阶高斯函数：$f(x,y)=e^{-\frac{x^2+y^2}{2\delta^2}}$
- 使用高斯函数可以尽量使得中心点的信息保留，比均匀卷积更能保留原始图像的信息。
- 高斯模糊的锚定位置必须是中心位置。
- 代码实现
```c++
GaussianBlur(src,dst,Size(5,5),0);
```

## 盒子模糊
- 盒子模糊即均值模糊，与高斯模糊不同，边缘的破坏会随着核大小增大会越严重。
$$K=\alpha\begin{bmatrix} 1 & 1\\1  &1\end{bmatrix}$$
$$\alpha=\begin{cases}\frac{1}{width·height}  & when \text{normalize=true} \\1  & otherwise\end{cases}$$

- 代码实现
```c++
boxBlur(src,dst,-1,Size(5,5),Point(-1,-1),true,BORDER_DEFAULT);
//设置任意方向的模糊
boxBlur(src,dst,-1,Size(15,1),Point(-1,-1),true,BORDER_DEFAULT);
```

## 锐化
### 锐化的梯度算子
- 数学描述

$$g(x,y)=f(x,y)-[f(x+1,y)+f(x-1,y)+f(x,y+1)+f(x,y-1)+4f(x,y)]$$

$$\begin{bmatrix}0 & -1 & 0\\-1 & 5 & -1\\0 & -1 & 0\end{bmatrix}$$

- 用原图减去二阶梯度边缘检测算子，将图像中边缘的增强加到原图中，实现锐化的效果。

### 代码实现

```c++
Mat mylaplacian = (Mat_<int>(3,3)<<0,-1,0,-1,5,-1,0,-1,0);
Mat dst;
filter2D(src,dst,CV_32F,mylaplacian,Point(-1,-1),0,BORDER_DEFAULT);
converScaleAbs(dst,dst);
imshow("sharpen filter",dst);
```
## USM锐化
- unsharp mark filter
- 拉普拉斯二阶卷积对于噪声非常敏感，而图像模糊则会将大的边缘变成小的边缘，将两者相减，边缘的差异就显现出来了。
$$sharp_image=w_1·blur-w_2·laplacian$$

- USM对大的边缘具有更好的锐化效果，图片效果更为自然。

### 高斯与中值模糊的USM增强

- 代码实现

```c++
Mat meanBlur,laplac,dst;
GaussianBlur(src,meanBlur,Size(3,3),0);
Laplacian(src,laplac,-1,1,1.0,BORDER_DEFAULT);
addWeight(meanBlur,1.0,laplac,-0.7,0,dst);
```

## 噪声
- 噪声产生的原因：设备原因，环境原因等
- 噪声的分类：椒盐噪声（黑白点）、高斯噪声等

### 椒盐噪声
```c++
RNG rng(12345);
Mat saltImg = src.clone();
int h=src.rows,w=src.cols,nums=100000;
for(int i=0;i<nums;i++){
  int x=rng.uniform(0,w),y=rng.uniform(0,h);
  if(i%2==1)saltImg.at<Vec3b>(y,x)=Vec3b(255,255,255);
  else saltImg.at<Vec3b>(y,x)=Vec3b(0,0,0);
}
```


### 高斯噪声

```c++
Mat noise = Mat::zeros(src.size(),image.type());
randn(noise,Scalar(15,15,15),Scalar(30,30,30));
Mat GaussImg;
add(image,noise,GaussImg);
```


## 去除噪声
- 原理：
  - 对于椒盐噪声，主要是处理离群点，将偏差减小，常见方法有中值、均值。均值会对原始图像造成扰动，而中值表现更好。
  - 对于高斯噪声，中值滤波处理后效果变得更模糊，高斯滤波处理效果也不好。比较好用的是边缘保留滤波


### 中值滤波
- 前面的滤波都属于线性滤波，中值滤波是一种统计滤波，常见的还有最小值滤波、最大值滤波、均值滤波

#### 代码实现

```c++
//中值滤波
medianBlur(src,dst,5);
```

### 高斯滤波
- 高斯滤波对于椒盐噪声和高斯噪声处理效果都不好。

#### 代码实现

```c++
//中值滤波
GaussianBlur(src,dst,Szie(5,5),0);
```

### 边缘保留滤波
- 中值滤波和高斯滤波，并没有考虑中心像素点对整个图像的贡献。锚定的像素点实际上是贡献最大的。所以中值滤波后的图像都被模糊掉了。高斯滤波没有考虑中心像素点和周围像素差值很大的时候，可能是边缘和梯度的值，应该减少中心像素点的权重。
- 双边滤波根据每个位置的邻域，构建不同的权重模板。简单定义：I：输入图像；w：权重；O：输出：
$$O(s_0)=\frac{1}{K}\sum_N{W(s,s_0)·I(s)}$$


#### 高斯双边模糊
- 权重有高斯核函数生成，组成为$w=w_s+w_c$
$$spatial space: w_s=\exp(\frac{-(s-s_0)^2}{2\delta_s^2})$$
$$color range: w_c=\exp(\frac{-(I(s)-I(s_0))^2}{2\delta_c^2})$$

- 代码事项

```c++
// src,dst,原来的尺寸上还是扩大还是缩小，颜色值空间，高斯核大小
bilateralFilter(src,dst,0,100,10);
```

- 高斯双边模糊，可以很好的去除图像中的噪声，经常用作于照片的磨皮。

#### 非局部均值滤波
- 相似像素块，权重比较大，不相似的权重比较小。搜索窗口为`l*l`的，i是`3*3`像素的模板，在窗口中进行均值计算。越相似权重越大，越不想死权重越小。最后将权重归一化。
- $w_{(p,q_1)},w_{(p,q_2)},w_{(p,q_3)}$

$$x_{NLM}(i)=\frac{1}{\sum_{j\in W_i}{w(i,j)}}\sum_{j\in W_i}{w(i,j)x(j)}$$
$$w(i,j)=exp{(-\sum_{k}{\frac{[x(l^k_j)-x(l^k_i)]^2}{h^2}})}$$

- h相当于$\theta$，可以决定非均质滤波模糊的程度。

- 代码实现

```c++
//彩色版本
//src,dst,h,color-h,模板边长,搜索窗口l
fastNIMeansDenoisingColored(src,dst,15,15,7,21);
//灰度版本
cvtColor(src,gray,COLOR_BGR2GRAY);
fastNIMeansDenoising(gray,dst2);
```

- 这个方法非常慢，但是对于高斯模糊后的图片处理非常好。

# 阈值分割
## 二值图像
- 灰度图像和二值图像：都是单通道，但是灰度图像取值范围是0~255，二值图像只有0和255。

## 方法概述
- 五种阈值分割的方法：设定阈值为：T

|方法||
|||
|THRESH_BINARY|大于T：255，否则：0|
|THRESH_BINARY_INV|小于T：255，否则：0|
|THRESH_TRUNC|小于T：原值，否则：T|
|THRESH_TOZERO|小于T：0，否则：原值|
|THRESH_TOZERO_INV|大于T：0，否则：原值|

- 代码实现

```c++
Mat gray,binary;
cvtColor(src,gray,COLOR_BGR2GRAY);
imshow("gray",gray);
threshold(gray,binary,127,255,THRESH_BINARY);
imshow("threshold"binary);
```

## 全局阈值分割
- 对图像进行二值化操作，最重要的就是计算阈值。前面使用一个自己定义的阈值，并不准确。
- 全局阈值分割是一种自动查找阈值的方法

### 均值阈值
- 数学描述
$$m=\frac{\sum_{i=0}^h\sum_{j=0}^w{p(i,j)}}{w·h}$$
$$p(i,j)=\begin{cases} 255&>=m \\0&<m\end{cases}$$

- 代码实现

```c++
cvtColor(src,gray,COLOR_BGR2GRAY);
Scalar m = mean(gray);
threshold(gray,binary,m[0],255,THRESH_BINARY);
```

### Otsu阈值
- 均值并不能真是反应图像的分布信息，使用直方图可以解决这个缺点
- 大津法原理就是找到一个分隔，使得类内之间差距比较小，类类之间差距比较大，这样就可以分布不同的类别

#### 过程
- 将原图直方图进行划分为2类，前景和背景，这里选择2作为分隔，
- 分别求取前景和背景的比重、平均值、方差

![直方图](/images/pasted-71.png)

- 根据求得的结果，计算类内方差
$$/theta^2_w=w_b·/theta_b^2+w_f·/theta_f^2$$

- 再选择其他作为分隔，找到最小方差的域作为分隔点

- 代码实现

```c++
int t = threshold(gray,binary,0,255,THRESH_BINARY|THRESH_OTSU);
```

- 大津法对单峰多峰都比较好

### 三角法
- 假设直方图效果是一个单脉冲的结构，那么做极值点到终点的辅助线就形成了一个三角，找到最长的d对应的h，保证$/alpha$和$/beta$是45°，此时的点就是阈值的分隔点T
- 通常接近峰值的T效果并不高，通常是乘以1.2的系数，作为最终的T

![三角法](/images/pasted-72.png)

$$h^2=d^2+d^2-->d=\sqrt{\frac{h^2}{2}}-->d=sin(0.7854)*h$$

- 代码实现

```c++
int t = threshold(gray,binary,0,255,THRESH_BINARY|THRESH_TRIANGLE);
```
- 缺点：仅对单峰效果比较好。常用于医疗图像的阈值分隔，这类图片的峰值单一。

## 自适应阈值
- 由于光照不均匀的原因，使用全局阈值会丢失一部分信息
- 原理：对模糊图像求差值再二值化

- 步骤
  1. 对原图进行盒子模糊，得到图像D
  2. 原图+偏置常量C，获得图像S
  3. 最终图像T=S-D+255

- 即：$T = origin_image+C-D : 255 if >0 else 0$
- 阈值分割的好坏取决于模糊的窗口大小、常量C，

### 自适应均值模糊分割
- 代码实现

```c++
//src,dst,max_value,方法,阈值模式,模糊窗口大小,常量C
adaptiveThreshold(gray,binary,255,ADAPTIVE_THRESH_MEAN_C,THRESH_BINARY,25,10);
```

### 自适应高斯模糊分割
- 代码实现

```c++
adaptiveThreshold(gray,binary,255,ADAPTIVE_THRESH_GAUSSIAN_C,THRESH_BINARY,25,10);
```

### 阈值函数
### 局部阈值分割
## 直方图
## 熵算法

# 边缘检测
- 边缘法线：单位向量在该方向图像像素强度变化最大
- 边缘方向：与边缘法线垂直
- 边缘位置或中心：图像边缘所在位置
- 边缘强度：沿法线方向的图像局部对比越大，越是边缘
- 边缘的种类
  1. 跃迁类型
  2. 屋脊类型

## 梯度
- 提取方法
  1. 高斯模糊
  2. 基于梯度：都是基于阈值
    - Robot算子
    - Sobel算子
    - Prewitt算子
  3. Canny

### Robot算子
- 数学描述
$$G_x=\begin{bmatrix}1 & 0 \\  0 & -1\end{bmatrix}$$
$$G_y=\begin{bmatrix}0 & 1\\0 & 1\end{bmatrix}$$

- L1梯度
$$|G|=|G_x|+|G_y|$$

- L2梯度
$$|G|=\sqrt{G_x^2+G_y^2}$$

- robot算子可以检测出比较明显的边缘，如果过图像的边缘清晰，可以使用robot算子。

### 代码实现

```c++
Mat robot_x = (Mat_<int>(2,2)<<1,0,0,-1);
Mat robot_y = (Mat_<int>(2,2)<<0,1,-1,0);
Mat grad_x,grad_y;
filter2D(src,grad_x,CV_32F,robot_x,Point(-1,-1),0,BORDER_DEFAULT);
filter2D(src,grad_y,CV_32F,robot_y,Point(-1,-1),0,BORDER_DEFAULT);
convertScaleAbs(grad_x,grad_x);
convertScaleAbs(grad_y,grad_y);
Mat dst;
add(grad_x,grad_y,dst);
```


## Roberts算子
## Prewitt边缘检测
- 数学描述

$$G_x=\begin{bmatrix}-1 & 0 & 1\\-1 & 0 & 1\\-1 & 0 & 1\end{bmatrix}$$
$$G_y=\begin{bmatrix}1 & 1 & 1\\0 & 0 & 0\\-1 & -1 & -1\end{bmatrix}$$


## Sobel边缘检测
### Sobel算子
- 数学描述

$$G_x=\begin{bmatrix}-1 & 0 & 1\\-2 & 0 & 2\\-1 & 0 & 1\end{bmatrix}$$
$$G_y=\begin{bmatrix}-1 & -2 & -1\\0 & 0 & 0\\1 & 2 & 1\end{bmatrix}$$

- Sobel算子边缘描述能力比robot算子好一些。

### 代码实现

```c++
Mat grad_x,grad_y;
Sobel(src,grad_x,CV_32F,1,0);
Sobel(src,grad_y,CV_32F,0,1);
convertScaleAbs(grad_x,grad_x);
convertScaleAbs(grad_y,grad_y);
Mat dst;
//add(grad_x,grad_y,dst);
addWeighted(grad_x,0.5,grad_y,0.5,0,dst);
```


## Scharr算子
### Scharr算子
- 数学描述
$$G_x=\begin{bmatrix}-3 & 0 & 3\\-10 & 0 & 10\\-3 & 0 & 3\end{bmatrix}$$
$$G_y=\begin{bmatrix}-3 & -10 & -3\\0 & 0 & 0\\3 & 10 & 3\end{bmatrix}$$

- Scharr算子对于图像边缘有极强的检测能力，如果图像边缘不容易获取，可以使用Scharr。

### 代码实现

```c++
Mat grad_x,grad_y;
Scharr(src,grad_x,CV_32F,1,0);
Scharr(src,grad_y,CV_32F,0,1);
convertScaleAbs(grad_x,grad_x);
convertScaleAbs(grad_y,grad_y);
Mat dst;
//add(grad_x,grad_y,dst);
addWeighted(grad_x,0.5,grad_y,0.5,0,dst);
```

## Kirsch算子和Robinson算子
## Canny边缘检测
- 整合了很多基础算法，得到很好的边缘检测效果
- 使用高低阈值，减少单一阈值造成图像边缘检测的不连续。

### 步骤
1. **模糊去噪声**
 - 使用高斯滤波
2. **提取梯度与方向**
  - 使用sobel算子，计算x,y方向的值，获取L2梯度
3. **非最大信号抑制**
  - 在前面获得的梯度方向，判断该像素与方向两侧相比，是否是最大，最大保留。否则丢弃
4. **高低阈值链接**
  - 设高阈值T1、低阈值T2，其中T1/T2=2~3
  - 策略：大于高阈值全部保留，小于低阈值全部丢弃，之间的可以连接到高阈值像素点的保留

### 代码实现

```c++
int t1=50;
Mat src;
void cannyChange(int,void*){
Mat edges,dst;
  //src,edges,低阈值,高阈值,周围几个梯度一起算,是否用l2梯度
  Canny(src,edges,t1,t1*3,3,false);
  imshow("edge",edges);
  //显示边缘的颜色
  //bitwise_and(src,src,dst,edges);
  //imshow("edge",dst);
}
//创建拖动条：拖动条名称，窗体名称，变量，阈值范围，回调函数，用户数据
imshow("input",src);
createTrackbar("threshold value:","input",&t1,100,cannyChange);
cannyChange(0,0);
```

## 二阶导数算子
- 对图像进行二阶导数，那么图像的边缘将则过0点。

![二阶导数](/images/pasted-70.png)


- 四邻域拉普拉斯算子

$$\begin{bmatrix}0 & 1 & 0\\1 & -4 & 1\\0 & 1 & 0\end{bmatrix}$$

- 八邻域拉普拉斯算子

$$\begin{bmatrix}1 & 1 & 1\\1 & -8 & 1\\1 & 1 & 1\end{bmatrix}$$

- 变种拉普拉斯算子

$$\begin{bmatrix}-1 & 2 & -1\\2 & -4 & 2\\-1 & 2 & -1\end{bmatrix}$$

- 缺点：对于细小变化比较大的情况，以及噪声特别敏感。

### 代码实现

```c++
//拉普拉斯的ksize必须是大于等于1的奇数，用于结果输出是当前值，还是周围3*3合起来的值
Laplacian(src,dst,-1,3,1.0,BORDER_DEFAULT);
```

## Laplacian算子
## 高斯拉普拉斯LoG边缘检测
## 高斯差分DoG边缘检测
## Marr-Hildreth边缘检测
## Harris角点检测
- 根据图像的像素点，在相互垂直的x或者y方向上，如果有较大的梯度，则这个点就是角点。
- 数学描述：
$$M=\begin{bmatrix} \sum_{S(p)}{(\frac{dI}{dx})^2} &\sum_{S(p)}{\frac{dI}{dx}\frac{dI}{dy}} \\ \sum_{S(p)}{\frac{dI}{dx}\frac{dI}{dy}}  &\sum_{S(p)}{(\frac{dI}{dx})^2}\end{bmatrix}$$

- 计算xy方向的偏导数：`Ixy`、`Ixx`、`Iyx`、`Iyy`，计算相应最大的梯度
- 求取梯度：
$$E(u,v)=\sum_{x,y}{w(x,y)[I(x+u,y+v)-I(x,y)]^2}$$

- 其中：$w(x,y)$：窗口算子，$I(x+u,y+v)-I(x,y)$：偏移，u和v就是移动的大小
$$E(u,v)\cong[u,v]\;M\;\begin{bmatrix}u\\v\end{bmatrix}$$

- 求解M的最大值
$$M=\sum_{x,y}{w(x,y)\begin{bmatrix}I_x^2  &I_xI_y \\I_xI_y  &I_y^2\end{bmatrix}}$$

- 求取R值越大，响应值越大。矩阵有两个变换：特征向量的变换，奇异值变换
$$R=\det{M}-k(trace\;M)^2$$
$$\det{M}=\lambda_1\lambda_2$$
$$trace\;M=\lambda_1+\lambda_2$$

- det求取矩阵的特征值`λ<sub>1<\sub>λ<sub>2<\sub>`
- 在边缘区域上，`λ<sub>1<\sub>λ<sub>2<\sub>`其中一个很大，在平坦区域，`λ<sub>1<\sub>λ<sub>2<\sub>`都很小，只有在角点区域才使得R很大
- k值是用于调节R的大小的，一般k=0.04~0.06


### 代码实现

```c++
int blocksize=2,ksize=3;double k=0.04;
//input,output,计算偏导时区域范围,sobel计算梯度窗口大小,k
cornerHarris(gray,dst,blocksize,ksize,k);
Mat dst_norm = Mat::zeros(dst.size,dst.type());
normalize(dst,dst_norm,0,255,NORM_MINMAX,-1,Mat());
convertScaleAbs(dst_norm,dst_norm);
//draw corners
for (int row=0;row<src.rows;row++){
  for (int col=0;col<src.cols;col++){
    int rsp=dst_norm.at<uchar>(row,col);
    if (rsp>150){
      circle(src,Point(col,row),5,Scalar(0,255,0),2,8,0);
    }
  }
}
```

## shi-tomas角点检测

- shi-tomas优化了R的求取方法，仅计算`λ<sub>1<\sub>λ<sub>2<\sub>`最小的值，假如超过了阈值，那么`λ<sub>1<\sub>λ<sub>2<\sub>`就都很大。简化了R值的计算

$$R=min(\lambda_1,\lambda_2$$)$$

```c++
vector<Point2f>corners;
double quality_level=0.01;
//input,output,最多检测多少个角点,角点检测接受的最小值,角点间最小距离,mask,blockSize,是否使用Harris角点检测器
goodFeaturesToTrack(gray,corners,200,quality_level,3,Mat(),3,false);
//draw corners
for (int i=0;i<corners.size();i++){
  circle(src,corners[i],5,Scalar(0,255,0),2,8,0);
}
```


# 几何形状检测和拟合
## 联通组件扫描
- 联通性的定义，四邻域和八邻域
- 联通组件标记CCL

### 扫描方法
- 基于像素的扫描方法
- 基于块的扫描
- 两部法扫描

- opencv采用的是基于块的决策表的方法：BBDT

- 代码实现

```c++
//对原图进行高斯模糊，目的是降噪
GaussianBlur(src,src,Size(3,3),0);
Mat gray,binary;
cvtColor(src,gray,COLOR_BGR2GRAY);
threshold(gray,binary,0,255,THRESH_BINARY|THRESH_OTSH);
//binary,与原图一样大小的CV32S的,8联通还是4联通,输出类型,使用联通方法
Mat label=Mat::zeros(binary.size(),CV_32S);
int num_label = connectedComponents(binary,label,8,CV_32S,CCL_DEFAULT);
vector<Vec3b>colorTable(num_label);
// brackgrand color
colorTable[0] = Vec3b(0,0,0);
for (int i=1;i<num_label;i++){
  colorTable[i] = Vec3b(rng.uniform(0,256),rng.uniform(0,256),rng.uniform(0,256));
}
Mat dst = Mat::zeros(src.size(),CV_8UC3);
int h=dst.rows,w=dst.cols;
for(int row=0;row<h;row++){
  for(int col=0;col<w;col++){
    int label_index = label.at<int>(row,col);
    dst.at<Vec3b>(row,col) = colorTable(label_index);
  }
}
putText(dst,format("number:%d",num_label),Point(50,50),FONT_HERSHEY_PLAIN,1.0,Scalar(0,255,0),1);
```

- 带有统计信息的
```c++
//stats：外接矩形位置，面积信息
//centroids：中心位置
Mat stats,centroids;
int num_label = connectedComponentsWithStats(binary,label,stats,centroids,8,CV_32S,CCL_DEFAULT);
for (int i=1;i<num_label;i++){
  //center
  int cx = centroids.at<double>(i,0);
  int cy = centroids.at<double>(i,1);
  //rectangle
  int x = stats.at<int>(i,CC_STAT_LEFT);
  int y = stats.at<int>(i,CC_STAT_TOP);
  int w = stats.at<int>(i,CC_STAT_WIDTH);
  int h = stats.at<int>(i,CC_STAT_HEIGHT);
  int area = stats.at<int>(i,CC_STAT_AREA);
  //绘制中心位置
  circle(dst,Point(cx,cy),3,Scalar(255,0,0),2,8,0);
  //最小矩形
  Rect box(x,y,w,h);
  rectangle(dst,box,Scalar(0,255,0),2,8);
putText(dst,format("number:%d",num_label),Point(x,y),FONT_HERSHEY_PLAIN,1.0,Scalar(0,255,0),1);
  //覆盖像素面积
  putText(dst,format("number:%d",area),Point(x,y),FONT_HERSHEY_PLAIN,1.0,Scalar(0,255,0),1);
}
```
## 轮廓
- 轮廓主要针对二值图像，0-1分割的点的集合。
### 发现
- 基于边界跟随的轮廓发现

|||
|-|-|
|findContours(binary,mode,method,contours)|发现轮廓的树形结构，最外层边界依次向里|
|drawContours(src,mode,index,color,thickness,linetype)|绘制轮廓|

```c++
vector<vector<Point>>contours;
vector<Vec4i>hierarchy;//层次信息
//RET_TREE:绘制所有的轮廓；RET_EXTERNAL：绘制最外层轮廓
findContours(binary,contours,hierarchy,RET_TREE,CHAIN_APPROX_SIMPLE,Point());
//src,contours,索引：-1代表绘制所有，颜色，线宽，线型：-1表示填充
drawContours(src,contours,-1,Scalar(0,0,255),2,8);
imshow("contour",src);
//单独循环每个轮廓
//for(size_t t=0;t<contours.size();t++){
//  drawContours(src,contours,t,Scalar(0,0,255),2,8);
//}
```

### 计算
#### 轮廓面积
- 格林公式
$$\iint\limits_{D}{\frac{\partial Q}{\partial x}- \frac{\partial P}{\partial y}}=\int{Pdx+Qdy}$$

- 离散版本
$$A=\sum^{n}_{k=0}{\frac{(x_{k+1}+x_k)(y_{k+1}-y_k)}{2}}$$

#### 轮廓周长
- L2距离

#### 代码实现

```c++
for(size_t t=0;t<contours.size();t++){
  double area = contourArea(contours[t]);
  //是否是闭合区域
  double leng = arcLength(contours[t],true);
  if (area<20||len<10)continue;
  //获取外接矩形
  Rect box = boundingRect(contours[t]);
  //绘制外接矩形
  rectangle(src,box,)Scalar(0,0,255),2,8,0);
  //获取最小外接椭圆
  RotatedRect rrt = minAreaRect(contours[t]);
  ellipse(src,rrt,Scalar(255,0,0),2,8);
  //获取最小外接矩形
  Point2f pts[4];
  rrt.points(pts);
  for (int i=0;i<4;i++){
    line(src,pts[i],pts[(i+1)%4],Scalar(0,255,0),2,8);
  }
  //最小外接矩形的角度
  printf("angle:%.2f\n",rrt.angle);
  //绘制轮廓 -1表示填充
  drawContours(src,contours,t,Scalar(0,0,255),-1,8);
}
```

### 匹配
#### 图像几何距
- 一阶矩：p+q=1
- 二阶矩：p+q=2
- n阶距：p+q=n
$$m_{p,q}=\sum_{x=0}^{M-1}\sum_{y=0}^{N-1}{f(x,y)x^py^q}$$

- 输入&f(x,y)&是二值图像，只有0,1两种结果
- $m_{1,0}$代表x方向的累积，$m_{0,1}$代表y方向的累积，$m_{0,0}$代表像素的累积。可以描述为数学期望
- $x_{avg}=\frac{m_{1,0}}{m_{0,0}}$代表x方向的平均，$y_{avg}=\frac{m_{0,1}}{m_{0,0}}$代表y方向的平均，结合起来，就得到图像的平均中心位置

- 中心距的表示方法：
$$\mu_{p,q}=\sum_{x=0}^{M-1}\sum_{y=0}^{N-1}{f(x,y)(x-x_{avg})^p(y-y_{avg})^q}$$

#### 图像Hu距
- Hu距
$$\eta_{p,q}=\frac{\mu_{p,q}}{m_{00}^{\frac{p+q}{2}+1}}$$

- Hu距7个值性质：旋转不变形，伸缩不变性
$$\phi_1 = \eta_{20}+ \eta_{02}$$
$$phi_2 = (\eta_{20}- \eta_{02})^2+4\eta_{11}$
$$phi_3 = (\eta_{30}- 3\eta_{12})^2+(3\eta_{21}- \mu_{03})^2$$
$$phi_4 = (\eta_{30}+\eta_{12})^2+(\eta_{21}+ \mu_{03})^2$$
$$phi_5 = (\eta_{30}-3\eta_{12})(\eta_{30}+\eta_{12})[(\eta_{30}+\eta_{12})^2-3(\eta_{21}-3\eta_{03})^2]  +(3\eta_{21}-\eta_{03})(\eta_{21}+\eta_{03})[3(\eta_{30}+\eta_{12})^2-(\eta_{21}+\eta_{03})^2]$$
$$phi_6 = (\eta_{20}-\eta_{02})[(\eta_{30}+\eta_{12})^2-(\eta_{21}+\eta_{03})^2] +4\eta_{11}(\eta_{30}+\eta_{12})(\eta_{21}+\eta_{03})$$
$$phi_7 = (3\eta_{21}-\eta_{03})(\eta_{30}+\eta_{12})[(\eta_{30}+\eta_{12})^2-3(\eta_{21}+\eta_{03})^2]  +(\eta_{30}-3\eta_{12})(\eta_{21}+\eta_{03})[3(\eta_{30}+\eta_{12})^2-(\eta_{21}+\eta_{03})^2]$$

#### 基于Hu的轮廓匹配
- 两个轮廓的参数计算公式：
- 轮廓A的Hu距
$$m_i^A=sign(h_i^A)·\log{h_i^A}$$

- 轮廓B的Hu距
$$m_i^B=sign(h_i^B)·\log{h_i^B}$$

![Hu的轮廓匹配](/images/pasted-73.png)

- 代码
```c++
//获取图像的边距省略了，这里匹配src2的第一个边距
vector<vector<Point>> contours1,contours2;
Moments mm2=moments(contours2[0]);
//计算src2的hu距
Mat h2;
HuMoments(mm2,hu2);
//遍历src1的边距
for(size_t t=0;t<contours1.size();t++){
  Moments mm = moments(contours1[t]);
  Mat hu;
  HuMoments(mm,hu);
  //比较hu距，Hu距具有伸缩不变形，旋转不变形，对于旋转和放缩在dist距离上只是稍微变化一点
  double dist = matchShapes(hu,hu2,CONTOURS_MATCH_I1,0);
  if (dist<1){
    drawContours(src1,contours1,t,Scalar(0,0,255),2,8);
  }
  //绘制轮廓的中心位置
  double cx=mm.mm10/mm.mm00;
  double cy=mm.mm01/mm.mm00;
  circle(src1,Point(cx,cy),3,Scalar(255,0,0),2,8,0);
}
```

### 逼近
- 轮廓的逼近，本质是减少编码点

```c++
for(size_t t=0;t<contours1.size();t++){
  Mat res;
  //src,dst,多边形逼近的精度值,是不是闭合区域
  approxPolyDP(contours[t],result,4,true);
  prinrf("corners:%d,columns:%d\n",res.rows,res.cols);
}
```

### 拟合
- 拟合圆，生成最相似的圆或者椭圆
```c++
for(size_t t=0;t<contours1.size();t++){
  //拟合圆fitElipse，拟合线段fitLine
  RotatedRect rrt=fitElipse(contours[t]);
  float w=rrt.size.width,h=rrt.size.height;
  Porint center=rrt.center;
  circle(src,center,3,Scalar(255,0,0),2,8,0);
  elipse(src,rrt,Scalar(0,255,0),2,8);
}
```

## 点集的最小外包
## 霍夫直线检测
- 直线在平面坐标有两个参数决定（截距b,斜率k）：$y=(-\frac{\cos{\theta}}{\sin{\theta}})x+(\frac{r}{\sin{\theta}})$

- 在极坐标空间两个参数决定（半径r,角度θ）:$r=x\cos{\theta}\sin{\theta}$

- 霍夫检测的原理就是利用极坐标方程，如果边缘上的点在一条直线上，尝试使用不同的θ值，获取对应r值，那么他们肯定相交于一点。利用这个点就可以得到直线方程。

![霍夫检测](/images/pasted-74.png)

|针对二值图像||
|-|-|
|HoughLines|霍夫直线，极坐标空间参数vector(ρ,θ)|
|HoughLinesP|霍夫直线，所有可能的线段和直线|

- 代码实现
```c++
vector<Vec3f>lines;
//input,output,r步长,θ步长,阈值：超过多少次相交才算直线,多尺度检测
HoughLines(binary,lines,1,CV_PI/180,100,0,0)
//绘制检测
Point pt1,pt2;
for (size_t i=0;i<lines.size();i++){
  float rho = lines[i][0]; //距离
  float theta = lines[i][1]; //角度
  float acc = lines[i][2];  // 累加值
  //绘制所有的直线
  double a=cos(theta);
  double b=sin(theta);
  double x0=a*rho,y0=b*rho;
  pt1.x=cvRound(x0+1000*(-b));
  pt1.y=cvRound(y0+1000*(a));
  pt2.x=cvRound(x0-1000*(-b));
  pt2.y=cvRound(x0-1000*(a));
  //可以根据角度分别画出不同直线
  if(rho>0){
    line(src,pt1,pt2,Scalar(0,0,255),2,8,0);
  }else{
    line(src,pt1,pt2,Scalar(0,255,255),2,8,0);
  }
}
```

```c++
Mat src=imread("1.jpg"),binary;
Canny(src,binary,80,160,3,false);
vector<Vec4i> lines;
//input,output,r步长,θ步长,阈值,线段最小长度,最大的线段剪短距离,
HoughLinesP(binary,lines,1,CV_PI/180,100,30,10);
Mat dst=Mat::zeros(src.size(),src.type());
for(int i=0;i<line.size();i++){
  line(dst,Point(line[i][0],line[i][1]),Point(line[i][2],line[i][3]),Scalar(0,0,255),2,8);
}
imshow("hough line p",dst);
```

## 霍夫圆检测
- 圆在平面坐标由三个参数决定$(x_0,y_0,r)$
- 参数方程描述：
$$x=x_0+r\cos{\theta}$$
$$y=y_0+r\sin{\theta}$$

- 霍夫圆检测的原理是，在同一个圆线上的三个点，同时做半径为r的圆，如果相交于三点内的一个点，证明三点共圆，并且交点就是中心点。

![霍夫圆检测](/images/pasted-75.png)

- 霍夫圆检测的时候，不会遍历所有的半径，需要指定
- 候选节点会基于梯度寻找，目的为了减少未知参数

```c++
Mat src=imread("1.jpg"),gray;
cvtColor(src,gray,COLOR_BGR2GRAY);
GaussianBlur(gray,gray,Size(9,9),2,2);
vector<Vec3f>circles;
//input,output,霍夫梯度,dp,minDist,Canny边缘提取,阈值,最小半径,最大半径
int minDist=10;
double minRadius=20,maxRadius=100;
HoughCircles(gray,circles,HOUGH_GRADIENT,2,minDist,100,100,minRadius,maxRadius);
for (size_t t=0;t<>circles.size();t++){
  Point center(circles[t][0],circles[t][1]);
  int radius=round(circles[t][2]);
  //圆
  cricle(src,center,radius,Scalar(0,0,255),2,8,0);
  //圆心
  cricle(src,center,2,Scalar(0,255,255),2,8,0);
}
imshow("input",src);
```

- 霍夫检测是对噪声很敏感的检测方法，图像尽量首先进行高斯模糊进行降噪。还有很多参数属于经验参数，需要多次调整。


# 形态学
- 形态学是图像处理的单独分支学科
- 灰度与二值图像处理中重要手段
- 由数学的集合论等相关理论中发展起来的
- 在机器视觉、ORC等领域有重要作用

![形态学](/images/pasted-76.png)


## 腐蚀与膨胀
- 腐蚀：最小值滤波，用最小值代替中心像素。
![upload successful](/images/pasted-78.png)

- 膨胀：最大值滤波，用最大值代替中心像素。膨胀操作可以将两个分离区域合并
![upload successful](/images/pasted-77.png)

### 结构元素形状
- 线型：水平、垂直
- 矩形：w·h
- 十字交叉：四邻域、八邻域

### 代码实现

```c++
//腐蚀操作
//结构元素：矩形元素，大小，中心位置
Mat kernel=getStructingElement(MORPH_RECT,Size(5,5),Point(-1,-1));
erode(binary,binary,kernel);
erode(src,src,kernel);
//膨胀操作
dilate(src,src,kernel);
```

## 开闭运算
### 开运算
- 开操作=腐蚀+膨胀
- 意义：删除小的干扰块，可以用户验证码干扰的清除

![开运算](/images/pasted-79.png)

### 闭运算
- 闭操作=膨胀+腐蚀
- 意义：填充合并区域

![闭运算](/images/pasted-80.png)

### 代码实现
```c++
Mat kernel=getStructingElement(MORPH_RECT,Size(5,5),Point(-1,-1));
//input,output,形态学操作枚举,结构元素,中心位置,连续进行形态学操作
morphologyEx(binary,dst,MORPH_OPEN,kernel,Point(-1,-1),1);
```

|||
|-|-|
|MORPH_OPEN|开操作|
|MORPH_CLOSE|闭操作|
|MORPH_GRADIENT|基本梯度|
|MORPH_TOPHAT|顶帽操作|
|MORPH_BLACKHAT|黑帽操作|
|MORPH_HITMISS|击中击不中|

### 结构元素的作用
- 结构元素是矩形时，闭操作之后的空洞呈方形。可以改变结构元素枚举类型：MORPH_CIRCLE
- 交友元素的size设置为(15,1)可以只提取横线。

## 形态学梯度
- 基本梯度 – 膨胀减去腐蚀之后的结果
- 内梯度 – 原图减去腐蚀之后的结果
- 外梯度 – 膨胀减去原图的结果

### 代码实现
```c++
Mat kernel=getStructingElement(MORPH_RECT,Size(5,5),Point(-1,-1));
Mat basic_grad,inter_grad,exter_grad;
//基本梯度
morphologyEx(binary,basic_grad,MORPH_GRADIENT,kernel,Point(-1,-1),1);
Mat dst1,dst2;
//内梯度
erode(gray,dst1,kernel);
subtract(gray,dst1,inter_grad);
//外梯度
erode(gray,dst2,kernel);
subtract(dst2,gray,exter_grad);
//利用基本梯度进行二值化
threshold(basic_grad,binary,0,255,THRESH_BINARY|THRESH_OTSH);
```

## 顶帽运算和底帽运算
- 顶帽：是原图减去开操作之后的结果
- 黑帽：是闭操作之后结果减去原图
- 顶帽与黑帽的作用是用来提取图像中微小有用信息块

```c++
Mat kernel=getStructingElement(MORPH_RECT,Size(5,5),Point(-1,-1));
morphologyEx(binary,dst,MORPH_TOPHAT,kernel,Point(-1,-1),1);
```

## 击中击不中

![击中击不中](/images/pasted-81.png)

```c++
//十字交叉的结构元素
Mat kernel=getStructingElement(MORPH_CROSS,Size(5,5),Point(-1,-1));
morphologyEx(binary,dst,MORPH_HITMISS,kernel,Point(-1,-1),1);
```


# 视频分析
## 视频文件操作
- 支持视频文件读写
- 摄像头与视频流

### 读取摄像头设备

```c++
VideoCapture capture(0);
if(!capture.isOpened())return;
namedWindow("frame",WINDOW_AUTOSIZE);
Mat frame;
while (true){
  bool ret = capture.read(frame);
  if(!ret)break;
  imshow("frame");
  char c=waitKey(50);
  if(c==27)break; //esc则退出
}
```

### 读取视频文件

```c++
VideoCapture capture("1.mp4");
//获取帧率
int fps = capture.get(CAP_PROP_FPS);
//获取宽度
int width = capture.get(CAP_PROP_FRAME_WIDTH);
//获取高度
int height = capture.get(CAP_PROP_FRAME_HEIGHT);
//获取总帧数
int total_frams
 = capture.get(CAP_PROP_FRAME_COUNT);
//视频类型
int type=capture.get(CAP_PROP_FOURCC)
```
### 读取网络视频文件

```c++
VideoCapture capture("http://www.xxx.com/1.mp4");
```

### 保存一段视频

```c++
Mat frame;
VideoWriter writer("./res.mp4",type,fps,Size(width,height),true);//是否是彩色
while (true){
  bool ret = capture.read(frame);
  if(!ret)break;
  //写入
  writer.write(frame);
  imshow("frame");
  char c=waitKey(50);
  if(c==27)break; //esc则退出
}
```

- opencv对于保存的限制，文件大小最好不要超过2GB
- 最好是保存到一定大小后，再新建文件保存。

### 析构
```c++
capture.release();
writer.release();
```


# 跟踪
## 基于颜色的对象跟踪
- 基于HSV色彩空间，跟踪一个红色的物体

```c++
//打开视频文件获取frame
Mat hsv,mask;
cvtColor(frame,hsv,COLOR_BGR2HSV);
//获取红色HSV对应的色彩范围
inRange(hsv,Scalar(0,43,46),Scalar(10,255,255),mask);
//处理一下图像的缺失空洞
Mat se=getStructuringElement(MORPH_RECT,Size(15,15));
morphologyEx(mask,mask,MORPH_OPEN,se);
//获取轮廓
vector<vector<Point>>contours;
vector<Vec4i>hierarchy;
findContours(mask,contours,hierarchy,RET_EXTERNAL,CHAIN_APPROX_SIMPLE,Point());
//drawContours(frame,contours,-1,Scalar(0,0,255),2,8);
//求最大面积的外接轮廓
int index=-1;
double max_area=0;
for(size_t t=0;t<contours.size();t++){
  double area=contourArea(contours[t]);
  double len=arcLength(contours[t],true);
  //if(area<100||len<10)continue;
  //Rectbox=boundingRect(contours[t]);
  //drawContours(src,contours,t,Scalar(0,0,255),2,8);
  if(max_area<area){
    index=t;max_area=area;
  }
}
if(index>0){
  RotatedRect rrt=minAreaRect(contours[index]);
  ellipse(frame,rrt,Scalar(255,0,0),2,8);
  circle(frame,rrt.center,4,Scalar(0,255,0),2,8,0);
}
imshow("detection",frame)
```


## 基于背景分析的对象跟踪
- 视频和图片的区别是视频具有连续性，每一帧相互之间有上下文的关系。合理利用前面的信息，对未来帧走势做出建模。

- 常用的方法
  1. KNN
  2. GMM
  3. Fuzzy lntegral模糊积分

- KNN、GMM对前景和背景进行像素级别的分割(某个像素只能是背景和前景)，模糊积分会得到像素背景和前景的概率
- GMM是基于高斯和最大似然，计算马氏距离，在一定范围内，判断当前像素是否是前景。如果大于，则会新增高斯权重模型，模型就新增一个组件，当组件满了，丢弃最开始的。这样模型就动态的更新和维护。

### 代码实现

```c++
//创建建模对象
//500帧进行背景和建模,马氏距离,是否检测阴影(置为true速度会比较慢),,,,
auto pMOG2 = createBackgroundSubtractorMOG2(500,16,false);
//如果想过滤变动不大的目标，可以将马氏距离设置很大
Mat mask,bg_image;
pMOG2->apply(frame,mask);
pMOG2->getBackgroundImage(frame,mask);
```

## 光流法
- 光流可以看成是图像结构光的变化或者图像亮度模式明显的移动
- 光流法分为：稀疏光流KLT与稠密光流
- 基于相邻视频帧进行分析
- 光流对光照变化很敏感，一般需要进行模糊

### 稀疏光流KLT
- 假设条件：1、亮度恒定；2、近距离移动；3、空间一致；
- 亮度恒定：假设x,y像素点移动了u,v，那么两个像素值应该相等
$$I(x,y,t)=I(x+u,y+v,t+1)$$

- 空间一致：假设窗口大小为5*5个像素点，基于亮度恒定，移动u,v
$$O=I_t(p_i)+\nabla I(p_i)·\begin{bmatrix}u  &v\end{bmatrix}$$

- 根据空间一致性，求取移动的u,v，可以使用最小二乘法求d
$$Ad=b$$
$$(A^TA)d=A^Tb$$
$$A=\begin{bmatrix}I_x(p_1)  &I_y(p_1) \\...  &... \\I_x(p_{25})  &I_y(p_{25}) \end{bmatrix}$$
$$b=\begin{bmatrix}I_x(p_1)   \\... \\ I_x(p_{25})    \end{bmatrix}$$
$$d=\begin{bmatrix}u    \\ v    \end{bmatrix}$$
$$M=A^TA=\begin{bmatrix}\sum{I_xI_x} &\sum{I_xI_y}  \\\sum{I_xI_y}  &\sum{I_yI_y} \end{bmatrix}$$
$A^Tb=\begin{bmatrix}\sum{I_xI_t}    \\ \sum{I_yI_t}    \end{bmatrix}$$

- M就是Harris角点检测的方程，也就是满足空间一致性的只有角点才可以
- 使用角点检测获取到移动位置距离

#### 代码实现

```c++
Mat gray,old_gray;
vector<Point2f>corners;
double quality_level=0.01;
int minDistance = 5;
goodFeaturesToTrack(gray,corners,200,quality_level,minDistance,Mat(),3,false);
vector<Point2f>pts[2];
pts[0].insert(pts[0].end(),corners.begin(),corners.end());
vector<unchar>status;
vector<float>err;
//重复迭代10，两次结果小于0.01就不再计算了
TermCriteria cireria=TermCriteria(TermCriteria::COUNT+TermCriteria::EPS,10,0.01);
while (true){
  //稀疏光流
  //前一帧,当前帧,前一帧角点,当前帧角点,每个角点状态(只有ok才会检测),错误,光流场的大小,金字塔层数,停止条件,flag
  calcOpticalFlowPyrLK(old_gray,gray,pts[0],pts[1],status,err,Size(21,21),3,cireria,0);
  size_t i=0,k=0;
  for(i=0;i<pts[1].size();i++){
    if(status[i]){
      //将有效的往前排
      pts[0][k]=pts[0][i];
      pts[1][k++]=pts[1][i];
      circle(frame,pts[1][i],2,Scalar(0,0,255),2,8,0);
      line(frame,pts[0][i],pts[1][i],Scalar(0,255,255),2,8,0);
    }
  }
  //update key points
  pts[0].resize(k);//把k后面全扔了
  pts[1].resize(k);//把k后面全扔了
  imshow("KLT",frame);
  //update frame
  std::swap(pts[1],pts[0]);
  cv::swap(old_gray,gray);
}
```

- 上面代码仅检测是否有效，会显示大量的点。下面增加移动距离的限制，使得跟踪效果更好一些
- 加上距离判断的话，有些角点就会被抛弃，需要初始化才能在下一次检测时继续做判断

```c++
Mat gray,old_gray;
vector<Point2f>corners;
double quality_level=0.01;
int minDistance = 5;
goodFeaturesToTrack(gray,corners,200,quality_level,minDistance,Mat(),3,false);
vector<Point2f>pts[2];
vector<Point2f>initPoints;
pts[0].insert(pts[0].end(),corners.begin(),corners.end());
initPoints.insert(initPoints.end(),corners.begin(),corners.end());
vector<unchar>status;
vector<float>err;
TermCriteria cireria=TermCriteria(TermCriteria::COUNT+TermCriteria::EPS,10,0.01);
while(true){
  calcOpticalFlowPyrLK(old_gray,gray,pts[0],pts[1],status,err,Size(21,21),3,cireria,0);
  size_t i=0,k=0;
  for(i=0;i<pts[1].size();i++){
    double dist = abs(pts[0].x-pts[1].x)+abs(pts[0].y-pts[1].y);
    if(status[i] && dist>2){
      //将有效的往前排
      initPoints[k]=initPoints[i];
      pts[0][k]=pts[0][i];
      pts[1][k++]=pts[1][i];
      circle(frame,pts[1][i],2,Scalar(0,0,255),2,8,0);
      line(frame,pts[0][i],pts[1][i],Scalar(0,255,255),2,8,0);
    }
  }
  //update key points
  pts[0].resize(k);//把k后面全扔了
  pts[1].resize(k);//把k后面全扔了
  initPoints.resize(k);//把k后面全扔了
  //绘制光流线
  //线的彩色
  vector<Scalar> lut;
  for (size_t t=0;t<initPoints.size();t++){
    int b=rng.uniform(0,255),g=rng.uniform(0,255),r=rng.uniform(0,255);
    lut.push_back(Scalar(b,g,r));
  }
  for (size_t t=0;t<initPoints.size();t++){
    line(frame,initPoints[t],pts[1][t],lut[t],2,8,0);
  }
  imshow("KLT",frame);
  //update frame
  std::swap(pts[1],pts[0]);
  cv::swap(old_gray,gray);
  //re-init
  if (pts[0].size()<40){
    goodFeaturesToTrack(gray,corners,200,quality_level,minDistance,Mat(),3,false);
    pts[0].insert(pts[0].end(),corners.begin(),corners.end());
  }
}
```

### 稠密光流
- 稠密光流法有很多种

### 基于多项式的稠密光流法
- 像素点跟周围的像素点是相关性的
- 基于二次多项式实现光流评估算法
- 对每个像素点都计算移动距离
- 基于总位移与均值评估

- 数学描述，x是一个2*2的矩阵，代表(x,y)
$$f_1(x)=x^TA_1x+b_1^Tx+c_1$$

- 移动d后
$$f_2(x)=f_1(x-d)=(x-d)^TA_1(x-d)+b_1^T(x-d)+c_1=x^TA_2x+b^T_2x+c_2$$
$$A_2=A_1$$
$$b_2=b_1-2A_1d$$
$$c2=d^TA_1d-b^T_1d+c_1$$

- 可以看到系数A不变，b和c的变化和d相关。求取d转成了求取系数
- 假定ROI窗口共用相同的系数，组成多约束方程求解，用最小二乘拟合，得到系数结果求解d，全局总位移。根据求解的d，又可以反推系数，不断迭代循环，直至系数足够精准。得到光流场

### 代码实现
- 笛卡尔坐标系转成极坐标系，使用HSV色彩空间进行展现出稠密光流场

```c++
Mat frame,preFrame,gray,preGray;
capture.read(preFrame);
cvtColor(preFrame,preGray,COLOR_BGR2GRAY);
Mat hsv=Mat::zeros(preFrame.size(),preFrame.type());
Mat mag=Mat::zeros(hsv.size(),CV_32FC1);
//角度
Mat ang=Mat::zeros(hsv.size(),CV_32FC1);
//x
Mat xpts=Mat::zeros(hsv.size(),CV_32FC1);
//y
Mat ypts=Mat::zeros(hsv.size(),CV_32FC1);
//光流输出
Mat_<Point2f>flow;
vector<Point2f>mv;
split(hsv,mv);
Mat dst;
while(true){
  cvtColor(frame,gray,COLOR_BGR2GRAY);
  //preframe,curframe,flow,金字塔层间的scale,金字塔层数,窗口大小,迭代次数,多项式,高斯配置权重,flag
  calcOpticalFlowFarneback(preGray,gray,flow,0.5,3,15,3,5,1.2,0);
  for(int row=0;row<flow.rows;row++){
    for(int col=0;col<flow.cols;col++){
      const Point2f &flow_xy=flow.at<Point2f>(row,col);
      xpts.at<float>(row,col)=flow_xy.x;
      ypts.at<float>(row,col)=flow_xy.y;
    }
  }
  //变为极坐标
  cartToPolar(xpts,ypts,mag,ang);
  ang=ang*180/CV_PI/2.0;
  normalize(mag,mag,0,255,NORM_MINMAX);
  convertScaleAbs(mag,mag);
  convertScaleAbs(ang,ang);
  mv[0]=ang;
  mv[1]=Scalar(255);
  mv[2]=mag;
  merge(mv,hsv);
  cvtColor(hsv,dst,COLOR_HSV2BRGR);
  imshow("dst",dst);
  imshow("frame",frame);
}
```

## 基于均值迁移的视频分析

### 均值迁移算法原理
- 随机在空间中画圆，计算包含在圆中的点的均值$\bar{x}\bar{y}$，计算圆的中心位置和均值x,y的位置距离，圆不断向均值移动，直到稳定位置，就到了目标。

### 均值迁移-MeanShift
- 基于直方图反向投影，提供ROI区域

### 自适应均值迁移-CA-MeanShift
- 在均值迁移的基础上，每次都重新拟合椭圆，自适应变化窗口大小。


### 代码实现
```c++
Mat frame,hsv,hue,mask,hist,backproj;
capture.read(frame);
int hsize=16;
float hranges[]={0,180};
Rect trackWindow;
bool init=true;
const float* ranges=hranges;
Rect selection=selectROI("window name",frame,true,false);
while(true){
  capture.read(frame);
  cvtColor(frame,hsv,COLOR_BGR2HSV);
  //选取黄色作为跟踪目标
  inRange(hsv,Scalar(26,43,46),Scalar(34,255,255),mask);
  //这里只搜索H通道，加快速度
  int ch[]={0,0};
  hue.create(hsv.size(),hsv.depth());
  //只取出1个通道：H
  //src,1,dst,
  mixChannels(&hsv,1,&hue,1,ch,1);
  if(init){
    //计算ROI的直方图
    init=false;
    //只计算H通道的
    Mat roi(hue,selection),maskroi(mask,selection);
    calcHist(&roi,1,0,maskroi,hist,1,&hsize,&ranges);
    normalize(hist,hist,0,255,NORM_MINMAX);
    trackWindow=selection;
  }
  //ht通道，通道数，0，hist，dst，ranges
  calcBackProject(&hue,1,0,hist,backproj,&ranges);
  //meanshift：
  //将backproj中不在mask部分去除
  backproj &= mask;
  meanShift(backproj,trackWindow,TermCriteria(TermCriteria::COUNT|
  //连续自适应均值偏移
  //RotatedRect rrt=CamShift(backproj,trackWindow,TermCriteria(TermCriteria::COUNT| TermCriteria::EPS),10,1);
  //ellipse(frame,rrt,Scalar(0,0,255),2,8);
  rectangle(frame,trackWindow,Scalar(0,0,255),3,LINE_AA);
  imshow("meanshift",frame);
}
```





