# Weekly Report

**Haozhe Feng**

**December 24,2017**

+++++

[TOC]

## The Tasks I have finished this week

This week, I quickly finish some works I previously left and focus on our suggest annotation task. 

I simply review some articles I have read and tried to implement to figure out which tools we have had some understanding and could be used in our research target. We all remember the idea professor Chen stressed that *the best way to learn a tool is to use it in our specific research problems*.

I also review and summarize our previous several discussions with professor Chen, and proposes the main ideal of our extension of suggest annotation research.

## A detailed research plan and some problems 

### Research targets and conditions

Our main research target is to do an extension of suggest annotation, and the extension part is mainly focused on 2 ideals :

1. Extend the 2D images to 3D images
2. Extend the semantic segmentation task to object detection or instance segmentation task.

Our target dataset is the lung nodule dataset from **LIDC** and **Shanghai Lung Hospital**, and we want to develop a system which can only use half or less data from the original dataset to achieve the same goal. So our task should satisfy these 2 conditions:

1. Our task's practical application is to reduce the cost of obtaining data labels, so the label part (which is always the annotation tasks for expert) should be complicated and consume much time. On this way can our system reduce a lot of workload.
2. Our task's base network model had better be an existed successful model so that we can focus on the suggest part. And the model should also be simple and easy to train.

### Some problems

#### Model structure problem

Based on the 1st condition, we think our task should be an **instance segmentation task**. I find that the detection of lung nodule is not a time-consuming task( just like many image classification labels), and the time consuming parts are **the segmentation part**. I guess that if we only do a suggestion on detection field, it may not have the practical application, so our system had better base on the instance segmentation problem, which can be used in many detection with segmentation tasks in medical image analysis field.

Based on the above conclusion and the 2nd condition, it is hard to find a useful and easy-training network which satisfy our 3D lung nodule instance segmentation task, so we may have to use part of the exist network structure and design a special network to achieve our goal(it may cost some time, but I for the moment have no idea how to get around this problem), and I figure out 2 possible ways to solve it:

1. For our problem is based on a 3D instance segmentation problem, we can extend the latest **Mask R-CNN**'s structure directly to our problem. Mask R-CNN is a successful instance segmentation model in 2D multi-class task based on the structure of **Faster R-CNN**, and we need to extend the 2D task into 3D task. It may be a good ideal, but no one have tried it and we may have some problems during the extension steps.
2. For our problem is based on lung nodule, we can use the 2017 kaggle data science bowl's [1st lung nodule detection code](https://github.com/lfz/DSB2017).However, the main 2 problems are：
   * The code is based on the kaggle's competition task ,which is not an instance segmentation task of lung nodule and if we want to use this structure, we should do some extension on the original model.
   * The aim of the code is to win the competence, and it leads to an important problem that most models don't care about the efficiency and only focus on the accuracy, which is contradict to our suggest annotation system.

So we may have to spend some time doing research on the speed-accuracy tradeoff and designing another structure based on the 2 structures above, and the article [Speed/accuracy trade-offs for modern convolutional object detectors](https://arxiv.org/abs/1611.10012) illustrates some experience and ways about it.

#### Dataset problem

We firstly want to use our own dataset from **Shanghai Lung Hospital**, but I come across some problems when cleaning it, and it is mainly on the annotation part.

The annotation of our dataset is like this:

| series_id  | diagnosis                         | date | anno1       | anno2                        |
| :--------- | :-------------------------------- | :--- | :---------- | :--------------------------- |
| 0800405883 | 左上叶：原位腺癌 <br> 左下叶：原位腺癌            |      | 346 248 50  | 435 231 318                  |
| 6011237880 | 右中叶结节1：肺内淋巴结  <br>右中叶结节2:原位腺癌     |      | 142 155 276 | 147 219 258                  |
| T12150146  | 右中叶：原位腺癌. <br>右上叶前后段结节：<br>纤维组织增生 |      | 171 248 107 | 134 219 91<br>（未手术 但应该是原位腺癌） |

The anno1 and anno2 represent the different lung nodules in one patient, and the main problem is that the annotation only gives the center coordinate of lung nodule and lack valuable information in diameter or lung nodule boundary, so it is hard for us to use this dataset in instance segmentation. Maybe we can use adversarial network structure to use it, but it may not have strong connection with our suggestive annotation target. 

So I think we may use the LIDC dataset with a good segmentation label for each nodule to develop our system first, and for our own dataset, we may have to spend some time in complete the annotation label of it. 

## A timetable of research

Here I list a time table of my research, and the final goal is to submit a paper to conference ECCV or ACCV or Journal Medical Image Analysis in June 2018.

### Task at hand

#### Task Target

1. Pass the exam week
2. Finish the data preprocessing job of LIDC dataset
3. Design the model structure of our extend suggest annotation system

#### Task Content

For the target 1, I should carefully review my courses this term.

For the target 2 and 3, I should study the 1st implementation of kaggle data science bowl 2017 and mask r-cnn structure. I also need to study about how to improve the efficient of our model(which can sacrifice some accuracy)

#### Deadline

For the target 1, the exam week will end in 25th January.

For the target 2, I will finish it before winter holiday

For the target 3, It may need a lot of experiment and I will finish the reading of existing literature before winter holiday.

#### Current Progress

The 2 and 3 target have just begun this week.

#### Problems and solutions

For the target 2, It is only a code task and what I need to do is read the source code in github and fineturn some parts.

For the target 3, I have mentioned the problems and possible solutions in this weekly report.

### Middle Task

#### Task Target

Find a suitable model structure for our extend suggest  annotation system to solve the instance segmentation problem.

#### Task Content

One of the core problem of our extend suggest annotation system is the model solving efficient instance segmentation task. Just as I mentioned in the weekly report, this model should focus on the trade off of efficiency and accuracy. 

#### Deadline

This task needs a lot of experiment on some different structures, so I plan to finish it in March 2018.

#### Current Progress

The target 3 in **Task at hand** part is the prepare of this target, and have begun this week.

#### Problems and solutions

I have mentioned some problems and possible solutions of it in this weekly report,

### Long-term Task

#### Task Target

Build the extend suggest annotation system in lung detection and instance segmentation task

#### Task Content

Build the extend suggest annotation system in lung detection and instance segmentation task, it should achieve the goal that we can only use about 30% data from dataset to train the instance segmentation model without big loss in accuracy. I want to publish this achievement in conference(ECCV,ACCV) or in journal(Medical Image Analysis) in June 2018.

#### Deadline

June 2018

#### Current Progress

I have begun the tasks at hand this week.

#### Problems and solutions

The main problem of the extend suggest annotation system is the evaluation of the training value of data. The time consumed in retraining a net to get the uncertainty of prediction is too long, and we have to select another way to solve it. 

I want to use the dropout method to measure the uncertainty of each data, which saves the time of retraining, but the result of this method should be tested in experiment.