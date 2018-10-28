# Task3.29

## 训练问题解释

注意到昨天网络有一个问题是它会先达到一个IoU的高峰，然后从这个高峰处IoU就一直降了下来，如果是过拟合也就罢了，但是问题是它在Train的部分的IoU继续勇攀高峰，Dice持续走低。这说明网络学歪了。

网络学歪了的表现是什么，我打开了test部分，发现网络的问题是把白色的也预测成黑色的，这样loss持续走低但是IoU不会继续降。

因此我需要一个类别均衡器，这应该就是因为黑白不均衡导致的。

## 重构训练数据集

我发现我的网络所预测的结果中有些结果效果非常差是因为它多预测了一个结节，而这些结节并没有被3个以上的医生所同时标注过，但是这个结节应该也是一个结节。这就导致了预测的过程中出现IoU非常低的情况了,而同时原始的gt图像的标注中也可能会出现一些错漏的现象，因此我的网络可以说是训练得比较成功的。

## 任务

1.修改原始网络，取消中间Deep Supervise的结构（或者可以改成Feature Pyramid）结构，但是Densenet似乎已经使用了这样一个想法了，因此这个Feature Pyramid有没有用还是另说，因此我需要预先把这个结构加进去

2.删除Softmax部分，为test与calculate loss部分增加softmax函数。

这样就可以使用`logsoftmax`函数来达成一些目的了

3.设置动态损失函数

注意到我们网络的后期弊端，我们的动态损失函数应该是类别均衡+Batch均衡的损失函数。

类别均衡包括按黑白比例增加黑色与白色的数目

4.设置一套完整的Loss输出以及Early Stop方法

设计loss输出的时候要进行切换，什么时候用这个loss，什么时候用那个要有讲究，总之要有一套备用切换

设计Early Stop：

Early Stop方法旨在模型没有进展的时候停止模型。

很简单的设计：

记录

4.设计一套新的Augmentation方案：

这套方案并不拘泥于带入Augmentation过的图片，它设置一个Augmentation property,按这个概率在Augmentation的参数中选取相应的参数来进行Augmentation，控制这个概率就可以实现训练过程中的多参数化.

5.设计一套新的训练集，测试集，验证集。

对于Segnet来说，需要由有标注的数据集充当训练，测试，验证

对于Adnet来说，需要有用于Segnet训练过的有标注的数据与无标注数据进行训练，为了区分训练过的分割与未训练过的分割。因此Adnet需要无标注的数据集分为训练，验证，测试3个方面，需要用于Segnet训练的有标注的数据集分为训练，验证，测试3方面，并给出比例。

因此我们的Dataloader需要3个proportion，分为：

segnet_train_dset

segnet_test_dset

segnet_val_dset

adnet_annotated_train_dset

adnet_annotated_test_dset

adnet_annotated_val_dset

adnet_unannotated_train_dset

adnet_unannotated_test_dset

adnet_unannotated_val_dset

## 任务结果

设计新的训练，验证，测试集之后效果好了不少，但是在训练方面似乎有一些问题了。新的数据集有1100个训练数据，然后它的测试数据为18:1:1，也就是说大概有50个训练的，50个测试的。

1.概率式的Data Augmentation还没有使用，我们会看一下它使用的效果怎么样。

2.Learning Rate衰减还没有使用，我需要在130个epoch之后减少它的learning rate。

3.设计动态损失函数，没用。Loss会一下子pia一下降低到20/30左右，然后VoE和dice没有啥用。但是我测试过了，如果只是用log softmax 函数进行的话，与bce loss是一个效果。

4.Early Stop方法

Early Stop方法我本来是想要记录5次VoE，然后进行线性拟合，如果拟合趋势是下降或者不动的话就继续训练，如果是上升的话就停止训练。但是这个策略似乎没那么有效。

## 可能的新尝试：

1.采用SGD(Nestrov)代替adam+学习率衰减进行训练。

2.大量采用Augmentation的方法进行训练（还未进行训练，但是现在的问题是训练数据集，测试数据集，验证数据集的损失都上不去了，这应该是学习率的问题，事实上Loss函数都下不去了）



3.尝试加入dice的Focalloss(OK，加入了平方项增大Focals）



## 在Github建立远程push

ssh-keygen -t rsa -C 630153601@qq.com

建立一个远端到这个的ssh

然后复制pub下的密钥到github

 ssh -T git@github.com -i github_rsa

表示把当前的给弄到远端去，这一步也可以通过config里进行配置

最后 git remote add rrd github:FengHZ\Research-Recording-Documents.git

就可以实现github远程ssh了



## 当务之急：

当务之急是实现efficient densenet中的卷积模块并把它扩展到我们的网络上来，解决内存不足的问题。

