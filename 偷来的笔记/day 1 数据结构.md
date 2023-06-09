---
title: day 1 数据结构
updated: 2023-03-27 12:12:39Z
created: 2023-03-08 09:33:00Z
latitude: 35.10467400
longitude: 118.35641400
altitude: 0.0000
---

> 注释部分为yhy加上的

### P6109

省集讲过。

> 我没听过啊，我得写写

> 思路有点像猫树，对横坐标进行线段树分治，但是不要把每个询问砍成 $O(\log n)$ 个线段树区间，而是在它“跨过中点”的那些节点上，把一个询问劈成左右两半，从中点开始向左向右分别处理这两段，因为是前缀/后缀的形式，可以直接维护历史最大值，而不需要考虑修改出现、结束的位置。

### P8512

做过

> 我没做过啊，我得写写

> 算了不写了，dyx写过这题的题解。

### P8337

对于一个区间来说其合法必须要满足颜色个数等于线性基大小，而线性基大小一定是 $2^k$，CF1100F 给出了一个 1log 的区间线性基做法，对右端点扫描线之后线性基每个主元选取最靠后的。颜色数为 $2^k$ 的右端点也是容易扫描线出来的。

那么做个扫描线，就可以统计出来对于每个左端点，其合法的右端点是若干个区间。

那么问题就变成了给定 $\mathcal{O}(n\log n)$ 个线段，然后做数点。

强制在线，扫描线之后搞个区间加的主席树可以直接 $\mathcal{O}(n\log^2n)$ 解决。能不能给力点？

考虑一条线段 $(l_1,l_2,r)$，性质是 $l_1,l_2$ 关于 $r$ 都是单调的。

> 三元组的意义是：对于一个 $k$，以 $r$ 为结尾，共有 $k$ 种颜色的区间们的左端点所组成之连续区间为 $[l_1,l_2]$ .
> 
> 可以证明这些左端点一定是连续的
> 
> 三元组中省去了一个 $k$，需要注意一下

将 $(l_1,l_2,r)$ 差分成 $(1,l_2,r)-(1,l_1,r)$，分开计算。那么问题变成了有若干满足左右端点分别单调的区间 $[l,r]$，对于询问 $[L,R]$，查询 $L\leq l\leq r\leq R$ 的 $l-L+1$ 之和，考虑这个一定是编号连续的一段区间，

> 这句话指的应该是 $[L,R]$ 中所有能产生贡献的贡献点（诸如 $(1,l_{1/2},r)$ 的三元组）的 $r$ 元构成一个连续区间。

找到其开头和结尾就能利用前缀和 $\mathcal{O}(1)$ 算出总贡献。也就是找一下 $L$ 在 $l$ 里的后继，以及 $R$ 在 $r$ 里的前驱。由于只有 $k\leq \log a$，所以对于每个 $k$ 线性预处理一下每个位置的前驱后继，以及什么信息的前缀和，使得可以 $\mathcal{O}(1)$ 询问。

> k指的是线性基大小的对数。

这样复杂度就是 $\mathcal{O}(n(\log A+\log n))$ 的了。

上面是 lxl 讲的时候做法，xtq 换另一个方向差分也能 $\mathcal{O}(1)$ 查询。

### P8265

不懂LCT

