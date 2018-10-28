# Weekly Report

**Haozhe Feng**

**December 30,2017**

+++++

[TOC]

## The Tasks I have finished this week

This week, I mainly focus on the review for final exam. In research, I mainly focus on some articles related with my research targets since I don't have much time practicing some ideas and write code.

I notice that although I have finished the **Faster R-CNN** task in VOC dataset (and learn some basic deep learning tricks), the Faster R-CNN structure itself is a little old structure(proposed in 2015) and there have been many improvement on it. So I read some papers about some extensions and improvement of the structure and write some reports about it, and they are:

1. [R-FCN](https://arxiv.org/abs/1605.06409)

   R-FCN is an improvement of Faster R-CNN in ROI pooling part. FCN is short for fully conventional network, which means all computation in network is convolution operation. It solves the last redundant part of Faster R-CNN, which is the fully connection part.

2. [Feature Pyramid Networks for Object Detection](https://arxiv.org/abs/1612.03144)

   FPN may be the first net proposing a way to use pyramid feature maps which is used in deep supervision structure to accelerate the convergence of loss function. 

3. [Focal Loss](https://arxiv.org/abs/1708.02002)

   After totally know the basic knowledge and main schools in Object Detection, I review this article and understand many new things about the advantages and draw backs in the one-stage and two-stage detection methods. It gives me a good reference in choosing methods.

4. [Speed/Accuracy trade-offs for modern object detectors](https://arxiv.org/abs/1611.10012)

   It is a very very long article describe the speed and accuracy trade-off in the model network structures. It provides rich and useful experiments in training details and efficiency for different models.

These 4 articles quite helps me learn more about the Object Detection field, and give me some intuition on how to choose model to solve our extend Suggest Annotation targets.

## The tasks I want to do next week

Next week, I will still focus on the final exam(and some course projects), but I will also spend as much time as I can on the research. 

I will follow the time table, and I also raise some problems in the email. I will correct the time table based on the advices of the problems.