# Task 4.13

[TOC]

任务是用现在的编码解码器考虑一个数据集与训练集，进行子部训练

这个训练的本质是什么？

如图是我们的自动编码器

![1523592489880](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\Taskmd\Auto Encodecoder)我们能看到，在进行全连接前后各有一层是特征提取层。因为我们的网络过于复杂，因此我们只需要对它进行微调就可以了。也就是说我们需要进行如下操作：

对每一个数据构建一个list,list是["img_name":[Part4 Output,Part 7 Input]

相当于把原来对称的$[X,X']$分成了3个部分$[X,X_1,Z,Z',X_1',X']$这个部分。我们只需要保持$[X_1,Z,Z',X_1']$这4个部分不动就可以了。

因此我们的训练流程为如下流程：

1.先训练一个自动编码机，训练多次

2.用自动编码机在原来的数据集的基础上生成一个新的数据集$X_1$对应了$X_1'$的输出，这个输出为Part 6的输出

3.我们在Linear Trans的中间加入矩阵C，此时我们优化的参数是Linear Part的参数与整个矩阵C，我们采用微调的方式优化参数，要求是让我们的线性表示层+矩阵C之后的特征与$X_1'$尽量接近。

4.用C进行聚类

因此我们需要：

1.构建新数据集

2.在原网络的基础上把Linear Trans部分输出来

3.写一个Linear参数的save与load parameter的接口，也就是说写个新的train函数，train_specular_cluster.

4.写一个原来的小函数，把Linear Part的参数存一下,输出一个新参数

