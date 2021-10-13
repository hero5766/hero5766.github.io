---
title: cppad
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2021-07-05 17:07:20
---
> 

<!-- more -->


# 简介[^1]
[^1]: [连接](https://www.jianshu.com/p/47ddf443d8fb)

最优化问题，等式约束/不等式约束

## ipopt配置
- [官方说明](https://coin-or.github.io/Ipopt/OPTIONS.html)


# 代码
- 最优化问题：
- $\begin{cases}minimize&&x_1*x_4*(x_1+x_2+x_3)+x_3\\subject\;to &&x_1*x_2*x_3*x_4\ge25\\&&x_1^2+x_2^2+x_3^2+x_4^2=40\\&&1\le x_1,x_2,x_3,x_4\le 5\end{cases}$

- 设置参数、初始状态、状态约束值、等式/不等式约束值、优化方程/等式及不等式方程。

```make
cmake_minimum_required(VERSION 3.19)
project(cppad_tst)
set(CMAKE_CXX_STANDARD 14)
add_executable(cppad_tst main.cpp)
target_link_libraries(cppad_tst ipopt)
```

```cpp
#include <iostream>
#include <cppad/ipopt/solve.hpp>

using namespace std;
namespace {
    using CppAD::AD;

    class FG_eval {
    public:
        typedef CPPAD_TESTVECTOR(AD<double>) ADvector;

        void operator()(ADvector &fg, const ADvector &x) {
            assert(fg.size() == 3);
            assert(x.size() == 4);
            AD<double> x1 = x[0];
            AD<double> x2 = x[1];
            AD<double> x3 = x[2];
            AD<double> x4 = x[3];
            fg[0] = x1 * x4 * (x1 + x2 + x3) + x3;
            fg[1] = x1 * x2 * x3 * x4;
            fg[2] = x1 * x1 + x2 * x2 + x3 * x3 + x4 * x4;
        }
    };
}
bool get_started(void) {
    bool ok = true;
    size_t i;
    typedef CPPAD_TESTVECTOR(double) Dvector;
    size_t nx = 4;
    size_t ng = 2;
    Dvector xi(nx);
    xi[0] = 1.;
    xi[1] = 5.;
    xi[2] = 5.;
    xi[3] = 1.;
    Dvector xl(nx), xu(nx);
    for (i = 0; i < nx; i++) {
        xl[i] = 1.;
        xu[i] = 5.;
    }
    Dvector gl(ng), gu(ng);
    gl[0] = 25.;
    gu[0] = 1.0e19;
    gl[1] = 40.;
    gu[1] = 40.;
    FG_eval fgEval;
    string options;
    options += "Integer print_level  0\n";
    options += "String sb  yes\n";
    options += "Integer max_iter  10\n";
    options += "Numeric tol    1e-6\n";
    options += "String derivative_test  second-order\n";
    options += "Numeric point_perturbation_radius 0.\n";

    CppAD::ipopt::solve_result<Dvector> solution;
    CppAD::ipopt::solve<Dvector, FG_eval>(options, xi, xl, xu, gl, gu, fgEval, solution);
    for (i = 0; i < nx; i++) {
        cout<<solution.x[i]<<","<<solution.zl[i]<<","<<solution.zu[i]<<endl;
    }
    ok &= solution.status == CppAD::ipopt::solve_result<Dvector>::success;
    double check_x[] = {1., 4.743, 3.82115, 1.379408};
    double check_zl[] = {1.087871, 0., 0., 0.};
    double check_zu[] = {0., 0., 0., 0.};
    double rel_tol = 1e-6;
    double abs_tol = 1e-6;
    for (i = 0; i < nx; i++) {
        ok &= CppAD::NearEqual(check_x[i], solution.x[i], rel_tol, abs_tol);
        ok &= CppAD::NearEqual(check_zl[i], solution.zl[i], rel_tol, abs_tol);
        ok &= CppAD::NearEqual(check_zu[i], solution.zu[i], rel_tol, abs_tol);
    }

    return ok;
}


int main() {
    std::cout << "Hello, World!" << std::endl;
    cout << get_started() << endl;
    return 0;
}
```