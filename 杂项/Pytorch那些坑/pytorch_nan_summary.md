# Memory Error & Nan Error BUG

这里是一个折腾了非常久的BUG的总结

[TOC]

## 问题简述

我这几天遇到的一个主要bug是关于`nan`的Bug与关于`memory error`的Bug。

这个Bug的主要表现形式是在训练过程中会输出：

```bash
RuntimeError: cuda runtime error (77) : 
an illegal memory access wasencountered at /opt/conda/conda-bld/pytorch_1512386481460/work/torch/lib/THC/generic/THCTensorCopy.c:70
```

这样的错误，或者是在运行的过程中出现Loss = Nan的情况。出现这个Bug后通过把学习率调的非常低的话这个Bug可以得到缓解或者消失，但是在训练了几千个epoch后这个Bug仍然重复出现，这是非常让人头疼的事情。我将我最终找到这个Bug的过程记录，并记录我在寻找Bug过程中所采取的方法，以及学长给的建议。

## 学长的建议与一些有用的小Tips：

1. 用model.train()与model.eval()来将神经网络进行训练与测试的转换

2. 用`optimizer.zero_grad()`来代替把网络的每个参数grad置零的操作

3. 将输入的0-255的图片转换为`-128-128`的图片

4. loss函数能用提供的就不要自己手写

   这一步最终证明解决了问题

## Memory Error与解决办法

我发现我的整个网络的操作中有一步记录式操作是做了一个`to_np`操作。似乎如果当`loss` 为Variable式的`cuda tensor`的时候，如果loss的参数是nan，那么将它转换为np的时候就会遇到一个memory error,但是这个解释似乎也不太对，因为：

```python
>>> a=torch.FloatTensor([float("nan")])
>>> a

nan
[torch.FloatTensor of size 1]

>>> from torch.autograd import Variable
>>> a=Variable(a)
>>> a
Variable containing:
nan
[torch.FloatTensor of size 1]

>>> a.cpu().data.numpy()
array([ nan], dtype=float32)
```

在这一步的时候它正常输出了一个nan，而并没有报错。当然我们并不是简单地把 它print出来，而是让它记在了tensorboard上，可能这个锅可以甩给tensorflow？

## Nan 的由来

在python里面，可以用`float("inf")`与`float("nan")`来给出无穷量inf与nan（not a number）

nan的识别可以用math模块里的`math.isnan(variable)`来进行鉴别。

对于pytorch而言，nan类型是可以求导的，也就是说，以下代码可以运行：

```python
import torch
from torch.autograd import Variable
a=float("nan")
b=torch.FloatTensor([a])
c=Variable(b,requires_grad=True)
d=c*c
d.backward()
d.grad
```

以下会返回一个None的空值，表示这里的梯度是不存在的。因此一旦loss出现了nan，那么会导致由这个nan进行求导的每一个值都是None，None进行运算后会将所有的神经网络参数全部变成Nan，这样就会导致网络崩溃。

## 导致Nan的原因

一开始我以为出现Nan是因为梯度爆炸，但是我发现我错了。梯度爆炸确实可能会导致nan，这是因为梯度爆炸的话假设一个梯度是inf，那么它会将对应的元素改成-inf，然后再运算的时候可能inf-inf就会导致神经网络参数有一个nan，继而导致nan扩散到所有的参数里面。

我针对这个可能性进行了探讨，就是对每一步神经网络的所有参数都用`numpy.isnan`进行分析，看看是不是有一步变成了nan，然后才导致的loss变成nan。这是一个看是否存在梯度爆炸的好思路，以后也可以常常用。用这个思路我证明了我的代码并不是因为梯度爆炸。

然后我发现，这个nan就是因为loss变成了nan而导致的。那么loss为什么会变成nan呢？一个可能的猜想是有几张图片非常不好，使得loss变成了nan。我们应该记录网络参数，然后对每个图片试过去，看看会不会出现loss=nan的操作。

我记录了神经网络参数，对每一张图片都过了一遍，发现并没有什么卵用，所有的图片在model.val()下loss都没有变成nan，而仅仅在model.train()下变成了nan。因为我为了防止梯度爆炸增加了`Batch Normalization`操作，而在`.train 与.val`下BN操作是不一样的。因为.train下BN是当前epoch的迭代结果，而.val下BN则是一个滑动平均，这就导致了train有nan，val没有nan的结果。

以上这些操作可以解决是否图片本身的问题。

那么剩下的唯一一个问题就是loss函数自己的问题了。loss函数中`log[0]`的答案是-inf，然后我们是做平均操作，这就可能导致-inf +inf =nan的情况发生。因此我采用加了一个小量果然nan消失了，或者说我们也可以直接用pytorch自带的loss函数，也可以有效消灭或解决nan。

以上就是我解决nan这个问题的几个思路，查梯度爆炸，查图片本身，查loss函数，只要知道nan是由什么运算产生的就很有思路。

## 遗留问题

为什么调整学习率就可以有效避免nan

现在新的训练中是否可能再出现nan。

## 接下来的任务

学习神经网络调参技巧并写报告。

我发现网络训练收敛的很快，应该可以用于suggest



