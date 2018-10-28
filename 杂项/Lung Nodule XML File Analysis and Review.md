# Lung Nodule XML File Analysis and Review

Luna16 Dataset上的`.mhd`是一个集成了所有slice的三维图片，而每一个`.mhd`的命名是对应的xml文档上的`SeriesInstanceUid`，这个id在整个xml文档是唯一的。

每一个`SeriesInstanceUid`有若干个`readingSession`，每一个`readingSession`下有若干`unblindedReadNodule`标签，每一个这个标签代表着一个结节Nodule，每一个Nodule下有若干个`roi`标签，因为结节是分布在很多个z上的，一个`roi`代表一个slice上的Nodule，同时一个`roi`有很多的`edgemap`，代表着不同的边界点，这就是整个Lung Nodule的 XML文件分析。