> 不，你懂
> 
> 假如我们用 LCT 中的虚实边来模拟树链剖分中的轻重边，那么只有在原树拼接、剪切、换根的时候才需要改变轻重关系（不能随便 `Access` 了，但是可以随意 `Splay`），这一点需要注意一下。
> 
> 先考虑换根的情况，经典做法是把一条根链对应的辅树全局翻转，但因为刚刚提到的原因，翻转完毕之后拍屁股走人是不正确的，我们需要把一些实边改为虚边。考虑一次换根之后需要改为虚边的实边只有 $O(\log n)$ 条，所以暴力修改是可取的。
> 
> 如何找到要断开的实边？考虑一个点 $u$ 的实儿子 $v$ 不再是重儿子的**必要**条件是 $siz(u)\geq 2siz(v)$，那么根据二的次幂的分布定律可知此条件等价于 $\exist k,siz(u) \geq 2^k > siz(v)$ .
> 
> 同时注意到在一条链所有需要修改的点中，各个点的 $k$ 都是唯一的（一个 $k$ 的取值最多对应一条需要修改的边），我们可以枚举 $k$，在此过程中找到最深的满足 $siz(x)\geq 2^k$ 的点 $x$，并判断它是否应该断开实边。
> 
> - 如何寻找 $x$？有单调性，二分。怎么二分？因为每条链都是一棵平衡树，本身就是二分结构，从根往下找就行了，从父节点往左子节点走的时候注意加上右子节点的大小，因为每棵辅树中非根节点的信息是不完整的。（这个过程中一定要及时 `Pushdown`）
> 
> - 找到之后如何判断是否应该断边？这是简单的，只需要看看 $x$ 最大的虚儿子是否在双关键字意义下比 $x$ 的实儿子大就可以了。这里使用 `set` 维护每个节点的虚儿子的双关键字大小。
> 
> 每次 `Access` 或者 `MakeRoot` 的时候执行一下上面的判断操作，就可以保证树的形态仍然是正确的。

> Link 或者 Cut 的情况，先把被链接的点 $x$ `Access`，然后把要连上的点 `MakeRoot`，连上虚边 $(x,y)$ 之后，对 $(Root_x,x)$ 这整条链执行上面的断边操作，树的形态仍然是正确的。因为受影响的只有连上新子树的这一条根链。

### P6780

以 $c$ 为大小分块，那么答案就是块内答案，或者一个块的后缀和它后面那个块的前缀拼起来。

考虑一次查询：

整块的块内答案，单点改的时候更新一下就行。散块的块内答案直接在最大子段和的线段树上查一下就行。

然后考虑跨块询问。现在仅讨论整块和整块的：考虑前面一个块的后缀选 $i$ 个的和是 $a_i$，后面一个块的前缀选 $j$ 个的和是 $b_j$，

> 这里的 $a,b$ 均指一个块内的序列，而不是整个序列。

那么就是查询 $\max\{a_i+b_j\},i+j\leq c$，巧妙的转化是将 $a$ reverse 成 $a'$，这样子就是查询 $\max\{a'_i+b_j\},1\leq i\leq j\leq c$，

> 换句话说，就是坐标轴上的横轴翻转，把副对角线三角改为便于维护的主对角线三角。

这样就线段树维护分治信息（维护 $a'$ 的区间 $\max$，$b$ 的区间 $\max$，以及答案即可）

> 具体是怎么维护的？把整个序列复制一份，各个块内部翻转，块的整体顺序不变。然后把这个翻转过的新序列向右移动一块，与上面的原序列配对。对这两个序列开一棵线段树共同维护。
> 
> 对于一个区间，维护原序列的最大值、翻转序列的最大值、这个区间的答案。上传方式不言自明。

> 什么？你问要是不能分成等长整块怎么办？补零啊！

如果有个散块的话，就是询问 $l\leq i\leq j\leq c$ 这样子的形式。

### P6105

首先查 $i+j-C$ 的 $\max$，直接最大值和次大值加起来。

然后查 $\max\{i+j\},i+j\leq C-1$，然后 $j$ reverse 一下，就变成上一题，为了满足 $i\neq j$ 还需要记录一下次大次小。

> 即“二者相加不超过某上限情况下的最大值”可以用这种方法做。
> 
> 形式化地，是
> $$\max_{i+j\leq K}\{f(a_i,b_j)\} = \max_{i\leq j\leq K}\{f(\overline{a}_i,b_j)\}$$
> 这里的情况就是 $a_i = b_i = [i\in S]\times i$

### P6018

做过（没写过代码）

> 可惜我没做过。

> 首先知道一个技巧，如果把一堆数分解二进制之后从低位到高位建立 01Trie（越靠近根越低），那么所有数 $+1$ 就相当于从根节点开始递归地交换两个儿子。

> 每个节点维护一个 01Trie，里面装这个点所有儿子的权值。第一种修改相当于对一个点的权值 Trie 全局 $+1$，并对他父亲的父亲的权值 Trie 单点修改。第二种修改就是普通的单点修改。查询的话每个 Trie 维护着异或和就行了。

