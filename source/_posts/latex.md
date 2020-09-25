---
title: Latex语法
author: hero576
date: 2020-06-04 23:39:14
tags:
  - skills
categories:
  - common
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
|$\Bigg\lVert\bigg\lVert\Big\lVert\big\lVert\lVert x \rVert \big\rVert\Big\rVert\bigg\rVert \Bigg\rVert$|`$\Bigg\lVert\big\lVert \Big\lVert \big\lVert \lVert x \rVert \big\rVert \Big\rVert \bigg\rVert \Bigg\rVert$`|

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
##### 定理
$$
\newtheorem{thm}{\bf Theorem}[section]
\begin{thm}\label{thm1}
Suppose system (\ref{l1}) satisfies Assumption (\ref{mim1}), the closed-loop system consisting of
system (\ref{l1}), the disturbance observer (\ref{g1}) and the proposed controller (\ref{n3}) is semi-globally ISS.
\end{thm} 
$$
```
$$
\newtheorem{thm}{\bf Theorem}[section]
\begin{thm}\label{thm1}
Suppose system (\ref{l1}) satisfies Assumption (\ref{mim1}), the closed-loop system consisting of
system (\ref{l1}), the disturbance observer (\ref{g1}) and the proposed controller (\ref{n3}) is semi-globally ISS.
\end{thm} 
$$
```

##### 引理
$$
\newtheorem{lemma}{Lemma}[section]
\begin{lemma} \label{lemma1}
\end{lemma}
$$
```
$$
\newtheorem{lemma}{Lemma}[section]
\begin{lemma} \label{lemma1}
\end{lemma}
$$
```

##### 证明
$$
\begin{proof}
***
\end{proof}
$$
```
$$
\begin{proof}
***
\end{proof}
$$
```

##### 假设
$$
\newtheorem{assumption}{Assumption}[section]
\begin{assumption}
***
\end{assumption}
$$
```
$$
\newtheorem{assumption}{Assumption}[section]
\begin{assumption}
***
\end{assumption}
$$
```


## 常用指令汇总

### 常用符号

#### 二元运算符 Binary operations

tag|latex|descript
-|-|-
$+$|`+`|加
$-$|`-`|减
$\times$|`\times`|乘
${\div}$|`{\div}`|除
$\pm$|`\pm`|加减
$\mp$|`\mp`|减加
$\triangleleft$|`\triangleleft`|正规子群
$\triangleright$|`\triangleright`|属于正规子群
$\cdot$|`\cdot`|点
$\setminus$|`\setminus`|减号集
$\star$|`\star`|星
$\ast$|`\ast`|星号
$\cup$|`\cup`|并集
$\cap$|`\cap`|交集
$\sqcup$|`\sqcup`|-
$\sqcap$|`\sqcap`|-
$\vee$|`\vee`|-
$\wedge$|`\wedge`|-
$\circ$|`\circ`|-
$\bullet$|`\bullet`|-
$\oplus$|`\oplus`|-
$\ominus$|`\ominus`|-
$\odot$|`\odot`|-
$\oslash$|`\oslash`|-
$\otimes$|`\otimes`|-
$\bigcirc$|`\bigcirc`|-
$\diamond$|`\diamond`|-
$\uplus$|`\uplus`|-
$\bigtriangleup$|`\bigtriangleup`|-
$\bigtriangledown$|`\bigtriangledown`|-
$\lhd$|`\lhd`|-
$\rhd$|`\rhd`|-
$\unlhd$|`\unlhd`|-
$\unrhd$|`\unrhd`|-
$\amalg$|`\amalg`|-
$\wr$|`\wr`|-
$\dagger$|`\dagger`|-
$\ddagger$|`\ddagger`|-

#### 二元关系符 Binary relations

tag|latex|descript
-|-|-
$<$|`<`|-
$>$|`>`|-
$=$|`=`|-
$\le$|`\le`|-
$\ge$|`\ge`|-
$\equiv$|`\equiv`|-
$\ll$|`\ll`|-
$\gg$|`\gg`|-
$\doteq$|`\doteq`|-
$\prec$|`\prec`|-
$\succ$|`\succ`|-
$\sim$|`\sim`|-
$\preceq$|`\preceq`|-
$\succeq$|`\succeq`|-
$\simeq$|`\simeq`|-
$\approx$|`\approx`|-
$\subset$|`\subset`|-
$\supset$|`\supset`|-
$\subseteq$|`\subseteq`|-
$\supseteq$|`\supseteq`|-
$\sqsubset$|`\sqsubset`|-
$\sqsupset$|`\sqsupset`|-
$\sqsubseteq$|`\sqsubseteq`|-
$\sqsupseteq$|`\sqsupseteq`|-
$\cong$|`\cong`|-
$\Join$|`\Join`|-
$\bowtie$|`\bowtie`|-
$\propto$|`\propto`|-
$\in$|`\in`|-
$\ni$|`\ni`|-
$\vdash$|`\vdash`|-
$\dashv$|`\dashv`|-
$\models$|`\models`|-
$\mid$|`\mid`|-
$\parallel$|`\parallel`|-
$\perp$|`\perp`|-
$\smile$|`\smile`|-
$\frown$|`\frown`|-
$\asymp$|`\asymp`|-
$:$|`:`|-
$\notin$|`\notin`|-
$\ne$|`\ne`|-

#### 箭头符号 Arrows

tag|latex|descript
-|-|-
$\gets$|`\gets`|-
$\to$|`\to`|-
$\longleftarrow$|`\longleftarrow`|-
$\longrightarrow$|`\longrightarrow`|-
$\uparrow$|`\uparrow`|-
$\downarrow$|`\downarrow`|-
$\updownarrow$|`\updownarrow`|-
$\leftrightarrow$|`\leftrightarrow`|-
$\Uparrow$|`\Uparrow`|-
$\Downarrow$|`\Downarrow`|-
$\Updownarrow$|`\Updownarrow`|-
$\longleftrightarrow$|`\longleftrightarrow`|-
$\Leftarrow$|`\Leftarrow`|-
$\Longleftarrow$|`\Longleftarrow`|-
$\Rightarrow$|`\Rightarrow`|-
$\Longrightarrow$|`\Longrightarrow`|-
$\Leftrightarrow$|`\Leftrightarrow`|-
$\Longleftrightarrow$|`\Longleftrightarrow`|-
$\mapsto$|`\mapsto`|-
$\longmapsto$|`\longmapsto`|-
$\nearrow$|`\nearrow`|-
$\searrow$|`\searrow`|-
$\swarrow$|`\swarrow`|-
$\nwarrow$|`\nwarrow`|-
$\hookleftarrow$|`\hookleftarrow`|-
$\hookrightarrow$|`\hookrightarrow`|-
$\rightleftharpoons$|`\rightleftharpoons`|-
$\iff$|`\iff`|-
$\leftharpoonup$|`\leftharpoonup`|-
$\rightharpoonup$|`\rightharpoonup`|-
$\leftharpoondown$|`\leftharpoondown`|-
$\rightharpoondown$|`\rightharpoondown`|-

