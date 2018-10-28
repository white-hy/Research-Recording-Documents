# 周报

**冯浩哲**

**2018.6.2**

++++++

[TOC]

## 本周工作

1. 完成本科生毕业论文答辩后的一系列后续事宜
2. 基于文献[5]的结构将原来的肺结节分割任务修正为检测任务，并跑通了整个流程。但是仍然还未复现出文献[5]的结果

## 下周的工作安排

1. 分析论文细节，努力复现出文献[5]的结果
2. 将我们在肺结节分割任务上有一些效果的整个推荐标注系统迁移到检测任务上，并观察其有效性
3. 文献[3]中提出了很多用深度学习来进行有效图像特征提取的方法，我们可以借鉴它的思路进行一系列实验，并改善我们的原系统。

## 文献阅读报告

本周我看了4篇文献，都是CVPR2018的spotlight论文，我将简要介绍文献内容，并思考相应文献的思想与方法对我们现在的工作的启示。

1. TieNet: Text-Image Embedding Network for Common Thorax Disease Classification and Reporting in Chest X-rays [1]

   * 文献内容

     - 针对的问题

       胸部X光图像分类问题

     - 拟解决的困难

       如何利用已有的医学报告文本信息进行诊断

     - 解决困难的方法

       用Joint Learning方法，构造一个CNN用于处理图像信息，构造一个RNN(LSTM结构)用于处理文本信息，然后把两个信息进行融合。

     - 达到的结果

       达到了胸部X光图像分类的state of art

   * 文献启示

     文献给出了一个如何将文本与图像信息同时使用的方法。在获取医学图像的过程中，如何对相应的报告进行充分利用一直是一个很大的难题，文献给出了针对这种难题的一个可行结构。

2. Revisiting Dilated Convolution: A Simple Approach for Weakly- and Semi- Supervised Semantic Segmentation[2]

   - 文献内容

     * 针对的问题

       图像半监督或弱监督语义分割问题

     * 拟解决的困难

       如何在粗略知道分割标注(比如只知道Bounding Box或者粗略的分割涂鸦，甚至是只知道一张图像的类别)情况下完成语义分割

     * 解决困难的方法

       文献采用语义分割中的空洞卷积(dilation convolution)方法，选取不同大小的空洞值$d$以在相同的卷积核下提取不同层次的特征，最后文献用classification activation maps方法对这些不同特征通过激活值的置信度检验来获取分割预测。

     * 达到的成果

       文献在pascal voc 2012数据集上对比其它的弱监督与半监督的方法达到了state of art的结果。

   - 文献启示

     这篇文献给出了如何利用空洞卷积提取不同层级特征，并利用这些特征与图像的分类标注进行图像语义分割任务的方法，在医学图像这种缺乏语义标注的情景下很有启发。

3. Unsupervised Feature Learning via Non-Parametric Instance Discrimination[3]

   - 文献内容

     - 针对的问题

       自然图像无监督图像特征提取问题

     - 拟解决的困难

       如何利用无监督方法提取图像并将这种特征与其它做比较

     - 解决困难的方法

       作者将每一张输入图像作为一个单独的类，要求提取的特征要将输入图像尽量分开，然后输入特征之间的相似度就可以变为图像之间的相似度

     - 达到的结果

       这种无监督的方法超过了最先进的ImageNet分类结果

   - 文献启示

     该模型与我们所研究的课题很像，即如何用无监督方法提取图像特征，我们可以尝试进行借鉴。

4. Discover Feature Engineering, How to Engineer Features and How to Get Good at It [4]

   * 文献内容
     - 针对的问题

       什么是特征工程，如何选择好的特征

     * 拟解决的困难

       对特征工程进行一个综述性介绍

     * 内容概要

       文章通过介绍什么是特征工程，如何选择特征以及选择正确的特征与错误特征会产生什么样的差异这三个角度介绍了特征工程，并给出了对图像，文本数据选择特征的方法

   * 文献启示

     本文的特征选择的评价性准则对我们的研究很有帮助


## 参考文献

[1]Wang X, Peng Y, Lu L, et al. TieNet: Text-Image Embedding Network for Common Thorax Disease Classification and Reporting in Chest X-rays[J]. arXiv preprint arXiv:1801.04334, 2018. 

[2]Wei Y, Xiao H, Shi H, et al. Revisiting Dilated Convolution: A Simple Approach for Weakly-and Semi-Supervised Semantic Segmentation[J]. arXiv preprint arXiv:1805.04574, 2018.

[3]Wu Z, Xiong Y, Yu S, et al. Unsupervised Feature Learning via Non-Parametric Instance-level Discrimination[J]. arXiv preprint arXiv:1805.01978, 2018. 

[4]Discover Feature Engineering, How to Engineer Features and How to Get Good at It . by [Jason Brownlee](https://machinelearningmastery.com/author/jasonb/) on September 26, 2014 in [Machine Learning Process](https://machinelearningmastery.com/category/machine-learning-process/) 

[5]Wu B, Zhou Z, Wang J, et al. Joint Learning for Pulmonary Nodule Segmentation, Attributes and Malignancy Prediction[J]. arXiv preprint arXiv:1802.03584, 2018. 







