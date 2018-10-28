# 周报

**冯浩哲**

**2018.6.10 **

++++++

[TOC]

## 本周工作

1. 完成本科生毕业论文答辩后的一系列后续事宜,包括优秀论文答辩等
2. 阅读文献
3. 复现肺结节检测问题state of art的结果。我按照文献[8]提出的整个流程进行复现，但是因为文献[8]细节并没有描述清楚，因此还未复现出相同结果。文献[8]的分类准确度为97.58%，但是我现在只达到87.5%，仍然差一些。

## 下周的工作安排

1. 分析论文细节，努力复现出state of art的结果
2. 将我们在肺结节分割任务上有一些效果的整个推荐标注系统迁移到检测任务上，并观察其有效性

## 文献阅读报告

本周我看了7篇文献，都是CVPR2018的spotlight论文，我将简要介绍文献内容，并思考相应文献的思想与方法对我们现在的工作的启示。

1. Decoupled Networks[1]

   * 文献内容

     - 针对的问题

       神经网络的运算操作解耦

     - 拟解决的困难

       如何优化神经网络中神经元的运算流程

     - 解决困难的方法

       作者将原来神经元运算$f(x,w)=w^Tx$解耦为$f(x,w)=h(||x||,||w||)g(\theta_{(x,w)})$,断言每一次神经元运算的结果都是一次分类，而角度代表不同类别之间的差异。作者提出采用不同的$h,g$函数来优化卷积操作的流程。

     - 达到的结果

       作者总结了前面在解耦网络上提出的对$g$函数的改进，并提出了一些有效的$h$函数。作者提出的网络在ImageNet等几个任务上都达到了state-of-art的结果。

   * 文献启示

     该文献提出了对神经元操作在大小与角度上的一种分类假设，并在MNIST手写数据集上用可视化方式验证了这种假设。基于这种假设，作者提出了优化神经元运算过程的一种方法。如果我们也研究这种问题，那么我们也完全可以使用这些方法。

2. spectral normalization for generative adversarial networks[2]

   - 文献内容

     * 针对的问题

       对抗神经网络中的Lipschitz连续性问题

     * 拟解决的困难

       GAN中的判别器D遵守Lipschitz连续性假设，而普通的deep model则没有这个要求。作者提出了一种满足这种要求的正则化方式

     * 解决困难的方法

       作者用谱正则化来完成这个过程，即对每一层的矩阵形式参数W计算其矩阵二范数
       $$
       \sigma(W)=max_{h:h\neq0}\frac{\Vert Wh \Vert _2}{\Vert h \Vert _2}
       $$
       然后用该范数对原权重进行优化
       $$
       W_{SN}(W)=\frac{W}{\sigma(W)}
       $$

     * 达到的成果

       用这种谱正则化方法可以很好地去控制整个判别函数的Lipschitz常数，比经典的WGAN方法要好。

   - 文献启示

     这篇文献给出了如何对对抗生成网络进行正则化的方法，而且结果还十分不错。

3. Spectral Norm Regularization for Improving the
   Generalizability of Deep Learning[3]

   - 文献内容

     - 针对的问题

       深度学习泛化性的问题

     - 拟解决的困难

       作者提出，如果对于test数据集的泛化性不强，很可能是网络在test数据集上对应于形变不稳定。

     - 解决困难的方法

       作者给出了基于最小化参数的矩阵范数$\frac{\Vert W_{\Theta,x} \xi \Vert_2}{\Vert \xi \Vert _2}$方法对参数进行正则化。

     - 达到的结果

       作者用这种正则化方法同时对比于其它正则化方法，发现这种方法在现在的神经网络模型上泛化能力要高

   - 文献启示

     我们也可以用这种方法提升泛化能力

4. Fast Dense Feature Extraction with CNNs that have Pooling or Striding Layers[4]

   - 文献内容

     - 针对的问题

       卷积神经网络中的计算冗余问题

     - 拟解决的困难

       作者提出，卷积神经网络上当stride=1的时候存在计算冗余(这当然是显然的)，同时卷积神经网络上的pooling操作只是对于局部进行的，并没有考虑所有的位置信息

     - 解决困难的方法

       计算冗余的方法作者给出了torch代码进行解决。但是在pooling操作上的问题，作者给出了一个变换池化操作：
       $$
       Pool_{S\times S}^{x,y} (I)= Pool_{S\times S}(SHIFT_{y,x}(I)) 
       $$
       然后作者利用多池化操作与Wrap操作重构池化过程，从而可以得到多角度特征。

     - 达到的结果

       作者用这种方法提升了神经网络的运行速度与运行效率，同时优化了池化操作。

