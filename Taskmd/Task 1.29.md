# Task 1.29

注意到可能很多医生标注了一个结节，那么我们最后聚类的结果需要啥，需要的是一个list，这是一个并查集，如图

```
[1,2,3,4,5,6,7,8,9]
[1,1,1,4,4,4,7,7,7]
```

这样每一类有多少个医生标注就显而易见了。

## 标注任务总结

我通过读取`xml`文档中的标注，将对应的`mhd`文档全部变成一个一个的小cube以实现自己做语义分割的任务。

我最后输出的结果有以下3种：

1. Cube from origin image

   这个是对原图的肺结节的一个切割。我们找到一个肺结节，以其为中心将周围64*64*64的区域全部切割出来，并平铺为64个64*64的小image作为我们需要的原始图像。一般我们在语义分割的时候可以再对这个64*64的图片进行裁剪

2. Pixel wise label for each cube

   对于每一个以某个结节为中心所cut的cube，我们做了一个pixel wise的 binary value label来显示每一个结节的具体轮廓位置以用于做语义分割。注意到一个图有可能有很多的结节（就算是一个64的cube也是如此），我在label中保留了其它的结节，这些信息也可以辅助训练。

3. A dictionary recording each nodule

   最后我们会有一个`.json`文件，记录了一个字典。字典的元素是这样命名的：

   `mhd_id`+`_`+`nodule_id`：

   `zmin:,zmax:,ymin:,ymax:,xmin:,xmax:`

   也就是说记录了每一个我们做成cube的nodule的bbox以及结节id，这是一个很宝贵的文件，我们可以通过这个追溯到原图，得到结节的一切信息。

根据我输出的结果，我可以尝试使用semantic segmentation来进行一些工作了。

几个小Tips：

1. 我们输出的分割图是经过了插值的，即先在原始的图片中填充，再插值回固定尺寸。
2. 我们分割图的填充取的是大水漫灌法，最后取的是所有标注医生的并，然后在此基础上边界腐蚀了2个像素
3. 我们最后的结果进行了筛选，只有3个及以上医生标记过的，bbox的height，width都大于5mm的结节才能入选。

## 网络分析

我初步准备采用deep supervised网络结构进行训练，首先看看它能不能进行分割，先试图训练出一个可以分割的网络。

Deep Supervised网络结构可以参考，它的基本思想是使用了一个Deep Supervised结构进行了分割训练，而最后将训练结果的loss进行拼接。它的损失函数为：
$$
L=L(X,W)+\sum_{d\in D}\eta_dL_d(X,W_d,\hat{w}_d)+\lambda(||W^2||+\sum_{d \in D}||\hat{w}_d||^2)
$$
我们大概可以这样设计：

## 任务

1. 弄清**Automatic 3D Cardiovascular MR Segmentationwith Densely-Connected Volumetric ConvNets**中所用到的网络结构，并改进到我们的网络中来

2. 写出一个网络以及相应的Dataloader，读取我们的输入并进行分割，然后输出相应的结果。

   ​


