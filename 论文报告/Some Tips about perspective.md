# Some Tips about perspective

感受野替代公式：

7\*7的感受野可以等价于3个3\*3的感受野的stack

5\*5的感受野可以等价于2个3\*3的感受野stack

3\*3的感受野可以等价于1\*3与3\*1的感受野stack

这个替代公式的来源就是
$$
s_{out}=\frac{s_{in}-kernel+2p}{stride}+1
$$
假设3个3*3感受野进行stack，同时padding为0，那么对于$s_{in}=7$，会得到此时$s_{out}=1$等价于直接进行$7\times7$卷积，这个公式应用到下面就得到了



