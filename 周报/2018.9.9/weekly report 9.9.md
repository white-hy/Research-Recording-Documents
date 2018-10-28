# 周报

**冯浩哲**

**2018.9.9 **

+++++

[TOC]

## Work

1. 阅读图割理论的相关资料，并将我们的推荐过程的目标函数用Ratio k-cut的损失函数替代。应用Ratio k-cut问题的损失函数使得我们的unsupervised suggestive annotation算法精度比之前提高了2个百分点。

2. 在设备有限的情况下采用kNN作为分类器，在肺结节分类问题上比较选取不同数目的数据时我们的算法与随机选取的优劣性。结果表明，应用简单的kNN作为分类器，我们的算法比随机选取平均高2.5%的精确度，我们需要把这个精确度再提高一点。

   ![1536485008986](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\周报\2018.9.9\Compared Results)

3. 工作时长：工作日每天10个小时，周末共16个小时，共66个小时

### 工作进度

| 项目                     | 进度                            | 截止时间 |
| ------------------------ | ------------------------------- | -------- |
| 投稿(CAD学报)            | 完成修改已提交编辑              |          |
| CVPR投稿(无监督推荐标注) | 新增Ratio k-cut方法取得显著效果 | 11.1     |

## Paper Reading

1. Interactive Medical Image Segmentation Using Deep Learning With Image-Specific Fine Tuning[1]

   - 文献内容

     - 针对的问题

       医学图像中的交互分割问题

     - 拟探索的问题

       如何对2D与3D医学影像实现交互实例分割

     - 解决困难的方法

       文献将P-Net语义分割神经网络与scribble-based分割流程相结合，在2D与3D医学影像上实现了基于微调策略的交互分割。作者的亮点在于在神经网络训练微调阶段提出了基于Grab-Cut方法的利用scribble信息的损失函数，以及利用scribble信息的神经网络参数更新权重。

     - 达到的结果

       作者用该方法在2D MRI多器官分割任务以及3D脑部肿瘤分割任务上达到了比所有传统交互方法都精确的结果，同时用了更少的交互次数。

       同时，作者也实验了该方法在Zero-Shot Learning上的潜力，证明了该方法极好的泛化能力。

   - 文献启示

     该文献是最新的深度交互分割技术在2D与3D医学图像上的应用，同时它对过去的交互分割技术进行了详细的总结，非常值得阅读。

2. Zero Shot Learning via Low-rank Embedded Semantic AutoEncoder[2]

   - 文献内容

     - 针对的问题

       用基于语义编码的自动编码器来完成Zero-Shot学习

     - 拟探索的问题

       Zero-Shot学习是指我们让模型能够学习到训练集之外的东西。假设一种样本未出现在训练集中，我们也能学到这个样本的特征。文献提出了一种使用自动编码器来进行Zero-Shot学习的方法。

     - 解决困难的方法

       该文献将关系矩阵$W$换为低秩矩阵，实践证明低秩约束在某些数据集上确实有用

     - 达到的结果

       作者用该方法在大型图像数据集上用低秩约束比SAE方法起到了$0.4\%$的提升

   - 文献启示

     该文献引入了低秩约束并给出了如何用优化方法完成这个约束的手段，但是其结果没有太大意义。

3. Spectral k-way ratio-cut partitioning and clustering[3]

   该文献用ratio k-cut来对图进行聚类，它主要提出了两个创新点：1)ratio k-cut方法的损失函数如何度量 2)用什么方法解ratio k-cut这一NP难问题

4. Fine-Tuning Convolutional Neural Networks for Biomedical Image Analysis: Actively and Incrementally[4]

   该文献基于主动学习技术提出了一种用于减少生物医学影像标注量的方法，文献提出了fine-tuning的方法来解决主动学习过程中模型训练问题，同时提出了基于Entropy和Diversity的两种对不确定度进行评估的指标

5. Understanding the Disharmony between Dropout and Batch Normalization by Variance Shift[5]

   文献分析了为什么在有Batch Normalization的网络中增加Dropout结果会下降的原因，发现Dropout后跟BN会出现方差偏移，提出的解决方法是把Dropout放在BN之后可以提高网络的表现。


## 参考文献

[1]Xu N, Price B, Cohen S, et al. Deep interactive object selection[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2016: 373-381. 

[2]Castrejon L, Kundu K, Urtasun R, et al. Annotating Object Instances with a Polygon-RNN[C]//CVPR. 2017, 1: 2. 

[3]Chan P K, Schlag M D F, Zien J Y. Spectral k-way ratio-cut partitioning and clustering[J]. IEEE Transactions on computer-aided design of integrated circuits and systems, 1994, 13(9): 1088-1096.

[4]Zhou Z, Shin J Y, Zhang L, et al. Fine-Tuning Convolutional Neural Networks for Biomedical Image Analysis: Actively and Incrementally[C]//CVPR. 2017: 4761-4772.

[5]Li X, Chen S, Hu X, et al. Understanding the disharmony between dropout and batch normalization by variance shift[J]. arXiv preprint arXiv:1801.05134, 2018.



