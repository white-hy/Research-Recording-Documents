# Decoupled Networks

## 核心思想

神经网络的本质是对特征进行$f(x,w)=w^{T}x$的做法，此时相当于是一个内积操作:

$<w,x>=\lVert w \rVert \lVert x \rVert * cos(\theta)$

此时我们把这个特征操作写成了
$h(\lVert w \rVert,\lVert x \rVert)*g(\theta_{(w,x)})$的过程，其中$h(\lVert w \rVert,\lVert x \rVert) = \lVert w \rVert \lVert x \rVert,g(\theta_{(w,x)})=cos(\theta)$。

本文的核心思想是给出三个传统卷积神经网络的假设：

1. CNN所学到的特征是自然解耦的，但是它并没有把整个过程表现出来。我们可以把整个过程给更加明晰化

2. CNN在类内特征上的差异表现在长度上，在类间特征的差异表现在角度上

3. DCNets比起传统CNN而言，它更容易收敛，更精确

其实大部分可以参考[知乎专栏](https://zhuanlan.zhihu.com/p/37598903)

## 解耦函数介绍

解耦网络

1. 有界解耦算子

- SphereConv: 令 h(\lVert w\rVert, \lVert x\rVert)=\alpha , 解耦形式为
  f_d(w, x) = \alpha \cdot g(\theta_{(w,x)})
- BallConv: 令 h(\lVert w\rVert, \lVert x\rVert)=\alpha\min{(\lVert x\rVert, \rho)/\rho} , 解耦形式为
  f_d(w, x) = \alpha\frac{\min{(\lVert x\rVert, \rho)}}{\rho}\cdot g(\theta_{(w,x)})
  其中 \rho 为饱和度阈值, 球体半径为 \alpha .
- TanhConv: 令 h(\lVert w\rVert, \lVert x\rVert)=\alpha\tanh(\lVert x\rVert/\rho) , 其解耦形式为
  f_d(w, x) = \alpha\tanh\left(\frac{\lVert x\rVert}{\rho}\right)\cdot g(\theta_{(w,x)})



2. 无界解耦算子

- LinearConv: 
  f_d(w, x) = \alpha\lVert x\rVert\cdot g(\theta_{(w, x)})
- SegConv: 用 \alpha 控制 \lVert x\rVert 较小时的斜率, \beta 控制 \lVert x\rVert 较大时的斜率. 
  f_d(w, x) = \left\{\begin{array}{ll}\alpha\lVert x\rVert\cdot g(\theta_{(w, x)}), & 0\leq\lVert x\rVert\leq \rho \\ (\beta\lVert x\rVert+\alpha\rho-\beta\rho)\cdot g(\theta_{(w,x)}), &\rho<\lVert x\rVert \end{array}\right.,
  易知, BallConv 和 LinearConv 是 SegConv 的特例.
- LogConv: \alpha 用于控制对数函数的形状
  f_d(w, x) = \alpha\log(1+\lVert x\rVert)\cdot g(\theta_{(w, x)})
- MixConv: 混合 LinearConv 和 LogConv
  f_d(w, x) = (\alpha\lVert x\rVert + \beta\log(1+\lVert x\rVert)) \cdot g(\theta_{(w, x)})

3. 解耦算子的性质

- 算子半径: 定义 \rho 为 SegConv 的算子半径, 强度函数 f_d(w, x) 在 \lVert x\rVert>\rho 或 \lVert x\rVert\leq\rho 时可以有不同的变化率. BallConv 的半径类似, 而 SphereConv, LinearConv, LogConv 不存在转折点, 所以定义算子半径为 0 .
- 有界性: (1)有界算子可以降低 问题的病态程度 (2)有界算子减小了输出的变差, 一定程度上减弱了 internal covariate shift 的问题.
- 光滑性: 光滑的强度函数可以加快收敛的速度和稳定性.

4. 角度激活函数

这里的角度激活函数主要来自于

- Linear angular activation: 
  g(\theta_{(w, x)})=-\frac{2}{\pi}\theta_{(w,x)}+1
- Cosine angular activation:
  g(\theta_{(w, x)})=\cos(\theta_{(w, x)})
- Sigmoid angular activation: 
  g(\theta_{(w, x)})=\frac{1+\exp(-\frac{\pi}{2k})}{1-\exp(-\frac{\pi}{2k})}\cdot\frac{1-\exp(\frac{\theta_{(w,x)}}{k}-\frac{\pi}{2k})}{1+\exp(\frac{\theta_{(w,x)}}{k}+\frac{\pi}{2k})}
- Square cosine angular activation: 
  g(\theta_{(w, x)})=sign(\theta_{(w, x)})\cos^2(\theta_{(w, x)})
  
  上图给出了强度函数和角度激活函数的曲线.

5. 加权的解耦算子

- 线性加权解耦算子: f_d(w, x) \leftarrow \lVert w\rVert f_d(w, x) .
- 非线性加权解耦算子: 
  - TanhConv: 
    f_d(w, x) = \alpha\tanh\left(\frac{1}{\rho}\lVert x\rVert\cdot\lVert w\rVert\right)\cdot g(\theta_{(w,x)})
    或者
    f_d(w, x) = \alpha\tanh\left(\frac{\lVert x\rVert}{\rho}\right)\cdot\tanh\left(\frac{\lVert w\rVert}{\rho}\right) g(\theta_{(w,x)})



6. 可学习的解耦算子

通过反向传播来学习算子半径 \rho .

优化 DCNets

1. 权重映射

以 SphereConv 为例, 我们计算对于 w 的导数

\frac{\partial}{\partial w}(\hat{w}^T\hat{x})=\frac{\hat{x} - \hat{w}^T\hat{x}\cdot\hat{w}}{\lVert w\rVert}

其中 \hat{w}=w/\lVert w\rVert , \hat{x} = x/\lVert x\rVert (即 w 和 x 均是归一化的, 这样二者的内积才与 SphereConv 解耦算子的形式一致). 应用权重映射 w\leftarrow s\cdot\hat{w} 来控制 \lVert w\rVert 使得梯度不至于太小. 

注意: 权重映射只能使用在标准解耦算子中, 不能应用在加权的解耦算子中, 因为加权的权重会影响函数的形式.

2. 加权梯度

可以通过乘 \lVert w\rVert 到梯度上来避免权重太小

\Delta w = \lVert w\rVert\cdot\frac{\partial}{\partial w}(\hat{w}^T\hat{x})=\hat{x}-\hat{w}^T\hat{x}\cdot\hat{w}

使用加权梯度反向传播也能避免权重范数的影响. 

3. 预训练的权重

解耦算子由于更高的非线性性, 在训练过程中容易陷入局部最优, 所以使用同样结构的 CNN 预训练来初始化 DCNet.

实验

1. 目标识别

- 加权的解耦算子
  
  加权解耦算子的表现整体上不如权重映射和加权梯度
- 优化技巧
  
  线性角度和余弦平方角度均不如余弦角度, 整体上 SegConv 算子配合余弦角度的表现是最好的. 同时解耦算子的网络中均未使用 Batch Normalization.
- 关于 ReLU
  
  没有 ReLU 的余弦平方角度比 Baseline 结果有显著的提高.
- 不同解耦算子的比较
  
  SegConv 的表现是最好的, 整体上不同解耦算子的表现差别不大, 但都要比 ResNet 的 Baseline 好得多. 

2. 对抗生成攻击



对白盒攻击和黑盒攻击解耦网络均比 Baseline 有更高的精度. 
