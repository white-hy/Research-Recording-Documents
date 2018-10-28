# 周报

**冯浩哲**

**2018.7.23**

++++++

[TOC]

## 本周工作

1. 按郝老师的要求完成了将本科生毕业论文改成一篇可以发表的小论文的任务，已经将小论文送郝老师修改
2. 阅读相关文献，包括CVPR2018的文献，以及自动编码器相关的文献

## 下周的工作安排

​	按老师指导修改小论文

## 文献阅读报告

1. Learning to Segment Every Thing

   - 文献内容

     - 针对的问题

       弱监督语义分割与实例分割任务

     - 拟探索的问题

       假设我们有一个Object Detections数据集，这个数据集对于每一个目标物体都给出了Bounding Box，但是只对少数物体给出了分割标注Mask，如何训练网络使得网络能把那些没有给分割标注的物体也分割出来

     - 解决困难的方法

       作者解决该问题的主要亮点在于使用了权重迁移函数，这是一个多层全连接层构成的函数，它能够将Bounding Box部分的权重迁移到输出分割标注部分的权重。

       如图所示，作者以Mask R-CNN的结构为基础，设计了这个系统结构。为了利用只有Bounding Box的数据，作者采用权重迁移函数，把Bounding Box部分的参数通过一个函数映射为Mask部分的参数，这样就可以利用无Mask的数据进行训练。![1532318305685](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\周报\2018.7.21\System Structure)

     - 达到的结果

       如图所示，该图中只有绿色部分有分割标注，但是网络成功地把所有区域都分割出来了。

       ![1532318962244](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\周报\2018.7.21\segment result)

   - 文献启示

     文献给出了一个如何利用部分标注数据进行实例分割的系统，解决了只有部分mask标注数据的实例分割问题，具有开创意义。

     同时，文献构造对比Baseline的手法很值得借鉴。文献构造的Baseline是一个Mask R-CNN加一个不输出分类，只输出分割标注的类不可知分割预测网络。这个系统也可以对部分标注的数据完成实例分割任务。文献提出的系统的效果比这个构造的Baseline高出了40%。

     

2. LSDA: Large Scale Detection through Adaptation


- 文献内容

  - 针对的问题

    图像分类到图像检测任务的参数迁移

  - 拟探索的问题

    假设我们有一个图像分类数据集，这个数据集对于每一类目标物体都给出了分类标注，但是只对少数几类物体给出了检测标注与bounding box，如何训练网络使得网络能把那些没有给检测标注的物体也检测出来并给出bounding box

  - 解决困难的方法

    作者采用了如下的网络结构，将检测任务的权重与分类任务的权重用一个简单的全零初始化的$\delta B$相连接，也就是令$W_i^d=W_i^c+\delta B_i$，这样就可以建立分类任务与检测任务的迁移关系。![1532331525400](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\周报\2018.7.21\detection system)

    作者这样建立联系是基于三个基本原则：

    1. 在从分类器到检测器的过程中，对于背景的认识是非常重要的。
    2. 分类器与检测器的参数之间有大部分类别不变量
    3. 只有少数随类别改变的量让分类器与检测器参数不一致

  - 达到的结果

    作者以只用有图像级标准的样本训练分类器，然后再把分类器扩展到RPN上的做法作为baseline，把它与作者提出的方法进行对比，作者的方法比baseline的mAP结果相对高出了50%。

- 文献启示

  该文献是文献[1]中提到的参数迁移方法的一个基础，它可以方便我们对不同任务的参数迁移手段进行直观理解。

3. Semantic Autoencoder for Zero-Shot Learning

   - 文献内容

     - 针对的问题

       用基于语义编码的自动编码器来完成Zero-Shot学习

     - 拟探索的问题

       Zero-Shot学习是指我们让模型能够学习到训练集之外的东西。假设一种样本未出现在训练集中，我们也能学到这个样本的特征。文献提出了一种使用自动编码器来进行Zero-Shot学习的方法。

     - 解决困难的方法

       文献利用了语义自编码器实现了Zero-shot Learning。作者构造了编码器E,它可以将输入X映射到$S=WX$，传统自编码器不对S加以限制，但是文献中令S编码了X的语义向量，表示类别标签或样本属性，然后再构造对应的解码器D,它将编码映射到输入空间$X'=W^TS$.文献训练自动编码器的时候既要求$X'$与$X$尽量相似，又要求S正确编码语义信息，这样把自动编码器的训练变成了有监督训练。

     - 达到的结果

       作者用该方法在Zero-Shot训练集上达到了state-of-art的泛化能力。

       同时作者指出用训练集所训练出来的编码器可以用在测试集的聚类任务上，也取得了最好的效果。

   - 文献启示

     该文献是自动编码器泛化能力的一个应用，它可以指导我们对于编码器的使用方式。

4. Zero Shot Learning via Low-rank Embedded Semantic AutoEncoder

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

[1]Jastrzębski S, Kenton Z, Ballas N, et al. DNN's Sharpest Directions Along the SGD Trajectory[J]. arXiv preprint arXiv:1807.05031, 2018. 

[2]Goodfellow I, Bengio Y, Courville A, et al. Deep learning[M]. Cambridge: MIT press, 2016. 

[3]Kodirov E, Xiang T, Gong S. Semantic autoencoder for zero-shot learning[J]. arXiv preprint arXiv:1704.08345, 2017. 

[4]Liu Y, Gao Q, Li J, et al. Zero Shot Learning via Low-rank Embedded Semantic AutoEncoder[C]//IJCAI. 2018: 2490-2496. 

[5]Liu Y, Gao Q, Yang Z, et al. Learning with Adaptive Neighbors for Image Clustering[C]//IJCAI. 2018: 2483-2489. 







