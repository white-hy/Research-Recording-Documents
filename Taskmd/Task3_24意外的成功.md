# Task 3.24

![52188122715](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\Taskmd\1521881227159.png)

经过训练，模型已经达到了在Test数据集上的良好收敛性。

然后我们得到的几个观点是：

1.模型的拟合能力非常重要

2.图像做Augmentation的过程中需要控制Augmentation与Raw Image的比率。

3.尝试增大Block Width然后再看看有没有什么可行方案

Data Augmentation的比率也是个超参数，一般可以设置为50%，多了的话似乎学不到。不同的Data Augmentation似乎也会出现一些问题。

## 任务

1. 把efficient densenet转移到3D模型上给予实现
2. 把data augmentation的最佳比例计算一下

