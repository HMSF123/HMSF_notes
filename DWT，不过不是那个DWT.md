---
title: DWT，不过不是那个DWT
updated: 2023-03-20 02:25:21Z
created: 2023-03-18 13:35:33Z
latitude: 36.70696200
longitude: 119.16174900
altitude: 0.0000
---

# Part -1 张量

这个名字听起来挺唬人的，但其实很简单。

一个 $N$ 阶张量就是一个 $N$ 维数组，里面的一个元素写作 $A_{i_1,i_2,\cdots,i_N}$。

两个张量的乘积定义为

$$
A_{i_1,i_2,\cdots,i_N}\times B_{i_1,i_2,\cdots,i_M} = C_{i_1,i_2,\cdots,i_{N+M}}
$$

# Part 0 分块矩阵与克罗内克积

分块矩阵，就是元素为矩阵的矩阵（嗯）

分块矩阵有一个性质，两分块矩阵相乘仍然服从矩阵乘法法则，只不过把普通乘法改为矩阵乘法，如下式：

$$
\begin{bmatrix}
A_{1,1} & \cdots & A_{1,K} \\
\vdots & \ddots & \vdots \\
A_{n,1} & \cdots & A_{n,K} \\
\end{bmatrix} \times
\begin{bmatrix}
B_{1,1} & \cdots & B_{1,m} \\
\vdots & \ddots & \vdots \\
B_{K,1} & \cdots & B_{K,m} \\
\end{bmatrix} \\=
\begin{bmatrix}
\sum_{i=1}^{K}A_{1,i}\times B_{i,1} & \cdots & \sum_{i=1}^{K}A_{1,i}\times B_{i,m} \\
\vdots & \ddots & \vdots \\
\sum_{i=1}^{K}A_{n,i}\times B_{i,1} & \cdots & \sum_{i=1}^{K}A_{n,i}\times B_{i,m} \\
\end{bmatrix}
$$

这个证明十分简易，自己画画就可以完成了。

然后介绍一下克罗内克积。

简单来说，我们把一个矩阵“嵌套”在另一个矩阵里面，我们就得到了**克罗内克积**，一般用符号 $\otimes$ 表示。

$$
A\otimes B = \begin{bmatrix}
a_{1,1}B & a_{1,2}B & \cdots & a_{1,m}B \\
a_{2,1}B & a_{2,2}B & \cdots & a_{2,m}B \\
\vdots & \vdots & \ddots & \vdots \\
a_{n,1}B & a_{n,2}B & \cdots & a_{n,m}B \\
\end{bmatrix}
$$

> 没错，这就是张量乘积的一种特殊形式。



# Part 0 沃尔什变换

我们都知道**傅里叶变换**，本质上