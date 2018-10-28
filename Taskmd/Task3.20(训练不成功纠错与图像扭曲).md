# Task3.20

我用Batch size =1 进行训练发现结果并不好，甚至在训练数据集上效果都十分不好。训练了11个小时Train的VoE都没有收敛到0.2以下真是Holy Fuck and crap

导致这个情况有2种可能，一种可能是数据集选取的问题，另一种可能是Batchsize选的不对，但是我甚至不知道Batch size 能产生这么大的影响。我现在先选择调大batchsize试一试，看看训练集上的TestVoE是不是能下降，如果不能下降也就GG了。

如果增大Batch Size不成功那我就要和原来的网络进行对比，看看是什么引发了这个不成功了。

## 我今天上午要做好什么

今天上午我主要需要做好data augmentation的工作。我需要研读lfz做data augmentation的代码然后看看怎么样。

如果真的是有关batchsize的部分，那么就要考虑如何减少网络参数，然后把Batchsize给用进来了。如何用进来是一个非常能减少内存消耗的结构然后把它用到我们的网络中是一个值得我们探索的步骤。

## Batchsize的变大确实取得了很大的成功

我将Batchsize变大之后，确实取得了比较大的成功，Loss函数一直在下降，因此我决定先看这篇文章，关于Don't Decay The Learning Rate, Increase the Batch Size.

这是一篇大力出奇迹的神文。好的方面是我大概能够了解batch size，学习率lr,以及momentum参数m三者的关系，并从中指导性选取一些参数。

坏的方面是它这个文章实在是太大力出奇迹了。5000个batch size可以减少训练时间太可怕了。只有google能做出来吧。

 我的Adversarial Net一直做不出来，不知道是为什么。可能是batchsize不够？我可能需要建立一个新的dataloader机制来做batchsize了。先训练着看。

## 主要做Augmentation部分的工作

对3D数据做Data Augmentation。

对于2D的 Data Augmentation包括以下3个部分的东西：

1.沿两边的镜像

2.仿射变换

对于3D可以逐slice做这个操作，也可以做长边翻转等等。

给一个图像可以自己试一试

ok，在2D上基本做好了

收获一个Blog,[神经网络深度理解Blog](http://colah.github.io/archive.html?utm_source=qq&utm_medium=social)

## 关于网络的改进

关于网络的改进则需要加入更大的batch size部分的贡献。

大概有2个方法可以对batch size进行贡献。一方面可以考虑减少稠密连接层的参数，另外一方面也可以选择用向量化的写法给弄到一起去。



