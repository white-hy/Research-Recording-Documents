# Weekly Report

**Haozhe Feng**

**2018.4.1**

++++++

[TOC]

## What I have done this week

This week I spend some time on things related to graduation, and don't make much progress on the adversarial topics I mentioned last week. I mainly complete and finished 2 tasks, which fully solved the semantic segment network structure and exceed the state-of-art result:

* Solve the problem of GPU memory and improve the performance of semantic segment network(for 3D lung nodule cube)

  Last week, I figure out a good network structure with hyper parameter using the densely connection, which can achieve 0.73 Dice(the state of art result is about 0.74).But I notice that there are few test images which our network will predict an extremely worse result.

  So I output the ground truth,predict label and raw image, find it is because the network predict some part as nodules which don't exists in ground truth:(**from left to right in order is ground truth,prediction and raw image**)![igure](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\周报\2018.4.1\figure1.png)

  I find that the extra part is a nodule,which is not annotated in ground truth because only 2 of the four doctors annotate this nodule,and in ground truth I only select these nodules annotated by more than 3 doctors. So I re-cluster the annotated nodules and set the parameter from 3 to 2 to include more nodules. This work extend the lung nodules from 740 to 1200 nodules, and the original network I proposes can't fit the extend dataset, so I decide to use more deep network, and the memory problem is the main problem need to solve.

  I  reduce the  memory consume of dense net structure by a factor of 5, so that I can try more deep network. Now I use a deep dense net with 3 denseblocks, each block has 12 layer and the growth rate is 12. I use drop rate 0.3, change all ReLu module to PReLu module, and augment the image with rotate and affine transform  and set the augment probability with 1/3, and **reach 0.76 Dice in test dataset and 0.74 in validation dataset**,which means I finally have a fully trained segment network to do next step's explore.

* Rebuild the unannotated dataset

  I have receive a fresh lung nodule dataset from Shanghai Lung Nodule hospital, which only annotated the center of lung nodule and the cluster of cancer for the nodule. I process this dataset and generate a branch of no pixel-wise annotation's dataset,which can be used for adversarial strategy..

  ​

## What I plan to do next week

Next week I will focus on the implementation of GAN and Adversarial Strategy I mentioned last week, and I think with this usable semantic segment network, I can get some results.