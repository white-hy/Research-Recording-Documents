# Weekly Report

**Haozhe Feng**

**December 17,2017**

++++++

[TOC]



## The tasks I have finished this week

For the mid-term examinations of some courses, I don't make some progresses this week. 

This week, I read article [Mask R-CNN](https://arxiv.org/abs/1703.06870) and understand the framework of the article. 

I also figure out some bugs appeared in the 2nd stage of Faster R-CNN. I try to replace the NMS step in Faster R-CNN by using [Focal Loss](https://arxiv.org/abs/1708.02002) part to solve the imbalance of positive and negative samples problem, which is the main problem NMS method tries to solve. But I find that the NMS also solves an important engineer problem, which is  the `out of memory` problem, and focal loss part is restricted by this problem. So, I think that if we want to extend the Object Detection Net into 3D, I have to realize the 3D NMS with cuda. It is not a difficult problem ,but may takes some time.



## The tasks I plan to do next week

With the guidance of Professor Danny Chen, Next week, I will do some researches on the **lung nodule detection and classification** problem with our fresh and accurate dataset from *Shanghai Pulmonary Hospital*.

I will also spend some time reviewing for the final exam, for the up coming exam week.