5. ### Inverted Residuals and Linear Bottlenecks: Mobile Networks for Classification, Detection and Segmentation

   * 文献内容
     - 针对的问题

       什么样的网络结构能够达到较好的性能与参数上的平衡

     * 拟解决的困难

       如何设计网络中间的特征提取瓶颈层，使得它能尽快地提取瓶颈。

     * 内容概要

       作者结合传统先做$1\times 1$卷积再做$3\times 3$卷积的特征分离提取操作(先提取深度特征，再提取广度特征)，提出了多层BottleNeck方法，包括先做$1\times 1$的2d卷积将特征进行膨胀，再做$3\times 3$的卷积将特征进行提取，最后再做一个$1\times1$的卷积将特征通道复原的操作。作者将其称为Bottleneck with expansion layer。

       然后作者又设计了一个**Inverted residual block**，它在Bottleneck with expansion layer上增加了Residual Network的跳层连接，从而统一了前后的feature。

       作者基于Inverted Residual Block构建了MobileNet V2，这个网络在运算复杂度与精度的trade-off评价指标上在CoCo数据集上达到了state-of-art的结果。

   * 文献启示

     该文献的网络结构非常值得借用。

6. Squeeze-and-Excitation Networks [6]

   - 文献内容

     - 针对的问题

       如何度量输出特征channel之间的关系

     - 拟解决的困难

       设计一个网络结构，可以对channel之间的关系建模

     - 内容概要

       网络提出了一个Squeeze-and-Excitation Network结构，它用如下方式对每一个channel进行赋权:

       ![1528541883186](C:\Users\JIAZHE~1\AppData\Local\Temp\1528541883186.png)

       首先将输入的特征经过传统特征提取(残差网络模块等)操作得到输出特征，然后将输出特征按通道求平均值，将channel数的向量进行全连接得到权重，最后反馈到原特征图上。

       

   - 文献启示

     文献提出的结构可以直接用在现有网络的后面，它对调整feature的重要性做出了贡献。

7. Convolutional Neural Networks with Alternately Updated Clique[7]

   - 文献内容

     - 针对的问题

       Densenet提出的稠密连接结构只能向前连接不能向后连接的特点

     - 拟解决的困难

       设计一个网络结构，可以让特征前向后向都可以进行复用

     - 内容概要

       网络提出了一个Clique网络结构，它基于densenet的网络结构实现了特征前向和后向的双层使用:

       ![1528542538378](C:\Users\JIAZHE~1\AppData\Local\Temp\1528542538378.png)

       同时在参数更新的时候我们采用两阶段法更新连接权重。

   - 文献启示

     该文献提出的将高维特征重复使用的方案在CIFAR10,CIFAR100等数据集上都用更少的参数达到了state of art的结果。但是我认为这个文献的图画的具有诱导性，因为事实上stage-II与stage-I的参数是分离的，图上让人看起来似乎是一样的了。

     这个文献的结构值得一看，但是不一定很有用。

     

     


## 参考文献

[1]Liu, Weiyang & Liu, Zhen & Yu, Zhiding & Dai, Bo & Lin, Rongmei & Wang, Yisen & Rehg, James & Song, Le. (2018). Decoupled Networks. 

[2]Wei Y, Xiao H, Shi H, et al. Revisiting Dilated Convolution: A Simple Approach for Weakly-and Semi-Supervised Semantic Segmentation[J]. arXiv preprint arXiv:1805.04574, 2018.

[3]Wu Z, Xiong Y, Yu S, et al. Unsupervised Feature Learning via Non-Parametric Instance-level Discrimination[J]. arXiv preprint arXiv:1805.01978, 2018. 

[4]Bailer C, Habtegebrial T A, Varanasi K, et al. Fast Dense Feature Extraction with CNNs that have Pooling or Striding Layers[J]. 

[5]Sandler M, Howard A, Zhu M, et al. Inverted Residuals and Linear Bottlenecks: Mobile Networks for Classification, Detection and Segmentation[J]. arXiv preprint arXiv:1801.04381, 2018. 

[6]Hu J, Shen L, Sun G. Squeeze-and-excitation networks[J]. arXiv preprint arXiv:1709.01507, 2017. 

[7]Yang Y, Zhong Z, Shen T, et al. Convolutional Neural Networks with Alternately Updated Clique[J]. arXiv preprint arXiv:1802.10419, 2018. 

[8]Wu B, Zhou Z, Wang J, et al. Joint Learning for Pulmonary Nodule Segmentation, Attributes and Malignancy Prediction[J]. 2018. 









