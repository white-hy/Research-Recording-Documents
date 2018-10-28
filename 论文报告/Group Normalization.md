# [Group Normalization](https://arxiv.org/abs/1803.08494)阅读报告

[TOC]

## 提纲

### 传统上有什么样的Batch Normalization方法，它们起到了什么样的结果，有什么局限

### Group Normalization方法的基本思想是什么，它应该怎样使用

## 传统BN方法简述

一图流：

![52342344179](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\论文报告\Batch_Norm_Picture)

传统的Batch Norm的想法其实很简单，就是假设我输出的是一个N维C个Channel的H*W的图片，那么我认为这个图片应该被中心化，也就是说我认为这个图片记录的特征需要做标准化，也就是把图片拉成一个向量，然后我认为这个向量是很多数据点组成的，它们满足一个分布（注意这里的语境是这些数据点满足了一个参数分布，而这个数据点并不是独立的），因此我们可以认为这些数据点是满足一个分布的。而BN的作用就是把这个数据点进行扩大，让这个假设更加地strong可以认为这样（因为这个假设是很弱的，但是如果这个假设我对每个数据点进行重复采样，那么这个假设会变得很强，或者更稳定。它可以极大避免数值弥散问题。

作者给出了一组数据图:

![52342734291](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\论文报告\batch_size)

在batch size为1的时候的Batch Norm就是Instance Norm，在Batch Size变大的时候相应起到了统计的结果。这里的作用就是用Group Norm来代替了batch，在batch size很小的时候起到了很好的结果。

Layer Normalization(LN)也是一个新思想，它的目的是在所有层做Norm。Weight Norm是对所有的过滤参数做Norm，也就是说是对整个参数的Norm手法。但是这些手法都不是很好。

关于Batch Renormalize这篇文章的手法似乎就是对均值和方差加了一个限制。

对于多GPU问题可以采用同步化BN，把这个问题变成了硬件问题。

### 关于群：

Group 计算。

Group卷积是一个为了解决显卡显存不足的操作，总之就是把输入的M个channel分为N个group，然后每一个group分别与上一层的M/N个channel独立连接。这就相当于说一个Filter此时不是在整个channel上进行连接，而是在channel所对应的Group上进行了连接。这样一方面可以并行化操作，另一方面也可以降维。

## Group Normalization的思想

从Alex Net的群卷积而言，本文就是从这个想法得到的灵感。具体而言就是直接把特征进行Group分类（给的Group是32默认，文中探究了如何设置group的这样的数据：

![52343188915](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\论文报告\groupnorm)

这个结果表示，一般选16个Channel一个Group，或者把Group Size设置为32都还行。或者也可以适当调整Group的数目。总之Group过多调Group，channel过少调channel这样是比较好的想法。

### 如何实现呢？

我觉得这个可以用高维思想来看待。比如输入一个N,C,H,W的图像，我们把它reshape为

[N,G,C/G,H,W]，然后做Instance 3d Normalization即可。

但是对3D呢？

