# 测试总结

1.前5000步训练C，后5000步训练按参数表

```python
            self.train_time = 6
            self.lr = 1e-3
            self.optim_beta = [0.9, 0.99]
            self.weight_decay = 1e-4
            self.load_flag = True
            self.only_load_lineartrans = True
            self.load_list = [1000, 1000]
            self.Z_lambda = 1
            self.norm_lambda = 1e-1
            self.diag_lambda = 1e4
```

进行训练所得到的结果为 在103数据集上test为0.73，Val为0.71，112上 test 0.7 val 0.74

稍微优越于随机选策略

2.直接训练C，不考虑整个网络的问题了所得到的结果正在尝试,它的参数为

```python
            self.train_time = 7
            self.lr = 1e-3
            self.optim_beta = [0.9, 0.99]
            self.weight_decay = 1e-4
            self.load_flag = True
            self.only_load_lineartrans = True
            self.load_list = [1000, 1000]
            self.Z_lambda = 5
            self.norm_lambda = 1e-1
            self.diag_lambda = 1e4
```

3.直接训练C，修改相应的参数为

```python
            self.train_time = 8
            self.lr = 1e-3
            self.optim_beta = [0.9, 0.99]
            self.weight_decay = 1e-4
            self.load_flag = True
            self.only_load_lineartrans = True
            self.load_list = [1000, 1000]
            self.Z_lambda = 50
            self.norm_lambda = 1e-2
            self.diag_lambda = 1e4
```

1. 梳理一下整个体系

   我们要做一个基于图像相似度聚类的完全无监督的推荐标注策略。这个推荐标注策略是用谱聚类完成的。

   我们现在提出的方案是：

   1.用深度自动编码器来提取特征

   2.用密度聚类+谱聚类进行筛选

   3.max k-cover方法选取具有代表性的图像

这个系统在肺结节分割任务上取得了良好的成果，但是仍然不够：

1.如何把它做到肺结节分类任务上？

这样就需要我们实现一个肺结节分类工具，然后把它试着做到肺结节分类任务上去。

2.深度自动编码器提取图像特征的方法为什么靠谱？你怎么证明它靠谱？

我们为什么用深度自动编码器提取图像特征？

主要是特征提取这一步非常奇怪，如果我们没有一点东西来说明的话似乎没那么有用

