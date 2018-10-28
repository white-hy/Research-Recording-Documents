# SSH and add user in linux

[TOC]

## 超有用的ssh

[SSH 基本用法](https://zhuanlan.zhihu.com/p/21999778?utm_source=qq&utm_medium=social)

## 配置云服务器

新建账户:

`useradd -m username`

改变账户密码

`passwd username`

如果出现了比如说进去之后一个$，这是指你当前配置的服务器没有shell，

此时你可以使用

`sudo chsh username /bin/bash`

就是把当前账户的shell变一下

或者在`/etc/passed`文件中将自己的账号后添加`/bin/bash`字符串

免密登陆一样的

















