# Task 3.23

Data Augmentation并没有work。训练曲线呈现这样的态势：

![52178326085](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\Taskmd\训崩的模型1)

简直是FUCK的情况。

反而当我把Flat Cube转成Cube的那段代码写对了的时候，Flat Cube的那个Dataset反而呈现出一种光速下降的态势，甚至测试集上的Test值都能突破0.4的下限了。这到底是为什么呢？

我昨天主要做了2个工作，主要是修改了整个网络关于如何使用更大的Batch进行Adversarial Net训练以及如何读取那些有Augmentation的数据的步骤。结果我发现做了Data Augmentation之后数据集的Loss一直下不去，这可能就是因为神经网络的拟合能力不足了吧。

我重新改了初始训练集的时候似乎也没有这种奇奇怪的成果。这也就是意味着Data Augmentation有点。。看脸？？

可以尝试的有以下几个步骤：

1.换一种Semantic Segnet结构，或者加深当前的层数，或者加深每个Block里的Growth Rate，比如把growthrate从4改为6我觉得这个比较现实。

2.先看看现在怎么样，测试一下程度

3.还有一个问题是训练带Adnet的Segmentation Network的时候应该有VoE与Accuracy两个标准，到底是采取阈值限制法还是采取步长限制法

4.最后一个问题是Adnet的结果怎么样。

我先得做一个比较靠谱的Segnet,然后做一个比较靠谱的Adnet（使用上一次训练出来Segnet的参数），然后对比一下看看结果。

最主要的问题其实还是内存空间的问题，这个问题非常关键，如果Memory炸了的话应该怎么办。所以我需要模拟参数，看看训练1步成不成。

关于Out Of Memory的问题：

测试：

在第一个只训练Segnet的时候，取block width为6，Segnet的Batchsize可以最多达到5

在第二个混合训练Adnet与Segnet的时候，取Block width为6，Adnet的Blockwidth为4，这个时候一个Batch size最大可以达到7

在第三个训练Segnet并用Adnet辅助训练的时候，取Block width为6,Adnet Blockwidth为4，Segnet的Batch size可以取到2，而Adnet只能取到1。

第一第二个都可以，但是主要是第三个的问题。可能是因为Adnet的backward比较耗时？

因此我现在目的是为了改善第三个部分的问题，也就是尝试用efficient densenet试一试。

 ## 阅读优秀的代码可以让人得到极大的进步

3个小目标：

1.把现在的这个demo看懂

2.把Densenet在2D上模拟一个数据跑通

3.把Densenet在3D上改装然后跑通

## 关于模型的问题

模型训崩了，就是训崩了。

 需要用Early Stop策略进行训练，而这里模型就是训崩了

![52179604614](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\Taskmd\训崩的模型2)



FUCKING训崩了的模型。

加入了Data Augmentation之后呢，我发现Test与Train的VoE下降居然惊人地一致了。

可能的原因有3个：

1.Augmented Data的比重太大了，使得模型拟合能力完全不够了，这样就会陷入不知道怎么拟合。

2.模型深度不够。

短期的第一个目标：把单单训练Segnet可以达到的训练成果降低到VoE是0.3以下。这样需要目标多一些，结果好一些。

第二个目标：

在这个基础上训练网络，然后再开始进行Adnet的训练或者各种聚类算法的训练。

Adnet和GAN可以同时进行，先搞GAN再调参数。

关于efficient-densenet的一些问题

1. 可以正常运行。
2. backward还没测试
3. 现在给定的参数只支持32*32图片的输入，输出的是n类
4. ​







