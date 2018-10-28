# Mask R-CNN

**Reporter**

**冯浩哲**

+++++

[TOC]

## 论文概要

论文立足于**Instance Segmentation**问题，提出了**Mask R-CNN**架构以解决该问题。传统的做法一般是将区域找出来了然后再分类，但是Mask R-CNN提出的思想是将**BBox Regression,Classification and Segmentation**并行发展，也就是说最后我们会同时获得BBox调整的参数，当前BBox的分类以及当前BBox的二值分割。整个文章可以着眼于以下3个问题上：

1. Mask R-CNN的框架是什么样的
2. 什么是ROI Align，它应该如何具体计算
3. 如何将分割与ground truth相对应

## Mask R-CNN的框架是什么样的

Mask R-CNN的框架基本与Faster R-CNN的框架一模一样，只是有2个不同：

1. 在Feature Projection与ROI Pooling的时候采用了浮点运算与双线性插值，这个称做ROI Align。
2. 在Box Regression,Classification这两个分支之外又多了一个生成Mask的部分，也就是segmentation。它所生成的segmentation为一个k个class的固定尺寸(本文中采用了14*14的大小)的二值图，实际计算损失的时候取ground truth所对应的第 i 个特征图即可。

## 什么是ROI Align，应该如何计算

Mask R-CNN的一个成功秘诀就是在Feature Projection与ROI pooling的时候用ROI Align,将原来需要取整的地方保留浮点数，然后再进行运算。Mask R-CNN认为这里的精度问题导致了Instance Segmentation的问题，因此它使用浮点+双线性插值的方法来解决这个问题。

以一个例子来解释吧：

假设我们有一个$(600,600)$的图，然后图中$(230,210),(450,430)$的区域映射到特征图中$(230/16,210/16),(450/16,430/16)$的区域中，然后对这个区域进行ROI Align映射到$(14,14)$区域大小的时候

我们采用以下手段：将$[230/16,450/16]$均分为14段，然后我们就有每一段的中心坐标了，这样2个轴上的中心坐标构成了$14*14=196$个点，每个点都是一个浮点数，则每个点都落在4个整数点的中心。使用这4个整数点对中心点进行双线性插值即得到该点的值，如此生成$14*14$的大小。

## 如何将分割与Ground Truth对应

论文中有一点没有明确的就是我得到的分割是一个$14*14$大小的Feature map，但是我的GT显然不是这个样子的，那应该怎么办呢？

这个我在查阅大量源码认为，这个其实需要对GT也做一次Resample到14*14大小，然后才能比较。因此这个网络其实就是一个不停resample的网络。在测试阶段的话，我们得到的结果需要再resample回预测之后的bbox一般大小，这当然是建立在bbox确实能将物体完美框出来这个事实上的，具体实现过程需要阅读源码的实现细节。