### P8336

别乳卡。

> 感觉像呐球，所以是 Boruvka . 没有学过 Boruvka 的话可以戳OIwiki。

> 需要这么一个前置知识：维护若干个集合，每个集合内颜色全部相同，支持合并集合、对于每个点查询颜色和他不一样的点中权值最小的那个。
> 
> 可以看到只需要维护全局最小值、除去全局最小值所在集合后的全局最小值，对于任意一次询问，答案必将在这两者之中出现。
> 
> 下面做法中出现的**所有数据结构**一律维护类似这两个值的东西，下面所说的“查询”也指查询这两个东西。

> 下面是yhy插入的部分

接下来分类讨论！

设某次查询时，问题中的点为 $(x_1,y_1)$，答案为 $(x_2,y_2)$

1. 答案与问题两个点对的两位都无关，那么直接维护 $siz(x) + siz(y)$ 的最小、异色次小，甚至不需要搜索整棵树，因为这种算法只会算多，即便两个点对存在父子关系，也会被刷新掉。
2. 答案与问题只有一维存在祖先关系，这样的两个询问之间的距离（边权）为 $siz(x_1) + siz(x_2) + siz(y_1)  - siz(y_2)$，那么直接暴力DFS整棵树，对于一个问题 $(x_1,y_1)$，查询 $y_1$ 子树中的 $y_2$ 最大值，并计算答案，不必关心 $x_2$ 与 $x_1$ 的父子关系，因为就算二者存在父子关系，按照上式计算一定大于等于真实权值，会被下面的做法刷新掉。（当然，为了处理父子关系在 $x$ 这一位上的情况，把上面的过程对称过来重复一遍）
3. 答案与问题的两位分别都是祖先关系，这一种情况还要再分出三种：
   1. 答案的两位都是问题的后代。这种情况下，需要求的东西为
        $$ \min_{(x_2,y_2),x_2\in Subtree(x_1),y_2\in Subtree(y_1)}\{siz(x_1) + siz(y_1) - siz(x_2) - siz(y_2)\}$$
        这是个树上子树（二维）数点问题。那么我们还是 DFS遍历整棵树，对于每个点维护一棵线段树，线段树 $u$ 中存放所有【 $x_k\in Subtree(u)$ 的 $(x_k,y_k)$ 的 $siz(x_k) + siz(y_k)$ 】，存储位置的下标为【所对应 $y_k$ 的DFN序号】。剩下的不言自明。需注意的是，父节点的线段树由所有子节点的线段树合并而来。
   2. 答案的一位是问题的祖先，一位是问题的后代。要求的是
      $$ \min_{(x_2,y_2),x_2\in Subtree(x_1),y_2\in Rootchain(y_1)}\{siz(x_1) - siz(x_2) + siz(y_2)  - siz(y_1)\}$$
      和
      $$ \min_{(x_2,y_2),x_2\in Rootchain(x_1),y_2\in Subtree(y_1)}\{siz(x_2) - siz(x_1) + siz(y_1) - siz(y_2)\}$$
      不妨假设假设问题的 $y$ 是答案的后代（第一种情况）。
      问题的 $x$ 是答案的祖先。那么我们还是 DFS，把所有【 $y_k\in Rootchain(u)$ 的 $(x_k,y_k)$ 的 $siz(y_k) - siz(x_k)$ 】塞进一棵线段树里，下标位置在【 $x_k$ 的DFN序号】，剩下的依然不言自明。需注意，这里不再线段树合并，而是一边DFS一边修改线段树里的东西。
      然后别忘了这玩意要进行两遍。
   3. 问题的两位都是答案的后代。这种情况和上一种类似，只不过查询从子树查询改成了根链查询，可以用全局平衡二叉树或者LCT维护，并且保存的权值改为 $siz(x_k)+siz(y_k)$，保存的位置为LCT中的 $x_k$ 点，每次查询只需要把根链 Access 出来就可以了。

当然，在进行以上的全部过程时，千万不要忘了每个点对既是问题也是可能答案。

