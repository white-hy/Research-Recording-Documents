# Learn DBSCAN

这篇文章写的非常好[DBSCAN](http://www.sthda.com/english/articles/30-advanced-clustering/105-dbscan-density-based-clustering-essentials/)

## 算法

整个算法的目标就是去识别稠密区域。如何定义稠密呢？可以认为是对一个给定点周围的object的数目。这就导致了对于DBSCAN有2个重要参数，一个是epsilon，另外一个是minimum points 。eps定义了x附近的一个小圆盘，被叫做x的epsilon领域。minpts表示在这个圆盘的eps半径内的最小点

定义边缘点：

x是一个边界点，如果它的邻居的数目小于最小点的数值，但是它又在某个点z的$\epsilon -neighborhood$内。如果一个点既不是核心点，又不是边界点，那么它就被叫做是噪声点或者是外围点

因此我们可以直接定义几个概念：

1.直接密度可达：

如果点A可以被点B 直接密度可达(direct density reachable)，当且仅当：

​	1.A在B的一个$\epsilon-neighborhood$内

​	2.B是一个核心点

2.密度可达

A是从B密度可达的，如果有一系列核心点可以从B连到A，也就是说中间有C，D，E这些核心点，A被C直接密度可达，C被D直接密度可达，D被E直接密度可达，B被E直接密度可达

3.密度连接

A与B是密度连接的，如果有一个核心点C，A与B都从C密度可达

因此，密度聚类就是被定义为一群具有密度连接性质的点。

一般MinPts数据集比较大也设的比较大，一般至少设置为3