#### 其他符号 Others
tag|latex|descript
-|-|-
$\because$|`\because`|-
$\therefore$|`\therefore`|-
$\dots$|`\dots`|-
$\cdots$|`\cdots`|-
$\vdots$|`\vdots`|-
$\ddots$|`\ddots`|-
$\forall$|`\forall`|-
$\exists$|`\exists`|-
$\nexists$|`\nexists`|-
$\Finv$|`\Finv`|-
$\neg$|`\neg`|-
$\prime$|`\prime`|-
$\emptyset$|`\emptyset`|-
$\infty$|`\infty`|-
$\nabla$|`\nabla`|-
$\triangle$|`\triangle`|-
$\Box$|`\Box`|-
$\Diamond$|`\Diamond`|-
$\bot$|`\bot`|-
$\top$|`\top`|-
$\angle$|`\angle`|-
$\measuredangle$|`\measuredangle`|-
$\sphericalangle$|`\sphericalangle`|-
$\surd$|`\surd`|-
$\diamondsuit$|`\diamondsuit`|-
$\heartsuit$|`\heartsuit`|-
$\clubsuit$|`\clubsuit`|-
$\spadesuit$|`\spadesuit`|-
$\flat$|`\flat`|-
$\natural$|`\natural`|-
$\sharp$|`\sharp`|-

### 希腊字母

tag|latex|descript
-|-|-
$\alpha$|`\alpha`|alpha
$\beta$|`\beta`|beta
$\gamma$|`\gamma`|gamma
$\delta$|`\delta`|delta
$\epsilon$|`\epsilon`|epsilon
$\varepsilon$|`\varepsilon`|epsilon
$\zeta$|`\zeta`|zeta
$\eta$|`\eta`|eta
$\theta$|`\theta`|theta
$\vartheta$|`\vartheta`|theta
$\iota$|`\iota`|iota
$\kappa$|`\kappa`|kappa
$\lambda$|`\lambda`|lambda
$\mu$|`\mu`|mu
$\nu$|`\nu`|nu
$\xi$|`\xi`|xi
$o$|`o`|omicron
$\pi$|`\pi`|pi
$\varpi$|`\varpi`|pi
$\rho$|`\rho`|rho
$\varrho$|`\varrho`|rho
$\sigma$|`\sigma`|sigma
$\varsigma$|`\varsigma`|sigma
$\tau$|`\tau`|tau
$\upsilon$|`\upsilon`|upsilon
$\phi$|`\phi`|phi
$\varphi$|`\varphi`|phi
$\chi$|`\chi`|chi
$\psi$|`\psi`|psi
$\omega$|`\omega`|omega
$\Gamma$|`\Gamma`|Gamma
$\Delta$|`\Delta`|Delta
$\Theta$|`\Theta`|Theta
$\Lambda$|`\Lambda`|Lambda
$\Xi$|`\Xi`|Xi
$\Pi$|`\Pi`|Pi
$\Sigma$|`\Sigma`|Sigma
$\Upsilon$|`\Upsilon`|Upsilon
$\Phi$|`\Phi`|Phi
$\Psi$|`\Psi`|Psi
$\Omega$|`\Omega`|Omega

#### 其他

tag|latex|descript
-|-|-
$\hbar$|`\hbar`|h bar
$\imath$|`\imath`|imath
$\jmath$|`\jmath`|jmath
$\ell$|`\ell`|lmath
$\Re$|`\Re`|Real Numbers
$\Im$|`\Im`|Pure Imaginary Numbers
$\aleph$|`\aleph`|aleph
$\beth$|`\beth`|beth
$\gimel$|`\gimel`|gimel
$\daleth$|`\daleth`|daleth
$\wp$|`\wp`|-
$\mho$|`\mho`|-
$\backepsilon$|`\backepsilon`|backepsilon
$\partial$|`\partial`|-
$\eth$|`\eth`|-
$\Bbbk$|`\Bbbk`|-
$\complement$|`\complement`|complement
$\circledS$|`\circledS`|circled S
$\S$|`\S`|sections
$\mathbb{ABC}$|`\mathbb{ABC}`|Blackboard bold/scripts
$\mathfrak{ABC}$|`\mathfrak{ABC}`|Fraktur typeface
$\mathcal{ABC}$|`\mathcal{ABC}`|Calligraphy/script
$\mathrm {ABC}$|`\mathrm {ABC}`|Roman typeface
$\mathrm{def}$|`\mathrm{def}`|def

### 分数微分


#### 分数 Fractions

tag|latex|descript
-|-|-
$\frac{ABC}{ABC}$|`\frac{ABC}{ABC}`|分数
$\tfrac{ABC}{ABC}$|`\tfrac{ABC}{ABC}`|小分数
$\mathrm{d}t$|`\mathrm{d}t`|微分
$\frac{\mathrm{d} y}{\mathrm{d} x}$|`\frac{\mathrm{d} y}{\mathrm{d} x}`|微分
$\partial t$|`\partial t`|偏微分
$\frac{\partial y}{\partial x}$|`\frac{\partial y}{\partial x}`|偏微分
$\nabla\psi$|`\nabla\psi`|Nabla算子
$\frac{\partial^2}{\partial x_1\partial x_2}y$|`\frac{\partial^2}{\partial x_1\partial x_2}y`|偏微分
$\cfrac{1}{a + \cfrac{7}{b + \cfrac{2}{9}}} =c$|`\cfrac{1}{a + \cfrac{7}{b + \cfrac{2}{9}}} =c`|连分数
$\begin{equation}   x = a_0 + \cfrac{1}{a_1            + \cfrac{1}{a_2            + \cfrac{1}{a_3 + \cfrac{1}{a_4} } } } \end{equation}$|`\begin{equation}   x = a_0 + \cfrac{1}{a_1            + \cfrac{1}{a_2            + \cfrac{1}{a_3 + \cfrac{1}{a_4} } } } \end{equation}`|连分数

#### 导数 Derivative

