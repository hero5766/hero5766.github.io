title: opencv
author: hero576
tags: []
categories: []
date: 2020-07-22 19:17:00
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
- 添加include库文件目录；
- 添加lib动态链接库目录；
- 添加附加依赖性，lib中的*.lib文件，在调试时使用带_d的；
- 添加opencv的dll文件到系统路径；

- 测试是否成功
```c
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

## 视频操作

# 色彩空间
## 常见色彩空间
### RGB
### HSV
### HLS
## 彩色图像的饱和度和亮度

# 卷积和傅里叶变换
## 卷积
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
```c
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
```c
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

```c
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

```c
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
```c
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
```c
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

```c
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
```c
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

```c
//分离
vector<Mat> rgbCh(3);
split(src, rgbCh);
Mat blue=rgbCh[0],green=rgbCh[1],red=rgbCh[2];
//合并
Mat dst;vector<Mat> merge_src;
merge_src.push_back(blue);merge_src.push_back(green);merge_src.push_back(red); //根据bgr的顺序
merge(merge_src,dst);
```

```c
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
```c
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

```c
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
- 直方图均衡化后的图像会有比较好的显示效果，举例：64*64大小的图像，8个bin，像素总数4096

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

```c
Mat src = imread("./1.jpg",IMREAD_GRAYSCALE),gray,dst;;
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

```c
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


```c
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
## 限制对比度的自适应直方图均值化



# 频率域滤波
## 低通滤波和高通滤波
## 带通和带阻滤波
## 自定义滤波
## 同态滤波

# 图像平滑
## 卷积
### 数学描述
- 连续定义
$$(f*g)(n)=\int_{\infty }^{-\infty} f(\tau )g(n-\tau)d\tau$$

- 离散定义
$$(f*g)(n)=\sum_{\tau=-\infty }^{\infty} f(\tau )g(n-\tau)$$

### 二维离散卷积

## 高斯平滑
## 均值平滑
## 中值平滑
## 双边滤波
## 联合双边滤波
## 导向滤波

# 图像模糊
## 锐化
## 噪声

# 阈值分割
## 方法概述
### 全局阈值分割
### 阈值函数
### 局部阈值分割
## 直方图
## 熵算法
## Otsu阈值
## 自适应阈值

# 形态学
## 腐蚀
## 膨胀
## 开闭运算
## 顶帽运算和底帽运算
## 形态学梯度

# 边缘检测
## 梯度
## Roberts算则
## PRewitt边缘检测
## Sobel边缘检测
## Scharr算子
## Kirsch算子和Robinson算子
## Canny边缘检测
## Laplacian算子
## 高斯拉普拉斯LoG边缘检测
## 高斯差分DoG边缘检测
## Marr-Hildreth边缘检测
## Harris角点检测
## shi-tomas角点检测

# 几何形状检测和拟合
## 点集的最小外包
## 霍夫直线检测
## 霍夫圆检测
## 轮廓
### 发现
### 计算
### 拟合
### 匹配
## 联通组件扫描

# 跟踪
## 基于颜色的对象跟踪

# 背景分析
## 光流法
