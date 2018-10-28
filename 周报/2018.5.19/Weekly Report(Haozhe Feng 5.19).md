# 周报

**冯浩哲**

**2018.5.19**

++++++

[TOC]

## 本周工作

本周我主要修订撰写毕业论文，并在5.16日提交了本科生毕业论文初稿。同时我写了一份工作计划，并准备按照工作计划实施工作。

## 下周的工作安排

下周我主要修改毕业论文，制作毕业论文答辩PPT，并在5.25日参与本科生毕业设计答辩。

## 文献阅读报告

本周我看了5篇文献，我将简要介绍文献内容，并思考相应文献的思想与方法对我们现在的工作的启示。

1. Intra-perinodular textural transition (Ipris): A

   3D Descriptor for Nodule Diagnosis on Lung CT[1]

   * 文献内容

     - 针对的问题

       肺结节良恶识别问题

     - 拟解决的困难

       如何利用肺结节的语义分割信息来进行良恶性判断

     - 解决困难的方法

       利用语义分割信息，将肺结节分为中心层，中间层与表层三个部分。在每一个肺结节的分层边界，利用梯度信息以及灰度信息构造手工特征，然后利用支持向量机对手工特征进行分类

     - 达到的结果

       在传统利用手工构造的特征进行识别的方法上达到了*state-of-art*的结果

   * 文献启示

     文献给出了一个构造肺结节图像特征的方法。联想到我们正在做的提取图像特征并利用这些特征进行分类的任务，我们可以将本文提出的特征与我们采用自动编码器所提取编码的特征进行比较，看看哪种特征比较好。

2. Learning Deep Representation for Graph Clustering[2]

   - 文献内容

     * 针对的问题

       机器学习中的谱聚类问题

     * 拟解决的困难

       谱聚类需要计算相似图矩阵的特征值，因此对大规模聚类问题谱聚类的计算代价过高。

     * 解决困难的方法

       文献将原来谱聚类中计算相似度矩阵$S \in R^{n*n}$的前k个特征值并给出相应特征向量的过程转变为利用自动编码器将相似度矩阵直接编码为$H\in R^{n*k}$的过程，利用自动编码器无监督训练的特性避开了特征根计算的步骤。

     * 达到的成果

       文献对经典的聚类数据集，如红酒数据集，新闻数据集进行了实验，在这些数据集上聚类结果都超过了谱聚类。

   - 文献启示

     这篇文献给出了一个基于自动编码器的比谱聚类更好的聚类方法，这个聚类方法在对于大规模数据进行聚类的时候可以显著减少计算负担。我们的课题所做的提取图像特征与聚类的整个流程往往需要对$1e4$数量的图像进行聚类，在这种大规模聚类问题上，我们可以使用这个聚类算法。

3. SegNet: A Deep Convolutional Encoder-Decoder Architecture for Robust  Semantic Pixel-Wise Labelling[3]

   - 文献内容

     - 针对的问题

       自然图像语义分割问题

     - 拟解决的困难

       * 用于语义分割的U-Net与FCN网络结构参数太多
       * 如何利用自动编码器进行语义分割任务

     - 解决困难的方法

       * 作者利用VGG网络构建了一个深度自动编码解码器，并将它用在了语义分割任务上
       * 作者将FCN与U-Net中上采样层的反卷积运算改成了反Maxpool运算，运算中只用了下采样过程中Maxpool运算时的特征位置信息，避免了U-Net中的特征concat操作与FCN中的反卷积操作，减少了参数数目

     - 达到的结果

       SegNet在道路与行人的语义分割任务做到了比FCN与U-Net都更好的结果，同时SegNet对每一张图片的处理速度是FCN与U-Net的两倍

   - 文献启示

     该模型是第一个直接采用自动编码器来进行语义分割的网络结构。考虑到我们的肺结节分类与分割任务中需要同时用到自动编码器，语义分割网络以及分类网络，我们可以借鉴本文的思想将这三个网络的结构合并为一个网络，从而简化模型结构，同时让整个流程联系更紧密。

4. Histograms of oriented gradients for human detection[4]

   * 文献内容
     - 针对的问题

       自然图像特征提取问题

     * 拟解决的困难

       如何仅利用图像的像素值信息，通过一个无监督方法来有效提取图像特征

     * 解决困难的方法

       文章提出将图像划分为若干个$8\times 8​$的bins，在每一个bins内对每一个像素计算基于像素值的梯度大小与方向(都用绝对值表示)，并以梯度方向作为划分构建直方图向量，用直方图向量作为该bin所对应的特征编码。最后作者把每个bins的特征编码进行concat，得到了整个图像所对应的特征编码。

     * 达到的结果

       HoG特征是现在最广泛使用的传统图像特征提取方法之一

   * 文献启示

     HoG特征具有无监督，以及相同尺寸图像所对应的特征编码长度一致的特点。我们可以用HoG特征作为特征提取方式，并与我们所用的深度自动编码器所提取的特征进行对比实验，从而看看我们的深度自动编码器所提取特征是否更好。

5. Distinctive image features from scale-invariant keypoints[5]

   * 文献内容
     - 针对的问题

       自然图像特征提取问题

     * 拟解决的困难

       与HoG类似，都是解决如何仅利用图像的像素值信息，通过一个无监督方法来有效提取图像特征

     * 解决困难的方法

       文章提出将输入图像Resample为不同尺度的图像，并在每一个尺度中采用不同方差的高斯模糊算子对图像进行处理。通过计算不同尺度中不同高斯模糊算子处理的图像结果之间的梯度来找出图像中对于尺度大小的若干不变点，然后以这些不变点为中心采用HoG方法编码不变点以及其周围区域的图像特征，最后将这些图像特征concat作为整个图像所对应的特征编码。

     * 达到的结果

       SIFT也是现在最广泛使用的传统图像特征提取方法之一。

   * 文献启示

     与HoG文献的启示类似，SIFT也可以作为特征提取方式，并与我们所用的深度自动编码器所提取的特征进行对比实验，从而看看我们的深度自动编码器所提取特征是否更好。但是SIFT特征对不同的输入图像找的关键点数目也不一致，因此相同尺寸的图像用SIFT编码所得的特征编码大小也不同，我们需要阅读更多的文章来获得如何用SIFT所提取的特征来进行聚类的思路。

## 参考文献

[1]Alilou M, Orooji M, Madabhushi A. Intra-perinodular Textural Transition (Ipris): A 3D Descriptor for Nodule Diagnosis on Lung CT[C]//International Conference on Medical Image Computing and Computer-Assisted Intervention. Springer, Cham, 2017: 647-655. 

[2]Tian F, Gao B, Cui Q, et al. Learning deep representations for graph clustering[C]//AAAI. 2014: 1293-1299. 

[3]Badrinarayanan V, Kendall A, Cipolla R. Segnet: A deep convolutional encoder-decoder architecture for image segmentation[J]. IEEE transactions on pattern analysis and machine intelligence, 2017, 39(12): 2481-2495. 

[4]Dalal N, Triggs B. Histograms of oriented gradients for human detection[C]//Computer Vision and Pattern Recognition, 2005. CVPR 2005. IEEE Computer Society Conference on. IEEE, 2005, 1: 886-893. 

[5]Lowe D G. Distinctive image features from scale-invariant keypoints[J]. International journal of computer vision, 2004, 60(2): 91-110. 







