# chap 4 在线问题

[TOC]

## 滑雪问题： 

可行策略：

策略N：

在第n，n<N次滑雪的时候租用装备，如果滑雪次数不少于N，那么在第N次滑雪的时候购买装备，以后每次滑雪不再租用。

在最坏情况界意义下该策略优于其它策略

## 搜索问题

某物品遗失在两条不同方向，无限延伸的道路中某处，但是不知道所在道路与确切位置。

从两条道路的交汇点出发寻找的最优方案：

可行方案是不断折返寻找，搜索半径不断扩大

交错数列：

算法1：

（1，-1，2，-2，3，-3....,k,-k...)

假设在(-k-eps)处

那么就是2+2+4+4+6+6+...+2k+2k+2(k+1)+k+eps=$$

因此最坏情况界大于2k+5

等差数列大于2k+4

而交错数列呢？

(1,-2,4,-8,16,-32...,$2^{k-1}$,$-2^k$,$2^{k+1}$,...,$-2^{2k-1}$,$2^{2k}$,$-2^{2k+1}$,...)

假设我们的物品在$-(2^{2k-1}+eps)$处，那么我们需要跑：

$2*2^0+2*2^1+....+2*2^{2k-1}+2*2^{2k}+2*2^{2k+1}+2^{2k-1}+eps=9*2^{2k-1}-2+eps$

比值小于等于9

搜索问题任意算法的比值不会总小于9.

怎么证明呢？

利用了单减数列与最后的结论，证明减少的时候是矛盾的



## 在线问题：

决策的时候没有掌握全部实例信息，已经做的决策在更多信息呈现后无法更改

##离线问题：

实例信息在决策前全部已知

##在线算法：

### 比如装箱问题：

FF（First Fit），LS都是在线算法（也就是不知道后面工件的大小），FFD，LPT（需要排序的）都不是在线算法

### 竞争比：

记$C^A(I)$为算法A求解实例I所得的目标函数值，$C^*(I)$为实例I在信息完全已知情况下的最优目标值

称：

1. 对于极小化问题，$c_A=inf\{r\geq 1|C^A(I)<=rC^*(I),\forall I\}$(注意最坏情况界是CA/C*)
2. 极大化问题$c_A=inf\{r\geq 1|C^*(I)\leq rC^A(I),\forall I\}$

为在线算法A的竞争比

因为信息不完全性，很可能出现任何算法都找不到最优解。

如果对于**任意的在线算法求解某问题**竞争比至少为$r$,那称这个在线问题的下界为r

如果一个在线问题的下界为r，A是该问题竞争比为r的在线算法，那么称这个算法为最好算法。

下界：

类型：离线算法最坏情况界下界，或者是在线算法竞争比下界

产生原因是算法自己局限性，比如FF算法绝对性能比至少为17/10

计算时间限制而产生了离线问题近似算法下界，一般用归约来证明，比如证明任意装箱问题算法绝对性能比至少为3/2

在线问题竞争比下界（因为信息不完全性导致的）

构造一系列实例，穷取所有可能在线算法没比如任意在线装箱问题算法绝对性能比至少为5/3

比如这个实例：

依次给出

$(\frac{1}{6}-2eps)*6,(1/3+eps)*6,(1/2+eps)*6$

假设先给出了第一组，如果CA大于2的话竞争比大于5/3，不可能，因此CA=1

那么此时给出了第二组条件，此时最优是3个，因为在第一个里面CA已经做出了决定，因此此时CA必须给3个，如果CA给了超过4个那么大于等于5/3竞争比了

因此此时在最后，6个对10个，5/3

得证

### 平行机排序的在线形式

在线形式问题叙述

工件按照顺序一个一个到达，在**确定工件$J_j$的加工机器时**，只知道$J_1,...,J_j$的加工时间，而不知道未到达工件的加工时间。

工件加工机器一旦指定就不能改变

$P2||C_{max}$的下界至少是3/2

而

$P3||C_{max}$的下界是5/3

LS算法是P2，P3最好的算法

