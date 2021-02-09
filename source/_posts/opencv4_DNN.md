---
title: opencv4神经网络模块
author: hero576
tags:
  - robotic
categories:
  - opencv
date: 2021-01-12 22:19:03
---
> 
<!--more-->

# 简介
## 介绍
- opencv4对深度神经网络提供了支持，使得opencv4对图像处理的能力大大拓展。[官方介绍](https://github.com/opencv/opencv/wiki/Deep-Learning-in-OpenCV)


# 网络模型
## YOLOV3
- [地址](https://pjreddie.com/darknet/yolo)
- 下载网络权重文件，网络配置yml文件+

# 使用

功能|说明
-|-
引入|`#include<opencv2/dnn.hpp>`
命名空间|`using namespace cv::dnn;`
初始化模型|`Net net = readNetFromCaffe(model_prototxt, model_caffe);`
设置计算后台|`net.setPreferableBackend(DNN_BACKEND_OPENCV);`
设置计算设备|`net.setPreferableTarget(DNN_TARGET_CUDA);`
打印模型结构|`vector<string> layer_names = net.getLayerNames();`<br>`for (auto i : layer_names) {`<br>`int id = net.getLayerId(i);`<br>`auto layer = net.getLayer(id);`<br>`//cout<<id<<layer << i << endl;`<br>`printf("layerid:%d,type:%s,name:%s\n", id,layer->type.c_str(), layer->name.c_str());`<br>`}`
定义测试图像|`Mat inputBlob = blobFromImage(src,1.,Size(w,h),Scalar(117., 117., 117.),true,false);`
计算输出|`Mat probMat = net.forward();`可以指定任意一层的输出<br>`Mat prob = probMat.reshape(1, 1);`<br>`Point classNum;double classProb;`<br>`minMaxLoc(prob, NULL, &classProb, NULL, &classNum);`


# 代码

- 需要准备训练好的模型权重文件，模型配置文件，配置文件中定义了模型结构、图像输入后的图像尺寸，压缩比例，均值等信息。[官方](https://github.com/opencv/opencv/blob/master/samples/dnn/models.yml)也给定了一些samples供使用。

## ssd


```cpp
#include <iostream>
#include<opencv2/dnn.hpp>
#include <opencv.hpp>
#include <fstream>
using namespace std;
using namespace cv;
using namespace cv::dnn;
String objNames[] = { "background","aeroplane","bicycle" ,"bird" ,"boat" ,"bottle" ,"bus" ,"car" ,"cat" ,
"chair" ,"cow" ,"diningtable" ,"dog" ,"horse" ,"motorbike" ,"person" ,"pottedplant" ,"sheep" ,"sofa" ,
"train" ,"tvmoitor" };
int main() {
    string model_path = R"(model\ssd\)";
    string model_cfg_file = model_path + "MobileNetSSD_deploy.caffemodel";
    string model_pro_file = model_path + "MobileNetSSD_deploy.prototxt";
    string model_label_file = model_path + "labelmap_det.txt";
    Net net = readNetFromCaffe(model_pro_file, model_cfg_file);
    net.setPreferableBackend(DNN_BACKEND_OPENCV);
    //net.setPreferableTarget(DNN_TARGET_CUDA);
    vector<string> layer_names = net.getLayerNames();
    for (auto i : layer_names) {
        int id = net.getLayerId(i);
        auto layer = net.getLayer(id);
        //cout<<id<<layer << i << endl;
        //printf("layerid:%d,type:%s,name:%s\n", id, layer->type.c_str(), layer->name.c_str());
    }
    Mat src = imread(R"(Pictures\1.jpg)");
    imshow("input", src);
    Mat blob = blobFromImage(src, 0.007843, Size(300, 300), Scalar(127.5, 127.5, 127.5), false, false);
    net.setInput(blob, "data");
    Mat detection = net.forward("detection_out");
    Mat detectionMat(detection.size[2], detection.size[3], CV_32F, detection.ptr<float>());
    float confidence_trhreshold = 0.5;
    for (int i = 0; i < detectionMat.rows; i++) {
        float score = detectionMat.at<float>(i, 2);
        if (score > confidence_trhreshold) {
            size_t objIndex = (size_t)(detectionMat.at<float>(i, 1));
            float tl_x = detectionMat.at<float>(i, 3) * src.cols;
            float tl_y = detectionMat.at<float>(i, 4) * src.rows;
            float br_x = detectionMat.at<float>(i, 5) * src.cols;
            float br_y = detectionMat.at<float>(i, 6) * src.rows;
            Rect box((int)tl_x, (int)tl_y, (int)(br_x - tl_x), (int)(br_y - tl_y));
            rectangle(src, box, Scalar(0, 0, 255), 2, 8, 0);
            putText(src, format("score:%.2f,index:%s",score, objNames[objIndex].c_str()), box.tl(), FONT_HERSHEY_PLAIN, 0.75, Scalar(255, 0, 25), 1, 8);
        }
    }
    imshow("res", src);
    waitKey(0);
    return 0;
}
```
