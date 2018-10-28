# 周报

**冯浩哲**

**2018.9.3 **

+++++



[TOC]



## Why did you switch from lung nodule segmentation to lung nodule classification? 

1. 肺结节分割任务的目的就是进行肺结节分类，单纯进行肺结节分割实际意义不明显，同时肺结节分割任务相关文献非常少，而肺结节分类任务相关文献较多，相关研究关注者更多，因此用肺结节分类任务讲述的故事更加有意义。同时当前肺结节分类任务state of art的结果(87.14%)[1]非常好复现，且我们已经复现出来了。而肺结节分割任务state of art的结果很难复现，论文对模型叙述不清，同时对数据预处理方法也有一些语焉不详。
2. 我们从上海肺科医院获取的数据集没有给分割标注，但是给出了更为细致的多分类标注，因此以肺结节分类任务作为目标问题对后续以上海肺科医院的数据集作为研究对象进行研究更有利。
3. 对于推荐标注算法而言，肺结节轮廓标注的工作量比肺结节良恶性标注的工作量小，因此针对肺结节良恶性标注问题的推荐标注策略更有实际意义。

## 本周任务

由于睿医实验室无空闲设备，本周我们并没有试验新的模型。本周我们主要以文献阅读为主，并尝试解决当前课题遇到的一个问题：**无监督推荐标注策略测试时间太长**。

我们现在所提出的无监督推荐标注策略的完整流程如下：

> 先无监督训练第一个神经网络提取图像特征，再对特征进行聚		类，选取具有代表性的数据，然后用具有代表性的数据作为训练集有监督训练第二个神经网络完成肺结节分类任务，并在验证集上进行验证。

整个完整流程需要训练2个神经网络，需要的训练时间为3天。如果采用五折交叉验证方法获取最终结果则需要将整个流程重复5次，即训练10个神经网络，花费2周时间才能精确得到某一个无监督推荐标注策略的效果，这对快速对策略进行调整造成了很大的困扰。

我们从两个方向尝试解决，一个是能否在策略调整的过程中将训练2个网络的过程简化为训练一个网络，而另一个是能否在策略调整的过程中用其它方法替代五折交叉验证。

在第一个方向上, 我们采用文献[2]中提到的方法，用无监督提取的图像特征训练一个分类器，并在策略调整的过程作为对第二个肺结节分类神经网络的替代。即先无监督训练第一个神经网络提取图像特征，再选择具有代表性的数据，然后用具有代表性数据的图像特征来有监督训练一个分类器（如k nearest neighbor, Naïve Bayes, Support Vector Machine)进行肺结节分类。我们先在该流程上对无监督推荐标注策略进行调整，等到策略细节较为完善的时候再在完整流程上进行验证以节约时间。

在第二个方向上，因为医学影像数据集大都没有划分训练集与验证集，因此要获得分类器在验证集上的结果，五折交叉验证无法避免，同时现在也没有文献提出对交叉验证的替代方法。我们讨论后想了一个替代方法如下：

假设整个数据集有1000个数据，同时假设验证集的数据分布与训练集的数据分布相同，我们先用无监督推荐标注策略在1000个数据中选N个最有代表性的数据作为训练集，然后用剩下的1000-N个数据作为验证集。用训练集训练得到的分类器在验证集上进行验证，验证结果越好说明我们挑选的数据越具有代表性和标注价值，也说明当前的无监督推荐标注策略越好。

本周我们同时学习并实验了一些机器学习算法来以图像特征作为输入完成分类任务，包括k nearest neighbor, Naïve Bayes, Support Vector Machine，其中k nearest neighbor 与 Support Vector Machine都达到了$82\%$的肺结节分类准确率，离state of art的肺结节分类结果$87.12%$仅差5%，因此可以作为一种替代分类模型用以快速调整无监督推荐标注策略，而具体选哪一种还要进行实验验证。

## 下周任务

1. 阅读相关文献，从Graph k-cut的角度来进行聚类研究
2. 对本周提出的解决推荐标注算法验证时间过长的策略进行验证
3. 如果睿医有空余设备则对无监督特征提取方法进行改进(如将该方法与深度自动编码器结合以从explainable角度对特征进行改进)，如果没有空余设备则阅读与深度聚类方法与特征提取方法相关的文献

## 文献阅读报告

1. Deep Interactive Object Selection[3]

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

2. Annotating Object Instances with a Polygon-RNN[4]

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

3. Pattern Recognition and Machine Learning[5]

- 文献内容

  该书阐述了一些经典的机器学习方法，包括分类方法，聚类方法等。本周我主要阅读了其中的k nearest neighbor, Naïve Bayes, Support Vector Machine 3种机器学习分类工具，并在我们的肺结节分类问题上进行了尝试。

## 参考文献

[1]Shen W, Zhou M, Yang F, et al. Multi-crop convolutional neural networks for lung nodule malignancy suspiciousness classification[J]. Pattern Recognition, 2017, 61: 663-673.

[2]Wu Z, Xiong Y, Stella X Y, et al. Unsupervised Feature Learning via Non-Parametric Instance Discrimination[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2018: 3733-3742.

[3]Xu N, Price B, Cohen S, et al. Deep interactive object selection[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2016: 373-381. 

[4]Castrejon L, Kundu K, Urtasun R, et al. Annotating Object Instances with a Polygon-RNN[C]//CVPR. 2017, 1: 2. 

[5]Nasrabadi N M. Pattern recognition and machine learning[J]. Journal of electronic imaging, 2007, 16(4): 049901.



