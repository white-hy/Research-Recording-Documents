# Task 3.27

## 远程jupyter notebook映射

如果我们有以下这几个情形之一的话，我们需要使用远程jupyter notebook映射：

1.需要用远程的机器跑一些比较大的函数

2.需要一个草稿纸来记录自己的工作日志

这个时候我们需要远程的jupyter notebook。

所谓远程的jupyter notebook，就是在远端打开一个jupyter notebook，然后在本地打开远程的这个端口。

想要得到这些，我们首先需要对jupyter notebook进行一些简单的配置。主要包括密码的配置，设置窗口，设置固定端口等，具体可以见[Jupyter基本配置](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html).

记得在配置的过程中需要将配置文件的`#`去掉更改才能生效。记得如果要密码的话需要把密钥放在password里面。所有基本配置都在这个官方文档内

然后配置完之后我们敲jupyter notebook会出现一个本地网络，比如如果我们设置的固定端口为1211，那么会跳出一个比如`localhost:1211`这样的本地端口，我们需要用一道命令把这个本地端口映射到我们所想要的端口上来。

我们一般用ssh来达到这个目的，基本命令为:

`ssh -NfL localhost:[localport]:localhost:[remoteport] remoteuser@ip`

这样在本地打开我们的localport，输入密码就可以登陆远程的jupyter notebook了

## 训练所遇到的问题

卧槽我改了个性化定值分配损失函数类别均衡以后训练结果像一坨翔一样，完全达不到原来的要求，我TM根本不知道为什么。现在只能说把老代码拿出来跑跑看看行不行的样子

## TensorboardX学习

所有的标量与记录全部按照`writer.addsth()`里面放进去。最后跑的时候会在当前的目录下生成一个run的文件夹，我们只需要像用tensorboard一样直接run就可以了。里面可以选择哪些要哪些不要，以及有不同的颜色等等。



