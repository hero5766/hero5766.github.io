---
title: opencv检测银行卡号
author: hero576
tags:
  - robotic
categories:
  - opencv
date: 2021-2-8 21:41:15
---
> 
<!-- more -->

# 输入
- 模板
![](/images/pasted-384.png)
- 待识别图像
![](/images/pasted-385.png)

# 代码
```py
import cv2
import numpy as np
import matplotlib.pyplot as plt
img_file = "credit_card.png"
tem_file = "reference.png"
img = cv2.imread(img_file)
tem = cv2.imread(tem_file)
def tem_process(img):
    img_cpy = img.copy()
    gray = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
    ret,binary = cv2.threshold(gray,10,255,cv2.THRESH_BINARY_INV)
    contours,hierarchy = cv2.findContours(binary.copy(),cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
    #cv2.drawContours(img_cpy,contours,-1,(255,0,0),3)
    #plt.imshow(img_cpy)
    temp_index_list = {}
    digits_template = {}
    for i,contour in enumerate(contours):
        x,y,w,h=cv2.boundingRect(contour)
        temp_index_list[x] = {'i':i,'pos':(x,y,w,h)}
    keys = list(temp_index_list.keys())
    keys.sort()
    for i,k in enumerate(keys):
        contour = temp_index_list[k].get("pos")
        #cv2.putText(img_cpy,"{}".format(i),temp_index_list[k].get("pos")[:2],cv2.FONT_HERSHEY_COMPLEX,0.75,(0,0,242))
        x,y,w,h = contour
        digits_template[i] = cv2.resize(binary[y:y+h,x:x+w],(57,88))
    #plt.imshow(img_cpy)
    return digits_template
digits = tem_process(tem)    
# 初始化卷积核
rectKernel = cv2.getStructuringElement(cv2.MORPH_RECT, (9, 3))
sqKernel = cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5))
#读取输入图像，预处理
image = cv2.resize(img,(300,int(img.shape[0]*(300/img.shape[1])),))
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
#礼帽操作，突出更明亮的区域
tophat = cv2.morphologyEx(gray, cv2.MORPH_TOPHAT, rectKernel) 
#ksize=-1相当于用3*3的
gradX = cv2.Sobel(tophat, ddepth=cv2.CV_32F, dx=1, dy=0,ksize=-1)
gradX = np.absolute(gradX)
(minVal, maxVal) = (np.min(gradX), np.max(gradX))
gradX = (255 * ((gradX - minVal) / (maxVal - minVal)))
gradX = gradX.astype("uint8")
#通过闭操作（先膨胀，再腐蚀）将数字连在一起
gradX = cv2.morphologyEx(gradX, cv2.MORPH_CLOSE, rectKernel) 
#THRESH_OTSU会自动寻找合适的阈值，适合双峰，需把阈值参数设置为0
thresh = cv2.threshold(gradX, 0, 255, cv2.THRESH_BINARY | cv2.THRESH_OTSU)[1] 
#再来一个闭操作
thresh = cv2.morphologyEx(thresh, cv2.MORPH_CLOSE, sqKernel) #再来一个闭操作
# 计算轮廓
threshCnts, hierarchy = cv2.findContours(thresh.copy(), cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
cnts = threshCnts
cur_img = image.copy()
cv2.drawContours(cur_img,cnts,-1,(0,0,255),3) 
locs = []
concour_img = image.copy()
# 遍历轮廓
for (i, c) in enumerate(cnts):
    # 计算矩形
    (x, y, w, h) = cv2.boundingRect(c)
    ar = w / float(h)
    # 选择合适的区域，根据实际任务来，这里的基本都是四个数字一组
    if ar > 2.5 and ar < 4.0:
        if (w > 40 and w < 55) and (h > 10 and h < 20):
            #符合的留下来
            locs.append((x, y, w, h))
            cv2.rectangle(concour_img,(x,y),(x+w,y+h),(255,0,0))
# 将符合的轮廓从左到右排序
locs = sorted(locs, key=lambda x:x[0])
output = []
# 遍历每一个轮廓中的数字
for (i, (gX, gY, gW, gH)) in enumerate(locs):
    # initialize the list of group digits
    groupOutput = []
    # 根据坐标提取每一个组
    group = gray[gY - 5:gY + gH + 5, gX - 5:gX + gW + 5]
    #cv_show('group',group)
    # 预处理
    group = cv2.threshold(group, 0, 255,
        cv2.THRESH_BINARY | cv2.THRESH_OTSU)[1]
    #cv_show('group',group)
    # 计算每一组的轮廓
    digitCnts,hierarchy = cv2.findContours(group.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    digits_x=[]
    for c in digitCnts:
        (x, y, w, h) = cv2.boundingRect(c)
        digits_x.append(x)
    indexs = np.argsort(digits_x)
    c_digits = [digitCnts[c] for c in indexs]
    
    # 计算每一组中的每一个数值
    for c in c_digits:
        # 找到当前数值的轮廓，resize成合适的的大小
        (x, y, w, h) = cv2.boundingRect(c)
        roi = group[y:y + h, x:x + w]
        roi = cv2.resize(roi, (57, 88))
        #cv_show('roi',roi)

        # 计算匹配得分
        scores = []
        # 在模板中计算每一个得分
        for (digit, digitROI) in digits.items():
            # 模板匹配
            result = cv2.matchTemplate(roi, digitROI,
                cv2.TM_CCOEFF)
            (_, score, _, _) = cv2.minMaxLoc(result)
            scores.append(score)
        # 得到最合适的数字
        groupOutput.append(str(np.argmax(scores)))
    # 画出来
    cv2.rectangle(image, (gX - 5, gY - 5),
        (gX + gW + 5, gY + gH + 5), (0, 0, 255), 1)
    cv2.putText(image, "".join(groupOutput), (gX, gY - 15),
        cv2.FONT_HERSHEY_SIMPLEX, 0.65, (0, 0, 255), 2)
    # 得到结果
    output.extend(groupOutput)
# 打印结果
#print("Credit Card Type: {}".format(FIRST_NUMBER[output[0]]))
print("Credit Card #: {}".format("".join(output)))
plt.imshow(image)
```
- 识别结果
![](/images/pasted-386.png)