> yhy 插入的部分结束
> 
> 震撼人心的大分讨，lxl不愧是你

---

### P8530

对第一维莫队，然后问题相当于每次插入/删除一个 $(p_i,a_i)$，查询 $l_2\leq p_i\leq r_2$ 中出现的所有颜色的权值和。然后考虑一个颜色的贡献在二维平面上的贡献，每次插入删除的时候就是 $\mathcal{O}(1)$ 个矩形加。需要支持查询前驱后继，还有矩形加，单点查值。

> 指的是在序列空间内查询新插入数的前驱后继（注意在前面插入的时候要查询后继），然后在询问空间里矩形加、单点查。
> 
> 其中矩形修改操作有 $O(n\sqrt{n})$ 次，单点查询操作有 $O(q)$ 次。

支持查询前驱后继可以用只删除的回滚莫队，然后用链表就能知道前驱后继。

> 具体来讲，回滚莫队因为在区间尾部只删不增，在区间头部也是只删不增，每次通过回滚操作复原。所以可以轻易对于每一种颜色维护链表，一个位置连向上一个、下一个相同颜色的位置，删的时候直接断开指针。

第二问是 $\mathcal{O}(n\sqrt n)$ 个矩形加，$\mathcal{O}(n)$ 个矩形查询（这里默认 $n,q$ 同阶了），用分块来平衡复杂度 $\mathcal{O}(1)$ 修改 $\mathcal{O}(\sqrt n)$ 查询。

后面怎么二维分块，先转成单点改，2-side（右下角） 求和，具体咋分，待补。

> 帮你补补
> 
> 为了平衡复杂度，我们要利用分块做到 $O(1)$ 矩形修改，$O(\sqrt{n})$ 单点查询。这显然是不现实的，所以考虑差分之后变成 $O(1)$ 单点修改，$O(\sqrt{n})$ 矩形查询。
> 
> PS：具体差分的方法是在区间平面上，对于每个颜色，从斜轴上向左上画2side矩形，把边的交点都赋上 -1，矩形的右下角赋上+1，求和就向右下角求和。
> 
> 好了，现在开始二维分块。二维分块有三层结构：大块、小块、散块。其中大块为至少有一边长为 $\sqrt[4]{n^3}$ 的矩形，小块为边长 $\sqrt{n}$ 的正方形，散块为至少有一边为 $1$ 的矩形。
> 本质上相当于一个两层的线段树。

![ZROIep1img0.png](../../_resources/ZROIep1img0.png)

> 上图就是一张二维分块的例图，每次单点修改的时候直接在对应的块里暴力修改，查询的时候整大直接加上，大块不整就把整小块加上，剩下的散块单独处理。在本题中可以枚举每列每行，因为本题中每列、每行最多有两个修改点。
> 
> 复杂度容易证明，这里略去了。

### P8528

> 老题面没了，这里备份一下。

