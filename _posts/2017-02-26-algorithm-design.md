---
layout: post
category : 算法
title: '常用算法设计思路'
tagline: ""
tags : [算法]
---

## 分治算法

### 原理

分而治之，就是把一个复杂的问题分成两个或更多的相同或相似的子问题，直到最后子问题可以简单的直接求解，原问题的解即子问题的解的合并。

对于一个规模为n的问题，若该问题可以容易地解决（比如说规模n较小）则直接解决，否则将其分解为k个规模较小的子问题，这些子问题互相独立且与原问题形式相同，递归地解这些子问题，然后将各子问题的解合并得到原问题的解。

### 步骤

1. 分解：将原问题分解为若干个规模较小，相对独立，与原问题形式相同的子问题

2. 解决：若子问题规模较小且易于解决时，则直接解。否则，递归地解决各子问题

3. 合并：将各子问题的解合并为原问题的解

<!--break-->

### 应用

- 二分搜索

- 排序算法（快速排序、归并排序）

- 傅立叶变换（快速傅立叶变换）

## 动态规划算法

### 原理

动态规划适用于有`重叠子问题`和`最优子结构性质`的问题

最优子结构性质。如果问题的最优解所包含的子问题的解也是最优的，我们就称该问题具有最优子结构性质（即满足最优化原理）。

最优子结构的意思是局部最优解能决定全局最优解（对有些问题这个要求并不能完全满足，故有时需要引入一定的近似）。简单地说，问题能够分解成子问题来解决。

子问题重叠性质。子问题重叠性质是指在用递归算法自顶向下对问题进行求解时，每次产生的子问题并不总是新问题，有些子问题会被重复计算多次。

动态规划在查找有很多重叠子问题的情况的最优解时有效。它将问题重新组合成子问题。为了避免多次解决这些子问题，它们的结果都逐渐被计算并被保存，从简单的问题直到整个问题都被解决。因此，动态规划保存递归时的结果，因而不会在解决同样的问题时花费时间。

### 步骤

1. 划分阶段：按照问题的时间或空间特征，把问题分为若干个阶段。在划分阶段时，注意划分后的阶段一定要是有序的或者是可排序的，否则问题就无法求解；

2. 确定状态和状态变量：将问题发展到各个阶段时所处于的各种客观情况用不同的状态表示出来。当然，状态的选择要满足无后效性；

3. 确定决策并写出状态转移方程：因为决策和状态转移有着天然的联系，状态转移就是根据上一阶段的状态和决策来导出本阶段的状态。所以如果确定了决策，状态转移方程也就可写出。但事实上常常是反过来做，根据相邻两段各状态之间的关系来确定决策；

4. 寻找边界条件：给出的状态转移方程是一个递推式，需要一个递推的终止条件或边界条件。

### 应用

- 动态规划求0/1背包问题

- 最长公共子序列

- Floyd-Warshall算法

- 斐波那契数列

```
array map [0...n] = { 0 => 0, 1 => 1 }
fib（n）
    if（map m does not contain key n）
        m[n] := fib(n − 1) + fib（n − 2）
    return m[n]
```

将前n个已经算出的数保存在数组map中，这样在后面的计算中可以直接应用前面的结果，从而避免了重复计算。算法的运算时间为O（n）。

## 贪心算法

### 原理

贪心算法在对问题求解时，总是做出在当前看来是最好的选择。整个问题的最优解一定由在贪心策略中存在的子问题的最优解得来的。对于大部分的问题，贪心法通常都不能找出最佳解（不过也有例外），因为他们一般没有测试所有可能的解。贪心法容易过早做决定，因而没法达到最佳解。

基本思路是从问题的某一个初始解出发一步一步地进行，根据某个优化测度，每一步都要确保能获得局部最优解。每一步只考虑一个数据，他的选取应该满足局部优化的条件。若下一个数据和部分最优解连在一起不再是可行解时，就不把该数据添加到部分解中，直到把所有数据枚举完，或者不能再添加算法停止。

从问题的某一初始解出发；

while 能朝给定总目标前进一步 do

求出可行解的一个解元素；

