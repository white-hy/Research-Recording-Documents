# Learning Rate

学习率会导致一些奇怪的错误，一般来说如果Pytorch中遇到一些难以理解的错误，而这些错误又死活调不出来，那么我们就可以考虑学习率了。

1. Loss爆炸，比如loss出现了NAN，此时查看神经网络的参数就会发现有一个参数出现了NAN，这就是所谓的梯度爆炸。当然这里我们也可以给梯度一个阈值

2. 出现了一个奇怪的错误，是这个

   ```powershell
   RuntimeError: cuda runtime error (77) : 
   an illegal memory access wasencountered at /opt/conda/conda-bld/pytorch_1512386481460/work/torch/lib/THC/generic/THCTensorCopy.c:70
   ```

   这个关于Cuda的Memory Error，出现的时候一般是非常让人摸不着头脑的，我经过细致的拆分才发现它主要是因为学习率过大了。在那个语义分割网络中，我设置学习率为0.01时出现了这个奇奇怪怪的错误，在重设学习率为1e-4后错误消失。

   其实这个问题本质上还是Nan的问题，我发现每次出现这一步的时候前面总会有一个loss变成了nan，所以就出现了这个问题。

   那么Loss为什么会变成Nan呢？难道是训练某张图片的时候出了问题？

   因此我需要在训练的时候分清：

   1.是不是某个图片的问题

   2.如果不是图片的问题，哪里出现了Nan


Error Information:

```shell
Begin the Epoch:0, Index:27's training
Epoch :0,Index:27,Loss:[ nan]
/home/feng/Suggest_Annotation_In_Object_Detection/Dataset/pixel-wise-cube/1.3.6.1.4.1.14519.5.2.1.6279.6001.252814707117018427472206147014_IL057_160776.png /home/feng/Suggest_Annotation_In_Object_Detection/Dataset/pixel-wise-cube/1.3.6.1.4.1.14519.5.2.1.6279.6001.252814707117018427472206147014_IL057_160776_label.png
Begin the Epoch:0, Index:28's training
THCudaCheck FAIL file=/opt/conda/conda-bld/pytorch_1512386481460/work/torch/lib/THC/generic/THCTensorCopy.c line=70 error=77 : an illegal memory access was encountered
Traceback (most recent call last):
  File "main.py", line 40, in <module>
    train_segnet.train()
  File "/home/feng/Suggest_Annotation_In_Object_Detection/train_test/train.py", line 164, in train
    print("Epoch :{},Index:{},Loss:{}".format(self._epoch, self._index, self.to_np(loss)))
  File "/home/feng/Suggest_Annotation_In_Object_Detection/train_test/train.py", line 256, in to_np
    x = x.cpu()
  File "/usr/share/Anaconda3/lib/python3.6/site-packages/torch/autograd/variable.py", line 301, in cpu
    return self.type(getattr(torch, type(self.data).__name__))
  File "/usr/share/Anaconda3/lib/python3.6/site-packages/torch/autograd/variable.py", line 285, in type
    return Type.apply(self, t)
  File "/usr/share/Anaconda3/lib/python3.6/site-packages/torch/autograd/_functions/tensor.py", line 181, in forward
    return i.type(dest_type)
  File "/usr/share/Anaconda3/lib/python3.6/site-packages/torch/cuda/__init__.py", line 370, in type
    return super(_CudaBase, self).type(*args, **kwargs)
  File "/usr/share/Anaconda3/lib/python3.6/site-packages/torch/_utils.py", line 38, in _type
    return new_type(self.size()).copy_(self, async)
RuntimeError: cuda runtime error (77) : an illegal memory access was encountered at /opt/conda/conda-bld/pytorch_1512386481460/work/torch/lib/THC/generic/THCTensorCopy.c:70
```

Fig1:

很小很小很小的白点

Fig2:

很小很小很小的白点

Fig3：

很小很小的白点

