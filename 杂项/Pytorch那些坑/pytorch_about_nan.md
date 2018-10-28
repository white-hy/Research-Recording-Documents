# 关于nan的一些思考

在python里面，可以用`float("inf")`与`float("nan")`来给出无穷量inf与nan（not a number）

nan的识别可以用math模块里的`math.isnan(variable)`来进行鉴别。

对于pytorch而言，nan类型是可以求导的，也就是说，以下代码可以运行：

```python
import torch
from torch.autograd import Variable
a=float("nan")
b=torch.FloatTensor([a])
c=Variable(b,requires_grad=True)
d=c*c
d.backward()
d.grad
```

以下会返回一个None的空值，表示这里的梯度是不存在的。

因此我们可以通过这个法子跳过或者是说找到哪些图片返回的loss是nan

当然，loss是nan是一个奇怪的问题。一般可能说有一个prediction是0，那么这个时候`torch.log(torch.FloatTensor[0])`也就有可能是-inf，然后-inf+inf就会出现nan的原因。从这一点入手考察。