![](https://cdn.luogu.com.cn/upload/image_hosting/0vu8cqkz.png)

> 其中 $1\leq n,m \leq 2\times 10^5$ .
> 时限 4s，空限 2GB .

把贡献差分一下，$[1,i-1]-[1,l-1]$，$[1,i-1]$ 对 $i$ 的贡献可以直接预处理。

然后考虑分块。前 $j$ 个块对 $i$ 的贡献也可以直接预处理（每次处理前 $j$ 个块，前缀和）。然后分块。

> 这两句话解决了整块的问题，下面的一切都是在处理散块。

再考虑散块的贡献，枚举散块里每个位置 $p$，对 $[l,r]$ 里面 $\geq a_p$ 的 $a_i$，让其 $b_i\gets b_i+1$．

> 这一句就是把散块贡献的处理再次离线出来的意思。

把 3-side 差分成 2-side，

> 这个平面横轴是下标，纵轴是下标上的值。刚刚考虑散块的贡献就相当于在这个平面上进行一次 3-side 加一。

然后问题就变成了 $\mathcal{O}(n\sqrt n)$ 次 2-side 加，$\mathcal{O}(n)$ 次单点查。转成单点加，2-side 查（注意是边修改边查询）。

> 差分的方法是 3-side 左角加一，右角减一。

> 有 n 个散块要处理，每个散块会拆出来 \sqrt{n} 个修改操作。

对横坐标搞个 cdq 分治，扫时间维，然后内层分块做就行了。$\mathcal{O}(n\sqrt n\log n)$．

> 不使用树状数组是为了平衡复杂度。

> 来自xtq的经验法则：常数方面：树状数组 < 分治 < 线段树 < 倍增 。倍增大是因为它的内存访问是完全随机的。

### P8527

看上去就是个卷积啊！但是这里是区间挪过去加，不是整体挪过去加。那就分块，散块直接改，整块的话那就是卷积，记录下多项式。最后每一个块 FFT 一下。

> 具体来说，把 $a$ 序列分块，每一次相当于把一个区间 $[l,r]$ 平移一段距离之后加到 $b$ 上，把这个区间分成散块和整块，散块直接暴力加，整块先留着最后处理。
> 接下来考虑把所有操作都进行完毕之后，每一个整块会被向后移动若干次，最后累加。如果把整块视作 $a_lx^l+\cdots+a_rx^r$，那么移动 $k$ 格就相当于乘上 $x^k$，如果要同时处理所有平移，乘上 $x^{k_1}+x^{k_2}+\cdots$ 就可以了。顺便注意这里可能会出现负数幂，所以应该乘上 $x^{k+n}$，最后在偏移 $n$ 格的位置统计答案。

> PS：块长为了平衡，应当取到 $B=n\sqrt{\frac{\log n}{q}}$

### P8526

区间的所有子区间的颜色数按位异或和。

> 看到子区间，想想扫描线。

> 定义两个扫描线的辅助序列 $a,b$，其中 $a_l = |\{A_i|l\leq i \leq r\}|$，$b_l = \mathop{\text{xor}}\limits_{j=l}^{r}{a_j}$

对 $r$ 作扫描线，问题变成：

1. $a$ 区间 $+1$；
2. $\forall 1\leq i\leq n,b_i\oplus=a_i$；
3. 查询 $b$ 区间异或和。

> 其实1,3操作中的区间都是后缀。

然后要维护这几个信息：

.....待补

> 帮你补补。
> 
> 考虑将 $a,b$ 序列分块维护，对于整块修改想办法打标记，对于散块修改想办法暴力重构。
> 
> 首先，自然需要维护每个整块内 $b_i$ 的异或和。
> 
> 考虑修改操作 2，进行此操作后，每一个整块都相当于异或上了 $a$ 在这一整块内的异或和，所以我们为了标记需要维护 $a$ 的异或和。
> 
> 然后考虑操作 1，某个整块 $[L,R]$ 进行完此操作后需要给【 $b$ 上的这个整块】打一个【总体异或上 $\text{xor}_{k=L}^{R}{a_k+1}$ 】的标记。暴力的想法是此时直接把 $a$ 打上 $+1$ 的tag并重构，然后计算出新的整块异或和。但是这样显然会爆炸，考虑如何优化。
> 
> 我们用一种类似于在时间上分块的策略，先把 $+1$ 的操作累加起来，等到它们积攒到一定程度之后统一处理。
> 
> 具体来说，对每一个整块维护 $a_i+j,j\in[1,B]$ 的异或和，其中 $B$ 是一个我们设定的阈值。我们一旦发现有 $a$ 的 `tag` 的值超过了这个阈值，就暴力重构 $a,b$ 两个序列上此块的所有标记。
> 
> 重构（下传标记）$a_i$ 是简单的，直接加上对应的 `tag` 就可以了，但是重构 $b$ 需要着重讨论，我们需要对于 $b_i,i\in[l,r]$ 中的每一个元素异或上一系列的 $a_i+tag_k$（会有很多不同的 $tag_k$），也就是在这段时间里累积的操作2序列。当然，还需要注意我们需要处理处一些新的 $a_i+j$ 的异或和，以便以后标记用。
> 
> 回过头来，我们梳理一下需要支持哪些操作：
> 
> - A. 散块重构（暴力加一、暴力异或，用在每次询问两端的散块）
> - B. $a$ 中整块 `tag++`
> - C. 给 $b$ 的某个块追加一个标记。
> - D. 标记累积超过阈值时，暴力重构 $b$ 的某个块，同时求出一些新的 $\text{xor}_{k=l}^{r}(a_i+j),j\in[1,B]$ .
> 
> 哈哈！我理解了！
> 
> 刚刚提到的四种操作中，前三个操作都是简单的，主要的难点在于操作D。而我们的目标是在 $O(\frac{n}{B})$ 的时间复杂度内完成一次操作D，因为这样的话如果取 $B=\sqrt{n}$，那么所有操作D的总用时为 $O(n)$ 级别的。
> 
> 考虑解决方法，一次操作 D 可以分成两部分：
> 
> 1. 对于每一个 $K\in[1,B]$，求出 $\text{xor}_{k=l}^{r}(a_i+K)$ .
> 2. 对于每一个 $i\in[L,R]$，求出 $b_i\,\text{xor}_{k=1}^{tagnum}(a_i + tag_k)$
> 
> 发现后面的这个本质上是前面的子问题，因为 $\forall k\leq tagnum,tag_k\in[1,B]$ . 所以我们想办法解决前面的那个操作就行了。
> 
> 接下来是第一处奇妙转化！因为异或操作是不进位的，我们把 $a_i+K$ 拆成单个的二进制位处理，依次计算每一位的结果后加起来，式子大概长这样：
> $$\sum_{d=1}^{dgt}\left(\mathop{\text{xor}}\limits_{i=L}^{R}{\left( (a_i+ K)\overleftarrow{[d]} \right)}\right)$$
> 
> 顺便介绍一下我~~乱写~~使用的符号：$x\overleftarrow{[d]}$ 表示把 $x$ 的二进制表达的倒数第 $d$ 位拿出来得到的数，$x\overleftarrow{[d,1]}$ 表示把 $x$ 的二进制表达的倒数**前** $d$ 位拿出来得到的数。以后要多次用到这两个东西，所以先说一下。
> 
> 但是这个式子依然不够优雅，它还需要我们计算 $a_i+K$ . 为了移除掉这个加法，我们尝试把进位单独拿出来考虑。
> $$\sum_{d=1}^{dgt}\left(\mathop{\text{xor}}\limits_{i=L}^{R}{\left( a_i\overleftarrow{[d]} \oplus K\overleftarrow{[d]} \oplus \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)}\right)$$
> 
> 看着挺乱的，但是梳理一下就会发现本质上就是把加法拆成了【异或+进位】的形式。
> 
> 我们又发现，现在和式中的每一项都各自占用恰好一个二进制位（邻位互不影响），所以我们可以把求和拿进异或里面。不过别急着拿，先稍加转化。
> 
> 第二次妙妙转化！我们发现 $K\in[1,B]$，但 $a_i$ 的值域要大得多，所以当 $d$ 大于某个值之后，会有 $K\overleftarrow{[d-1,1]} = K,K\overleftarrow{[d]} = 0$，也就是说，和式如果这样拆成两半：
> $$\begin{gather*} \sum_{d=1}^{dgt(\sqrt[]{n})}\left(\mathop{\text{xor}}\limits_{i=L}^{R}{\left( a_i\overleftarrow{[d]} \oplus K\overleftarrow{[d]} \oplus \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)}\right)          \\+\sum_{d=dgt(\sqrt[]{n})+1}^{dgt}\left(\mathop{\text{xor}}\limits_{i=L}^{R}{\left( a_i\overleftarrow{[d]} \oplus K\overleftarrow{[d]} \oplus \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)}\right)\end{gather*}$$
> 就可以对下面的那个化简：
> $$
> \begin{gather*}
> \sum_{d=1}^{dgt(\sqrt[]{n})}\left(
> \mathop{\text{xor}}\limits_{i=L}^{R}{\left( a_i\overleftarrow{[d]} \oplus 
> K\overleftarrow{[d]} \oplus
> \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)}\right)          \\
> +\sum_{d=dgt(\sqrt[]{n})+1}^{dgt}\left(
> \mathop{\text{xor}}\limits_{i=L}^{R}\left( a_i\overleftarrow{[d]}  \oplus 
> \Big[a_i\overleftarrow{[d-1,1]} + K \geq 2^{d-1}  \Big]2^{d-1}
> \right)
> \right)
> \end{gather*}$$
> 就是现在！交换下方式子的运算顺序：
> $$\begin{gather*} \sum_{d=1}^{dgt(\sqrt[]{n})}\left(\mathop{\text{xor}}\limits_{i=L}^{R}{\left( a_i\overleftarrow{[d]} \oplus K\overleftarrow{[d]} \oplus \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)}\right)          \\+\mathop{\text{xor}}\limits_{i=L}^{R}{  \left( \left(\sum_{d=dgt(\sqrt[]{n})+1}^{dgt} a_i\overleftarrow{[d]}\right)  \oplus \left(\sum_{d=dgt(\sqrt[]{n})+1}^{dgt}\Big[a_i\overleftarrow{[d-1,1]} + K \geq 2^{d-1}  \Big]2^{d-1}\right)\right)}\end{gather*}$$
> 接下来要干什么不言自明：
> $$\begin{gather*} \sum_{d=1}^{dgt(\sqrt[]{n})}\left(\mathop{\text{xor}}\limits_{i=L}^{R}{\left( a_i\overleftarrow{[d]} \oplus K\overleftarrow{[d]} \oplus \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)}\right)          \\+\mathop{\text{xor}}\limits_{i=L}^{R} \left( a_i\overleftarrow{[dgt,dgt(\sqrt[]{n}) ]} \right)  \oplus \mathop{\text{xor}}\limits_{i=L}^{R}\left(\sum_{d=dgt(\sqrt[]{n})+1}^{dgt}\Big[a_i\overleftarrow{[d-1,1]} + K \geq 2^{d-1}  \Big]2^{d-1}\right)\end{gather*}$$
> 下方左侧的式子可以 $O(1)$ 求（维护全体异或和后删掉后若干位），下方右侧的式子要么为 $0$，要么本质上是求一个 `lowbit`，总之稍加思索就能在 $O(\sqrt{n})$ 内做完。现在问题就解决了一半了，我们主要关注上方的式子：
> $$ \sum_{d=1}^{dgt(\sqrt[]{n})}\left(\mathop{\text{xor}}\limits_{i=L}^{R}{\left( a_i\overleftarrow{[d]} \oplus K\overleftarrow{[d]} \oplus \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)}\right)$$
> 还是把求和拿到里面：
> $$\mathop{\text{xor}}\limits_{i=L}^{R}{\left(  \sum_{d=1}^{dgt(\sqrt[]{n})}a_i\overleftarrow{[d]} \oplus  \sum_{d=1}^{dgt(\sqrt[]{n})} K\overleftarrow{[d]} \oplus  \sum_{d=1}^{dgt(\sqrt[]{n})}\Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)}$$
> 化简：
> $$\mathop{\text{xor}}\limits_{i=L}^{R}\left(  a_i\overleftarrow{[dgt(\sqrt{n}) , 1]} \right)\oplus \mathop{\text{xor}}\limits_{i=L}^{R} \left( K\overleftarrow{[dgt(\sqrt{n}) , 1]} \right)  \oplus\mathop{\text{xor}}\limits_{i=L}^{R}\left( \sum_{d=1}^{dgt(\sqrt[]{n})}\Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)$$
> 前两个式子都能在 $O(\sqrt{n})$ 的时间内对所有 $K$ 计算出值来，最终我们只需要找到方法快速计算这个：
> $$\mathop{\text{xor}}\limits_{i=L}^{R}\left( \sum_{d=1}^{dgt(\sqrt[]{n})}\Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)$$
> 发现求和如果在异或里面仍无从下手，重新把它拿出来：
>  $$\sum_{d=1}^{dgt(\sqrt[]{n})} \mathop{\text{xor}}\limits_{i=L}^{R}\left( \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]2^{d-1}\right)$$
>  利用奇偶性：
>  $$
>  \sum_{d=1}^{dgt(\sqrt[]{n})}
>  \begin{cases}
>  2^{d-1} & \sum_{i\in[L,R]} \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]为奇数 \\ \\
>  0 &  \sum_{i\in[L,R]} \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}  \Big]为偶数\\
>  \end{cases} $$
> 没有异或运算了！现在一切的一切归纳为一个函数：
> $$
> f(d,K) = \sum_{i\in[L,R]} \Big[a_i\overleftarrow{[d-1,1]} + K\overleftarrow{[d-1,1]} \geq 2^{d-1}\Big]
> $$
> 因为解析式里只用到了 $K$ 的最后若干位。所以状态空间仍然有重叠，可能会有两个不一样的 $K$ 对应同一个 $f(d,K)$ . 因此定义新函数 $F(d,x)$ 如下，其中 $d\in[1,dgt(\sqrt{n})],x<2^{d-1}$
> $$
> F(d,x) = \sum_{i\in[L,R]} \Big[a_i\overleftarrow{[d-1,1]} + x \geq 2^{d-1}\Big]
> $$
> 现在空间只有 $O(\sqrt{n})$ 级别了。
> 
> 接下来是第三个妙妙转化！我们考虑从小到大枚举 $d$，通过类似动态规划的方法求出 $F$ 函数的取值。
> 
> 考虑 $a_i + x$ 的倒数第 $t+1$ 位接受到的后面低位的进位是怎么加出来的，一共有四种情况：
> $$\begin{array}{c|c|c|c}
> 编号 & a_i\overleftarrow{[t]} & x\overleftarrow{[t]} & (a_i+x)\overleftarrow{[t-1]}的进位 \\ \hline
> 1& 1 & 1 & 1\\
> 2& 0 & 1 & 1\\
> 3& 1 & 1 & 0\\
> 4& 1 & 0 & 1\\
> \end{array}$$
> 将前两种情况归为一类，此时如果不考虑 $a_i\overleftarrow{[t]}$，在倒数第 $t$ 位上会有两个 $1$；把后两种情况编为一类，此时如果不考虑 $a_i\overleftarrow{[t]}$，在倒数第 $t$ 位上只有一个 $1$ . 接下来对两类分别转移：
> $$
> \begin{gather*}
> F(d+1,y\oplus 2^{d-1}) \leftarrow F(d+1,y\oplus 2^{d-1}) + F(d,y) \\
> F(d+1,y) \leftarrow F(d+1,y) + F(d,y)
> \end{gather*}$$

