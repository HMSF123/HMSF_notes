---
title: ZROI Day 11
updated: 2023-03-09 13:12:03Z
created: 2023-03-09 13:11:12Z
latitude: 35.10467400
longitude: 118.35641400
altitude: 0.0000
---

# Petrozavodsk Winter-2014. Moscow SU Tapir Contest.

### A. Defense of the Ancients

把答案可能的点 $(\sum a,\sum b)$ 标出来，那么答案一定在下凸壳上。

然后结论是，值域为 $V$ 的整点组成的下凸壳是 $\mathcal{O}(V^{2/3})$ 的。https://webspace.maths.qmul.ac.uk/a.fink/polygon6.pdf 经典结论？/jy

然后考虑求出这个下凸壳。

就是先把最边上的两个找出来。然后考虑找出点 $p$ 和 $q$ 之间的下凸壳，就找出点 $o$ 使得 $pq$ 和 $po$ 围成的三角形的有向面积最大。把式子写出来发现就是按照某个权值排序的前 $k$ 小加起来，nth_element．

### C. Combinations Strike Back

单次询问做法：第 $i$ 个数出现次数为 $b_i$，答案就是 $[x^k]\prod\frac{1-x^{b_i+1}}{1-x}$，需要上个分治 NTT．

应该对经典结论产生敏感/fn 不同的 $b$ 只有 $\mathcal{O}(\sqrt n)$ 个，所以对于每个不同的 $b$，暴力乘除单项式就行了。

$\mathcal{O}(n\log^2n+n\sqrt n)$．

### E. Linguistic Estimations

先 reverse 一下。建出 SAM。

然后 $(A,B)$ 合法即为 $endpos(A)$ 中 $\geq |AB|$ 的部分和 $endpos(AB)$ 相同。

那么考虑从 $A$ 去找 $AB$，那么容易知道只有最大的 endpos 来源的那个儿子的子树才可能出现合法的 $AB$，

那么把这个边设置成重边，搞出来这个重（实）链剖分。

这样合法的 $A,AB$ 就只能出现在同一条重链上了。对于每条重链单独考虑。

回顾一下前面的条件，实际上就是 $\max\{endpos(A)\backslash endpos(AB)\}<|AB|$．

既然已经作了重链剖分，对每个节点 $x$ 求出 $mx_x$ 表示轻儿子的 endpos 最大值（也就是走重儿子时被剔除去的 endpos 最大值），这样 $A$ 和 $AB$ 合法当且仅当这条链上的 $mx$ 最大值 $<|AB|$．

然后考虑你当前这个节点如果代表的串长为 $[l,r]$，那么对于其中一个 $x$，其合法的 $A$ 就是重链上连续的一段祖先。那么求出链上最大值最后一个 $<l$ 的，第一个 $>r$ 的，那么就分为了三段贡献，用个什么数据结构支持区间求和就行。

### F. Passing Finals

任意填 $k-1$ 个，然后求解行列式，如果那个 $x$ 的系数不为 $0$ 就有解
如果无解就再继续随机。（注意不要每次特殊填同一个未定位置，因为其系数可能为 0 而其余的未定位置系数不为 0）
随机的概率大概是 Schwartz-Zippel 那个东西。

但是一元多项式好像不能高消求行列式？

但是注意到有：行列式等于它任意一行(列)的各元素与其对应的代数式余子式乘积之和。

所以对于那个变量来说算出它对应的代数余子式就行。

最后这样还需要算这一行剩下所有元素的代数余子式，复杂度就是 $\mathcal{O}(n^4)$．

### H. Angel and Demon

不想记了/shui

> 拒绝偷懒！只有像yx一样内卷才能救SLYZOI！

# Petrozavodsk Summer-2015. Moscow IPT Contest.

### B. Game With A Fairy

主要思路是随机乱搞/zk 直接贺题解。

![image-20230130112739292](note.assets/image-20230130112739292.png)

### D. Maximal Common Subpair

容易想到 $S+\#+T$ 这样拼一下。

然后考虑哪些 $x$ 是可能的，就是正串后缀自动机上每个节点，endpos 在前半部分后半部分都是最靠左，且 len　最大的那个串（因为只需要满足 $x$ 右端点 $<$ $y$ 左端点，所以 $x$ 左端点可以尽可能往左）。

可能的 $y$ 也是一样在反串的 SAM 上找一下就行。

最后扫描线一下就能得到答案了。

### J. Two Airlines

蚌埠住了，正睿刚偷过，但是我没打。

http://zhengruioi.com/contest/1305/problem/2488

做法是一样的。

思路就是维护前 $i$ 个点的哈密顿回路，然后记录切换边权的点 $x$，假设 $x$ 前面是 $a$，后面是 $b$．

如果没有这样的 $x$ 那么可以随便插一下。

否则对于当前要插入的点 $u$，询问一下 $(x,u)$，如果和 $(x,a)$ 相同那么就把 $u$ 插入到 $x,b$ 之间，然后把 $(x,b)$ 问出连上；

否则把 $u$ 插入到 $a,x$ 之间然后把 $(a,x)$ 问出来连上。

总询问次数是 $2n-2$．

# XVII Open Cup named after E.V. Pankratiev. Grand Prix of Japan.

###  C. House Moving

从大到小两边放，这个是比较容易观察到的性质，先证明中间不会有空位，否则可以往一边移动，证明完占据了的一定是一段前缀和一段后缀之后，在前缀/后缀里面内部可以交换让大的靠边更优。

$f_{i,j}$ 表示从大到小考虑放前 $i$ 个，然后放到开头的总和为 $j$ 的总代价，但是这样难以转移。

考虑将代价均摊到每个间隔上，也就是 $f_{i,j}$ 只记录相邻两个都放了数的间隔的贡献，这样就能转移了。如果填在左边就会产生 $j\cdot(sum-j)$，放在右边就是 $(j+P_i)\cdot (sum-j-P_i)$．

最后统计答案的时候再加上剩余间隔的代价贡献就行，也就是 $f_{n,i}+(n-m+1)\cdot(sum-i)\cdot i$．

### E. Eel and Grid

/shui

【待补】

### J. Travel in Sugar Country

nnd 李赛考过这个套路还是没看出来。

用前缀和转成 $\sum |s_{p_i}-s_{p_{i-1}}|$，然后用 JOI Open 2016 摩天大楼 那个套路，从小到大插入，然后记录当前连续段个数以及总和，费用提前计算一下就行。还需要特殊记录开头和末尾是否已经被选了。

### CF1098D

https://www.cnblogs.com/do-while-true/p/16879572.html

### CF1630F

这么有意思/jy

【待补】