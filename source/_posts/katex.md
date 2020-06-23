title: katex语法
author: hero576
date: 2020-06-04 23:39:14
tags:
---
> latex就是一个方便编辑数学公式的一个库，简单汇总一下他的用法。
<!--more-->

## Latex介绍
- Donald为了更好的在他的著作里编写数学公式，发明了Tex这种宏语言，专门排版数学公式。Leslie封装了Tex，并自定义了更多的宏指令，成为了LaTeX。现在是国际通用的排版系统，很多论文收稿的排版要求都是LaTex，word是不接收的。
- 妈咪说公布了一个在线的公式编辑的网站，生成latex更方便了，非常好用：[www.latexlive.com](https://www.latexlive.com)

## 基本语法
- LaTex Math的语法多且杂，我们是没法完全记住这些语法的。查询手册在手，天下我有，这里比较推荐名校莱斯Rice大学的一个语法手册，莱斯大学[LaTex Math在线PDF手册](https://www.caam.rice.edu/~heinken/latex/symbols.pdf)。

### 排版格式
- 行内公式排版：左右$好像不能有空格
```
$c = \sqrt{a^{2}+b_{xy}^{2}+e^{x}}$
```

$c = \sqrt{a^{2}+b_{xy}^{2}+e^{x}}$

- 块公式排版：
```
$$ c = \sqrt{a^{2}+b_{xy}^{2} +e^{x}} $$
```

$$ c = \sqrt{a^{2}+b_{xy}^{2} +e^{x}} $$


### 转义
- 以下几个字符：`# $ % & ~ _ ^ \ { }`有特殊意义，需要表示这些字符时，需要转义，即在每个字符前加上`\`。
- `\boxed`命令给公式加一个方框。

### 希腊字母
-  希腊字母有大写和小写之分，这个大小写是由LaTex的首字母是否大小写来控制的。

|希腊字母|LaTeX形式|
|--|--|
|$\alpha \Alpha$|`\alpha \Alpha`|
|$\beta \Beta$|`\beta \Beta`|
|$\gamma \Gamma$|`\gamma \Gamma`|
|$\delta \Delta$|`\delta \Delta`|
|$\epsilon \varepsilon \Epsilon$|`\epsilon \varepsilon \Epsilon`|
|$\zeta \Zeta$|`\zeta \Zeta`|
|$\eta \Eta$|`\eta \Eta`|
|$\theta \vartheta \Theta$|`\theta \vartheta \Theta`|
|$\iota \Iota$|`\iota \Iota`|
|$\kappa \Kappa$|`\kappa \Kappa`|
|$\lambda \Lambda$|`\lambda \Lambda`|
|$\mu M$|`\mu \Mu`|
|$\xi \Xi$|`\xi \Xi`|
|o O|`o O `|
|$\pi \Pi$|`\pi \Pi`|
|$\rho \varrho \Rho$|`\rho \varrho \Rho`|
|$\sigma \Sigma$|`\sigma \Sigma`|
|$\tau \tau$|`\tau \Tau`|
|$\upsilon \Upsilon$|`\upsilon \Upsilon`|
|$\phi \varphi \Phi$|`\phi \varphi \Phi`|
|$\chi \Chi$|`\chi \Chi`|
|$\psi \Psi$|`\psi \Psi`|
|$\omega \Omega$|`\omega \Omega`|

### 上下标
- 上下标如果多余一个字符或符号，需要用{}括起来。

|符号|LaTex形式|
|--|--|
|$x^1$|`x^1`|
|$x_1$|`x_1`|

### 根号
- 形式：`\sqrt[开方次数，默认为2]{开方公式}`

|符号|LaTex形式|
|--|--|
|$x_{ij}^2\quad \sqrt{x}\quad \sqrt[3]{x}$|`x_{ij}^2\quad \sqrt{x}\quad \sqrt[3]{x}`|

- `\quad`表示添加空格，或者`\;`。

### 分数
- `\frac`表示分数，
- 字号工具环境设置：
  - `\dfrac`命令把字号设置为独立公式中的大小；
  - `\tfrac`则把字号设置为行间公式中的大小。

|符号|LaTex形式|
|--|--|
|$\frac{1}{2} \dfrac{1}{2}$|`\frac{1}{2} \dfrac{1}{2}`|
|$\frac{1}{2} \tfrac{1}{2}$|`\frac{1}{2} \tfrac{1}{2}`|

### 三角函数、对数、指数

|符号|LaTex形式|
|--|--|
|$\tan$|`\tan`|
|$\sin$|`\sin`|
|$\cos$|`\cos`|
|$\lg$|`\lg`|
|$\arcsin$|`\arcsin`|
|$\arctan$|`\arctan`|
|$\min$|`\min`|
|$\max$|`\max`|
|$\exp$|`\exp`|
|$\log$|`\log`|

### 运算符
#### 简单的四则运算
- `+ - * / =` 直接输入

#### 集合符号
- 特殊运算则用以下特殊命令

|符号|LaTex形式|
|--|--|
|$1\pm1$|`\pm`|
|$\times$|`\times`|
|$\div$|`\div`|
|$\cdot$|`\cdot`|
|$\cap$|`\cap`|
|$\cup$|`\cup`|
|$\geq$|`\geq`|
|$\leq$|`\leq`|
|$\neq$|`\neq`|
|$\approx$|`\approx`|
|$\equiv$|`\equiv`|
|$\in$|`\in`|
|$\notin$|`\notin`|
|$\ni$|`\ni`|
|$\subset$|`\subset`|


#### 和、积、极限、积分
- 和、积、极限、积分等运算符，这些公式在行内公式被压缩，以适应行高，可以通过`\limits`和`\nolimits`命令显示制动是否压缩

|符号|LaTex形式|
|--|--|
|$\sum$|`\sum`|
|$\prod$|`\prod`|
|$\lim$|`\lim`|
|$\int$|`\int`|

#### 多重积分

|符号|LaTex形式|
|--|--|
|$\int$|`\int`|
|$\iint$|`\iint`|
|$\int \int$|`\int \int`|
|$\iiint$|`\iiint`|
|$\int \int \int$|`\int \int \int`|
|$\iiiint$|`\iiiint`|
|$\int \int \int \int$|`\int \int \int \int`|
|$\idotsint$|`\idotsint`|
|$\int \dots \int$|`\int \dots \int`|

### 箭头

|符号|LaTex形式|
|--|--|
|$\leftarrow$|`\leftarrow`|
|$\rightarrow$|`\rightarrow`|
|$\leftrightarrow$|`\leftrightarrow`|
|$\longleftarrow$|`\longleftarrow`|
|$\longleftrightarrow$|`\longleftrightarrow`|
|$\Longrightarrow$|`\Longrightarrow`|

- `\xleftarrow`和`\xrightarrow`可根据内容自动调整

### 注音和标注

|符号|LaTex形式|
|--|--|
|$\bar{x}$|`\bar{x}`|
|$\acute{x}$|`\acute{x}`|
|$\mathring{x}$|`\mathring{x}`|
|$\vec{x}$|`\vec{x}`|
|$\grave{x}$|`\grave{x}`|
|$\dot{x}$|`\dot{x}`|
|$\hat{x}$|`\hat{x}`|
|$\tilde{x}$|`\tilde{x}`|
|$\ddot{x}$|`\ddot{x}`|
|$\check{x}$|`\check{x}`|
|$\breve{x}$|`\breve{x}`|
|$\dddot{x}$|`\dddot{x}`|

### 括号
- 用`() [] {} \lange \rangle`表示 `() [] {} ⟨⟩`

### 分隔符

|符号|LaTex形式|
|--|--|
|$\overline{xxx}$|`\overline{xxx}`|
|$\overleftrightarrow{xxx}$|`\overleftrightarrow{xxx}`|
|$\underline{xxx}$|`\underline{xxx}`|
|$\underleftrightarrow{xxx}$|`\underleftrightarrow{xxx}`|
|$\overleftarrow{xxx}$|`\overleftarrow{xxx}`|
|$\overbrace{xxx}$|`\overbrace{xxx}`|
|$\underleftarrow{xxx}$|`\underleftarrow{xxx}`|
|$\underbrace{xxx}$|`\underbrace{xxx}`|
|$\overrightarrow{xxx}$|`\overrightarrow{xxx}`|
|$\widehat{xxx}$|`\widehat{xxx}`|
|$\underrightarrow{xxx}$|`\underrightarrow{xxx}`|
|$\widetilde{xxx}$|`\widetilde{xxx}`|
|$\Bigg( \bigg( \Big( \big((x) \big) \Big) \bigg) \Bigg)$|`$\Bigg( \bigg( \Big( \big((x) \big) \Big) \bigg) \Bigg)$`|
|$\Bigg[ \bigg[ \Big[ \big[[x] \big] \Big] \bigg] \Bigg]$|`$\Bigg[ \bigg[ \Big[ \big[[x] \big] \Big] \bigg] \Bigg]$`|
|$\Bigg\{ \bigg\{ \Big\{ \big\{\{x\} \big\} \Big\} \bigg\} \Bigg\}$|`$\Bigg\{ \bigg\{ \Big\{ \big\{\{x\} \big\} \Big\} \bigg\} \Bigg\}$`|
|$\Bigg\langle \bigg\langle \Big\langle \big\langle\langle x \rangle \big\rangle \Big\rangle \bigg\rangle \Bigg\rangle$ |`$\Bigg\langle \bigg\langle \Big\langle \big\langle\langle x \rangle \big\rangle \Big\rangle \bigg\rangle \Bigg\rangle$ `|
|$\Bigg\lvert \bigg\lvert \Big\lvert \big\lvert\lvert x \rvert \big\rvert \Big\rvert \bigg\rvert \Bigg\rvert$|`$\Bigg\lvert \bigg\lvert \Big\lvert \big\lvert\lvert x \rvert \big\rvert \Big\rvert \bigg\rvert \Bigg\rvert$`|
|$\Bigg\lVert\big\lVert\Big\lVert\big\lVert\lVert x \rVert \big\rVert\Big\rVert\bigg\rVert \Bigg\rVert$|`$\Bigg\lVert\big\lVert \Big\lVert \big\lVert \lVert x \rVert \big\rVert \Big\rVert \bigg\rVert \Bigg\rVert$`|

### 省略号
- 省略号用 \dots \cdots \vdots \ddots表示 ，\dots和\cdots的纵向位置不同，前者一般用于有下标的序列

|符号|LaTex形式|
|--|--|
|$\dots$|`\dots`|
|$\cdots$|`\cdots`|
|$\vdots$|`\vdots`|
|$\ddots$|`\ddots`|

$$ x_1, x_2, \dots, x_n\quad 1,2,\cdots,n\quad \vdots\quad \ddots $$

### 空白间距

|语法|	格式|	实例|	显示|
|--|--|--|--|
|quad空格|	`a \quad b`	|$a \quad b$|	|一个m的宽度|
|两个quad空格|	`a \qquad b`|$a \qquad b$|两个m的宽度|
|大空格|	`a \: b`|$a \: b$|1/3m宽度|
|中等空格|	`a \; b`|$a \; b$|2/7m宽度|
|小空格|	`a \, b`|$a \, b$|1/6m宽度|
|没有空格|	`ab`|$ab$|没有空格|
|缩进空格|	`a \! b`|$a \! b$|缩进1/6m宽度|

### 复杂公式
- 分段函数是非常复杂的，这时候会用到`LaTex`的`cases`语法，用`\begin{cases}`和`\end{cases}`围住即可，中间则用`\\`来分段
#### 矩阵
$$
\begin{array}{ccc}
x_1 & x_2 &\dots\\
x_3 & x_4 &\dots\\
\vdots&\vdots&\ddots
\end{array}
$$
```
$$
\begin{array}{ccc}
x_1 & x_2 &\dots\\
x_3 & x_4 &\dots\\
\vdots&\vdots&\ddots
\end{array}
$$
```
$$
\begin{pmatrix} 
a & b\\ 
c & d \\
\end{pmatrix}
\quad
\begin{bmatrix} 
a & b \\ 
c & d \\
\end{bmatrix}
\quad
\begin{Bmatrix} 
a & b \\ 
c & d \\
\end{Bmatrix}
\quad
\begin{vmatrix} 
a & b \\ 
c & d \\
\end{vmatrix}
\quad
\begin{Vmatrix} 
a & b \\ 
c & d \\
\end{Vmatrix}
$$
```
$$
\begin{pmatrix} 
a & b\\ 
c & d \\
\end{pmatrix}
\quad
\begin{bmatrix} 
a & b \\ 
c & d \\
\end{bmatrix}
\quad
\begin{Bmatrix} 
a & b \\ 
c & d \\
\end{Bmatrix}
\quad
\begin{vmatrix} 
a & b \\ 
c & d \\
\end{vmatrix}
\quad
\begin{Vmatrix} 
a & b \\ 
c & d \\
\end{Vmatrix}
$$
```

$$
(
\begin{smallmatrix} 
a & b \\ 
c & d 
\end{smallmatrix}
) 
$$
```
$$
(
\begin{smallmatrix} 
a & b \\ 
c & d 
\end{smallmatrix}
) 
$$
```

#### 长公式
  - 无需对齐可使用`multline`；需要对齐使用`split`；用`\\`来分行；用`&`设置对齐的位置
$$
\begin{multline}    
x = a+b+c+{} \\     
d+e+f+g  
\end{multline}
$$
```
$$
\begin{multline}    
x = a+b+c+{} \\     
d+e+f+g  
\end{multline}
$$
```

$$
\begin{split}
x = {} & a + b + c +{}\\    
       & d + e + f + g
\end{split}
$$
```
$$
\begin{split}
x = {} & a + b + c +{}\\    
       & d + e + f + g
\end{split}
$$
```

#### 公式组
  - 不需要对齐的公式组用`gather`；需要对齐使用`align`:
$$
\begin{gather}
a = b+c+d\\
x = y+z\\
5 = 4+1\\
\end{gather}
$$
```
$$
\begin{gather}
a = b+c+d\\
x = y+z\\
5 = 4+1\\
\end{gather}
$$
```

$$
\begin{align}
a &=b+c+d \\
x &=y+z\\
5 &= 4+1
\end{align}
$$

```
$$
\begin{align}
a &=b+c+d \\
x &=y+z\\
5 &= 4+1
\end{align}
$$
```

#### 分支公式
  - 分段函数通常用`cases`次环境携程分支公式
$$
y=\begin{cases}
-x,\quad x\leq 0\\
x, \quad x>0
\end{cases} 
$$
```
$$
y=\begin{cases}
-x,\quad x\leq 0\\
x, \quad x>0
\end{cases} 
$$
```

#### 定理、引理、证明、假设