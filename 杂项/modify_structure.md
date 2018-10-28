## modify code structure

这里我发现adversarial策略与suggest策略完全是2个不一样的东西在，因此我应该新建2个分支去做它。

在Suggest策略里面，我的流程应该是这样的：

1.对数据集进行8：1：1的随机划分为train,test,val数据集

2.对train数据集进行auto encoder decoder的训练

3.用auto encoder decoder 对train数据集进行编码

4.输出编码结果，进行聚类，聚类应该选择指标等

5.用聚类的方法跑3次，看看最后的结果怎么样

6.用随机选的方法跑3次，看最后结果怎么样

7.比较结果，得出一些结论

在这里我需要：

1.做数据预处理部分的时候代入auto encoder decoder 的部分

2.将cluster方法独立写成一个class，用getattr来获得

3.聚类或者实现以上的一些步骤。

实现任务的时候应该规定一个新的路径用来存放我们所需要的东西，比如说我们可以mkdir一个文件夹写着result,然后在里面存储所有需要的文件。对于每一次训练都应该有目的地记录它的tag等信息。一定要做到滴水不漏。

要求：

算了重构代码等一下把

