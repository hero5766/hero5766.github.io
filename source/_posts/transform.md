---
title: 坐标系转换
author: hero576
tags:
  - algorithm
categories:
  - math
date: 2021-05-15 11:27:00
---
# 全局坐标与局部坐标相互转换[^1]
[^1]: [连接](https://blog.csdn.net/weixin_39602967/article/details/110935396)

```py
def transformGlobalToLocal(x,y,psi,ptsx,ptsy):
    x_pos = []
    y_pos = []
    assert len(ptsx)==len(ptsy),"error"
    for i in range(len(ptsx)):
        x_pos.append(np.cos(psi) * (ptsx[i] - x) + np.sin(psi) * (ptsy[i] - y))
        y_pos.append(-np.sin(psi) * (ptsx[i] - x) + np.cos(psi) * (ptsy[i] - y))
    return x_pos,y_pos
def transformLocalToGlobal(x,y,psi,x_pos,y_pos):
    ptsx = []
    ptsy = []
    assert len(x_pos)==len(y_pos),"dim error"
    for i in range(len(x_pos)):
        ptsx.append(np.cos(psi) * x_pos[i] + -np.sin(psi) * y_pos[i] + x)
        ptsy.append(np.sin(psi) * x_pos[i] + np.cos(psi) * y_pos[i] + y)
    return ptsx,ptsy

def main():
    ptsx = [-61.09,-78.29172,-93.05002,-107.7717,-123.3917,-134.97]
    ptsy = [92.88499,78.73102,65.34102,50.57938,33.37102,18.404]
    psi_unity = 3.837348
    psi = 4.016634
    x = -74.76527
    y = 84.16312
    steering_angle = -0.1665902
    throttle = 1
    speed = 20.35116
    next_x = [-15.4600169051846,6.42984847749198,26.1671664043421,46.9338419093793,70.1543380846207,89.0638601705846]
    next_y = [4.90631258290863,0.774989399278558,-1.97051600855438,-3.80873960226945,-4.76822449803267,-4.06204247415754]
    x_pos,y_pos = transformGlobalToLocal(x,y,psi,ptsx,ptsy)
    transformLocalToGlobal(x,y,psi,x_pos,y_pos)
```
