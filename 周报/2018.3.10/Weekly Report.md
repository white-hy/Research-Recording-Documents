# Weekly Report

**Haozhe Feng**

**2018.1.8**

++++++

[TOC]

## What I have done this week

This week, I mainly focus on the implementation of the suggest technology in 3D lung nodule segmentation problem, I also pay some time on the thesis proposal and proposal presentation.

I want to do a summary of my work progress, and here are the 3 main points:

* **Finish the building of the dataset**

  I have completed the data clean and built a public lung nodule segmentation database. I select 740 lung nodule and their pixel-wise segmentation labels and cut a $64 \times 64 \times 64$ cube(with one voxel representing $1mm \times 1mm \times 1mm$) around the center of each cube, the database is like this:

  ![dababase](C:\Users\Jiazhe Love Junjun\Desktop\Research-Recording-Documents\周报\2018.3.10\dababase.png)

  All the cube are choice by 2 rules:

  1. Annotated by 3 or 4 doctors with pixel level annotation
  2. The diameter of the nodule should be in the range $(5mm,30mm)$

  The final boundary annotation is the union of all doctors annotations. And I erode the union boundary by 2 pixel.

* Tried some networks to do segmentation

  I decided to directly train a semantic segmentation networks and straightly extend the 2D problem to 3D problem. I have finish the network model buildings and try different models. I mainly use the state-of-art dense-net to do this task, and the experiment show that this network converge quickly(about 7 minutes), which can be used for the feature proposal and annotation value calculation part.

  However, I also meet a problem ,which is that the generalization of the model is not satisfying, for the test VoE (or dice) can't be reduced below 0.2, which means the model overfits the train data. I change the parameter of dropout, L2 regularization and Batch Normalization part, but the problem still exists. I suppose my model (which converge quickly) is not deep enough, so I add more deep blocks and the validation result has reduced to 0.45.

  I deduce that I may have to use 2 networks to complete suggest annotation, one light network for feature extraction and suggestion, and another deep network for real segmentation.

## What I plan to do next week

This Friday, Professor Danny Chen has come to our lab and given me some useful suggestions, and they are :

1. Yifei Lu has do some work on the suggest system,and Chen said that some other teams also focus on transfer the system from 2D to 3D, so my work may have to compete with their works,which is a little difficult.

2. Since Professor Chen also has given me an article about the adversarial network , he describes another interesting topic which is similar to suggest system ,and it is about how to judge and criticize the result from a network. For example, if I build a network which is used for segment the lung nodule and get some results, how to judge if the result is good is a big problem. We may need an expert to tell us which part of the result need to be improved, so that we can improve our segment system. 

   It's obviously that the information from an expert is very expensive ,and a natural ideal is that we can use the origin data to build a model to do the judgement, or we can build a model to suggest some areas for doctor to judge,which is similar to suggestion system.

Danny suggests that I can focus on the 2th suggestion, to see if we can extend the [Deep Adversarial Network](https://link.springer.com/chapter/10.1007/978-3-319-66179-7_47)'s work, or if we can extend the suggest system to this topic.

Next week I will focus on the 2th part first,and rereading the 1st article to see if there are some ideals to help. 

The main ideal for these 2 extension directions are all use the adversarial thought,which need quite a lot of experiments.