由所有解元素组合成问题的一个可行解；

### 步骤

1. 建立数学模型来描述问题；

2. 把求解的问题分成若干个子问题；

3. 对每一子问题求解，得到子问题的局部最优解；

4. 把子问题的解局部最优解合成原来解问题的一个解。

### 应用

- 背包问题

- 最小生成树的Prim算法和Kruskal算法

- Dijkstra的单源最短路径

- Chvatal的贪心集合覆盖启发式

- 哈夫曼编码

## 回溯法

### 原理

回溯法（backtracking）是暴力搜寻法中的一种。回溯法是一种选优搜索法，按选优条件向前搜索，以达到目标。采用试错的思想，它尝试分步的去解决一个问题。但当探索到某一步时，发现原先选择并不优或达不到目标，就退回一步重新选择，这种走不通就退回再走的技术为回溯法，而满足回溯条件的某个状态的点称为`回溯点`。

回溯法通常用最简单的递归方法来实现，在反复重复上述的步骤后可能出现两种情况：

- 找到一个可能存在的正确的答案

- 在尝试了所有可能的分步方法后宣告该问题没有答案

### 步骤

1. 针对所给问题，定义问题的解空间；

2. 确定易于搜索的解空间结构；

3. 以深度优先方式搜索解空间，并在搜索过程中用剪枝函数避免无效搜索。

### 应用

- 八皇后问题

- 0-1背包问题

## 分支限界法

### 原理

分枝界限法是组合优化问题的有效求解方法。分支限界法常以广度优先或以最小耗费（最大效益）优先的方式搜索问题的解空间树。

分支定界法的基本思想是对有约束条件的最优化问题的所有可行解（数目有限）空间进行搜索。该算法在具体执行时，把全部可行的解空间不断分割为越来越小的子集（称为分支），并为每个子集内的解的值计算一个下界或上界（称为定界）。

在每次分支后，对凡是界限超出已知可行解值那些子集不再做进一步分支。这样，解的许多子集（即搜索树上的许多结点）就可以不予考虑了，从而缩小了搜索范围。这一过程一直进行到找出可行解为止，该可行解的值不大于任何子集的界限。因此这种算法一般可以求得最优解。

将问题分枝为子问题并对这些子问题定界的步骤称为分枝定界法。

分枝决策

（1）从最小下界分枝（优先队列式分枝限界法）：每次算完界限后，把搜索树上当前所有叶节点的界限进行比较。找出限界最小的节点，此结点即为下次分枝的结点。

优点：检查子问题较少，能较快地求得最佳解；

缺点：要存储很多叶节点的界限及对应的耗费矩阵，花费很多内存空间。

（2）从最新产生的最小下界分枝（队列式（FIFO）分枝限界法）：从最新产生的各子集中按顺序选择各结点进行分枝，对于下界比上界还大的节点不进行分枝。

优点：节省了空间；

缺点：需要较多的分枝运算，耗费的时间较多。

这两个原则更进一步说明了，在算法设计中的时空转换概念。

### 步骤

1. 设求解最大化问题，解向量为X=(x1,…,xn)，xi的取值范围为Si，`|Si|=ri`。在使用分支限界搜索问题的解空间树时，先根据限界函数估算目标函数的界[down, up]，然后从根结点出发，扩展根结点的r1个孩子结点，从而构成分量x1的r1种可能的取值方式；

2. 对这r1个孩子结点分别估算可能的目标函数bound(x1)，其含义：以该结点为根的子树所有可能的取值不大于bound(x1)，即：bound(x1)≥bound(x1,x2)≥…≥ bound(x1,…,xn)；

3. 若某孩子结点的目标函数值超出目标函数的下界，则将该孩子结点丢弃；否则，将该孩子结点保存在待处理结点表PT中；

4. 再取PT表中目标函数极大值结点作为扩展的根结点，重复上述；

5. 直到一个叶子结点时的可行解X=(x1,…,xn)，及目标函数值bound(x1,…,xn)。

### 应用

- 旅行商问题，即TSP问题（Traveling Salesman Problem）

- 选址问题

- 背包问题