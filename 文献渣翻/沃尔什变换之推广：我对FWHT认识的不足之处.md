---
title: 沃尔什变换之推广：我对FWHT认识的不足之处
updated: 2023-03-19 13:16:26Z
created: 2023-03-19 12:59:51Z
latitude: 35.10467400
longitude: 118.35641400
altitude: 0.0000
---

> 原文：[Generalized FWHT: How I realized I did not understand FWHT](https://codeforces.com/blog/entry/96003)
>
> 作者：errogorn@codeforces.com

我们使用记号 $\&,|,\oplus$ 分别表示按位与、或、异或。

# FWHT

给定两个数组 $A,B$，我们想要得到数组 $C$，满足 $C_{i} = \sum_{j\star k}A_{j}B_{k}$ 。对于普通的 FFT 而言，这里的 $\star$ 运算就是简单的加法。

当我在寻找FFT的推广时，我找到了能够完成FWHT的代码。它可以在 $\star$ 为三种按位运算中任一的情况下完成上式的计算。

