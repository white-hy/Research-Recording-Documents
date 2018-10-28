#  Task 2.27

## 今天的任务主要是对超参数的一个实验，迭代，反省与整理，并根据自己的情况做一些实验来看看哪些Trick起作用了，哪些Trick没有起作用



## 训练总结与问题发现

以每次200张图片，训练了1083时出现梯度爆炸现象。

在这个现象中，我们的学习率被调到了`lr=1e-4`的这个程度，同时从0开始训练，对辅助训练的参数设置为eta=1以及eta_decay=0.99，这样确实在早期起到了让Loss快速下降的用途，也就是说确实加快了网络的收敛。

网络在15000步的时候Loss下降的速度变慢了，这是因为此时eta经过迭代已经变成了0.04，这就使得中间Deep Supervise 的那一部分没有作用了，在失去了深度监督机制后网络收敛变慢。同时Test VOE下降逐渐变慢。

Adam作为自适应学习率方法为什么会造成梯度爆炸呢？考虑Adam的迭代：
$$
m_t=\beta_{1}m_{t-1}+(1-\beta_1)g_t\\
v_t=\beta_2v_{t-1}+(1-\beta_2)g_{t}^{2}
$$
这个在`Some tips of training.md`文件中已经讨论过了，应该不会出现梯度爆炸的情况。那到底是为什么呢？



林志文学长的三个建议：

1.试试在训练后面使用SGD+Momentum方法并主动调节学习率

2.试试10个size的小数据集上快速迭代试试，看看会不会有类似情况复现

3.试试把loss函数改为系统自带的

然后我需要阅读以下内容，做实验并写报告：

1.南大Lambda实验室魏同学的[Must Know Tips/Tricks in Deep Neural Networks](http://lamda.nju.edu.cn/weixs/project/CNNTricks/CNNTricks.html)

2.知乎上的一些内容

3.[Practical Recommendations for Gradient-based Training of deep architectures](https://arxiv.org/pdf/1206.5533.pdf)

4.[What is xavier initialization](http://philipperemy.github.io/xavier-initialization/)

## Xavier

如果初始化参数太小，那么输入信号在每一个单元里就会收缩，以至于最后没有了。如果初始化参数太大，那么信号就会在每一个layer内迅速爆炸，使得最后的输出非常混乱。使用Xavier初始化，能够让权重不太大，也不太小。

其本质就是让输入X与输出Y的方差一致。最后得到的一个结果是权重应该以0为均值，然后方差为$Var(W_i)=\frac{2}{n_{in}+n_{out}}$

据说这个初始化是可以无脑用的？我可以在如果网络跑了好多次然后仍然不收敛的时候用。

## 分步任务

先试试在10个size的小数据集上的结果。

10个小size的数据集上并没有出现梯度爆炸的问题，但是意外出现了一个问题，就是关于Deep Supervised 机制的问题。我使用了Deep Supervised机制，但是并没有加快收敛，反而是减慢了收敛，这个问题是很奇怪的。

而且在10个小size的数据集上收敛得要比大数据集快，这是个很奇怪的问题。

收敛应该是一样快的，应该是深度监督机制出了问题。

我如下设计新的网络，然后看看结果怎么样：

1. 去除原网络中深度监督机制
2. 在网络中增加对于梯度的阈值限制以避免梯度爆炸问题（似乎还是一个不太好用的模块，跳过）
3. 在若干步（500个epoch）后更改Adam方法为SGD with Momentum，以获取更稳定的收敛，或者避免梯度爆炸问题。

### Some tips for training

把输入图片range改为（-1，1）比0-255要更好。0-255分的很散，然后不利于训练。