tag|latex|descript
-|-|-
$\dot{ABC}$|`\dot{ABC}`|一阶导数
$\ddot{ABC}$|`\ddot{ABC}`|二阶导数
${ABC}'$|`{ABC}'`|一阶导数
${ABC}''$|`{ABC}''`|二阶导数
${ABC}^{(n)}$|`{ABC}^{(n)}`|n阶导数

#### 模算术 Modular arithmetic
tag|latex|descript
-|-|-
$a \bmod b$|`a \bmod b`|模除
$a \equiv b \pmod{m}$|`a \equiv b \pmod{m}`|同余
$\gcd(m, n)$|`\gcd(m, n)`|最大公约数
$\operatorname{lcm}(m, n)$|`\operatorname{lcm}(m, n)`|最小公倍数

### 根式角标

#### 根式 Radicals
tag|latex|descript
-|-|-
$\sqrt{ABC}$|`\sqrt{ABC}`|开平方
$\sqrt[]{ABC}$|`\sqrt[]{ABC}`|开方

#### 上下标 Sub&Super
tag|latex|descript
-|-|-
$^{ABC}$|`^{ABC}`|上标
$_{ABC}$|`_{ABC}`|下标
$_{ABC}^{ABC}$|`_{ABC}^{ABC}`|混合上下标
$_{ABC}^{ABC}$|`_{ABC}^{ABC}`|左侧混合上下标
$\sideset{_1^2}{_3^4}X_a^b$|`\sideset{_1^2}{_3^4}X_a^b`|混合

#### 重音符及其他 Accents and Others
tag|latex|descript
-|-|-
$\hat{ABC}$|`\hat{ABC}`|-
$\check{ABC}$|`\check{ABC}`|-
$\grave{ABC}$|`\grave{ABC}`|-
$\acute{ABC}$|`\acute{ABC}`|-
$\tilde{ABC}$|`\tilde{ABC}`|-
$\breve{ABC}$|`\breve{ABC}`|-
$\bar{ABC}$|`\bar{ABC}`|-
$\vec{ABC}$|`\vec{ABC}`|-
$\not{ABC}$|`\not{ABC}`|-
$^{\circ}$|`^{\circ}`|-
$\widetilde{ABC}$|`\widetilde{ABC}`|-
$\widehat{ABC}$|`\widehat{ABC}`|-
$\overleftarrow{ABC}$|`\overleftarrow{ABC}`|-
$\overrightarrow{ABC}$|`\overrightarrow{ABC}`|-
$\overline{ABC}$|`\overline{ABC}`|-
$\underline{ABC}$|`\underline{ABC}`|-
$\overbrace{ABC}$|`\overbrace{ABC}`|-
$\underbrace{ABC}$|`\underbrace{ABC}`|-
$\overset{ABC}{ABC}$|`\overset{ABC}{ABC}`|-
$\underset{ABC}{ABC}$|`\underset{ABC}{ABC}`|-
$\stackrel\frown{ABC}$|`\stackrel\frown{ABC}`|-
$\overline{ABC}$|`\overline{ABC}`|-
$\overleftrightarrow{ABC}$|`\overleftrightarrow{ABC}`|-
$\overset{ABC}{\leftarrow}$|`\overset{ABC}{\leftarrow}`|-
$\overset{ABC}{\rightarrow}$|`\overset{ABC}{\rightarrow}`|-
$\xleftarrow[]{ABC}$|`\xleftarrow[]{ABC}`|-
$\xrightarrow[]{ABC}$|`\xrightarrow[]{ABC}`|-

### 极限对数

#### 极限 Limits
tag|latex|descript
-|-|-
$\lim$|`\lim`|极限
$\lim_{x \to 0}$|`\lim_{x \to 0}`|极限
$\lim_{x \to \infty}$|`\lim_{x \to \infty}`|极限
$\textstyle \lim_{x \to 0}$|`\textstyle \lim_{x \to 0}`|极限
$\max_{ABC}$|`\max_{ABC}`|极大
$\min_{ABC}$|`\min_{ABC}`|极小

#### 对数指数 Logarithms and exponentials
tag|latex|descript
-|-|-
$\log_{ABC}{ABC}$|`\log_{ABC}{ABC}`|对数
$\lg_{ABC}{ABC}$|`\lg_{ABC}{ABC}`|常用对数
$\ln_{ABC}{ABC}$|`\ln_{ABC}{ABC}`|自然对数
$\exp$|`\exp`|指数

#### 界限 Bounds
tag|latex|descript
-|-|-
$\min x$|`\min x`|最小
$\max y$|`\max y`|最大
$\sup t$|`\sup t`|最小上界（上确界）
$\inf s$|`\inf s`|最大下界（下确界）
$\lim u$|`\lim u`|极限
$\limsup w$|`\limsup w`|上极限
$\liminf v$|`\liminf v`|下极限
$\dim p$|`\dim p`|维数
$\ker\phi$|`\ker\phi`|零空间（核）

### 三角函数

#### 三角函数 Trigonometric functions
tag|latex|descript
-|-|-
$\sin$|`\sin`|正弦
$\cos$|`\cos`|余弦
$\tan$|`\tan`|正切
$\cot$|`\cot`|余切
$\sec$|`\sec`|正割
$\csc$|`\csc`|余割

#### 反三角函数 Inverse trigonometric functions
tag|latex|descript
-|-|-
$\sin^{-1}$|`\sin^{-1}`|反正弦
$\cos^{-1}$|`\cos^{-1}`|反余弦
$\tan^{-1}$|`\tan^{-1}`|反正切
$\cot^{-1}$|`\cot^{-1}`|反余切
$\sec^{-1}$|`\sec^{-1}`|反正割
$\csc^{-1}$|`\csc^{-1}`|反余割
$\arcsin$|`\arcsin`|反正弦
$\arccos$|`\arccos`|反余弦
$\arctan$|`\arctan`|反正切
$\operatorname{arccot}$|`\operatorname{arccot}`|反余切
$\operatorname{arcsec}$|`\operatorname{arcsec}`|反正割
$\operatorname{arccos}$|`\operatorname{arccos}`|反余割

#### 双曲函数 Hyperblic functions
tag|latex|descript
-|-|-
$\sinh$|`\sinh`|双曲正弦
$\cosh$|`\cosh`|双曲余弦
$\tanh$|`\tanh`|双曲正切
$\coth$|`\coth`|双曲余切
$\operatorname{sech}$|`\operatorname{sech}`|双曲正割
$\operatorname{csch}$|`\operatorname{csch}`|双曲余割

#### 反双曲函数 Inverse hyperbolic functions
tag|latex|descript
-|-|-
$\sinh^{-1}$|`\sinh^{-1}`|反双曲正弦
$\cosh^{-1}$|`\cosh^{-1}`|反双曲余弦
$\tanh^{-1}$|`\tanh^{-1}`|反双曲正切
$\coth^{-1}$|`\coth^{-1}`|反双曲余切
$\operatorname{sech}^{-1}$|`\operatorname{sech}^{-1}`|反双曲正割
$\operatorname{csch}^{-1}$|`\operatorname{csch}^{-1}`|反双曲余割

### 积分运算

#### 积分 Integral
tag|latex|descript
-|-|-
$\int$|`\int`|积分
$\int_{ABC}^{ABC}$|`\int_{ABC}^{ABC}`|积分
$\int\limits_{ABC}^{ABC}$|`\int\limits_{ABC}^{ABC}`|积分

#### 双重积分 Double integral
tag|latex|descript
-|-|-
$\iint$|`\iint`|双重积分
$\iint_{ABC}^{ABC}$|`\iint_{ABC}^{ABC}`|双重积分
$\iint\limits_{ABC}^{ABC}$|`\iint\limits_{ABC}^{ABC}`|双重积分

#### 三重积分 Triple integral
tag|latex|descript
-|-|-
$\iiint$|`\iiint`|三重积分
$\iiint_{ABC}^{ABC}$|`\iiint_{ABC}^{ABC}`|三重积分
$\iiint\limits_{ABC}^{ABC}$|`\iiint\limits_{ABC}^{ABC}`|三重积分

#### 曲线积分 Closed line or path integral
tag|latex|descript
-|-|-
$\oint$|`\oint`|曲线积分
$\oint_{ABC}^{ABC}$|`\oint_{ABC}^{ABC}`|曲线积分

### 大型运算

#### 求和 Summation
tag|latex|descript
-|-|-
$\sum$|`\sum`|求和
$\sum_{ABC}^{ABC}$|`\sum_{ABC}^{ABC}`|求和
${\textstyle \sum_{ABC}^{ABC}}$|`{\textstyle \sum_{ABC}^{ABC}}`|求和

#### 乘积余积 Product and coproduct
tag|latex|descript
-|-|-
$\prod$|`\prod`|连乘积
$\prod_{ABC}^{ABC}$|`\prod_{ABC}^{ABC}`|连乘积
${\textstyle \prod_{ABC}^{ABC}}$|`{\textstyle \prod_{ABC}^{ABC}}`|连乘积
$\coprod$|`\coprod`|余积
$\coprod_{ABC}^{ABC}$|`\coprod_{ABC}^{ABC}`|余积
${\textstyle \coprod_{ABC}^{ABC}}$|`{\textstyle \coprod_{ABC}^{ABC}}`|余积

#### 并集交集 Union and intersection
tag|latex|descript
-|-|-
$\bigcup$|`\bigcup`|并集
$\bigcup_{ABC}^{ABC}$|`\bigcup_{ABC}^{ABC}`|并集
${\textstyle \bigcup_{ABC}^{ABC}}$|`{\textstyle \bigcup_{ABC}^{ABC}}`|并集
$\bigcap$|`\bigcap`|交集
$\bigcap_{ABC}^{ABC}$|`\bigcap_{ABC}^{ABC}`|交集
${\textstyle \bigcap_{ABC}^{ABC}}$|`{\textstyle \bigcap_{ABC}^{ABC}}`|交集

#### 析取合取 Disjunction and conjunction
tag|latex|descript
-|-|-
$\bigvee$|`\bigvee`|析取
$\bigvee_{ABC}^{ABC}$|`\bigvee_{ABC}^{ABC}`|析取
${\textstyle \bigvee_{ABC}^{ABC}}$|`{\textstyle \bigvee_{ABC}^{ABC}}`|析取
$\bigwedge$|`\bigwedge`|合取
$\bigwedge_{ABC}^{ABC}$|`\bigwedge_{ABC}^{ABC}`|合取
${\textstyle \bigwedge_{ABC}^{ABC}}$|`{\textstyle \bigwedge_{ABC}^{ABC}}`|合取

### 括号取整

#### 括号 Brackets
tag|latex|descript
-|-|-
$\left (  \right )$|`\left (  \right )`|圆括号
$\left [  \right ]$|`\left [  \right ]`|方括号
$\left \langle  \right \rangle$|`\left \langle  \right \rangle`|角括号
$\left \{  \right \}$|`\left \{  \right \}`|花括号
$\left |  \right |$|`\left |  \right |`|单竖线，绝对值
$\left \|  \right \|$|`\left \|  \right \|`|双竖线，范
$\left \lfloor  \right \rfloor$|`\left \lfloor  \right \rfloor`|取整函数
$\left \lceil  \right \rceil$|`\left \lceil  \right \rceil`|取顶函数

#### 常用 Commons
tag|latex|descript
-|-|-
$\binom{ABC}{ABC}$|`\binom{ABC}{ABC}`|二项式系数
$\left [ 0,1 \right )$|`\left [ 0,1 \right )`|开闭区间
$\left\langle\psi\right|$|`\left\langle\psi\right|`|左矢
$\left | \psi  \right \rangle$|`\left | \psi  \right \rangle`|右矢
$\left \langle \psi  | \psi  \right \rangle$|`\left \langle \psi  | \psi  \right \rangle`|态矢量内积

### 数组矩阵

tag|latex|descript
-|-|-
$\begin{matrix}…&…\end{matrix}$|`\begin{matrix}…&…\end{matrix}`|矩阵
$\begin{bmatrix}…&…\end{bmatrix}$|`\begin{bmatrix}…&…\end{bmatrix}`|方括号矩阵
$\begin{pmatrix}…&…\end{pmatrix}$|`\begin{pmatrix}…&…\end{pmatrix}`|圆括号矩阵
$\begin{vmatrix}…&…\end{vmatrix}$|`\begin{vmatrix}…&…\end{vmatrix}`|单竖线矩阵
$\begin{Vmatrix}…&…\end{Vmatrix}$|`\begin{Vmatrix}…&…\end{Vmatrix}`|双竖线矩阵
$\begin{Bmatrix}…&…\end{Bmatrix}$|`\begin{Bmatrix}…&…\end{Bmatrix}`|花括号矩阵
$\left\{\begin{matrix}…&…\end{matrix}\right.$|`\left\{\begin{matrix}…&…\end{matrix}\right.`|左单括号矩阵
$\left.\begin{matrix}…&…\end{matrix}\right\}$|`\left.\begin{matrix}…&…\end{matrix}\right\}`|右单括号矩阵
$\begin{cases}…& \text{ if } x=…\end{cases}$|`\begin{cases}…& \text{ if } x=…\end{cases}`|条件等式
$\begin{align*}…&…\end{align*}$|`\begin{align*}…&…\end{align*}`|多行对齐等式









## 公式模板

### 代数

tag|latex|descript
-|-|-
$$\left(x-1\right)\left(x+3\right)$$|`\left(x-1\right)\left(x+3\right)`|algebra_1
$$\sqrt{a^2+b^2}$$|`\sqrt{a^2+b^2}`|algebra_2
$$\left ( \frac{a}{b}\right )^{n}= \frac{a^{n}}{b^{n}}$$|`\left ( \frac{a}{b}\right )^{n}= \frac{a^{n}}{b^{n}}`|algebra_3
$$\frac{a}{b}\pm \frac{c}{d}= \frac{ad \pm bc}{bd}$$|`\frac{a}{b}\pm \frac{c}{d}= \frac{ad \pm bc}{bd}`|algebra_4
$$\frac{x^{2}}{a^{2}}-\frac{y^{2}}{b^{2}}=1$$|`\frac{x^{2}}{a^{2}}-\frac{y^{2}}{b^{2}}=1`|algebra_5
$$\frac{1}{\sqrt{a}}=\frac{\sqrt{a}}{a},a\ge 0\frac{1}{\sqrt{a}}=\frac{\sqrt{a}}{a},a\ge 0$$|`\frac{1}{\sqrt{a}}=\frac{\sqrt{a}}{a},a\ge 0\frac{1}{\sqrt{a}}=\frac{\sqrt{a}}{a},a\ge 0`|algebra_6
$$\sqrt[n]{a^{n}}=\left ( \sqrt[n]{a}\right )^{n}$$|`\sqrt[n]{a^{n}}=\left ( \sqrt[n]{a}\right )^{n}`|algebra_7
$$x ={-b \pm \sqrt{b^2-4ac}\over 2a}$$|`x ={-b \pm \sqrt{b^2-4ac}\over 2a}`|algebra_8
$$y-y_{1}=k \left( x-x_{1}\right)$$|`y-y_{1}=k \left( x-x_{1}\right)`|algebra_9
$$\left\{\begin{matrix}    x=a + r\text{cos}\theta \\     y=b + r\text{sin}\theta  \end{matrix}\right.$$|`\left\{\begin{matrix}    x=a + r\text{cos}\theta \\     y=b + r\text{sin}\theta  \end{matrix}\right.`|algebra_10
$$\begin{array}{l}    \text{对于方程形如：}x^{3}-1=0 \\    \text{设}\text{:}\omega =\frac{-1+\sqrt{3}i}{2} \\    x_{1}=1,x_{2}= \omega =\frac{-1+\sqrt{3}i}{2} \\    x_{3}= \omega ^{2}=\frac{-1-\sqrt{3}i}{2}  \end{array}$$|`\begin{array}{l}    \text{对于方程形如：}x^{3}-1=0 \\    \text{设}\text{:}\omega =\frac{-1+\sqrt{3}i}{2} \\    x_{1}=1,x_{2}= \omega =\frac{-1+\sqrt{3}i}{2} \\    x_{3}= \omega ^{2}=\frac{-1-\sqrt{3}i}{2}  \end{array}`|algebra_11
$$\begin{array}{l}    a\mathop{{x}}\nolimits^{{2}}+bx+c=0 \\    \Delta =\mathop{{b}}\nolimits^{{2}}-4ac \\    \left\{\begin{matrix}    \Delta \gt 0\text{方程有两个不相等的实根} \\    \Delta = 0\text{方程有两个不相等的实根} \\    \Delta \lt 0\text{方程有两个不相等的实根}  \end{matrix}\right.     \end{array}$$|`\begin{array}{l}    a\mathop{{x}}\nolimits^{{2}}+bx+c=0 \\    \Delta =\mathop{{b}}\nolimits^{{2}}-4ac \\    \left\{\begin{matrix}    \Delta \gt 0\text{方程有两个不相等的实根} \\    \Delta = 0\text{方程有两个不相等的实根} \\    \Delta \lt 0\text{方程有两个不相等的实根}  \end{matrix}\right.     \end{array}`|algebra_12

### 几何

tag|latex|descript
-|-|-
$$\Delta A B C$$|`\Delta A B C`|geometry_1
$$a \parallel c,b \parallel c \Rightarrow a \parallel b$$|`a \parallel c,b \parallel c \Rightarrow a \parallel b`|geometry_2
$$l \perp \beta ,l \subset \alpha \Rightarrow \alpha \perp \beta$$|`l \perp \beta ,l \subset \alpha \Rightarrow \alpha \perp \beta`|geometry_3
$$\left.\begin{matrix}    a \perp \alpha \\    b \perp \alpha  \end{matrix}\right\}\Rightarrow a \parallel b$$|`\left.\begin{matrix}    a \perp \alpha \\    b \perp \alpha  \end{matrix}\right\}\Rightarrow a \parallel b`|geometry_4
$$P \in \alpha ,P \in \beta , \alpha \cap \beta =l \Rightarrow P \in l$$|`P \in \alpha ,P \in \beta , \alpha \cap \beta =l \Rightarrow P \in l`|geometry_5
$$\alpha \perp \beta , \alpha \cap \beta =l,a \subset \alpha ,a \perp l     \Rightarrow a \perp \beta$$|`\alpha \perp \beta , \alpha \cap \beta =l,a \subset \alpha ,a \perp l     \Rightarrow a \perp \beta`|geometry_6
$$\left.\begin{matrix}    a \subset \beta ,b \subset \beta ,a \cap b=P \\     a \parallel \partial ,b \parallel \partial   \end{matrix}\right\}\Rightarrow \beta \parallel \alpha$$|`\left.\begin{matrix}    a \subset \beta ,b \subset \beta ,a \cap b=P \\     a \parallel \partial ,b \parallel \partial   \end{matrix}\right\}\Rightarrow \beta \parallel \alpha`|geometry_7
$$\alpha \parallel \beta , \gamma \cap \alpha =a, \gamma \cap \beta =b \Rightarrow a \parallel b$$|`\alpha \parallel \beta , \gamma \cap \alpha =a, \gamma \cap \beta =b \Rightarrow a \parallel b`|geometry_8
$$A \in l,B \in l,A \in \alpha ,B \in \alpha \Rightarrow l \subset \alpha$$|`A \in l,B \in l,A \in \alpha ,B \in \alpha \Rightarrow l \subset \alpha`|geometry_9
$$\left.\begin{matrix}    m \subset \alpha ,n \subset \alpha ,m \cap n=P \\     a \perp m,a \perp n  \end{matrix}\right\}\Rightarrow a \perp \alpha$$|`\left.\begin{matrix}    m \subset \alpha ,n \subset \alpha ,m \cap n=P \\     a \perp m,a \perp n  \end{matrix}\right\}\Rightarrow a \perp \alpha`|geometry_10
$$\begin{array}{c}    \text{直角三角形中,直角边长a,b,斜边边长c} \\    a^{2}+b^{2}=c^{2}  \end{array}$$|`\begin{array}{c}    \text{直角三角形中,直角边长a,b,斜边边长c} \\    a^{2}+b^{2}=c^{2}  \end{array}`|geometry_11

### 不等式

tag|latex|descript
-|-|-
$$a > b,b > c \Rightarrow a > c$$|`a > b,b > c \Rightarrow a > c`|inequality_1
$$a > b,c > d \Rightarrow a+c > b+d$$|`a > b,c > d \Rightarrow a+c > b+d`|inequality_2
$$a > b > 0,c > d > 0 \Rightarrow ac bd$$|`a > b > 0,c > d > 0 \Rightarrow ac bd`|inequality_3
$$\begin{array}{c}    a \gt b,c \gt 0 \Rightarrow ac \gt bc \\    a \gt b,c \lt 0 \Rightarrow ac \lt bc  \end{array}$$|`\begin{array}{c}    a \gt b,c \gt 0 \Rightarrow ac \gt bc \\    a \gt b,c \lt 0 \Rightarrow ac \lt bc  \end{array}`|inequality_4
$$\left | a-b \right | \geqslant \left | a \right | -\left | b \right |$$|`\left | a-b \right | \geqslant \left | a \right | -\left | b \right |`|inequality_5
$$-\left | a \right |\leq a\leqslant \left | a \right |$$|`-\left | a \right |\leq a\leqslant \left | a \right |`|inequality_6
$$\left | a \right |\leqslant b \Rightarrow -b \leqslant a \leqslant \left | b \right |$$|`\left | a \right |\leqslant b \Rightarrow -b \leqslant a \leqslant \left | b \right |`|inequality_7
$$\left | a+b \right | \leqslant \left | a \right | + \left | b \right |$$|`\left | a+b \right | \leqslant \left | a \right | + \left | b \right |`|inequality_8
$$\begin{array}{c}    a \gt b \gt 0,n \in N^{\ast},n \gt 1 \\    \Rightarrow a^{n}\gt b^{n}, \sqrt[n]{a}\gt \sqrt[n]{b}  \end{array}$$|`\begin{array}{c}    a \gt b \gt 0,n \in N^{\ast},n \gt 1 \\    \Rightarrow a^{n}\gt b^{n}, \sqrt[n]{a}\gt \sqrt[n]{b}  \end{array}`|inequality_9
$$\left( \sum_{k=1}^n a_k b_k \right)^{\!\!2}\leq    \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)$$|`\left( \sum_{k=1}^n a_k b_k \right)^{\!\!2}\leq    \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)`|inequality_10
$$\begin{array}{c}    a,b \in R^{+} \\     \Rightarrow \frac{a+b}{{2}}\ge \sqrt{ab} \\     \left( \text{当且仅当}a=b\text{时取“}=\text{”号}\right)  \end{array}$$|`\begin{array}{c}    a,b \in R^{+} \\     \Rightarrow \frac{a+b}{{2}}\ge \sqrt{ab} \\     \left( \text{当且仅当}a=b\text{时取“}=\text{”号}\right)  \end{array}`|inequality_11
$$\begin{array}{c}    a,b \in R \\     \Rightarrow a^{2}+b^{2}\gt 2ab \\     \left( \text{当且仅当}a=b\text{时取“}=\text{”号}\right)  \end{array}$$|`\begin{array}{c}    a,b \in R \\     \Rightarrow a^{2}+b^{2}\gt 2ab \\     \left( \text{当且仅当}a=b\text{时取“}=\text{”号}\right)  \end{array}`|inequality_12
$$\begin{array}{c}    H_{n}=\frac{n}{\sum \limits_{i=1}^{n}\frac{1}{x_{i}}}= \frac{n}{\frac{1}{x_{1}}+ \frac{1}{x_{2}}+ \cdots + \frac{1}{x_{n}}} \\ G_{n}=\sqrt[n]{\prod \limits_{i=1}^{n}x_{i}}= \sqrt[n]{x_{1}x_{2}\cdots x_{n}} \\ A_{n}=\frac{1}{n}\sum \limits_{i=1}^{n}x_{i}=\frac{x_{1}+ x_{2}+ \cdots + x_{n}}{n} \\ Q_{n}=\sqrt{\sum \limits_{i=1}^{n}x_{i}^{2}}= \sqrt{\frac{x_{1}^{2}+ x_{2}^{2}+ \cdots + x_{n}^{2}}{n}} \\ H_{n}\leq G_{n}\leq A_{n}\leq Q_{n}  \end{array}$$|`\begin{array}{c}    H_{n}=\frac{n}{\sum \limits_{i=1}^{n}\frac{1}{x_{i}}}= \frac{n}{\frac{1}{x_{1}}+ \frac{1}{x_{2}}+ \cdots + \frac{1}{x_{n}}} \\ G_{n}=\sqrt[n]{\prod \limits_{i=1}^{n}x_{i}}= \sqrt[n]{x_{1}x_{2}\cdots x_{n}} \\ A_{n}=\frac{1}{n}\sum \limits_{i=1}^{n}x_{i}=\frac{x_{1}+ x_{2}+ \cdots + x_{n}}{n} \\ Q_{n}=\sqrt{\sum \limits_{i=1}^{n}x_{i}^{2}}= \sqrt{\frac{x_{1}^{2}+ x_{2}^{2}+ \cdots + x_{n}^{2}}{n}} \\ H_{n}\leq G_{n}\leq A_{n}\leq Q_{n}  \end{array}`|inequality_13

### 积分

tag|latex|descript
-|-|-
$$\frac{\mathrm{d}}{\mathrm{d}x}x^n=nx^{n-1}$$|`\frac{\mathrm{d}}{\mathrm{d}x}x^n=nx^{n-1}`|calculous_1
$$\frac{\mathrm{d}}{\mathrm{d}x}e^{ax}=a\,e^{ax}$$|`\frac{\mathrm{d}}{\mathrm{d}x}e^{ax}=a\,e^{ax}`|calculous_2
$$\frac{\mathrm{d}}{\mathrm{d}x}\ln(x)=\frac{1}{x}$$|`\frac{\mathrm{d}}{\mathrm{d}x}\ln(x)=\frac{1}{x}`|calculous_3
$$\frac{\mathrm{d}}{\mathrm{d}x}\sin x=\cos x$$|`\frac{\mathrm{d}}{\mathrm{d}x}\sin x=\cos x`|calculous_4
$$\frac{\mathrm{d}}{\mathrm{d}x}\cos x=-\sin x$$|`\frac{\mathrm{d}}{\mathrm{d}x}\cos x=-\sin x`|calculous_5
$$\int k\mathrm{d}x = kx+C$$|`\int k\mathrm{d}x = kx+C`|calculous_6
$$\frac{\mathrm{d}}{\mathrm{d}x}\tan x=\sec^2 x$$|`\frac{\mathrm{d}}{\mathrm{d}x}\tan x=\sec^2 x`|calculous_7
$$\frac{\mathrm{d}}{\mathrm{d}x}\cot x=-\csc^2 x$$|`\frac{\mathrm{d}}{\mathrm{d}x}\cot x=-\csc^2 x`|calculous_8
$$\int \frac{1}{x}\mathrm{d}x= \ln \left| x \right| +C$$|`\int \frac{1}{x}\mathrm{d}x= \ln \left| x \right| +C`|calculous_9
$$\int \frac{1}{\sqrt{1-x^{2}}}\mathrm{d}x= \arcsin x +C$$|`\int \frac{1}{\sqrt{1-x^{2}}}\mathrm{d}x= \arcsin x +C`|calculous_10
$$\int \frac{1}{1+x^{2}}\mathrm{d}x= \arctan x +C$$|`\int \frac{1}{1+x^{2}}\mathrm{d}x= \arctan x +C`|calculous_11
$$\int u \frac{\mathrm{d}v}{\mathrm{d}x}\,\mathrm{d}x=uv-\int \frac{\mathrm{d}u}{\mathrm{d}x}v\,\mathrm{d}x$$|`\int u \frac{\mathrm{d}v}{\mathrm{d}x}\,\mathrm{d}x=uv-\int \frac{\mathrm{d}u}{\mathrm{d}x}v\,\mathrm{d}x`|calculous_12
$$f(x) = \int_{-\infty}^\infty  \hat f(x)\xi\,e^{2 \pi i \xi x}  \,\mathrm{d}\xi$$|`f(x) = \int_{-\infty}^\infty  \hat f(x)\xi\,e^{2 \pi i \xi x}  \,\mathrm{d}\xi`|calculous_13
$$\int x^{\mu}\mathrm{d}x=\frac{x^{\mu +1}}{\mu +1}+C, \left({\mu \neq -1}\right)$$|`\int x^{\mu}\mathrm{d}x=\frac{x^{\mu +1}}{\mu +1}+C, \left({\mu \neq -1}\right)`|calculous_14

### 矩阵

tag|latex|descript
-|-|-
$$\begin{pmatrix}     1 & 0 \\     0 & 1   \end{pmatrix}$$|`\begin{pmatrix}     1 & 0 \\     0 & 1   \end{pmatrix}`|array_1
$$\begin{pmatrix}     a_{11} & a_{12} & a_{13} \\     a_{21} & a_{22} & a_{23} \\     a_{31} & a_{32} & a_{33}   \end{pmatrix}$$|`\begin{pmatrix}     a_{11} & a_{12} & a_{13} \\     a_{21} & a_{22} & a_{23} \\     a_{31} & a_{32} & a_{33}   \end{pmatrix}`|array_2
$$\begin{pmatrix}     a_{11} & \cdots & a_{1n} \\     \vdots & \ddots & \vdots \\     a_{m1} & \cdots & a_{mn}   \end{pmatrix}$$|`\begin{pmatrix}     a_{11} & \cdots & a_{1n} \\     \vdots & \ddots & \vdots \\     a_{m1} & \cdots & a_{mn}   \end{pmatrix}`|array_3
$$\begin{array}{c}    A=A^{T}  \\    A=-A^{T}  \end{array}$$|`\begin{array}{c}    A=A^{T}  \\    A=-A^{T}  \end{array}`|array_4
$$O = \begin{bmatrix}     0 & 0 & \cdots & 0 \\     0 & 0 & \cdots & 0 \\     \vdots & \vdots & \ddots & \vdots \\     0 & 0 & \cdots & 0   \end{bmatrix}$$|`O = \begin{bmatrix}     0 & 0 & \cdots & 0 \\     0 & 0 & \cdots & 0 \\     \vdots & \vdots & \ddots & \vdots \\     0 & 0 & \cdots & 0   \end{bmatrix}`|array_5
$$A_{m\times n}=   \begin{bmatrix}     a_{11}& a_{12}& \cdots  & a_{1n} \\     a_{21}& a_{22}& \cdots  & a_{2n} \\     \vdots & \vdots & \ddots & \vdots \\     a_{m1}& a_{m2}& \cdots  & a_{mn}   \end{bmatrix}   =\left [ a_{ij}\right ]$$|`A_{m\times n}=   \begin{bmatrix}     a_{11}& a_{12}& \cdots  & a_{1n} \\     a_{21}& a_{22}& \cdots  & a_{2n} \\     \vdots & \vdots & \ddots & \vdots \\     a_{m1}& a_{m2}& \cdots  & a_{mn}   \end{bmatrix}   =\left [ a_{ij}\right ]`|array_6
$$\begin{array}{c}    A={\left[ a_{ij}\right]_{m \times n}},B={\left[ b_{ij}\right]_{n \times s}} \\     c_{ij}= \sum \limits_{k=1}^{{n}}a_{ik}b_{kj} \\     C=AB=\left[ c_{ij}\right]_{m \times s}     = \left[ \sum \limits_{k=1}^{n}a_{ik}b_{kj}\right]_{m \times s}  \end{array}$$|`\begin{array}{c}    A={\left[ a_{ij}\right]_{m \times n}},B={\left[ b_{ij}\right]_{n \times s}} \\     c_{ij}= \sum \limits_{k=1}^{{n}}a_{ik}b_{kj} \\     C=AB=\left[ c_{ij}\right]_{m \times s}     = \left[ \sum \limits_{k=1}^{n}a_{ik}b_{kj}\right]_{m \times s}  \end{array}`|array_7
$$\mathbf{V}_1 \times \mathbf{V}_2 =   \begin{vmatrix}     \mathbf{i}& \mathbf{j}& \mathbf{k} \\     \frac{\partial X}{\partial u}& \frac{\partial Y}{\partial u}& 0 \\     \frac{\partial X}{\partial v}& \frac{\partial Y}{\partial v}& 0 \\   \end{vmatrix}$$|`\mathbf{V}_1 \times \mathbf{V}_2 =   \begin{vmatrix}     \mathbf{i}& \mathbf{j}& \mathbf{k} \\     \frac{\partial X}{\partial u}& \frac{\partial Y}{\partial u}& 0 \\     \frac{\partial X}{\partial v}& \frac{\partial Y}{\partial v}& 0 \\   \end{vmatrix}`|array_8

### 三角

tag|latex|descript
-|-|-
$$e^{i \theta}$$|`e^{i \theta}`|trigonometry_1
$$\left(\frac{\pi}{2}-\theta \right )$$|`\left(\frac{\pi}{2}-\theta \right )`|trigonometry_2
$$\text{sin}^{2}\frac{\alpha}{2}=\frac{1- \text{cos}\alpha}{2}$$|`\text{sin}^{2}\frac{\alpha}{2}=\frac{1- \text{cos}\alpha}{2}`|trigonometry_3
$$\text{cos}^{2}\frac{\alpha}{2}=\frac{1+ \text{cos}\alpha}{2}$$|`\text{cos}^{2}\frac{\alpha}{2}=\frac{1+ \text{cos}\alpha}{2}`|trigonometry_4
$$\text{tan}\frac{\alpha}{2}=\frac{\text{sin}\alpha}{1+ \text{cos}\alpha}$$|`\text{tan}\frac{\alpha}{2}=\frac{\text{sin}\alpha}{1+ \text{cos}\alpha}`|trigonometry_5
$$\sin \alpha + \sin \beta =2 \sin \frac{\alpha + \beta}{2}\cos \frac{\alpha - \beta}{2}$$|`\sin \alpha + \sin \beta =2 \sin \frac{\alpha + \beta}{2}\cos \frac{\alpha - \beta}{2}`|trigonometry_6
$$\sin \alpha - \sin \beta =2 \cos \frac{\alpha + \beta}{2}\sin \frac{\alpha - \beta}{2}$$|`\sin \alpha - \sin \beta =2 \cos \frac{\alpha + \beta}{2}\sin \frac{\alpha - \beta}{2}`|trigonometry_7
$$\cos \alpha + \cos \beta =2 \cos \frac{\alpha + \beta}{2}\cos \frac{\alpha - \beta}{2}$$|`\cos \alpha + \cos \beta =2 \cos \frac{\alpha + \beta}{2}\cos \frac{\alpha - \beta}{2}`|trigonometry_8
$$\cos \alpha - \cos \beta =-2\sin \frac{\alpha + \beta}{2}\sin \frac{\alpha - \beta}{2}$$|`\cos \alpha - \cos \beta =-2\sin \frac{\alpha + \beta}{2}\sin \frac{\alpha - \beta}{2}`|trigonometry_9
$$a^{2}=b^{2}+c^{2}-2bc\cos A$$|`a^{2}=b^{2}+c^{2}-2bc\cos A`|trigonometry_10
$$\frac{\sin A}{a}=\frac{\sin B}{b}=\frac{\sin C}{c}=\frac{1}{2R}$$|`\frac{\sin A}{a}=\frac{\sin B}{b}=\frac{\sin C}{c}=\frac{1}{2R}`|trigonometry_11
$$\sin \left ( \frac{\pi}{2}-\alpha \right ) = \cos \alpha$$|`\sin \left ( \frac{\pi}{2}-\alpha \right ) = \cos \alpha`|trigonometry_12
$$\sin \left ( \frac{\pi}{2}+\alpha \right ) = \cos \alpha$$|`\sin \left ( \frac{\pi}{2}+\alpha \right ) = \cos \alpha`|trigonometry_13

### 统计

tag|latex|descript
-|-|-
$$C_{r}^{n}$$|`C_{r}^{n}`|statistics_1
$$\frac{n!}{r!(n-r)!}$$|`\frac{n!}{r!(n-r)!}`|statistics_2
$$\sum_{i=1}^{n}{X_i}$$|`\sum_{i=1}^{n}{X_i}`|statistics_3
$$\sum_{i=1}^{n}{X_i^2}$$|`\sum_{i=1}^{n}{X_i^2}`|statistics_4
$$X_1, \cdots,X_n$$|`X_1, \cdots,X_n`|statistics_5
$$\frac{x-\mu}{\sigma}$$|`\frac{x-\mu}{\sigma}`|statistics_6
$$\sum_{i=1}^{n}{(X_i - \overline{X})^2}$$|`\sum_{i=1}^{n}{(X_i - \overline{X})^2}`|statistics_7
$$\begin{array}{c}    \text{若}P \left( AB \right) =P \left( A \right) P \left( B \right) \\     \text{则}P \left( A \left| B\right. \right) =P \left({B}\right)  \end{array}$$|`\begin{array}{c}    \text{若}P \left( AB \right) =P \left( A \right) P \left( B \right) \\     \text{则}P \left( A \left| B\right. \right) =P \left({B}\right)  \end{array}`|statistics_8
$$P(E) ={n \choose k}p^k (1-p)^{n-k}$$|`P(E) ={n \choose k}p^k (1-p)^{n-k}`|statistics_9
$$P \left( A \right) = \lim \limits_{n \to \infty}f_{n}\left ( A \right )$$|`P \left( A \right) = \lim \limits_{n \to \infty}f_{n}\left ( A \right )`|statistics_10
$$P \left( \bigcup \limits_{i=1}^{+ \infty}A_{i}\right) = \prod \limits_{i=1}^{+ \infty}P{\left( A_{i}\right)}$$|`P \left( \bigcup \limits_{i=1}^{+ \infty}A_{i}\right) = \prod \limits_{i=1}^{+ \infty}P{\left( A_{i}\right)}`|statistics_11
$$\begin{array}{c}    P \left( \emptyset \right) =0 \\    P \left( S \right) =1  \end{array}$$|`\begin{array}{c}    P \left( \emptyset \right) =0 \\    P \left( S \right) =1  \end{array}`|statistics_12
$$\begin{array}{c}    \forall A \in S \\    P \left( A \right) \ge 0  \end{array}$$|`\begin{array}{c}    \forall A \in S \\    P \left( A \right) \ge 0  \end{array}`|statistics_13
$$P \left( \bigcup \limits_{i=1}^{n}A_{i}\right) = \prod \limits_{i=1}^{n}P \left( A_{i}\right)$$|`P \left( \bigcup \limits_{i=1}^{n}A_{i}\right) = \prod \limits_{i=1}^{n}P \left( A_{i}\right)`|statistics_14
$$\begin{array}{c}    S= \binom{N}{n},A_{k}=\binom{M}{k}\cdot \binom{N-M}{n-k} \\    P\left ( A_{k}\right ) = \frac{\binom{M}{k}\cdot \binom{N-M}{n-k}}{\binom{N}{n}}  \end{array}$$|`\begin{array}{c}    S= \binom{N}{n},A_{k}=\binom{M}{k}\cdot \binom{N-M}{n-k} \\    P\left ( A_{k}\right ) = \frac{\binom{M}{k}\cdot \binom{N-M}{n-k}}{\binom{N}{n}}  \end{array}`|statistics_15
$$\begin{array}{c}    P_{n}=n! \\    A_{n}^{k}=\frac{n!}{\left( n-k \left) !\right. \right.}  \end{array}$$|`\begin{array}{c}    P_{n}=n! \\    A_{n}^{k}=\frac{n!}{\left( n-k \left) !\right. \right.}  \end{array}`|statistics_16

### 数列

tag|latex|descript
-|-|-
$$a_{n}=a_{1}q^{n-1}$$|`a_{n}=a_{1}q^{n-1}`|sequence_1
$$a_{n}=a_{1}+ \left( n-1 \left) d\right. \right.$$|`a_{n}=a_{1}+ \left( n-1 \left) d\right. \right.`|sequence_2
$$S_{n}=na_{1}+\frac{n \left( n-1 \right)}{{2}}d$$|`S_{n}=na_{1}+\frac{n \left( n-1 \right)}{{2}}d`|sequence_3
$$S_{n}=\frac{n \left( a_{1}+a_{n}\right)}{2}$$|`S_{n}=\frac{n \left( a_{1}+a_{n}\right)}{2}`|sequence_4
$$\frac{1}{n \left( n+k \right)}= \frac{1}{k}\left( \frac{1}{n}-\frac{1}{n+k}\right)$$|`\frac{1}{n \left( n+k \right)}= \frac{1}{k}\left( \frac{1}{n}-\frac{1}{n+k}\right)`|sequence_5
$$\frac{1}{n^{2}-1}= \frac{1}{2}\left( \frac{1}{n-1}-\frac{1}{n+1}\right)$$|`\frac{1}{n^{2}-1}= \frac{1}{2}\left( \frac{1}{n-1}-\frac{1}{n+1}\right)`|sequence_6
$$\frac{1}{4n^{2}-1}=\frac{1}{2}\left( \frac{1}{2n-1}-\frac{1}{2n+1}\right)$$|`\frac{1}{4n^{2}-1}=\frac{1}{2}\left( \frac{1}{2n-1}-\frac{1}{2n+1}\right)`|sequence_7
$$\frac{n+1}{n \left( n-1 \left) \cdot 2^{n}\right. \right.}= \frac{1}{\left( n-1 \left) \cdot 2^{n-1}\right. \right.}-\frac{1}{n \cdot 2^{n}}$$|`\frac{n+1}{n \left( n-1 \left) \cdot 2^{n}\right. \right.}= \frac{1}{\left( n-1 \left) \cdot 2^{n-1}\right. \right.}-\frac{1}{n \cdot 2^{n}}`|sequence_8
$$\begin{array}{c}    \text{若}\left \{a_{n}\right \}、\left \{b_{n}\right \}\text{为等差数列}, \\    \text{则}\left \{a_{n}+ b_{n}\right \}\text{为等差数列}  \end{array}$$|`\begin{array}{c}    \text{若}\left \{a_{n}\right \}、\left \{b_{n}\right \}\text{为等差数列}, \\    \text{则}\left \{a_{n}+ b_{n}\right \}\text{为等差数列}  \end{array}`|sequence_9
$$(1+x)^{n} =1 + \frac{nx}{1!} + \frac{n(n-1)x^{2}}{2!} + \cdots$$|`(1+x)^{n} =1 + \frac{nx}{1!} + \frac{n(n-1)x^{2}}{2!} + \cdots`|sequence_10

### 物理

tag|latex|descript
-|-|-
$${E_p} = -\frac{{GMm}}{r}$$|`{E_p} = -\frac{{GMm}}{r}`|physics_4
$$\oint_L { \mathord{ \buildrel{ \lower3pt \hbox{$ \scriptscriptstyle \rightharpoonup$}} \over E} } \cdot { \rm{d}} \mathord{ \buildrel{ \lower3pt \hbox{$ \scriptscriptstyle \rightharpoonup$}}  \over l}  = 0$$|`\oint_L { \mathord{ \buildrel{ \lower3pt \hbox{$ \scriptscriptstyle \rightharpoonup$}} \over E} } \cdot { \rm{d}} \mathord{ \buildrel{ \lower3pt \hbox{$ \scriptscriptstyle \rightharpoonup$}}  \over l}  = 0`|physics_6
$$d \vec{F}= Id \vec{l} \times \vec{B}$$|`d \vec{F}= Id \vec{l} \times \vec{B}`|physics_8
$$Q = I ^ { 2 } R \mathrm { t }$$|`Q = I ^ { 2 } R \mathrm { t }`|physics_13
$${E_k} = hv - {W_0}$$|`{E_k} = hv - {W_0}`|physics_15
$${y_0} = A \cos ( \omega {t} + { \varphi _0})$$|`{y_0} = A \cos ( \omega {t} + { \varphi _0})`|physics_19

### 化学

tag|latex|descript
-|-|-
$$\ce{SO4^2- + Ba^2+ -> BaSO4 v}$$|`%此公式需要在【设置】中开启mhchem扩展支持 具体用法请参考【帮助】2.11.2  \ce{SO4^2- + Ba^2+ -> BaSO4 v}`|需要Mhchem扩展支持
$$\ce{A v B (v) -> B ^ B (^)}$$|`%此公式需要在【设置】中开启mhchem扩展支持 具体用法请参考【帮助】2.11.2  \ce{A v B (v) -> B ^ B (^)}`|需要Mhchem扩展支持
$$\ce{Hg^2+ ->[I-]  $\underset{\mathrm{red}}{\ce{HgI2}}$  ->[I-]  $\underset{\mathrm{red}}{\ce{[Hg^{II}I4]^2-}}$}$$|`%此公式需要在【设置】中开启mhchem扩展支持 具体用法请参考【帮助】2.11.2  \ce{Hg^2+ ->[I-]  $\underset{\mathrm{red}}{\ce{HgI2}}$  ->[I-]  $\underset{\mathrm{red}}{\ce{[Hg^{II}I4]^2-}}$}`|需要Mhchem扩展支持
$$\ce{Zn^2+  <=>[+ 2OH-][+ 2H+]  $\underset{\text{amphoteres Hydroxid}}{\ce{Zn(OH)2 v}}$  <=>[+ 2OH-][+ 2H+]  $\underset{\text{Hydroxozikat}}{\ce{[Zn(OH)4]^2-}}$}$$|`%此公式需要在【设置】中开启mhchem扩展支持 具体用法请参考【帮助】2.11.2  \ce{Zn^2+  <=>[+ 2OH-][+ 2H+]  $\underset{\text{amphoteres Hydroxid}}{\ce{Zn(OH)2 v}}$  <=>[+ 2OH-][+ 2H+]  $\underset{\text{Hydroxozikat}}{\ce{[Zn(OH)4]^2-}}$}`|需要Mhchem扩展支持
