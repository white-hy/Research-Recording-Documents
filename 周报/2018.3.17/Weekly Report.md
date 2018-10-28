# Weekly Report

**Haozhe Feng**

**2018.3.17**

++++++

[TOC]

## What I have done this week

Since Professor Danny Chen gave me some valuable and attractive suggestions last week,I have done some basic works on them. 

Firstly, I read 4 articles about the **deep adversarial network** and the **state-of-art semantic segmentation jobs**, and they are:

1. Paper[1] describe a method to use the unannotated data and deep adversarial net to improve the performance of semantic segmentation network.
2. Paper[2] is the paper from which paper[1] mainly gets inspirations. Paper[2] proposes a method to use deep adversarial net to criticize segmentation network by evaluate the similarity between ground truth and segment prediction. It also explains why the adversarial strategy successes here( because the traditional semantic segmentation network predict label by per pixel, which lack the consistency in continuity.)
3. Paper[3],[4] proposes a new convolution structure called dilation convolution, and uses it to build a more efficient and precise semantic segmentation network.

Then, I implement the deep adversarial net on my lung nodule dataset. The main problem is choosing a suitable network structure when extend the ideal to 3D dataset.I use the dense block and redesign a network for adversarial net, choose the recommended parameters in paper and train two network. The criticism from adversarial net quite improve the segment network's performance, but the improvement is not very high. I decides to try various combinations of hyper-parameter and see if the strategy works on 3D dataset.

## What I plan to do next week

In this Friday's group meeting, Professor Chen proposes an ideal about suggest annotation. The basic suggest annotation ideal is using iteration method to get the valuable data, it needs experts' annotation in each iteration. A problem is that if we can propose a method which give suggestions of the entire dataset in one step.

****

A natural ideal is that we can **cluster the unannotated dataset** into many different clusters, and one cluster represents a specific feature. We only need to choose some data from each cluster to build a sub-dataset simply and the sub-dataset can achieve good results. In this ideal, **the cluster method is important** . The traditional cluster method is **use some morphology or PCA to encode the picture**, and use k-means or hierarchical method to do clustering work. These encode methods only **focus on several important features** and will loss many useful features, so my ideal is that if we can use deep learning to do cluster for images?

I have read the paper about deep cluster methods[5], which proposes a training method for the encoders of image. My basic ideal is that if we can use **generative adversarial network**  to encode the unannotated images' features and use the encoded features as the input for cluster. **The steps I assume are these:**

1. Train a GAN to encode the feature of the image by generate the similar images of input. I think GAN can do this job.
2. Choose some features from GAN ,use trainable operations(such as fully connection, convention) to combine these features to form the input of cluster.
3. Use the training strategy from [5] to train a cluster

I have some problems on these steps. Firstly, I don't know how to choose correct representative features from GAN. Then, I'm also not sure if I can extend [5]'s strategy in my dataset. 

All of these problems need attempts, and plan to try this ideal next week, and I hope it works.  If I can get some suggestion and guidance on this ideal, it will be very nice and helpful. 

## Reference

\[1\][Semantic Segmentation using Adversarial Networks](https://arxiv.org/abs/1611.08408)

\[2\][Deep Adversarial Networks for Biomedical Image Segmentation Utilizing Unannotated Images](https://link.springer.com/chapter/10.1007/978-3-319-66179-7_47)

\[3\][Multi-Scale Context Aggregation By Dliated Convolutions](https://arxiv.org/abs/1511.07122)

\[4\][Understanding Convolution for Semantic Segmentation](https://arxiv.org/abs/1702.08502)

\[5\][Unsupervised Deep Embedding for Clustering Analysis](http://proceedings.mlr.press/v48/xieb16.pdf)