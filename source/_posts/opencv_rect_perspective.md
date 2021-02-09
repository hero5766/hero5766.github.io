---
title: opencv透视变换
author: hero576
tags:
  - robotic
categories:
  - opencv
date: 2021-2-1 21:41:15
---
> 
<!-- more -->

# 输入
- 待识别图像
![](/images/pasted-387.jpg)


# 代码
```py
import cv2
import numpy as np
import matplotlib.pyplot as plt
# 设置参数
def order_points(pts):
    # 一共4个坐标点
    rect = np.zeros((4, 2), dtype = "float32")
    # 按顺序找到对应坐标0123分别是 左上，右上，右下，左下
    # 计算左上，右下
    s = pts.sum(axis = 1)
    rect[0] = pts[np.argmin(s)]
    rect[2] = pts[np.argmax(s)]
    # 计算右上和左下
    diff = np.diff(pts, axis = 1)
    rect[1] = pts[np.argmin(diff)]
    rect[3] = pts[np.argmax(diff)]
    return rect
def four_point_transform(image, pts):
    # 获取输入坐标点
    rect = order_points(pts)
    (tl, tr, br, bl) = rect
    # 计算输入的w和h值
    widthA = np.sqrt(((br[0] - bl[0]) ** 2) + ((br[1] - bl[1]) ** 2))
    widthB = np.sqrt(((tr[0] - tl[0]) ** 2) + ((tr[1] - tl[1]) ** 2))
    maxWidth = max(int(widthA), int(widthB))

    heightA = np.sqrt(((tr[0] - br[0]) ** 2) + ((tr[1] - br[1]) ** 2))
    heightB = np.sqrt(((tl[0] - bl[0]) ** 2) + ((tl[1] - bl[1]) ** 2))
    maxHeight = max(int(heightA), int(heightB))
    # 变换后对应坐标位置
    dst = np.array([
        [0, 0],
        [maxWidth - 1, 0],
        [maxWidth - 1, maxHeight - 1],
        [0, maxHeight - 1]], dtype = "float32")
    # 计算变换矩阵
    M = cv2.getPerspectiveTransform(rect, dst)
    warped = cv2.warpPerspective(image, M, (maxWidth, maxHeight))
    # 返回变换后结果
    return warped
def resize(image, width=None, height=None, inter=cv2.INTER_AREA):
    dim = None
    (h, w) = image.shape[:2]
    if width is None and height is None:
        return image
    if width is None:
        r = height / float(h)
        dim = (int(w * r), height)
    else:
        r = width / float(w)
        dim = (width, int(h * r))
    resized = cv2.resize(image, dim, interpolation=inter)
    return resized
# 读取输入
image = cv2.imread("images/ocr/receipt.jpg")
#坐标也会相同变化
ratio = image.shape[0] / 500.0
orig = image.copy()
image = resize(orig, height = 500)
plt.imshow(image)
# 预处理
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
gray = cv2.GaussianBlur(gray, (5, 5), 0)
edged = cv2.Canny(gray, 75, 200)
# 展示预处理结果
plt.imshow(edged)
# 轮廓检测
cnts = cv2.findContours(edged.copy(), cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)[0]
cnts = sorted(cnts, key = cv2.contourArea, reverse = True)[:5]
# 遍历轮廓
for c in cnts:
    # 计算轮廓近似
    peri = cv2.arcLength(c, True)
    # C表示输入的点集
    # epsilon表示从原始轮廓到近似轮廓的最大距离，它是一个准确度参数
    # True表示封闭的
    approx = cv2.approxPolyDP(c, 0.02 * peri, True)
    # 4个点的时候就拿出来
    if len(approx) == 4:
        screenCnt = approx
        break
# 展示结果
cv2.drawContours(image, [screenCnt], -1, (0, 255, 0), 2)
plt.imshow( image)
# 透视变换
warped = four_point_transform(orig, screenCnt.reshape(4, 2) * ratio)
# 二值处理
warped = cv2.cvtColor(warped, cv2.COLOR_BGR2GRAY)
ref = cv2.threshold(warped, 100, 255, cv2.THRESH_BINARY)[1]
plt.imshow(ref)
# 展示结果
plt.imshow(resize(orig, height = 650))
```
- 识别结果
![](/images/pasted-388.jpg)
