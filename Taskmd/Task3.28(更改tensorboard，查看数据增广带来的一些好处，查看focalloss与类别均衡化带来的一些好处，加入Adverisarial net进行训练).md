# Task 3.28

[Python 并行计算](https://abcdabcd987.com/python-multiprocessing/)

## 如何定义损失频率，记录训练数据的频率，记录loss的频率？

注意到一次epoch是50次batch的训练，我们可以采用以下方法来记录batch与训练参数：

1.每个epoch5个batch之后记录一次loss，loss是5个batch 的loss的平均

2.每个epoch后进行一次Test

3.每10个epoch进行一次image test

4.每10个epoch记录一下参数，并且记录test最好的参数

## 给出所谓的提前终止的训练方式：

如何定义这里的提前终止训练呢。

我们发现训练往往会出现一个，当损失函数loss不断下降，但是此时Dice，或者说Test VoE这种东西并没有下降反而上升的时候，我们认为这是提前终止训练。

也就是说，我们这里的Early Stop定义为：

定义一个队列，这个队列只有

## 训练记录

兜兜转转了这么久，还是回到了原点。

已知：

用6的blockwidth进行训练，训练在1-2个小时即可收敛到最小值，但是在这之后会疯狂发散，完全不知道这是为什么。

而且发散的时候会出现loss一直降，但是测度发散的问题。