问题在于怎么重构，后面有点高，让我缓缓。

### P7722

先转化成单点修改 $a,b,c$，查询 $i<j<k\leq r,a_i=b_j=c_k$ 的 $(i,j,k)$ 个数。

根号分治，总出现次数 $<sqrt$ 的，暴力把这个颜色的 dp 数组改下，然后区间加一下就行。

$>sqrt$ 的，对于每个颜色搞个分块维护动态 dp，$\mathcal{O}(\sqrt n)$ 单点修改 $\mathcal{O}(1)$ 查询矩阵连乘积就行。当然实现上不能直接写成矩阵乘法的形式，还是只维护有用的点值这样子。

### UOJ 712

被 $cmin$ 的 $a_i$ 一定是单调的，而且形成了一个凸包，称这个为”被影响的“。

那就分成两部分，在凸包上的，和不在凸包上的，然后还有把不在凸包上的移动到凸包上。

在凸包上的和不在凸包上的都能用线段树来解决。

求出每个点什么时候被移到凸包上，就整体二分。

### UOJ 715

美丽的树是没有被点亮的节点形成一个连通块，然后对 $a$ 搞线段树维护每个前缀的 "没有被点亮的节点形成的森林的" $V-E$ 的个数，值是 $1$ 的才是美丽的树。假设这个权值是 $a$．

然后考虑答案相当于有多少条边是连接亮点和暗点的，假设这个值是 $b$．

然后考虑一条边的贡献，相当于对 $a$ 的区间减，对 $b$ 的区间加，询问就是问 $a_i=1$ 的 $b_i$ 的和。

线段树就行了。维护 $a$ 的区间 $\min$ 以及 $a_i=\min$ 的 $b_i$ 的和。
