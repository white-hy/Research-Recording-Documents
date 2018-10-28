# Generate images with perceptual similarity metrics（感知器相似矩阵）

## Abstract

目的：

generate sharp high resolution images from compressed abstract representation

贡献1：

1.用深层神经网络提取的feature来计算距离

2.提出一个更好的损失函数（这也是我们的一个套路）

工具：

1.AlexNet

2.变分自动编码器

## Introduction

关于图像生成任务的损失函数研究

1. 二次损失函数会导致图像模糊，这是因为特征的损失所导致的，因此图像空间的损失导致对这些局部细节的平均化（相当于多了一个滤波器），因此重构就变模糊了。
2. 

