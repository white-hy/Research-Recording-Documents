# 2.25日工作计划

首先先把3D的分割Suggest做好，现在的任务就是先把网络训练那块给做好。

1. 复习假期前写的代码，看看能不能训练，以及训练到哪一步了
2. 开始训练，先做一个能分割的网络出来

##  一些训练的笔记

对比两篇文章（第一篇是只有Deep supervised，第二篇是有Densely Connection的网络）

第一篇提出的Loss函数是：
$$
L=L(X;W)+\sum_{d\in D}\eta_dL_d(X;W_d,\hat{w}_d)+\lambda(||W||^2+\sum_{d\in D}||\hat{w}_d||^2)
$$
但是它似乎没有细说训练过程。第二篇提出的网络结构是带稠密连接的，但是在训练过程中，它只是说神经网络先由Gauss分布初始化，由随机梯度下降Momentum方法进行优化，初始学习率设置为0.05。学习率下降策略是
$$
(1-\frac{iter}{max_{iter}})^{power} *lr
$$
最大迭代次数取为15000,而幂次取为0.9。这也就是说这是一个递减函数。

但是里面没有详细说明两部分参数应该怎么取的问题。

在我今天的训练过程中，我的损失函数暂时为没有正则化的损失函数，即为
$$
L=L(X;W)+\eta_dL_d(X;W_d,\hat{w}_d)
$$
我暂时进行如下的参数设置：

* $\eta_d$是指另外一个预测的权重，它应该在训练过程中衰减，我把它设置为学习率的10倍

* 学习率初始设置为0.1，然后按照上面提到的下降策略（或者这里的最大迭代次数可以取到5000）

* 每一次训练batch size设置为4，每次输入为从数据集中随机选取的200张图片，因此50次训练可以作为一个迭代

* 在train过程用交叉熵作为损失函数，而经过10个iteration后我们应该随机选择50张图片出来test一下。我们需要在test过程中，我们可以用volumetric overlap error 来进行估计。用R代表我们的预测，用G代表GT，那么
  $$
  VOE(R,G)=(1-\frac{R\cap G}{R \cup G})*100\%
  $$
  越小代表损失越小。

* 在每100次后迭代我们需要做一个可视化看看训练得怎么样了，此时我们应该在图片中随机选取10张，对其进行预测，将预测后的输出与真实输出做成图片输出出来，看看怎么样。

今天我需要完成并调试通以上任务。



## Some notes

### FUCK

卧槽过高的学习率会导致一个奇怪的错误，就是

```shell
RuntimeError: cuda runtime error (77) : an illegal memory access wasencountered at /opt/conda/conda-bld/pytorch_1512386481460/work/torch/lib/THC/generic/THCTensorCopy.c:70
```



这个FUCKING DOG 一样的错误。通过调整学习率就可以把这个错误弄好



### 关于Weight  Decay

Weight Decay(wd)是用在优化过程中的衰减系数，可以这么理解：
$$
w_i \leftarrow w_{i}-\eta*\nabla w_i -\eta*wd*w_i
$$

也就是说这是一个正则化项，目的是让权重不至于太大

