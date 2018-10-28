# 周报

**冯浩哲**

**2018.7.23**

++++++

[TOC]

## 本周工作

1. 按郝老师的要求完成了将本科生毕业论文改成一篇可以发表的小论文的任务，已经将小论文送郝老师修改
2. 阅读相关文献，包括CVPR2018的文献，以及自动编码器相关的文献

## 下周的工作安排

	按老师指导修改小论文

## 文献阅读报告

1. Deep Interactive Object Selection[1]

   - 文献内容

     - 针对的问题

       计算机视觉中的交互式选择物体分割问题

     - 拟探索的问题

       假设我们有一张图像，里面有很多个物体，我们如何选择一个物体使神经网络将其分割出来

     - 解决困难的方法

       作者提出了一个交互式分割策略，只需要用户在图像上对目标物体，以及背景进行鼠标点击，那么就可以利用原图像+点击生成的距离图像输入深度卷积神经网络进行目标物体分割。这个过程是交互式的，增加点击就可以增加分割的精度。

       该网络在训练过程中由按某些策略随机生成的点击结合原图像作为输入，将目标物体的Binary Mask作为label计算损失函数，训练过程与语义分割神经网络类似。

       ![1532664183066](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\周报\2018.8.4\1process)

     - 达到的结果

       如图所示，该图中我们对某一只狗进行选取，仅对物体与背景进行了3次点击，然后利用深度神经网络生成概率图，再用图割算法将物体分割出来。

       ![1532664416231](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\周报\2018.8.4\1result)

   - 文献启示

     该文献给出了一种如何进行交互式目标物体分割的方法，这在医疗图像处理中非常有用。

2. Annotating Object Instances with a Polygon-RNN[2]


- 文献内容

  - 针对的问题

    如何减少图像语义标注所需要的工作量

  - 拟探索的问题

    减少获得语义分割标注的工作量。

  - 解决困难的方法

    作者将逐像素的标注手法进行了改变，认为对于分割任务，网络只需要输出一个对目标待分割物体的多边形就可以满足分割要求，不必细致地进行逐像素标注。

    作者提出了一个能够生成多边形坐标的网络结构如下：

    ![1532673991564](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\周报\2018.8.4\2process)

    该网络由一个VGG构成的CNN进行特征提取并预测多边形的第一个顶点，然后再构建一个RNN用以预测序列化的顶点坐标，它的输入是图像特征与预测的顶点，输出是多边形顶点坐标。

  - 达到的结果

    ![1532675066337](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\周报\2018.8.4\2result)

    作者成功用多边形作为边界分割输出，并在**Cityscapes**数据集上用多边形分割结果达到了IoU=78.4%的成果，同时将标注的过程加快了7.3倍

- 文献启示

  该文献也提出了降低标注工作量的方法，而这个方法与我们用于降低医疗图像标注量的方法都有区别，但是非常值得借鉴。

3. Interactive Medical Image Segmentation Using Deep Learning With Image-Specific Fine Tuning[3]

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

4. Zero Shot Learning via Low-rank Embedded Semantic AutoEncoder[4]

   - 文献内容

     - 针对的问题

       用基于语义编码的自动编码器来完成Zero-Shot学习

     - 拟探索的问题

       Zero-Shot学习是指我们让模型能够学习到训练集之外的东西。假设一种样本未出现在训练集中，我们也能学到这个样本的特征。文献提出了一种使用自动编码器来进行Zero-Shot学习的方法。

     - 解决困难的方法

       该文献是对文献[3]的扩展，主要是将文献[3]中的矩阵$W$换为低秩矩阵，实践证明低秩约束在某些数据集上确实有用

     - 达到的结果

       作者用该方法在大型图像数据集上用低秩约束比SAE方法起到了$0.4\%$的提升

   - 文献启示

     该文献是文献[3]的扩展，它引入了低秩约束并给出了如何用优化方法完成这个约束的手段，但是其结果没有太大意义。

5. Learning with Adaptive Neighbors for Image Clustering[5]

   文献内容

   - 针对的问题

     Graph-based 聚类算法对于初始输入的相似度矩阵依赖过强，无法自适应调整。

   - 拟探索的问题

     对于给定聚类个数c的聚类算法而言，很多Graph-based 聚类算法过于依赖图像的关系矩阵A，同时依赖于将原始图像转化为关系矩阵A的过程。

   - 解决困难的方法

     该文献提出了一种针对输入的关系矩阵A进行自适应计算相似度矩阵S的方法，这个方法通过对S对应的拉普拉斯图矩阵的秩进行限制来进行迭代优化。

     同时文献给出了一个从原始数据点集X映射到关系矩阵A的迭代方法，可以用在图像聚类问题上。

   - 达到的结果

     作者在基准数据集上进行了测试，这些数据集包括一个有224*224像素图片的图像数据集。作者把自己的方法与SC，K-Means，CLR方法进行了对比，证明作者的方法是State of art的方法。

   文献启示

   该文献提出的聚类算法可以用在我们的推荐标注聚类任务上。


## 参考文献

[1]Xu N, Price B, Cohen S, et al. Deep interactive object selection[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2016: 373-381. 

[2]Castrejon L, Kundu K, Urtasun R, et al. Annotating Object Instances with a Polygon-RNN[C]//CVPR. 2017, 1: 2. 

[3]Wang G, Li W, Zuluaga M A, et al. Interactive medical image segmentation using deep learning with image-specific fine-tuning[J]. IEEE Transactions on Medical Imaging, 2018. 

[4]Liu Y, Gao Q, Li J, et al. Zero Shot Learning via Low-rank Embedded Semantic AutoEncoder[C]//IJCAI. 2018: 2490-2496. 

[5]Liu Y, Gao Q, Yang Z, et al. Learning with Adaptive Neighbors for Image Clustering[C]//IJCAI. 2018: 2483-2489. 







