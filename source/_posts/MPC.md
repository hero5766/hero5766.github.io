---
title: MPC原理
author: hero576
tags: 
  - algorithm
categories:
  - control
date: 2021-05-20 10:10:00
---


![数学模型](/images/pasted-395.png)
![阿克曼转向角](/images/pasted-396.png)

# MPC简介
## 介绍


# 车辆运动学

## 数学模型
![]()
- 只考虑车辆在二维平面(`xoy`)上运动
- 状态量(states)：`x`,`y`,`$\phi$`
- 控制量(control)：`$v_r$`,`$\delta_f$`。即：脚踏板(pedals)，转向(st+eering wheel)
  - rear,front

## 状态空间[^1]
[^1]: [博客](https://blog.csdn.net/tingfenghanlei/article/details/85046120)，[古月居](https://www.guyuehome.com/33800)，[视频教程](https://www.bilibili.com/video/BV1HQ4y1P7bJ?p=4&spm_id_from=pageDriver)

![阿克曼转向]()
- 前轮左右两个轮的转角有一个差值，叫做阿克曼转向，可以保证转向外侧和内侧走过的圆弧圆心，在后轮两中心延长线的一个点上。此时模型可以简化为自行车的两轮模型。
- `$v_f\cos{\delta_f}=v_r$`：前轮速度与后轮速度在径向上的约束
- `$v_f\sin{\delta_f}=v_y$`：横向速度
- `$v_y=v_r\tan{\delta_f}$`
- `$\varphi=\frac{v_r\tan{\delta_f}}{l}$`
  - `$\omega=\frac{v_y}{l}$`：角速度的线速度/半径

- 状态空间：`$\xi=\begin{bmatrix}X\\Y\\\varphi\end{bmatrix}$`

- 控制空间：`$u=\begin{bmatrix}v_r\\\delta_f\end{bmatrix}$`

- 非线性


## 线性化
- 将变量相乘的非线性关系，转化为线性关系。直接转换做不到，可以使用参考值与实际值的差值来计算

- 由前面已知关系：`$\begin{cases}\dot{X}=v_r\cos{\varphi}=f_1\\\dot{Y}=v_r\sin{\varphi}=f_2\\\dot{\varphi}=\frac{v_r\tan{\delta_f}}{l }=f_3\\\end{cases}$`
  - `$\dot{X}$`表示大地坐标系下瞬时的水平坐标变化，也就是导数

- 假设状态向量符合函数关系：`$\dot{\xi}=f(\xi,u)$`

- 根据近似线性化(相对应的是反馈线性化，缺点是通用性不好，不同模型都要重新推导)，在参考点(`$\xi_r$`)处利用泰勒展开
 > - 泰勒公式`$f(x)=\frac{f(x_0)}{0!}+\frac{f'(x_0)}{1!}(x-x_0)+\dots+\frac{f^{(n)}(x_0)}{n!}(x-x_0)^n+R_0(x)$`
 > - 对于一阶泰勒展开，需要计算导数，在向量空间求导，可以求解雅克比矩阵
 > - `$J_f(x_1,\dots,x_n)=\begin{bmatrix}\frac{\partial{y_1}}{\partial{x_1}}&&\dots&&\frac{\partial{y_1}}{\partial{x_n}}\\\vdots&&\ddots&&\vdots\\\frac{\partial{y_m}}{\partial{x_1}}&&\dots&&\frac{\partial{y_m}}{\partial{x_n}}\end{bmatrix}$`

- `$\dot{\xi}=f(\xi,u)\approx f(\xi_r,u_r)+J_x(\xi-\xi_r)+J_u(u-u_r)= f(\xi_r,u_r)+\frac{\partial f}{\partial \xi}(\xi-\xi_r)+\frac{\partial f}{\partial u}(u-u_r)$`

- 用`$\tilde{\xi}=\xi-\xi_r$`表示误差
- 一阶泰勒展开与参考状态相减`$\dot{\xi_r}=f(\xi_r,u_r)$`，得到两个雅克比矩阵

- `$\dot{\tilde{\xi}}=\dot{\xi}-\dot{\xi_r}=\frac{\partial f}{\partial \xi}\tilde{\xi}+\frac{\partial f}{\partial u}\tilde{u}=A\tilde{\xi}+B\tilde{u}$`，此时状态和控制表示为了线性关系
  - 其中：`$\tilde{\xi}=\begin{bmatrix}x-x_r\\y-y_r\\\varphi-\varphi_r\end{bmatrix}$`
  - `$\tilde{u}=\begin{bmatrix}v_r\\\delta_f\end{bmatrix}$`
  - 利用雅克比矩阵，求A：`$A=\frac{\partial f}{\partial \xi}=\begin{bmatrix}\frac{\partial{f_1}}{\partial{\xi_1}}&&\frac{\partial{f_1}}{\partial{\xi_2}}&&\frac{\partial{f_1}}{\partial{\xi_3}}\\\frac{\partial{f_2}}{\partial{\xi_1}}&&\frac{\partial{f_2}}{\partial{\xi_2}}&&\frac{\partial{f_2}}{\partial{\xi_3}}\\\frac{\partial{f_3}}{\partial{\xi_1}}&&\frac{\partial{f_3}}{\partial{\xi_2}}&&\frac{\partial{f_3}}{\partial{\xi_3}}\end{bmatrix}=\begin{bmatrix}0&&0&&-v_r\sin{\varphi_r}\\0&&0&&v_r\sin{\varphi_r}\\0&&0&&0\end{bmatrix}$`
  - `$B=\frac{\partial f}{\partial u}=\begin{bmatrix}\frac{\partial{f_1}}{\partial{u_1}}&&\frac{\partial{f_1}}{\partial{u_2}}\\\frac{\partial{f_2}}{\partial{u_1}}&&\frac{\partial{f_2}}{\partial{u_2}}\\\frac{\partial{f_3}}{\partial{u_1}}&&\frac{\partial{f_3}}{\partial{u_2}}\end{bmatrix}=\begin{bmatrix}\cos{\varphi_r}&&0\\\sin{\varphi_r}&&0\\\frac{\tan\delta_f}{l}&&\frac{v_r}{l\cos^2\delta_f}\end{bmatrix}$`


## 离散化
- 使用前向欧拉(Forward-Euler)做近似化
- `$\dot{\tilde{\xi}}=\frac{\tilde{\xi}(k+1)-\tilde{\xi}(k)}{T}=A\tilde{\xi}(k)+B\tilde{u}(k)$`

- `$\tilde{\xi}(k+1)=(I+TA)\tilde{\xi}(k)+(TB)\tilde{u}(k)=\tilde{A}\tilde{\xi}(k)+\tilde{B}\tilde{u}(k)$`
  - 其中：`$\tilde{A}=I+TA=\begin{bmatrix}1&&0&&-Tv_r\sin{\varphi_r}\\0&&1&&Tv_r\sin{\varphi_r}\\0&&0&&1\end{bmatrix}$`
  - `$\tilde{B}=TB=\begin{bmatrix}T\cos{\varphi_r}&&0\\T\sin{\varphi_r}&&0\\\frac{T\tan\delta_f}{l}&&\frac{Tv_r}{l\cos^2\delta_f}\end{bmatrix}$`

- 至此，我们得到了线性时变的状态空间

## 组合
- 状态和控制进行组合
- `$\begin{cases}\xi(k+1)=\tilde{A}\xi(k)+\tilde{B}\triangle u(k)\\\eta(k)=\tilde{C}\xi(k)\end{cases}$`
  - 其中：`$\xi(k)=\begin{bmatrix}\tilde{x}(k)\\\tilde{u}(k-1)\end{bmatrix}$`
  - `$\tilde{x}(k)=x(k)-x_r(k)$`，表示k时刻状态与参考值的偏差
- 带入到公式中，可得
- `$\triangle u(k)=\tilde{u}(k)-\tilde{u}(k-1)=u(k)-u_r(k)-u(k-1)+u_r(k-1)$`
- `$u(k)=u_r(k)+(u(k-1)-u_r(k-1))+\triangle u$`
- 其中`$u(k)$`是最终控制量，`$u_r(k)$`是参考控制量，`$(u(k-1)-u_r(k-1))$`是历史控制量，`$\triangle u$`最优化得到的控制量。

## 预测
- 根据状态空间，我们可以推出以下关系：
- `$\tilde{\xi}(k+1)=\tilde{A}\tilde{\xi}(k)+\tilde{B}\tilde{u}(k)$`
- `$\tilde{\xi}(k+2)=\tilde{A}^2\tilde{\xi}(k)+\tilde{A}\tilde{B}\tilde{u}(k)+\tilde{B}\tilde{u}(k+1)$`
- `$\tilde{\xi}(k+3)=\tilde{A}^3\tilde{\xi}(k)+\tilde{A}^2\tilde{B}\tilde{u}(k)+\tilde{A}\tilde{B}\tilde{u}(k+1)+\tilde{B}\tilde{u}(k+2)$`
- `$\cdots$`
    - 预测时域：当`$N_p=4$`时，可以得到`$\tilde{\xi}(k+1)\; \tilde{\xi}(k+2)\; \tilde{\xi}(k+3)\; \tilde{\xi}(k+4)$`四个状态空间
    - 控制时域：当`$N_c=3$`时，已知`$\tilde{u}(k+1)\;\tilde{u}(k+2)\;\tilde{u}(k+3)$`三个控制空间

- 简写为：`$Y=\Psi\xi(k)+\Theta\triangle u(k)$`
  - 其中：`$Y=\begin{bmatrix}\tilde{\xi}(k+1)\\\cdots\\\tilde{\xi}(k+N_p)\end{bmatrix}$`
  - `$\Psi=\begin{bmatrix}\tilde{A}\\\tilde{A}^2\\\cdots\\\tilde{A}^{N_p}\end{bmatrix}$`
  - `$\xi(k)=\begin{bmatrix}\tilde{x_k}\\\tilde{y_k}\\\tilde{\varphi_k}\end{bmatrix}$`
  - `$\Theta=\begin{bmatrix}\tilde{B}&&0&&\cdots&&\cdots\\\tilde{A}\tilde{B}&&\tilde{B}&&0&&\cdots\\\vdots&&\vdots&&\vdots&&\vdots\\\tilde{A}^{N_p-1}\tilde{B}&&\cdots&&\cdots&&\tilde{B}\end{bmatrix}$`，此时为`$N_p=N_c+1$`的情况，当`$N_p>N_c+1$`时，后面的系数为0，即不考虑控制情况
  - `$U=\begin{bmatrix}u(k)\\u(k+1)\\\vdots\\u(k+N_c)\end{bmatrix}$`


## 优化
- 根据控制规律，使用HSL求解最优化问题。
- 目标：1）尽快收敛到参考值：`$Y->Y_r$`；2）控制输入尽可能小：`$\min{U}$`
- 求解就是让目标的偏差最小，使用加权平方和调整不同的值。
- 想要轨迹尽量满足参考值： `$\begin{bmatrix}x_1->x_{1ref}\\y_1->y_{1ref}\\\varphi_1->\varphi_{1ref}\\\cdots\\x_{N_p}->x_{N_pref}\\y_{N_p}->y_{N_pref}\\\varphi_1->\varphi_{1ref}\\\end{bmatrix}$`
- 构建损失函数：`$L=(Y-Y_r)^TQ(Y-Y_r)+U^TRT$`
- 由于我们只能控制变量U，所以上面的损失函数现阶段无法全部控制。
- 定义偏差：`$E=\Psi\xi-\Psi\xi_r=\Psi\xi-Y_{ref}$`
    - 预测公式为：`$Y=\Psi\xi(k)+\Theta u(k)$`
    - 可得：`$Y-Y_{ref}=E+\Theta U$`

- 损失函数可变化为：`$L=E^TQE+(\Theta U)^TQ(\Theta U)+2E^TQ(\Theta U)+U^TRU$`
  - 其中第一项没有U，可以当做控制函数的常数
- `$L=U^T(\Theta^TQ\Theta+R)U+2E^2Q\Theta U$`

- 可以构建为二次规划问题：`$\begin{cases}\min&&\frac{1}{2}U^THU+f^TU\\s.t. &&lb\leq U\leq ub\end{cases}$`

- 最终可得U的各时刻的输出`$U=\begin{bmatrix}u(k)\\u(k+1)\\\cdots\\u(k+N_c)\end{bmatrix}$`。

**松弛因子的引入**
- 代价函数：`$L=\sum\|\eta-\eta_r\|^2_Q+\sum\|u\|^2_R+\rho\epsilon^2$`
-

## 反馈控制
- 与传统控制方式不同，MPC通过优化求解出控制向量后，系统状态更新再得到预测，再根据预测得到新的控制向量，不断迭代。

- `$u(k)=u_r(k)+(u(k-1-u_r(k-1))+\triangle u(k)$`


# 代码

- tonyxxq/MPC-Control
- PythonRobotics/cubic_spline_planner.py 
- udacity官方：CarND-MPC-Project/install_Ipopt_CppAD.md at master
- [安装方式](https://github.com/tonyxxq/MPC-Control)



